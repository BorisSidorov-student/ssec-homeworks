# Конспект: работа с LUKS

Подключаю новый носитель к VM и проверяю список устройств:

    lsblk -p -o +FSTYPE | grep -v loop

Подготавливаю зашифрованный раздел с помощью `cryptsetup luksFormat`:

    sudo cryptsetup luksFormat -y -v --type luks2 /dev/sdc

В выводе появится предупреждение о перезаписи данных:

    WARNING!
    ========
    This will overwrite data on /dev/sdc irrevocably.
    Are you sure? (Type 'yes' in capital letters):

Далее предлагается придумать кодовую фразу:

    Enter passphrase for /dev/sdc:

Расшифровываем устройство и назначаем ему имя:

    sudo cryptsetup open /dev/sdc secker_disk

Форматирую полученное виртуальное блочное устройство (заполнение нулями):

    sudo dd if=/dev/zero of=/dev/mapper/secker_disk

Создаю файловую систему `ext4`:

    sudo mkfs.ext4 /dev/mapper/secker_disk

Создаю точку монтирования:

    mkdir ./.secret_info

Монтирую раздел:

    sudo mount /dev/mapper/secker_disk ./.secret_info/

Копирую данные в зашифрованную директорию:

    sudo cp *_ps ./.secret_info/

Размонтирую устройство:

    sudo umount /home/vm01/.secret_info

Закрываю виртуальное блочное устройство:

    sudo cryptsetup close /dev/mapper/secker_disk