# Настройка Майнинга Chia на Raspberry Pi 4
Гайд описывает настройку Raspberry PI для майнинга Chia с использованием готовых HDD с плотами. Предполагается, что подключиться к компьютеру с нодой нет возможности.

## Ресурсы
* https://medium.com/geekculture/how-to-use-your-raspberry-pi-as-a-chia-harvester-66440c17d318
* https://github.com/Chia-Network/chia-blockchain/wiki/Raspberry-Pi

##  Подготовка
Для настройки необходимо иметь:
* Raspberry Pi 4
* MicroSD карту
* Адаптер для подключения SD карты к главному ПК
* HDD, заполненные плотами
* Переходники SATA - USB 3.0

## Настройка ОС
Chia ПО работает только с 64-х битными ОС. Можно использовать [2020-08-20-raspios-buster-arm64](https://downloads.raspberrypi.org/raspios_arm64/images/raspios_arm64-2020-08-24/) (протестирована, все работает)
Другие варианты:
* Ubuntu Server 20.04 LTS (Pi 3/4) 64 bit (протестирована сообеществом)
* [rapios-raspios_arm64](https://downloads.raspberrypi.org/raspios_arm64/images/) (версия 2021-05-28 - определяет плоты как невалидные)

Создание образа - [официальный imager](https://www.raspberrypi.org/software/)

## Настройка системы
1. Убедиться, что есть все, для компиляции небинарных файлов: 
```bash
sudo apt-get install -y build-essential python3-dev
```
2. Убедиться, что установлен 64 битный Python
```bash
python3 -c 'import platform; print(platform.architecture())'
```
3. Увеличить [SWAP](https://pimylifeup.com/raspberry-pi-swap-file/)

Опционально можно установить и настроить [TeamViewer host](https://www.teamviewer.com/en/download/raspberry-pi/#:~:text=The%20TeamViewer%20Host%20for%20Raspberry,of%20users%20around%20the%20world.) или [VNC](https://www.raspberrypi.org/documentation/remote-access/vnc/) (доступ внутри локальной сети)

## Установка Chia

```bash
git clone https://github.com/Chia-Network/chia-blockchain.git -b latest
cd chia-blockchain

sh install.sh

. ./activate # произойдет вход в Chia venv. Для выхода нужно ввести команду "deactivate"

chia init

# Можно проинициализировать Chia используя certificate authority с главного ПК. Это позволит не вводить мнемоническую фразу. CA можно найти - [Your User Name]/.chia/mainnet/config/ssl/ca. Скопировать папку на Raspberry и ввести команду
chia init -c path/to/ca

chia plots add -d path/to/harddrive
```

Дополнительно можно установить GUI:
```bash
sh install-gui.sh
cd chia-blockchain-gui
npm run electron &
```