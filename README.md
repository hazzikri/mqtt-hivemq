# MQTT HiveMQ Tutorial 🚀

Tutorial ini membimbing Anda membuat sistem komunikasi sederhana menggunakan **MQTT** dengan broker publik **HiveMQ**, terdiri dari dua skrip: **publisher** (pengirim pesan) dan **subscriber** (penerima pesan).

---

## 📦 Apa Itu MQTT?

> MQTT (Message Queuing Telemetry Transport) adalah protokol ringan berbasis publish/subscribe yang cocok untuk komunikasi antar perangkat IoT.

* Komunikasi berlangsung melalui **broker** (misalnya `broker.hivemq.com`)
* Perangkat bisa menjadi **Publisher** (mengirim pesan) atau **Subscriber** (menerima pesan berdasarkan topik)

---

## 📁 Struktur Project

```
mqtt-hivemq-tutorial/
├── publisher.py       # Mengirim pesan ke topik MQTT
├── subscriber.py      # Menerima pesan dari topik MQTT
├── .gitignore         # Mengabaikan file/folder yang tidak perlu
└── README.md          # Dokumentasi project
```

---

## 🧰 Persiapan

### 1. Clone Repository

```bash
git clone https://github.com/<username>/mqtt-hivemq.git
cd mqtt-hivemq-tutorial
```

### 2. Buat Virtual Environment (Opsional tapi Disarankan)

```bash
python -m venv venv
venv\Scripts\activate   # Windows
# atau:
source venv/bin/activate # Linux/macOS
```

### 3. Install Dependensi

```bash
pip install paho-mqtt
```

---

## 🧪 Menjalankan Aplikasi

### ✅ 1. Jalankan Subscriber

Subscriber akan mendengarkan pesan dari topik `test/topic`:

```bash
python subscriber.py
```

Output:

```
Connected with result code 0
Received `Hello MQTT 0` from `test/topic` topic
```

### 📤 2. Jalankan Publisher

Publisher akan mengirim pesan ke topik yang sama:

```bash
python publisher.py
```

Output:

```
Sent `Hello MQTT 0` to `test/topic`
Sent `Hello MQTT 1` to `test/topic`
```

> 📝 Jalankan publisher dan subscriber di terminal berbeda.

---

## 📜 Kode Program

### subscriber.py

```python
import paho.mqtt.client as mqtt

def on_connect(client, userdata, flags, rc):
    print(f"Connected with result code {rc}")
    client.subscribe("test/topic")

def on_message(client, userdata, msg):
    print(f"Received `{msg.payload.decode()}` from `{msg.topic}` topic")

if __name__ == "__main__":
    client = mqtt.Client()
    client.on_connect = on_connect
    client.on_message = on_message
    client.connect("broker.hivemq.com", 1883, 60)
    client.loop_forever()
```

### publisher.py

```python
import time
import paho.mqtt.client as mqtt

if __name__ == "__main__":
    client = mqtt.Client()
    client.connect("broker.hivemq.com", 1883, 60)
    client.loop_start()
    topic = "test/topic"
    count = 0
    while True:
        message = f"Hello MQTT {count}"
        result = client.publish(topic, message)
        if result[0] == 0:
            print(f"Sent `{message}` to `{topic}`")
        else:
            print(f"Failed to send message to {topic}")
        count += 1
        time.sleep(2)
```

---

## 🛠 Troubleshooting

* ❗ **Warning:**
  Jika muncul pesan `DeprecationWarning: Callback API version 1`, itu aman diabaikan untuk tahap belajar.

* 🔒 Port 1883 harus terbuka, pastikan tidak diblokir firewall atau antivirus.

* ✅ HiveMQ public broker bisa down sewaktu-waktu, jika tidak terkoneksi, coba lagi nanti.

* 🔡 MQTT topik **case-sensitive**: `Test/Topic` ≠ `test/topic`

---

## 📌 Catatan Tambahan

* Untuk data terenkripsi gunakan broker TLS (`port 8883`) dan aktifkan `client.tls_set()`
* Untuk payload JSON: parsing bisa dilakukan di `on_message()`

---

## 🎉 Selesai

Selamat! 🎊 Kamu sekarang sudah berhasil menjalankan komunikasi MQTT menggunakan HiveMQ dan Python.

---

## 🧠 Referensi

* [HiveMQ Public Broker](https://www.hivemq.com/public-mqtt-broker/)
* [paho-mqtt Documentation](https://www.eclipse.org/paho/index.php?page=clients/python/index.php)

---

Created with ❤️ by Hafidz Azzikri
