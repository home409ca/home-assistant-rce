# home-assistant-rce
integration for Rynkowa cena energii elektrycznej (RCE)

**Dodano opóźnienie czasowe przy imporcie danych.
Zdarza się, że HA pobierał dane wcześniej niż był zapis danych do panelu energia - wówczas błędnie była przypisana stawka do godziny.**

**Zmodyfikowano plik calendar.py**

Integracja do Home Assistant pozwalająca na użycie informacji o Rynkowej Cenie Energii
ze strony https://www.pse.pl/dane-systemowe/funkcjonowanie-rb/raporty-dobowe-z-funkcjonowania-rb/podstawowe-wskazniki-cenowe-i-kosztowe/rynkowa-cena-energii-elektrycznej-rce

Integrację należy pobrać do folderu custom_components/rce
i dodać poprzez ustawienia HA - dodaj integrację (nie przez edycje konfiguracji yaml)
![image](https://github.com/PePeLLee/home-assistant-rce/assets/61408245/2fd4b0e5-10ac-48d8-9072-c141a9c8f838)

Integracja jest dostępna jako kalendarz:

![image](https://github.com/PePeLLee/home-assistant-rce/assets/61408245/fb708945-b5b4-4eb9-a991-c913a078aba0)

