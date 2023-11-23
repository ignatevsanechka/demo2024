# demo2024

## №1

## Описание задания.
Выполните базовую настройку всех устройств:

1) Соберите топологию согласно рисунку. Все устройства работают на OC Linux - Debian
2) Присвоить имена в соответствии с топологией
3) Рассчитайте IP-адресацию IPv4 и IPv6. Необходимо заполнить таблицу №1. При необходимости отредактируйте таблицу.
4) Пул адресов для сети офиса BRANCH - не более 16. Для IPv6 пропустите этот пункт.
5) Пул адресов для сети офиса HQ - не более 64. Для IPv6 пропустите этот пункт.
  
## Топология №1.

![image](https://github.com/ignatevsanechka/demo2024/assets/149755492/f2e3ea82-d5ef-40a7-9fc4-aa9a5abb8841)

### Таблица №1 (разбита на подсети).

| Имя устройства | Итерфейс |  IPv4/IPv6   | Маска/Префикс |       Шлюз       |
| -------------- | -------- | ------------ | ------------- |    ----------    |
|                | ens 192  | 10.12.17.10  | /24           | 10.12.25.254     |
| ISP            | ens 224  | 192.168.0.162| /30           |                  |
|                | ens 256  | 192.168.0.164| /30           |                  |
| HQ-R           | ens 192  | 192.168.0.2  | /25           |                  |
|                | ens 224  | 192.168.0.161| /30           | 192.168.0.162    |
| BR-R           | ens 192  | 192.168.0.130| /27           |                  |
|                | ens 224  | 192.168.0.163| /30           | 192.168.0.164    |
| HQ-SRV         | ens 192  | 192.168.0.1  | /25           | 192.168.0.2      |
| BR-SRV         | ens 192  | 192.168.0.129| /27           | 192.168.0.130    |
### Проверил IP с помощью команды 
```
ip a
```
### Зашёл в файл настройки конфигурации интерфесов
```
nano /etc/network/interfaces
```
### Назначил IP на интерфейсы в соответствии с таблицей адресации
### ISP
```
auto ens192
iface ens192 inet static
address 10.12.17.10
netmask 255.255.255.0
gateway 10.12.25.254

auto ens224
iface ens224 inet static
address 192.168.0.162
netmask 255.255.255.252

auto ens256 
iface ens256 inet static
address 192.168.0.164
netmask 255.255.255.252
```
### HQ-R
```
allow-hotplug ens192
iface ens192 inet static
address 192.168.0.2
netmask 255.255.255.128

allow-hotplug ens224
iface ens224 inet static
address 192.168.0.161
netmask 255.255.255.252
gateway 192.168.0.162
```
### BR-R
```
allow-hotplug ens 192
iface ens192 inet static
address 192.168.0.130
netmask 255.255.255.224

allow-hotplug ens224
iface ens224 inet static
address 192.168.0.163
netmask 255.255.255.252
gateway 192.168.0.164
```
### HQ-SRV
```
allow-hotplug ens192
iface ens192 inet static
address 192.168.0.1
netmask 255.255.255.128
gateway 192.168.0.2
```
### BR-SRV
```
allow-hotplug ens192
iface ens192 inet static
address 192.168.0.129
netmask 255.255.255.128
gateway 192.168.0.130
```
### Сохранил конфигурацию комбинацией клавиш
```
Ctrl+S
```
### Вышел из конфигурационного файла комбинацией клавиш
```
Ctrl+X
```
### Перезагрузил сервис работы с сетью
```
Systemctl restart networking.service
```
### По аналогии делаем тоже самое с другими машинами
## №1.2
### Описание задания
- Настройте внутреннюю динамическую маршрутизацию по средствам FRR. Выберите и обоснуйте выбор протокола динамической маршрутизации из расчёта, что в дальнейшем сеть будет масштабироваться.
### Установка пакета frr
```
apt-get install frr
```
### После установки frr проверил его состояние
```
systemctl status frr
```
![image](https://github.com/ignatevsanechka/demo2024/assets/149755492/cc0335e0-cf5f-4e4e-8666-f5b6c21dd823)
### Зашел в файл
```
nano /etc/frr/daemons
```
### Изменил значение
```
ospfd = yes
```
### Перезагрузил службу frr
```
systemctl restart frr
```
### Зашел в настройку маршрутизации на ISP
```
vtysh
```

![котяра](https://i0.wp.com/dianaurban.com/wp-content/uploads/2017/07/01-cat-stretching-feet.gif?resize=500%2C399&ssl=1)
