# manual_kernel_update
Вся информация по автоматической установке ванильного ядра указана в файле readme.md 
# Ручная сборка ядра

#Устанавливаем зависимости

yum install -y wget gcc flex bison ncurses-devel openssl-devel bc elfutils-libelf-devel perl

cd /usr/src/

скачиваем последнюю версию ядра

echo "Download archive"

wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.5.1.tar.xz

tar xf linux-5.5.1.tar.xz

делаем софтлинк для удобства

ln -s linux-5.5.1 linux

удаляем архив с ядром

rm -rf linux-5.5.1.tar.xz

копируем старый конфиг ядра в папку с новым

cp /boot/config-3* /usr/src/linux/.config

rm -f /boot/*3.10*

cd linux

Создаем конфиг на основе старого

echo "Make oldconfig"

yes "" | make oldconfig

# Собираем и устанавливаем ядро

echo "Start make"

make --jobs=6

make modules

echo "Start install"

make modules_install

make install


# обновляем загрузччик
grub2-mkconfig -o /boot/grub2/grub.cfg

grub2-set-default 0

echo "Grub update done."

# перезагрузка ВМ

shutdown -r now
