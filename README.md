# РУССКИЙ
## Развертывание одноразового VPN сервера

Цель проект - сохранить деньги, если вы не используете VPN каждый день, а только для учёбы, как в моём случае. </br>
Время развертывания: ~10 минут

Плейбук выполняет следующее:
1. Обращается к API хостинг провайдера
2. Создает VPS сервер в США с 1 CPU, 1G RAM, 10G SAS диска
3. После развертывания конфигурирует WireGuard VPN на нём
4. Кидает на новый сервер скрипт который создает новых пиров VPN и генерирует конфиг и QR код с последующей отправкой в телеграм.

## Требования:
1. Telegram Bot. Зарегистрировать бота через @BotFather и отправить боту сообщение.
+ Получить токен бота и ID чата. Положить эти данные в vars/telegramBot.yml
2. Neoserver. Зарегистрироваться по моей ссылке на сайте провайдера https://neoserver.ru/?ref_link=e5833447cf46f3b34cffb2b8204b598b, используйте промокод ```63ouhh22``` и пополните баланс.</br>
ИСПОЛЬЗУЯ МОЮ РЕФЕРАЛЬНУЮ ССЫЛКУ ВЫ ВЫРАЖАЕТЕ МНЕ БЛАГОДАРНОСТЬ ЗА ПРОДЕЛАННЫЙ МНОЙ ТРУД.  
+ Следуйте в "Виртуальные серверы" > "Прочее" > API. Получите ключ и положите его в vars/VPSproviderKey.yml
+ Следуйте в "Виртуальные серверы" > "Прочее" > "SSH ключи". Загрузите заранее сгенерированный публичный ключ. Далее правой кнопкой мыши по ключу и нажмите "Посмотреть код страницы". Из консоли разработчика вашего браузера вы сможете получить id вашего публичного ключа. Положите id ключа в vars/body_serverCreateRequest.json в поле <b>ssh_key</b> </br>
Приватный ключ должен лежать в корневом каталоге рядом с плейбуком с названием private.key

## Как использовать:
1. Запустить
```bash
ansible-playbook vpndeploy --tags create,configure,adduser
```
Эта команда создаст сервер, настроит его, добавит VPN пользователя и отправит данные для подключения к VPN вам в чате телеграм.
Примерно через 10 минут вы получите QR код и конфигурационный файл в чате от вашего телеграм бота.
Отсканируйте QR или импортируйте конфиг в клиентское приложение WireGuard. Переименуйте туннель при необходимости (например, vpn).</br>
Можете начать пользоваться вашим одноразовым VPN сервером.

2. Если нужно больше пользователей, отредактируйте vars/defaultVpnUser.yml </br>
alexander 3 </br>
и запустите
```bash
ansible-playbook vpndeploy --tags adduser
```
Увеличивайте число каждый раз, так как это влияет на IP адрес, который получает пользователь в конфигурации. 2 одинаковых IP адреса буду конфликтовать и пользователи не смогут одновременно подключиться к VPN.

3. После того как попользовались VPN-ом, если он вам больше не нужен, просто удалите его использую команду
```bash
ansible-playbook vpndeploy --tags delete
```
Таким образом, вы не будете платить за VPN в те дни, в которые им не пользуетесь.

# ENGLISH
## OneTime VPN server deployment

This project's aim is to safe your money, if you don't use VPN every day, but sometimes you need it for educational needs for example like for me. </br>
Time to deploy: ~10 min

This ansible playbook:
1. Contacts API of NeoServer hosting provider
2. Creates VPS server in USA with 1 CPU, 1G RAM, 10G SAS disk
3. After deployment configures the newly created server with wireguard VPN
4. Pushes script to that server that is used for adding VPN peers and generates VPN client configuration and QR code with following sending in telegram.

## Requirements:
1. Telegram Bot. Register telegram bot with @BotFather and send a message to it.
+ Get botToken and chatId and put them into vars/telegramBot.yml
2. Neoserver. Register https://neoserver.ru/?ref_link=e5833447cf46f3b34cffb2b8204b598b with my referal link, use promocod ```63ouhh22``` and refill your balance.</br>
Using my referal link you'll say thanks to me for my work.  
+ Go to "Виртуальные серверы" > "Прочее" > API. Get your personal KEY and put it into vars/VPSproviderKey.yml
+ Go to "Виртуальные серверы" > "Прочее" > "SSH ключи". Upload an ssh public key. Then right click on it and and choose "View page code". From developers console of your browser you are able to get an id of your public key. Put the id your public key into vars/body_serverCreateRequest.json in <b>ssh_key</b> field. </br>
Private key must be placed in the root folder of this ansible play with the name private.key

## How to use:
1. Run
```bash
ansible-playbook vpndeploy --tags create,configure,adduser
```
This will create a server, configure it and add a user (create vpn peer) and send VPN connection data to you in telegram.
After about 10 minutes you will receive qr-code and config file in chat from your telegram bot.
Scan QR or import config in WireGuard client app. Rename tunnel if needed (for ex. vpn). Start using your own OneTime VPN.

2. If need more users, change vars/defaultVpnUser.yml </br>
alexander 3</br>
and run
```bash
ansible-playbook vpndeploy --tags adduser
```
Increment number every time as this affects IP address user gets in configuration. Two same IP addresses will conflict and users won't be able to connect to a VPN server at the same time.

3. After using your VPN, if a server doesn't need any more, just remove it with
```bash
ansible-playbook vpndeploy --tags delete
```
So, you won't pay for the days, you don't use vpn.