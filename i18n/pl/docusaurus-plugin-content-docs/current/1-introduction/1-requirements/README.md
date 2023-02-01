---
title: Wymagania systemu YetiForce
preview: requirements.jpg
keywords:
  - serwer
  - wymagania
  - systemu
  - YetiForce
  - LAMP
  - LEMP
tags:
  - serwer
  - wymagania
  - systemu
  - LAMP
  - LEMP
description: Jakie są wymagania dla systemu YetiForce
---

## Wstęp

YetiForce stawia pewne wymagania serwerom WWW - dostosowanie serwera jest kluczowe dla prawidłowej instalacji, jak również stabilnej pracy systemu. Nieprawidłowa konfiguracja serwera jest najczęstszą przyczyną problemów pojawiających się w YetiForce.

Większość profesjonalnych serwerów WWW spełnia wymagania jakie podajemy poniżej. Jeżeli jednak zdarzy się, że Twój serwer nie spełnia wszystkich wymagań, poproś swojego administratora, aby poprawił konfigurację i podaj mu link do tego artykułu. Jeżeli Twój administrator nie chce zmienić konfiguracji, to czas najwyższy przejść na inny serwer, ponieważ to serwer powinien dostosować się do Ciebie, a nie Ty do serwera.

Podczas instalacji system weryfikuje aktualną konfigurację serwera i pokazuje elementy, które są niepoprawne i należy im zmienić parametr. Należy pamiętać, że przedstawiane wymagania poniżej nie są idealne dla każdego, a więc w przypadku bardziej wymagających klientów wymagania te powinny zostać zweryfikowane i zoptymalizowane.

:::tip
Zawsze aktualna i pełna konfiguracja znajduje się na github i w module [Weryfikacja konfiguracji serwera](/administrator-guides/logs/server-configuration/) na wersji stabilnej:

- https://gitstable.yetiforce.com/index.php?parent=Settings&module=ConfReport&view=Index&block=14&fieldid=65
- https://github.com/YetiForceCompany/YetiForceCRM/tree/stable/tests/setup

:::

:::warning
Wymagania są zależne od wersji systemu, przed rozpoczęciem sprawdź jakie wymagania narzuca wersja systemu, którą instalujesz.

Poniżej opisane wymagania dotyczą wersji [![Latest Stable Version](https://poser.pugx.org/yetiforce/yetiforce-crm/v/stable)](https://packagist.org/packages/yetiforce/yetiforce-crm)
![release date](https://img.shields.io/github/release-date/YetiForceCompany/YetiForceCRM)
:::

## Możliwe problemy

import DocCardList from '@theme/DocCardList';

<DocCardList />

## Oprogramowanie bazowe dla serwera (LAMP/LEMP)

- **System operacyjny - Debian, Ubuntu, RedHat, Mint** - działa na większości dystrybucji linuksowych. Nie zalecamy systemu operacyjnego MS Windows, jak również MS Windows Server. Pomimo, że nasz system dobrze sobie radzi na serwerach Windows, to nie są one optymalne pod kątem działania aplikacji WWW.
- **Serwer WWW**

  - **Nginx `1.23` (rekomendowany)** - poprawnie działa również na wcześniejszych wersjach, jednakże zalecamy najnowsze stabilne wersje oprogramowania. Możesz również używać oprogramowania alternatywnego, ale kompatybilnego z tym oprogramowaniem.
  - **Apache `2.4`** - poprawnie działa również na wcześniejszych wersjach tj. `2.1, 2.2, 2.3`, jednakże zalecamy najnowsze stabilne wersje oprogramowania. Możesz również używać oprogramowania alternatywnego, ale kompatybilnego z tym oprogramowaniem.
    :::warning
    System YetiForce nie działa prawidłowo z rozszerzeniem WWW ModSecurity
    :::

- ** Baza danych**

  - **MariaDB `10.6` (rekomendowany)** - zalecamy najnowsze stabilne wersje oprogramowania. Nie zalecamy wersji starszych niż wersja `10.1`.
  - **MySQL `5.7`, `8.0`** - poprawnie działa również na wcześniejszej wersji tj. `5.6`, jednakże zalecamy najnowsze stabilne wersje oprogramowania. Możesz również używać oprogramowania alternatywnego, ale kompatybilnego z tym oprogramowaniem.

- **PHP `7.4` (rekomendowany), `8.0`**, `8.1` (w trakcie testów). Zalecamy używanie najnowszych stabilnych wersji oprogramowania (np. `7.4.x`).

## Wymagania dla silnika baz danych (MariaDB/MySQL)

Pliki konfiguracyjne np. `/etc/my.cnf`, `/etc/mysql/my.cnf`, `/etc/mysql/conf.d/`, `my.ini`

- SQL_MODE nie powinno zawierać STRICT_TRANS_TABLE i ONLY_FULL_GROUP_BY
- ENGINE = InnoDB powinno być dostępne i domyślnie włączone. (wyłącz --skip-innodb)

```ini
[client]
default-character-set  			= utf8

[mysql]
default-character-set  			= utf8

[mysqld]
default_storage_engine			= InnoDB
default-character-set  			= utf8
character-set-server 			= utf8
collation-server     			= utf8_general_ci
init-connect 					= 'SET NAMES utf8'
sql-mode 						= ''
#sql-mode						= "ONLY_FULL_GROUP_BY,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION"
wait_timeout					= 600
connect_timeout					= 600
net_read_timeout				= 600
net_write_timeout				= 600
interactive_timeout				= 600
max_allowed_packet				= 128M
query_cache_size      	  		= 128M
innodb_default_row_format		= 'dynamic'
innodb_lock_wait_timeout		= 600
innodb_large_prefix 			= ON
innodb_file_per_table 			= ON
innodb_ft_min_token_size		= 2
innodb_stats_on_metadata		= OFF
innodb_open_files				= 1000
innodb_io_capacity				= 1000
ft_min_word_len 				= 2
table_open_cache				= 1000
table_definition_cache			= 1400
bulk_insert_buffer_size	= 16M

sort_buffer_size				= 4M
read_buffer_size				= 2M
read_rnd_buffer_size			= 1M
join_buffer_size 				= 16M
max_connections					= 100
innodb_flush_method				= O_DIRECT
```

:::important
Przykładowa konfiguracja MariaDB/MySQL:

- Ostatnia stabilna wersja: https://github.com/YetiForceCompany/YetiForceCRM/blob/stable/tests/setup/db/mysql.cnf
- Wersja deweloperska: https://github.com/YetiForceCompany/YetiForceCRM/blob/developer/tests/setup/db/mysql.cnf

:::

## Wymagania dla PHP

Pliki konfiguracyjne np. `/etc/php/8.0/fpm/php.ini`, `/etc/php/8.0/cli/php.ini`

```ini reference title="Ostatnia stabilna wersja: https://github.com/YetiForceCompany/YetiForceCRM/blob/stable/tests/setup/php/prod.ini"
https://github.com/YetiForceCompany/YetiForceCRM/blob/stable/tests/setup/php/prod.ini
```

```ini reference title="Wersja deweloperska: https://github.com/YetiForceCompany/YetiForceCRM/blob/developer/tests/setup/php/dev.ini"
https://github.com/YetiForceCompany/YetiForceCRM/blob/developer/tests/setup/php/dev.ini
```

### Konfiguracja bibliotek zewnętrznych

#### Obowiązkowe

- imap
- PDO
- pdo_mysql
- mysqlnd (MySQL Native Driver)
- openssl
- curl
- gd
- pcre
- xml
- json
- session
- dom
- zip
- mbstring
- soap
- fileinfo
- iconv
- intl
- SPL
- Reflection
- SimpleXML
- bcmath
- filter
- ctype
- hash

#### Opcjonalne

- exif - Biblioteka zalecana do zwiększenia bezpieczeństwa, umożliwia pracę z metadanymi obrazów
- ldap - Biblioteka wymagana podczas aktywnego logowania za pomocą LDAP/AD
- OPcache - Poprawia wydajność PHP poprzez przechowywanie skompilowanego kodu bajtowego w pamięci współdzielonej
- apcu - Biblioteka przyspieszająca wydajność systemu, zalecana podczas pracy większej ilości użytkowników lub większego obciążenia systemu
- imagick - Biblioteka zalecana do zwiększenia bezpieczeństwa, zabezpiecza i weryfikuje potencjalnie niebezpieczne pliki graficzne
- pdo_sqlsrv - Biblioteka wymagana podczas aktywnej integracji z Wapro ERP lub Comarch

#### Zabronione

- uopz - rozszerzenie powoduje zawieszanie i blokowanie systemu

### FPM

```ini
php_admin_value[error_log] = /var/log/php_fpm_errors.log
clear_env = no
request_terminate_timeout = 600
pm.process_idle_timeout = 600s;
pm.max_requests = 5000
catch_workers_output = yes
```

:::important
Przykładowa konfiguracja FPM:

- Ostatnia stabilna wersja: https://github.com/YetiForceCompany/YetiForceCRM/blob/stable/tests/setup/fpm/www.conf
- Wersja deweloperska: https://github.com/YetiForceCompany/YetiForceCRM/blob/developer/tests/setup/fpm/www.conf

:::

## Serwer WWW

- Apache: https://github.com/YetiForceCompany/YetiForceCRM/blob/stable/tests/setup/apache/.htaccess
- Nginx: https://github.com/YetiForceCompany/YetiForceCRM/blob/stable/tests/setup/nginx

### Szyfrowanie HTTPS i HTTP2

Zalecamy aby połączenie z serwerem, na którym znajduje się nasz CRM, było zawsze szyfrowane. Jeśli połączenie z serwerem nie zostało nawiązane za pomocą protokołu HTTPS, komunikacja może zostać podsłuchana lub zmieniona przez osoby postronne.

### Uprawnienia do folderów i plików

Uprawnienia do folderów i plików często są głównym źródłem problemów dla osób instalujących aplikacje na swoich serwerach VPS i serwerach dedykowanych. Wszystkie pliki i foldery CRM powinny mieć tego samego właściciela. Zalecamy taką konfigurację plików `644` i folderów `755`, aby wgrane pliki bez zmiany uprawnień na folderach i plikach miały z poziomu przeglądarki pełen dostęp do odczytu i zapisu (aby przeglądarka miała takie uprawnienia). Należy pamiętać, że sama aplikacja podczas pracy wykonuje różne operacje takie jak odczyt, zapis, jak również tworzenie i usuwanie plików. Jeżeli nie wiesz jak prawidłowo skonfigurować uprawnienia, poproś o to swojego administratora, wysyłając mu link do tego artykułu.

- Configuration directory `config/`
- Application data directory `app_data/`
- User privileges directory `user_privileges/`
- Tabdata File `user_privileges/tabdata.php`
- Menu file `user_privileges/menu_0.php`
- User privileges file `user_privileges/user_privileges_1.php`
- Cache Directory `cache/`
- Address book directory `cache/addressBook/`
- Image Cache Directory `cache/images/`
- Import Cache Directory `cache/import/`
- Logs directory `cache/logs/`
- Session directory `cache/session/`
- Cache templates directory `cache/templates_c/`
- Cache upload directory `cache/upload/`
- Vtlib test directory `cache/vtlib/`
- Vtlib Test HTML Directory `cache/vtlib/HTML/`
- Cron modules directory `cron/modules/`
- Modules Directory `modules/`
- Storage Directory `storage/`
- Product Image Directory `storage/products/`
- User Image Directory `storage/users/`
- Contact Image Directory `storage/contacts/`
- MailView attachments directory `storage/OSSMailView/`
- Logo Directory `public_html/layouts/resources/Logo/`
- E-mail client `public_html/modules/OSSMail/`
- Third party libraries `public_html/libraries/`

### Problemy z SELinux

:::warning
Gdy na serwerze jest zainstalowany SELinux w trybie enforcing często występują problemy z uprawnieniami.
Należy zwrócić uwagę na parametry konfiguracyjne:

- httpd_unified
- httpd_can_network_connect

https://www.nginx.com/blog/using-nginx-plus-with-selinux/
https://docs.nextcloud.com/server/latest/admin_manual/installation/selinux_configuration.html
https://www.getpagespeed.com/server-setup/nginx/nginx-selinux-configuration

```bash
sudo setsebool -P httpd_unified 1
sudo setsebool -P httpd_can_network_connect on
```

:::

## Wspierane przeglądarki

Użytkownik powinien mieć zawsze najnowszą wersję przeglądarki, ponieważ tylko najnowsze wersje wspierają najnowsze technologie webowe. Poniżej umieściliśmy przeglądarki w kolejności w jakiej użytkownik powinien dokonywać wyboru. Oznacza to, że np. przeglądarka Safari jest gorszą przeglądarką dla aplikacji YetiForce niż jest Google Chrome.

1. Google Chrome, Microsoft Edge
2. Firefox,
3. Opera, Brave, Vivaldi
4. Apple Safari
5. Internet Explorer
6. Inne

### Dodatkowa konfiguracja z użyciem .htaccess

Część serwerów umożliwia zmianę konfiguracji za pomocą pliku `.htaccess`, poniżej pokazujemy kilka przykładów.

#### Apache module

:::warning
Poniższa konfiguracja działa tylko jak ustawiono w konfiguracji Apache np. `httpd.conf` "AllowOverride Options" lub"AllowOverride All"
:::

```apacheconf
<IfModule mod_php5.c>
	php_flag	log_errors				On
	php_flag	display_errors			Off
	php_value	error_log				cache/logs/phpError.log
	php_value	memory_limit			512M
	php_flag	output_buffering		On
	php_flag	zlib.output_compression	Off
	php_flag	file_uploads			On
	php_value	upload_max_filesize		100M
	php_value	post_max_size			50M
</IfModule>
## FastCGI module
<IfModule fcgid_module.c>
	FcgidIOTimeout		600
	FcgidConnectTimeout	600
	FcgidBusyTimeout	600
	FcgidIdleTimeout	600
</IfModule>
<IfModule mod_fcgid.c>
	IdleTimeout			600
	ProcessLifeTime		600
	IPCConnectTimeout	600
	IPCCommTimeout		600
	BusyTimeout			600
</IfModule>
```

## Dodatkowa konfiguracja z użyciem user.ini

Istnieje możliwość szybkiej zmiany konfiguracji PHP przez utworzenie pliku user.ini w głównym katalogu `$_SERVER['DOCUMENT_ROOT']`, aktualnie wspiera to CGI/FastCGI.

Jednak należy pamiętać, że nie wszystkie parametry można w ten sposób konfigurować (https://www.php.net/manual/en/configuration.changes.modes.php).

Więcej informacji znajduje się na stronie: https://secure.php.net/manual/en/configuration.file.per-user.php

Przykładowy plik: https://github.com/YetiForceCompany/YetiForceCRM/blob/stable/.user.ini

## Wymagania na czas aktualizacji systemu

### MariaDB/MySQL

```ini
connect_timeout = 3600
connect_timeout = 3600
net_read_timeout = 3600
net_write_timeout = 3600
wait_timeout = 3600
wait_timeout = 3600
innodb_lock_wait_timeout = 3600
```

### PHP

```ini
max_execution_time = 3600
max_input_time = 3600
default_socket_timeout = 3600
```

### PHP FPM

```ini
pm.process_idle_timeout = 3600s
request_terminate_timeout = 3600
```

### NGINX

```nginx
server {
   ...
   client_body_timeout 3600;
   send_timeout 3600;

 location ~ \.php$ {
   ...
   fastcgi_send_timeout 3600;
   fastcgi_read_timeout 3600;
   keepalive_timeout 3600;
   proxy_connect_timeout 3600;
   proxy_send_timeout 3600;
   proxy_read_timeout 3600;
 }
 location ~ ^(.+\.php)(.*)$ {
   ...
   fastcgi_send_timeout 3600;
   fastcgi_read_timeout 3600;
   keepalive_timeout 3600;
   proxy_connect_timeout 3600;
   proxy_send_timeout 3600;
   proxy_read_timeout 3600;
 }
}
```