# Secret Sing-Box (SSB)

[**English version**](https://github.com/BLUEBL0B/Secret-Sing-Box/blob/main/Docs/README-EN.md)

### Простая настройка прокси с использованием протоколов Trojan и VLESS и терминированием TLS на NGINX или HAProxy
Данный скрипт предназначен для полной и быстрой настройки защищённого прокси-сервера с ядром [Sing-Box](https://sing-box.sagernet.org) и маскировкой при помощи [NGINX](https://nginx.org/ru/) или [HAProxy](https://www.haproxy.org). Два варианта настройки на выбор:

- Все запросы к прокси принимает NGINX, запросы передаются на Sing-Box только при наличии в них правильного пути (транспорт WebSocket или HTTPUpgrade)

![nginx-ru](https://github.com/user-attachments/assets/19eb8ac7-9feb-41cf-a744-b935546d7b58)

- Все запросы к прокси принимает HAProxy, пароли Trojan считываются из первых 56 байт запроса с помощью скрипта на Lua, запросы передаются на Sing-Box только при наличии в них правильного пароля Trojan (транспорт TCP) — метод [FPPweb3](https://github.com/FPPweb3)

![haproxy-ru](https://github.com/user-attachments/assets/b0a6ade5-df2b-4baf-8ca2-cc52cc5433bc)

Оба варианта настройки делают невозможным обнаружение Sing-Box снаружи, что повышает уровень безопасности.

> [!IMPORTANT]
> Рекомендуемая ОС для сервера: Debian 11/12 или Ubuntu 22.04/24.04. Достаточно 512 Мб оперативной памяти, 5 Гб на диске и 1 ядра процессора. Для настройки понадобится IPv4 на сервере и свой домен, прикреплённый к аккаунту Cloudflare ([Как настроить?](https://github.com/BLUEBL0B/Secret-Sing-Box/blob/main/Docs/cf-settings-ru.md)). Запускайте от имени root на чистой системе. Рекомендуется обновить систему и перезагрузить сервер перед запуском скрипта.

> [!NOTE]
> С правилами маршрутизации для России. Открытые порты на сервере: 443 и SSH.
>
> Данный проект создан в образовательных и демонстрационных целях. Пожалуйста, убедитесь в законности ваших действий перед использованием.
 
### Включает:
1) Настройку сервера Sing-Box
2) Настройку обратного прокси на NGINX или HAProxy на 443 порту, а также сайта-заглушки
3) TLS сертификаты Cloudflare с автоматическим обновлением
4) Настройку безопасности (опционально)
5) Мультиплексирование для оптимизации соединений
6) Включение BBR
7) Настройку WARP
8) Возможность настройки цепочек из двух и более серверов
9) Клиентские конфиги Sing-Box с правилами маршрутизации для России
10) Автоматизированное управление конфигами пользователей
11) Страницу для удобной выдачи подписок

### Настройка сервера:

Для настройки сервера запустите на нём эту команду:

```
bash <(curl -Ls https://raw.githubusercontent.com/BLUEBL0B/Secret-Sing-Box/master/Scripts/install-server.sh)
```

Затем просто введите необходимую информацию:

![pic-1-ru](https://github.com/user-attachments/assets/f1248d11-a930-4acc-9f66-40851ecf380e)

> [!CAUTION]
> Пароли, UUID, пути и другие данные на изображении выше даны для примера. Не используйте их на своём сервере.

В конце скрипт покажет ссылки на клиентские конфиги, рекомендуется их сохранить.

-----

Чтобы вывести дополнительные настройки, введите команду:

```
sbmanager
```

Далее следуйте инструкциям:

![pic-2-ru](https://github.com/user-attachments/assets/be64a418-21dc-4f8d-bd55-97e5518aaf48)

Пункт 5 синхронизирует настройки в клиентских конфигах всех пользователей, что позволяет не редактировать конфиг каждого пользователя отдельно. При добавлении в конфиги новых наборов правил (rule sets) с помощью пункта 5.2, они будут автоматически загружены на сервер, если это наборы правил от [SagerNet](https://github.com/SagerNet/sing-geosite/tree/rule-set).

### Ключи WARP+:

Чтобы активировать ключ WARP+, введите эту команду, заменив ключ на свой:

```
warp-cli registration license CMD5m479-Y5hS6y79-U06c5mq9
```

### Настройка клиентов:
> [!IMPORTANT]
> На некоторых устройствах может не работать "stack": "system" в настройках tun-интерфейса в клиентских конфигах. В таких случаях рекомендуется заменить его на "gvisor" с помощью пункта 4 в sbmanager.

[Android и iOS](https://github.com/BLUEBL0B/Secret-Sing-Box/blob/main/Docs/Sing-Box-Android-iOS-ru.md). Инструкция дана для Android, на iOS интерфейс приложения отличается, но настройки аналогичны.

[Windows](https://github.com/BLUEBL0B/Secret-Sing-Box/blob/main/Docs/Sing-Box-Windows-ru.md). Рекомендован данный способ, так как он обеспечивает более полные настройки маршрутизации, но можно также вставить ссылку в клиент [Hiddify](https://github.com/hiddify/hiddify-app/releases/latest). Если при использовании Hiddify не проксируются некоторые приложения, то измените параметры конфигурации > режим работы > VPN.

[Linux](https://github.com/BLUEBL0B/Secret-Sing-Box#%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%BA%D0%BB%D0%B8%D0%B5%D0%BD%D1%82%D0%BE%D0%B2). Запустите команду ниже и следуйте инструкциям или используйте клиент [Hiddify](https://github.com/hiddify/hiddify-app/releases/latest).
```
bash <(curl -Ls https://raw.githubusercontent.com/BLUEBL0B/Secret-Sing-Box/master/Scripts/sb-pc-linux-ru.sh)
```

### Вы можете поддержать проект, если он полезен для вас:
- USDT (BEP20): 0xe2FeA540a9F1f85C2bfA3e6949c722393B5d636A
- USDT (TRC20): TFN44R1PnhyX29vBqv9Z4cB5wH7MrVyFoC
- Bitcoin (BIP84): bc1qhn2ghk3pcpsrr6l9ywfryvqfzvyx8gs2wnpz89
- Litecoin (BIP84): ltc1q7quvcq3gtlwf2yuk370vhf2syad8ee4we9huj4
- Toncoin (TON): UQCWmIBsU-EZJSH3rhghbtSOtKQBmb5y74mkjbohpDWZ6l-H

Или можете поставить звезду :star:

### Звёзды по времени:
[![Stargazers over time](https://starchart.cc/BLUEBL0B/Secret-Sing-Box.svg?variant=adaptive)](https://starchart.cc/BLUEBL0B/Secret-Sing-Box)
