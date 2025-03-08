# databricks_weerdata

# 🌦️ Real-time Weerdata Analyse met Databricks 🚀

## 📌 Over dit project
Dit project haalt **real-time weerdata** op via de **OpenWeather API** en verwerkt deze in **Databricks Community Edition**. De data wordt opgeslagen in **Databricks File Store (DBFS)** en geanalyseerd met **PySpark en Databricks SQL**.

✅ **Data ophalen met OpenWeather API**  
✅ **Opslag in Databricks File Store (DBFS) in Parquet-formaat**  
✅ **Data-analyse met PySpark en SQL**  
✅ **Verwijderen van dubbele records**  
✅ **Automatische timestamp per meting**  

---

## 📊 Voorbeeld Data
| Stad      | Temperatuur (°C) | Vochtigheid (%) | Weerbeschrijving | Timestamp             |
|-----------|----------------|----------------|------------------|-----------------------|
| Amsterdam | 11.9           | 59             | Onbewolkt        | 2025-03-08 20:42:05   |
| Rotterdam | 12.03          | 54             | Onbewolkt        | 2025-03-08 20:40:10   |
| Utrecht   | 9.41           | 58             | Onbewolkt        | 2025-03-08 20:40:10   |

---

## 🛠️ Hoe werkt het?
1. **OpenWeather API** haalt actuele weergegevens per stad op.  
2. **PySpark** converteert de gegevens naar een **DataFrame**.  
3. **Data wordt opgeslagen in DBFS (Parquet-formaat)** voor analyses.  
4. **Databricks SQL queries** analyseren trends per stad.  

---

## 🖥️ Installatie & Gebruik
Wil je dit project zelf draaien? Volg deze stappen:

### 🔹 **1. Open Databricks Community Edition**
Registreer gratis op **[Databricks Community](https://community.cloud.databricks.com/)**.

### 🔹 **2. Open een nieuw Notebook en plak de code**
- Kopieer de **Python-code** uit `weerdata_notebook.dbc`.
- Stel je eigen **OpenWeather API-sleutel** in.

### 🔹 **3. Run het script**
Druk op **Shift + Enter** in Databricks om het script uit te voeren.

---

## 🗂️ Databricks SQL Query Voor Data-analyse
Gebruik deze **SQL-query** in Databricks om een overzicht per stad te krijgen:

```sql
SELECT stad, AVG(temperatuur) as gemiddelde_temperatuur
FROM parquet.`dbfs:/mnt/weather_data/weerdata.parquet`
GROUP BY stad
ORDER BY gemiddelde_temperatuur DESC;
