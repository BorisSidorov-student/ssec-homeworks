# Конспект: эксперимент с AppArmor

Смотрю статус AppArmor и загруженные профили:

    sudo aa-status

Копирую бинарный файл `man` для резервной копии:

    sudo cp /usr/bin/man /usr/bin/man.backup

Подменяю бинарник `ping` вместо `man`:

    sudo cp /usr/bin/ping /usr/bin/man

Пробую пропинговать адрес `8.8.8.8` через `man` (ожидаемо получаю ошибку доступа):

    sudo man 8.8.8.8

Выгружаю профиль `man` из AppArmor:

    sudo apparmor_parser -R /etc/apparmor.d/usr.bin.man

Снова пробую пропинговать – теперь доступ есть (профиль отключён):

    sudo man 8.8.8.8

Загружаю профиль `man` обратно:

    sudo apparmor_parser -a /etc/apparmor.d/usr.bin.man

После этого доступ к некорректному использованию `man` снова блокируется.