# Dataset used in this project consist of 8 Datasets: 

1. Dataset Mine-Master 
2. Dataset Weather Data
3. Dataset Equipment Inventory
4. Dataset Road Condition 
5. Dataset Effective Capacity
6. Dataset Production Plan
7. Dataset Production Constrains
8. Dataset Shipping Schedule

## Tetang Dataset
### 1. Dataset Mine Master (dummy): Tautan Dataset Mine Master 

Dataset ini berisikan list tambang yang dimiliki perusahaan. Terdiri atas kolom sebagai berikut. 
- Mine_id	 	: ID pembeda lokasi tambang 
- Mine_name	 	: Nama tambang 
- location 	 	: Lokasi tambang (Kabupaten)
- region 	 	: Lokasi tambang (Provinsi)
- Status 	 	: Status keaktifan tambang (aktif/tidak aktif) 
- Start_date	 	: Waktu tambang mulai beroperasi
- Remarks	 	: Catatan tambahan masing-masing tambang

### 2. Dataset Weather (dataset asli) : Tautan Dataset Weather

Sumber Dataset: ETA 5

Dataset ini merupakan dataset yang berisi record kondisi cuaca yang tercatat sejak tambang aktif digunakan. Merupakan hasil proses data satelit berdasarkan lokasi dan waktu tambang dari dataset ETA5. Berisikan kolom sebagai berikut. 
- Weather_id 	 	: ID pembeda cuaca
- Mine_id 		: Lokasi cuaca tercatat (berdasarkan mine_id)
- Date		 	: Tanggal cuaca tercatat
- Rainfall mm	 	: Banyak curah hujan dalam satu hari (satuan 
 milimeter)
- Temperature c	: Besar suhu rata-rata(satuan derajat celcius)
- Wind speed KMH	: Kecepatan angin rata-rata (satuan KM/H)
- Humidity pct	: Rata-rata kelembaban udara (persentase)
- Remark		: Klasifikasi cuaca (Cerah, Mendung, dll)

### 3. Dataset Equipment Inventory (Dummy): Tautan Dataset Equipment Inventory

Dataset ini berisikan list perlengkapan dan alat berat yang digunakan dalam tambang. Dataset ini juga memberikan status keaktifan perlengkapan dan kapasitas yang dimiliki alat. Kolom dataset Equipment Inventory adalah sebagai berikut. 
- Equipment id	: ID pembeda perlengkapan
- Mine id		: Lokasi penggunaan perlengkapan
- Equipment type	: Tipe alat berat 
- Brand 		: Merek alat berat 
- Model			: Model dari merek alat
- Base capacity ton	: Kapasitas dasar alat 
- Status			: Status perlengkapan (aktif/maintenance)


### 4. Dataset Road Condition (Dummy): Tautan Dataset Road Condition

Dataset road condition berisikan rute jalan yang ditempuh alat angkut melalui jalur darat beserta keterangan kondisi jalan. Dataset ini memiliki kolom sebagai berikut. 
- Road id 		: ID ruas jalan
- Mine id		: Lokasi ruas jalan berdasarkan tambang 
- Segment name 	: Segmen jalan
- Condition level	: Kondisi jalan 
- Accessibility		: Persentase aksesibilitas jalan
- Last inspection	: Tanggal terakhir inspeksi
- Remark 		: Keterangan tambahan mengenai kondisi jalan

### 5. Dataset Production Plan (Dummy): Tautan Dataset Production Plan

Dataset ini berisikan data historis rencana penambangan setiap minggu sejak tambang digunakan. Dataset ini juga menyajikan pencapaian asli produksi tambang. Kolom yang ada dalam dataset Production Plan adalah sebagai berikut. 
- Plan id		: ID rencana setiap minggu 
- Mine id		: Lokasi tambang dari rencana 
- Week start		: Tanggal rencana mingguan dibuat 
- Planned output ton: Rencana output mingguan (dalam ton)
- Actual output ton	: Output mingguan yang terealisasi (dalam ton)
- Target variance pct: Persentase varians ketercapaian rencana
- Status			: Status ketercapaian rencana 
- Updated by		: Manager perencanaan

### 6. Dataset Effective Capacity (Dummy): Tautan Dataset Effective Capacity

Dataset Effective Capacity merupakan dataset berisikan data historis kondisi alat tertentu yang menjadi faktor utama produksi dalam rencana produksi mingguan. Kolom dari dataset effective capacity adalah sebagai berikut. 
- Effcap id		: ID kapasitas efektif
- Plan id		: ID perencanaan
- Mine id		: ID lokasi tambang
- Equipment id	: ID perelengkapan yang menjadi highlight 
- Equipment type	: Jenis perlengkapan
- Week start		: Tanggal rencana produksi mingguan
- Road condition	: Kondisi jalan pada minggu perencanaan
- Weather condition	: Kondisi cuaca pada minggu perencanaan
- Availibility pct 	: Persentase kesediaan alat secara keseluruhan
- Effective capacity 	: Kapasitas efektif alat (dalam ton)
- Remark 		: Status keadaan kapasitas efektif alat

### 7.Dataset Production Constraint (Dummy): Tautan Dataset Production Constraint

Production Constrain merupakan dataset realisasi production plan dan kendala yang saat pelaksanaan kerja. Dataset ini memiliki kolom sebagai berikut. 
- Constraint id 	: ID Constrain 
- Mine id		: Lokasi tambang
- Equipment id	: ID Perlengkapan
- Week start 		: Tanggal mulainya rencana produksi mingguan
- Constraint type	: Jenis kendala
- Capacity value 	: Nilai realisasi
- Unit			: Unit nilai realisasi
- Update data		: Tanggal pengisian data
- Remarks		: Keterangan Kendala 

### 8. Dataset Shipping Schedule (Dummy): Tautan Dataset Shipping Schedule
Dataset shipping schedule berisikan data historis jadwal pengiriman. Dataset ini memiliki kolom sebagai berikut. 
- Shipment id		: ID pengiriman
- Mine id		: Lokasi tambang dari output yang dikirimkan
- Week start		: Tanggal mulainya rencana pengiriman
- Vessel name		: Kapal pengiriman 
- Destination port	: Lokasi tujuan pengiriman
- Coal tonnage	: Berat output batubara 
Etd 			: Estimasi waktu berangkat
Eta			: Estimasi waktu sampai
Status 		: Status pengiriman
