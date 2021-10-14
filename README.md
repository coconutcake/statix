<p align="center">
  <a href="" rel="noopener">
 <img width=200px height=200px src="http://mign.pl/img/logostatix.png" alt="Project logo"></a>
</p>

<h3 align="center">Statix</h3>

<div align="center">

[![Stauts](https://img.shields.io/travis/coconutcake/statix)](https://travis-ci.org/github/coconutcake/statix)
[![Requirements Status](https://requires.io/github/coconutcake/statix/requirements.svg?branch=main)](https://requires.io/github/coconutcake/statix/requirements/?branch=main)

</div>

---

<p align="center"> Zintegrowany projekt aplikacji django na kontenerach dockera do zarzadzania procesami routera Mikrotik poprzez wdrozenie konfiguratora webowego do obslugi ruchu sieciowego.
    <br> 
</p>

## 📝 Zawartość
- [O projekcie](#about)
- [Konfiguracja](#config)
- [Uruchomienie](#getting_started)
- [API](#api)

## 🧐 O projekcie <a name = "about"></a>  

Aplikacja daje mozliwość obsługi ruchu sieciowego na urzadzeniach Mikrotik poprzez panel aplikacji webowej, w którym mozna zdefiniowac reguły i zdarzenia. Projekt oparty jest na bibliotece `Pyshark`. Dzięki takiemu rozwiazaniu mozna na żywo rejestrowac co sie dzieje w sieci i dokonywać momentalnych zmian. Można zastosować na mikrokomputerach z procesorem ARMowych tj. Raspberry PI.


## ⚙️ Konfiguracja <a name = "config"></a>

#### 1. Konfiguracja stacku:

Za pomocą `docker-compose.yml` możliwa jest konfiguracja stacku za pomocą zmiennych środowiskowych dla poszczególnych usług:

#### postgres:

```
# Nazwa bazy danych dla aplikacji
POSTGRES_DB: app

# Nazwa Usera django do logowania na baze postgres
POSTGRES_USER: django_app

# Hasło Usera aplikacji django do logowania na baze postgres
POSTGRES_PASSWORD: asdasd123
```

#### djangoapp:

```
# Adres serwera django
ADDRESS=0.0.0.0

# Port servera django
PORT=8877

# Adres servera nginx na którego bedą wysyłane zapytania (Swagger), zmień na adres cloudowy jeśli pracujesz na chmurze!
SERVER_URL=https://127.0.0.1:5555/

# Silnik bazodanowy dla django
DB_ENGINE=django.db.backends.postgresql

# Nazwa bazy danych postgres
DB_NAME=app

# Nazwa Usera django do logowania na baze postgres
DB_USER=django_app

# Hasło Usera aplikacji django do logowania na baze postgres
DB_PASSWORD=asdasd123

# Adres kontenera z bazą danych
DB_ADDRESS=postgres

# Port bazy postgres
DB_PORT=5432

# Nazwa bazy do testów
DB_TESTS=tests

# Typ uruchomianego serwera opcje: developer, production
APPSERVER=developer
```

#### nginx:

```
# Adres aplikacji django, która zostanie upstremowana do servera nginx
UPSTREAM_APP_URL=djangoapp:8877

# Proxy pass
PROXY_PASS=djangoapp

# Port wystawianego servera HTTP
HTTP_SERVER_PORT=8833

# Port wystawianego servera HTTPS
HTTPS_SERVER_PORT=5555

# Ip lub domena severa (zmiana niekonieczna)
SERVER_NAME=default_server_ip
```

#### 2. Konfiguracja routera Mikrotik:






## 🚀 Uruchomienie <a name = "getting_started"></a>

Wykonaj klona jesli masz juz zainstalowanego dockera:
```
git clone https://github.com/coconutcake/statix.git
```

Po pobraniu klona, przejdz do folderu i zbuduj obrazy poleceniem:

```
docker-compose up --build
```

Aplikacja powinna być dostępna.
Aby zalogować sie na panel administracyjny należy pierw utworzyć konto superadmina.

```
docker exec -it djangoapp sh -c "python3 app/manage.py createsuperuser"
```

jesli uzywasz Windowsa, bedziesz musial użyć winpty:

```
winpty docker exec -it djangoapp sh -c "python3 app/manage.py createsuperuser"
```


Aby sworzyc token dla utworzonego usera - USER to login (email)

```
docker exec -it djangoapp sh -c "python3 app/manage.py drf_create_token USER" 
```

Możliwe jest równiez utworzenie tokena przez wbudowany CMS


## 🚀 Uwagi końcowe <a name = "result"></a>

- Dla serwera lokalnego ADRES moze być adresem petli zwrotnej - 127.0.0.1, dla cloudowego bedzie do adres servera cloudowego. Pamietaj o konfiguracji `docker-compose.yml` opisanej w sekcji Konfiguracja
- pole <(pk)> w adreach to pk obiektu do ktorego sie odwołujemy
