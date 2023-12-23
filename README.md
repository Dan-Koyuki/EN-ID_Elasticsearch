# Unveiling Elasticsearch : From Origins to Operations
## Sejarah
Elasticsearch merupakan sebuah engine pencarian yang didasarkan pada Lucene search library. Merupakan sebuah engine analisa dan pencari RESTful yang terdistribusi, yang memberikan kemampuan untuk melakukan pencarian real-time terdistribusi. Elasticsearch memberikan aplikasi dan websites kemampuan pencarian yang cepat dan powerful. Elasticsearch yang didasarkan pada Apache Lucene memberikan beberapa tambahan fitur seperti distributed indexing, real-time search, dan juga support untuk data baik yang terstruktur maupun tidak.
## Pengembangan
### Timeline
Lucene dibuat pada 1999. Dengan original author, Dough Cutting, membuatnya pada versi ke-enam. Meskipun Lucene merupakan engine pencari yang baik dan berdampak besar, awalnya Lucene di desain untuk hanya berkerja pada satu mesin. Pada perkembangannya, sebuah komunitas yang antusias tumbuh, banyak orang mulai melakukan hal-hal inovatif dengan menggunakan Lucene.
"Compass!" yang dibuat pada 2006 oleh Shay Bannon, seorang founder dari Elasticsearch. "Compass!" merupakan sebuah Object Mapping framework yang mendapatkan sejumlah komunitas open-source. Berpikir untuk "hibernate" dari Lucene, Shay bekerja keras untuk membuat "Compass!" bekerja selancar mungkin.
Setelah "Compass!" versi ke-dua dirilis, Shaw merasa bahwa dia ingin mengembangakan project tersebut pada tingkat yang lebih tinggi ketika dia mengembangkan versi ke-tiga dari "Compass!".
Shay Bannon berhenti mengembangkan "Compass!" pada tahun 2009 setelah menyadari bahwa engine pencari tersebuh harus berada pada satu aplikasi. Sehingga, Shay memfokuskan dirinya pada Elasticsearch, sebuah engine RESTful terdistribusi untuk banyak variasi data dan aplikasi.
Elasticsearch dirilis pada publik pada tahun 2011 dan Elasticsearch Inc. didirikan pada 2012, bersama dengan beberapa program lain yang tidak kalah bagusnya seperti Kibana dan Logstash. Program-program tersebut kemudian disebut sebagain ELK stack.
Pada 2015, Elasticsearch memulai servicenya pada Elastic Cloud
Pada 2021, Elasticsearch memodifikasi lisensinya menjadi open-service sebagai respon untuk kurangnya persetujuan dengan AWS dan pengguna lainnya yang mengeksploitasi Elasticsearch dengan menarik pembayaran untuk servicenya tanpa berkontribusi pada komunitas Elasticsearch.
### Architecture Evolution
Kembali pada saat Elasticsearch muncul lebih dari 10 tahun lalu, format untuk database berpusat pada NoSQL. Pada waktu itu, database di-setting untuk mendistribusikan data pada banyak mesin yang "murah", memastikan bahwa setiap bagian data disebarkan pada mesin-mesin tersebut. Untuk memastikan ketahanan data, beberapa salinan "shards" di simpan pada "nodes" yang berbeda untuk mengatasi kegagalan dan membantu dalam mengembalikan data yang corrupt.
Untuk memperkuat integritas data, "Transaction log" dibuat, menjaga data dengan mengalirkan data pada local disks sebelum server crash atau restart.
Elastricsearch mengadopsi fungsi tersebut, memanfaatkan engine Lucene untuk menyediakan API yang komprehensif pada query, memanfaatkan full-text search, type-as-you-search, geo-search dan analytical queries.
Komunitas menerima Elasticsearch dengan baik, banyak perusahaan menggunakannya, membuat popularitasnya melambung tinggi. Elasticsearch kemudian mulai mengembangkan arsitektur data tier nya.
Pada lingkup produksi, data yang baru cenderung mendapatkan prioritas dibandingkan dengan data lama. Sebagai contoh, pada saat service outage, fokusnya adalah untuk mengambil atau mengamankan metriks dari beberapa hari lalu dibandingkan denagn beberapa tahun lalu. Hal tersebut membuat Elasticsearch memperkenalkan Hot, Cold, dan Frozen data tier, dirancang untuk data dengan karakteristik yang berbeda.
Hot tier untuk data terbaru, menangani operasi writes dan reads pada Hot nodes.
Cold tier memberikan hardware yang lebih efisiend dalam biaya untuk meng-hosting data yang kurang banyak diakses (data lama). Merupakan sebuah opsi yang budget-friendly yang tidak perlu untuk menyimpan banyak salinan untuk data, memberikan kesempatan lebih untuk mengorganisir data.
Kemudian, Elasticsearch memperkenalkan Frozen tier, memanfaatkan penyimpanan blob seperti AWS's S3 untuk menyimpan dan mengkueri data. Fitur ini, yang dikenal sebagai snapshot yang dapat dicari, tidak hanya menunjukkan keberhasilan tetapi juga memberikan kinerja yang sangat baik ketika Elasticsearch beroperasi.
## Konfigurasi
### Cluster
pada Elasticsearch, sebuah 'Cluster' merupakan koleksi atau kumpulan dari satu atau lebih 'nodes' yang bekerja sama untuk memberikan kemampuan indexing dan search data yang tersimpan pada nodes.
Beberapa key aspect pada Cluster Elasticsearch:
- Nodes
individual instance dari elasticsearch yang sedang berjalan pada sebuah mesin atau server. Nodes pada Cluster berkomunikasi satu sama lain untuk berbagi data, mengeksekusi query, dan menjaga Cluster State.
- Cluster State
merupakan kumpulan informasi tentang keseluruhan cluster seperti index, mappings, shards, dan state dari setiap node. Cluster state dijaga dan di update oleh master-eligible nodes and sangat penting untuk suatu cluster agar berfungsi dengan baik.
- Master Node
dalam suatu cluster, satu atau lebih node di desain sebagain master-eligible nodes. Bertanggung jawab untuk mengatur Cluster State, koordinasi action seperti membuat atau menhapus index, menangani node yang bergabung atau meninggalkan cluster.
- Sharding dan Replication
Data pada elasticsearch dibagi menjadi unit yang lebih kecil yang bernama Shard. Setiap shard merupakan Lucene Index yang memegang sebagian porsi data.
Shard dapat di replikasi kepada berbagai nodes untuk mentolerasi kesalahan dan  meningkatkan ketersediaan.
Replica berfungsi sebagai backup yang memberikan ketahanan terhadap kegagalan node.
- Cluster Health
Cluster health di representasikan dengan code warna, diantaranya:
**** Hijau: semua primary dan replica shard aktif
**** Kuning: semua primary shard aktif, namun beberapa replica menghilang atau tidak tersedia.
**** Merah: beberapa primary shard tidak aktif, pertanda data hilang pada shard tersebut.
- Discovery dan Node Communication
Nodes pada cluster perlu untuk "menemukan" dan berkomunikasi satu sama lain. Elasticsearch memberikan beberapa cara discovery seperti unicast, multicast, atau cloud-based discovery agar nodes dapat menemukan dan bergabung dengan sebuah cluster
- Scaling and Growth
Elasticsearch cluster dapat berkembang (scaled) dengan menambah jumlah nodes untuk mendistribusikan data dan beban kerja, meningkatkan performa dan kapasitas ketika data volumenya naik.
### Nodes
Dalam elastricsearch, sebuah 'node' merupakan satu instance dari service Elasticsearch yang sedang berjalan dalam suatu cluster. Nodes bertanggung jawab untuk menyimpan data, mengeksekusi query, dan berpartisipasi dalam cluster indexing dan kemampuan search.
key aspect dalam node configuration:
- Node Types
** Master-eligible nodes: Nodes yang bertanggung jawab untuk actions yang kurang lebih satu level dengan cluster seperti mengatur metadata, membuat dan menghapus index, serta memilih cluster master node. Tidak semua node harus master-eligible.
** Data Nodes: menyimpan data, mengeksekusi pencarian dan indexing requests, dan menangani query. nodes ini memengang shard dari data index.
** Ingest Nodes: Node dimana ingest pipeline berjalan, memperbolehkan pre-processing dari documents sebelum peng-index-an
** Client Nodes: Hanya mengalihkan atau memberikan jalan pada request kepada data node yang sesuai.
- Node Settings
** Node Name: Memberikan nama yang unique pada setiap node untuk membantu dalam mengindentifikasi dan mengatur node-node tersebut dalam suatu cluster
** Roles: Node dapat memiliki roles berdasarkan tanggung jawab yang mereka pegang (Node type)
- Hardware Resources
Eleasticsearch dapat memakan banyak resource. Penyetelan CPU yang baik, memory, dan storage resource untuk nodes sangatlah penting untuk performa yang optimal. mengalokasikan heap memory untuk elasticsearch berdasarkan system memory yang tersedia sangatlah penting.
- Networking Settings
mengatur network setting untuk mengontrol bagaimana nodes berkominikasi dalam cluster, termasuk host binding, transport port, dan network publishing settings.
- Discovery Settings
Mengatur mekanisme discovery untuk memperboolehkan node untuk menemukan dan bergabung dalam cluster.
- Node Attributes
Memberikan custom attribute pada nodes dapat membantu dalam proses routing atau filtering.
- Plugins and Modules
Menghidupkan atau mematikan elasticsearch plugin dan modules pada setiap node berdasarkan pada kebutuhan.

Node Configuration dapat dilakuan melalui Elasticsearch configuration files, environment variable, atau API calls.
