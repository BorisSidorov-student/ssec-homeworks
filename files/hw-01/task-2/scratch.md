# Конспект команд и файлов для анализа сетевого трафика

## Установка и подготовка
Установка пакета `tshark` из репозитория Ubuntu:

    apt install tshark

## Фильтры в Wireshark
Примеры фильтров:

    _ws.col.info contains "SYN"
    ip.src == 192.168.1.67

## Захват трафика
Просмотр доступных интерфейсов:

    sudo tshark -D

Запуск захвата пакетов на целевом IP-адресе с записью в файл:

    sudo tshark -i vg-bridge -w sync_scan.pcapng

## Команды сканирования Nmap

### SYN-сканирование
    sudo nmap -sS 192.168.1.67

### FIN-сканирование
    sudo nmap -sA 192.168.1.67

### Xmas-сканирование
    sudo nmap -sX 192.168.1.67

### UDP-сканирование (диапазон портов 22-100)
    sudo nmap -sU 192.168.1.67 -p22-100

## Полученные файлы захвата
- `2_syn_scan.pcapng`
- `fin_scan.pcapng`
- `sync_scan.pcapng`
- `syn_scan.pcapng`
- `udp2_scan.pcapng`
- `udp_scan.pcapng`
- `Xmas_scan.pcapng`