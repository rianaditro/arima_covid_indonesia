Catatan untuk Project Skripsi : Peramalan Covid-19 menggunakan ARIMA
Step 1 : Ingest Data
Data Source :,
    1. Scrap Web : https://kawalcovid19.com
    2. Spreadsheet : https://docs.google.com/spreadsheets/d/1ma1T9hWbec1pXlwZ89WakRk-OfVUQZsOCFl4FwZxzVw/edit
    3. API : https://documenter.getpostman.com/view/16605343/Tzm6nwoS#f53234f9-efee-493c-b7b7-cafb21dec4a1
    4. Bigquery open data set : bigquery-public-data:covid19_open_data.covid19_open_data"
Untuk penggunaan covid19_open_data cukup perhatikan beberapa kolom ini:
    date
    wikidata_id             // Kodenya bisa di cek di google
    country_code            // ID
    country_name            // Indonesia
    aggregation_level       // 0 = nasional, 1 = provinsi, 2 = kota/kabupaten
    new_confirmed           // Data update h+2
    cumulative_confirmed
    cumulative_deceased
    subregion1_code         // Provinsi, cth : JT
    subregion1_name         // Provinsi, cth : Jawa Tengah
    cumulative_recovered

    Kasus aktif = Total Kasus - (Total Sembuh+Total Kematian)

Contoh :
SELECT
  date AS tanggal,
  new_confirmed AS harian_baru,
  cumulative_confirmed AS total_positif,
  cumulative_deceased AS total_meninggal,
  cumulative_recovered AS total_sembuh,
  (cumulative_coginfirmed - (cumulative_deceased + cumulative_recovered)) AS kasus_aktif
FROM
  `bigquery-public-data.covid19_open_data.covid19_open_data`
WHERE
  country_name='Indonesia'
  AND aggregation_level=0
  AND 
ORDER BY
  tanggal DESC

