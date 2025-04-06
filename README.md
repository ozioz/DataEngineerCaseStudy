# Data Engineer Case Study
## Shipment Data Pipeline with MongoDB, PostgreSQL & Airflow

![ETL Pipeline Diagram](https://miro.medium.com/v2/resize:fit:1400/1*5nQhSFh6Q82nYHv-0sXnAg.png)  
* ETL Pipeline Mimarisi*

---

## 📌 Case Study Objectives
✅ **MongoDB**'de shipment koleksiyonu oluşturma  
✅ **ETL Pipeline** ile veriyi PostgreSQL'e taşıma  
✅ **Airflow** ile orchestration  
✅ **Dockerize** edilmiş çözüm  

---

## 🛠️ Technical Stack
| Component       | Technology          |
|----------------|--------------------|
| Database       | MongoDB 6.0 + PostgreSQL 13 |
| Orchestration  | Apache Airflow 2.5.1 |
| ETL Tools      | PyMongo + SQLAlchemy |
| Containerization | Docker + Docker Compose |

---

## 🚀 Quick Start
### 1. Sistem Gereksinimleri
```bash
docker --version  # >=20.10
docker-compose --version  # >=2.0

Servisleri Başlatma
bash
Copy
docker-compose up -d --build
3. Airflow UI Erişim
🔗 http://localhost:8080

Username: admin

Password: admin

📂 Project Structure
bash
Copy
.
├── dags/
│   ├── data_generation_dag.py  # MongoDB veri üretim DAG'i
│   └── etl_pipeline_dag.py     # ETL DAG'i
├── scripts/
│   ├── data_generator.py       # Test verisi üretimi
│   └── etl_pipeline.py         # ETL mantığı
├── docker/
│   ├── mongo-init.js           # MongoDB şema ayarları
│   └── postgres-init.sql       # PostgreSQL şema
├── Dockerfile                  # Özelleştirilmiş Airflow imajı
└── docker-compose.yml          # Servis konfigürasyonu
🔧 Key Features
1. Data Generation
python
Copy
# Örnek MongoDB Dokümanı
{
  "shipment_id": "SHIP_123",
  "date": ISODate("2023-01-01T00:00:00Z"),
  "parcels": ["P001", "P002"],
  "address": {
    "street": "123 Main St",
    "city": "Istanbul",
    "zip": "34000"
  }
}
2. ETL Pipeline
python
Copy
# Temel İşlemler
1. MongoDB'den UTC timestamp'leri çek
2. 'Europe/Istanbul' zaman dilimine dönüştür
3. PostgreSQL'de 3 normalizasyon tablosuna yaz:
   - shipments (shipment_id PK)
   - parcels (FK: shipment_id)
   - addresses (FK: shipment_id)
3. Airflow DAGs
Airflow UI

🧪 Validation Queries
MongoDB
javascript
Copy
// Koleksiyondaki doküman sayısı
db.shipments.countDocuments({})

// Zaman damgası kontrolü
db.shipments.findOne({}, {date: 1})
PostgreSQL
sql
Copy
-- İlişkisel bütünlük kontrolü
SELECT s.shipment_id, COUNT(p.*) 
FROM shipments s
LEFT JOIN parcels p ON s.shipment_id = p.shipment_id
GROUP BY s.shipment_id;

-- Zaman dilimi dönüşümü
SELECT shipment_id, date AT TIME ZONE 'Europe/Istanbul' 
FROM shipments;
🚨 Troubleshooting
Problem	Çözüm
ModuleNotFoundError	docker-compose down -v && docker-compose up -d --build
Airflow bağlantı hataları	docker-compose restart airflow
PostgreSQL başlamazsa	rm -rf data/postgres


📄 Checklist
MongoDB koleksiyon yapısı

Zaman dilimi dönüşümü

Normalizasyon (3 tablo)

Airflow DAG'leri

Dockerize çözüm

Logging ve hata yönetimi

