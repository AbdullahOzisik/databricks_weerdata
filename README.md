1. OpenWeather API instellen
API Key verkrijgen
Registreer op OpenWeather API

Ga naar je profiel > API Keys

Kopieer jouw persoonlijke API Key

Test de API met een voorbeeld-URL:

bash
Kopiëren
Bewerken
curl "http://api.openweathermap.org/data/2.5/weather?q=Amsterdam&appid=JOUW_API_KEY&units=metric"
2. Databricks Notebook (PySpark)
In het Databricks-notebook gebeurt het volgende:

Ophalen van data met requests.get() (of dbutils.notebook.run)

Parsen van JSON met json.loads()

Transformeren naar een PySpark DataFrame

Selecteren van kolommen zoals temperatuur, luchtvochtigheid, tijd, locatie

Wegschrijven naar Azure Blob Storage met .write.mode('append').csv(...)

3. Azure Blob Storage
Zorg ervoor dat je:

Een Storage Account hebt met een Container

De juiste access key of SAS-token toevoegt aan je Databricks-secret scope of als config

Je outputfolder instelt in het formaat:

php-template
Kopiëren
Bewerken
wasbs://<container>@<storageaccount>.blob.core.windows.net/output/weather/
4. (Optioneel) Power BI integratie
In Power BI kun je een directe verbinding maken met Azure Blob Storage:

Gebruik de connector Azure Blob Storage

Koppel het pad naar je CSV-bestanden

Transformeer eventueel via Power Query

Resultaat
Een werkende data pipeline waarmee weerdata elk uur automatisch wordt opgehaald, opgeschoond en opgeslagen in de cloud. Hiermee wordt de basis gelegd voor real-time dashboards, rapportages of het voeden van andere dataproducten.
