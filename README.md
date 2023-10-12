# home-assistant-rce
integration for Rynkowa cena energii elektrycznej (RCE)

**Dodano opóźnienie czasowe przy imporcie danych.
Zdarza się, że HA pobierał dane wcześniej niż był zapis danych do panelu energia - wówczas błędnie była przypisana stawka do godziny.**

Zmodyfikowano plik calendar.py - przebudowano definicję "async_update" 

    async def async_update(self):
        """Retrieve latest state."""
        now = datetime.now(ZoneInfo(self.hass.config.time_zone))

        now = now.replace(minute=2, second=0)
        if now < self.last_network_pull + timedelta(minutes=30):
            return
        self.last_network_pull = now
        self.cloud_response = None
        await self.hass.async_add_executor_job(self.fetch_cloud_data)

        if self.cloud_response is None or self.cloud_response.status_code != 200:
            return False
        self.ev.clear()

        csv_output = csv.reader(self.cloud_response.text.splitlines(), delimiter=";")

        now = now.replace(minute=2).replace(second=0)
        self.csv_to_events(csv_output, now)

        self.cloud_response = None
        await self.hass.async_add_executor_job(self.fetch_cloud_data_1)

        if self.cloud_response is None or self.cloud_response.status_code != 200:
            return False

        csv_output = csv.reader(self.cloud_response.text.splitlines(), delimiter=";")

        now = now.replace(minute=2).replace(second=0) + timedelta(days=1)
        self.csv_to_events(csv_output, now)

Integracja do Home Assistant pozwalająca na użycie informacji o Rynkowej Cenie Energii
ze strony https://www.pse.pl/dane-systemowe/funkcjonowanie-rb/raporty-dobowe-z-funkcjonowania-rb/podstawowe-wskazniki-cenowe-i-kosztowe/rynkowa-cena-energii-elektrycznej-rce

Integrację należy pobrać do folderu custom_components/rce
i dodać poprzez ustawienia HA - dodaj integrację (nie przez edycje konfiguracji yaml)
![image](https://github.com/PePeLLee/home-assistant-rce/assets/61408245/2fd4b0e5-10ac-48d8-9072-c141a9c8f838)

Integracja jest dostępna jako kalendarz:

![image](https://github.com/PePeLLee/home-assistant-rce/assets/61408245/fb708945-b5b4-4eb9-a991-c913a078aba0)

