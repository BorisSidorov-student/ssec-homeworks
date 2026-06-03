# Конспект по использованию eCryptfs

Создание зашифрованной директории:

    # создаю директорию в которой будет шифроваться информация
    mkdir .encrypted_dir

    # создаю директорию, для точки монтирования зашифрованной директории
    mkdir decrypted_dir

    # создаю кодовую фразу для связки ключей ядра (фразу нужно запомнить)!
    ecryptfs-add-passphrase
    Passphrase: (запоминаем фразу!) 
    Inserted auth tok with sig [e3ecf1c1889b2615] into the user session keyring

    # полученную сигнатуру пример [e3ecf1c1889b2615], нужно записать!
 
    # монтирую директорию для зашифрованных данных в точку монтирования созданную ранее
    mount -t ecryptfs /home/vm01/.encrypted_dir/ /home/vm01/decrypted_dir/
    Select key type to use for newly created files: 
     1) passphrase
     2) tspi
    Selection:  # выбираем 1, кодовое слово которое придумывали ранее

    # выбираем самый современный стандарт шифрования (aes) цифра 1
    Select cipher: 
     1) aes: blocksize = 16; min keysize = 16; max keysize = 32
     2) blowfish: blocksize = 8; min keysize = 16; max keysize = 56
     3) des3_ede: blocksize = 8; min keysize = 24; max keysize = 24
     4) twofish: blocksize = 16; min keysize = 16; max keysize = 32
     5) cast6: blocksize = 16; min keysize = 16; max keysize = 32
     6) cast5: blocksize = 8; min keysize = 5; max keysize = 16
 
    # выбираем максимальную длину ключа 2
    Select key bytes: 
     1) 16
     2) 32
     3) 24

    # спрашивает можно ли хранить незашифрованные данные, говорим нет
    Enable plaintext passthrough (y/n) [n]:

    # спрашивает, шифровать ли имена файлов, говори да y
    Enable filename encryption (y/n) [n]:

    # применяем сигнатуру которую ранее создали, просто жмем enter
    Filename Encryption Key (FNEK) Signature [e3ecf1c1889b2615]:

    # далее подтверждаем процесс монтирования и соглашаемся сохранить кеш сигнатуры ключа в локальном файле, для того чтобы повторно не спрашивать все эти пункты после повторного монтирования зашифрованной директории

    # далее проверяю точку монтирования новой директории по типу файловой системы
    mount -l -t ecryptfs

    # пробуем что-либо скопировать в директорию ТОЧКУ МОНТИРОВАНИЯ (./decrypted_dir/) не в ДИРЕКТОРИЮ которую монтируем!
    cp -p *_ps ./decrypted_dir/

    # можем проверить что данные скопировались
    $ ls -l ./decrypted_dir/
    total 58316
    -rw-rw-r-- 1 vm01 vm01 59660846 Sep  3  2025 go1.25.1.linux-amd64.tar.gz
    -rw-rw-r-- 1 vm01 vm01       30 Oct 14  2024 user_2_ps
    -rw-rw-r-- 1 vm01 vm01       30 Oct 14  2024 user_5_ps
    -rw-rw-r-- 1 vm01 vm01    12146 Oct 14  2024 user_root_ps

    # далее размонтируем точку монтирования
    sudo umount ./decrypted_dir

    # проверим директорию после размонтирования
    $ ls -l ./decrypted_dir/
    total 0

    # а что если проверить директорию которую монтировали?
    $ ls -l ./.encrypted_dir/
    total 58316
    -rw-rw-r-- 1 vm01 vm01    12288 Oct 14  2024 ECRYPTFS_FNEK_ENCRYPTED.FWbXvD5.W7ga3EZ1tfUXCjBBlKyWrdg.ViPL2mmouzTbCRDdtKXbCvM16U--
    -rw-rw-r-- 1 vm01 vm01    12288 Oct 14  2024 ECRYPTFS_FNEK_ENCRYPTED.FWbXvD5.W7ga3EZ1tfUXCjBBlKyWrdg.ViPLpCYU3fglZhPbNorSfIPUJk--
    -rw-rw-r-- 1 vm01 vm01    20480 Oct 14  2024 ECRYPTFS_FNEK_ENCRYPTED.FWbXvD5.W7ga3EZ1tfUXCjBBlKyWrdg.ViPLZRQ7DGSLCPMGUc.h5asvsE--
    -rw-rw-r-- 1 vm01 vm01 59670528 Sep  3  2025 ECRYPTFS_FNEK_ENCRYPTED.FXbXvD5.W7ga3EZ1tfUXCjBBlKyWrdg.ViPL0H0mBUZ8XcypuZvmno2z85Svj6SY1Wqp5RUEt7JuGpk-

# Автоматизированное шифрование домашней директории

    sudo adduser --encrypt-home new_user

    # переключаемся на нового пользователя и создаем какие-нибудь файлы
    $ su - new_user
    Password:
    $ touch password.txt

    # выходим из-под нового пользователя и заходим от root и пробуем проверить домашнюю директорию нового пользователя
    root@iternal:~# ls -l /home/new_user/
    total 0
    lrwxrwxrwx 1 new_user new_user 56 Jun  2 05:52 Access-Your-Private-Data.desktop -> /usr/share/ecryptfs-utils/ecryptfs-mount-private.desktop
    lrwxrwxrwx 1 new_user new_user 52 Jun  2 05:52 README.txt -> /usr/share/ecryptfs-utils/ecryptfs-mount-private.txt