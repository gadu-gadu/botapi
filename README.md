# Szybki start
BotAPI jest darmową platformą pozwalającą na szybkie i proste tworzenie botów (umożliwiających tworzenie czatów) w sieci GG. Wystawia zestaw funkcji za pomocą protokołu HTTP, które umożliwiają odbieranie i wysyłanie wiadomości poprzez skrypt/program stworzony przez użytkownika BotAPI.

## Najważniejsze cechy platformy BotAPI:
* biblioteka dostępna w języku PHP i Python
* brak limitów
* odpowiadanie na wiadomości przesłane od użytkownika do bota
* wysyłanie wiadomości od bota do użytkownika
* zmiana statusu i opisu bota
* połączenia z platformą mogą być szyfrowane (wsparcie HTTPS)
## Co jest potrzebne do stworzenia własnego bota?
Aby uruchomić własnego bota, będziesz potrzebował:
serwer HTTP, na którym umieścisz swój skrypt (jeżeli nie masz własnego serwera to zobacz jak możesz uruchomić bota na Google App Engine
skrypt wysyłający/odbierający wiadomości zgodnie z protokołem BotAPI
numer Gadu-Gadu (wraz z hasłem) pod którym będzie działał bot.

## Prosty skrypt bota
Najszybciej i najprościej można stworzyć własnego bota korzystając z biblioteki BotAPI dostępnej w wersji PHP i Python. Przykładowy, bardzo prosty, skrypt bota może wyglądać tak:

```php
<?php
require_once('MessageBuilder.php');
$M=new MessageBuilder();
switch (file_get_contents("php://input")) {
    case "cześć": $M->addText('Cześć :)'); break;
    case "kim jesteś?": $M->addText('Jestem botem.'); break;
    default: $M->addText('Nie rozumiem...');
}
$M->reply();
```
Lub tak:
```py
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import cgi
from google.appengine.ext import webapp
from google.appengine.ext.webapp.util import run_wsgi_app
from MessageBuilder import *
from PushConnection import *

class MainHandler(webapp.RequestHandler):
    def post(self):
        M = MessageBuilder()
        msg = cgi.escape(self.request.body)
        if msg == 'cześć': M.addText('Cześć :)')
        elif msg == "kim jesteś?": M.addText('Jestem botem.')
        else: M.addText('Nie rozumiem...')
        self.response.out.write(M.reply(self))

application = webapp.WSGIApplication([('/', MainHandler)],debug=True)

def main():
    run_wsgi_app(application)

if __name__ == "__main__":
    main()
```

Sprawi on, że z naszym botem będzie można rozmawiać tak, jak widać to w oknie z prawej strony. Jeśli ktoś napisze do takiego bota cześć, bot odpowie mu *Cześć :),* jeśli spyta *kim jesteś?*, odpowie *Jestem botem.*. W każdym innym przypadku odpowie *Nie rozumiem....* Oczywiście taki skrypt można własnoręcznie modyfikować i rozszerzać w oparciu o dostępne metody opisane w dokumentacji BotAPI. 

**Pamiętaj o umieszczeniu w katalogu z Twoim skryptem plików biblioteki BotAPI.** W wersji PHP będą to **MessageBuilder.php i PushConnection.php** natomiast w wersji Python odpowiednio **MessageBuilder.py i PushConnection.py**. Bibliotekę BotAPI możesz pobrać z tego repozytorium. Polecamy zapoznanie się z innymi przykładami. Zwróć również uwagę na to, by wszystkie pliki bota były zapisane w kodowaniu UTF-8.

http://boty.gg.pl/

