# About

### Simple Kafka streaming for learning, from Postgres to Postgres using `Change Data Capture`

## Steps

- Install docker & docker-compose
- Jalankan :
    ```
    docker-compose up -d
    ```
- Cek status docker
    ```
    docker ps
    ```
- Install packages connector :
    - Masuk ke docker container `connect` dengan cara :
        ```
        docker exec -it connect bash
        ```
    - Jalankan perintah berikut satu per satu, enter > pilih nomor 2 > pilih `'y'` sampai akhir proses
        ```
        confluent-hub install confluentinc/kafka-connect-jdbc:latest
        ```
        ```
        confluent-hub install debezium/debezium-connector-postgresql:latest
        ```
- Connect ke `postgres1` dan `postgres2`
- Buat dummy table pada `postgres1`
- Buka terminal baru, lalu jalankan untuk melihat log process untuk connector
    ```
    docker logs -f connect
    ```
- Sesuaikan parameter yang ada di dalam file `source_connector_json.json`. Config ini berfungsi untuk connector yang akan membaca data dari source DB (`postgres1`)

- Sesuaikan parameter yang ada di dalam file `sink_connector_json.json`. Config ini berfungsi untuk connector yang akan menulis data ke DB `postgres2`, dan akan membuat table fisik bahkan mengupdate datanya secara otomatis.

- Submit source connector :
    ```
    Method  : POST 
    URL     : localhost:8083/connectors
    Body    : Ada didalam file `source_connector_json.json`
    ```
  Cek lognya

- Open di browser untuk Kafka UI http://localhost:8080, klik broker > topics > klik topicsnya > Messages. Cek apakah sudah ada datanya atau belum, jika sudah lanjut step dibawah ini

- Submit sink connector :
    ```
    Method  : POST 
    URL     : localhost:8083/connectors
    Body    : Ada didalam file `sink_connector_json.json`
    ```
  Cek lognya

- Cek pada DB `postgres2`

