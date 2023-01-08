# SIG_TEORI_2101092059
<int 1>
1.	Locate the tl_2019_06_tract.zip file in the QGIS Browser and expand it. Select the tl_2019_06_tract.shp file and drag it to the canvas.
 
2.	The Select Transformation dialog will prompt to convert from EPSG:4269 to EPSG:4326. This dialog presents several transformations to convert between the coordinates between these projections. Leave the selection to the default choice and click OK.
 
3.	You will see the layer tl_2019_06_tract loaded in the Layers panel. This layer contains the boundaries of census tracts in California. Right-click on the tl_2019_06_tract layer and select Open Attribute Table.
 
4.	Examine the attributes of the layer. To join a table with this layer, we need each feature's unique and common attribute. In this case, there are 8057 individual tract records with the GEOID field. This column can link this layer with any other layer or table containing the same ID.
 
5.	To load the tabular data, click on Open Data Source Manager.
 
6.	In the Data Source Manager dialog, choose Delimited Text. Then in the right, click on the ... next to File name and browse to the unzipped folder with the California population CSV.
 
7.	Now under Sample Data, we can inspect the data even before loading it as a layer. The representation shows that the data table contains 2 header rows.
 
8.	To eliminate the additional header row, under Record and Fields Options set the Number of header line to discard to 1. Now the table will contain proper column headers. Since this layer contains only tabular data, select No geometry (attribute only table) under Geometry Definition. Click Add to add it as a layer and then click Close to close this dialog box.
 
9.	The CSV will now be imported as a table to QGIS and appear as ACST5Y2019.S0101 in the Layers panel. Now right-click on the layer and select Open Attribute Table.
 
10.	The ID column contains the unique id for each record, which can be used to join this table with the tl_2019_06_tract layer. If you compare the values of the ID with the GEOID column from the tl_2019_06_tract. you will notice that it is prefixed with 1400000US. To merge these two tables successfully, the values must match exactly. Let's remove this prefix and add a new column with the last 11 characters which contain the value that is an exact match.
 
11.	To create a new column with the last 11 digits, open the Processing Toolbox by going to Processing ‣ Toolbox, and search and locate the Vector table ‣ Field calculator algorithm.
 
12.	In the Field calculator dialog, select ACST5Y2019.S0101 as the Input layer, enter geoid in Field name, and select string in Result Field type. Now search for substr in expressions. We can use this function to extract the required part from the id field.
 
13.	Enter the below expression. We use the substr function and extract the value from position -11 (negative value is counted from the end). The final result can be viewed in the Preview section. Click Run.
substr("id", -11)
 
14.	Now a new layer Calculated will be loaded in the canvas, lets inspect the attribute table. A new column geoid with the value that can be matched with the cencus tract will be present.
 
15.	To create a table join, open the Processing Toolbox by going to Processing ‣ Toolbox, and search and locate the Vector general ‣ Join attributes by field value algorithm.
 
16.	In the Join attributes by field value dialog, select tl_2019_06_tract as the Input layer and GEOID as the Table field. Select Calculated as the Input layer 2 and geoid as the Table field 2. Under Layer2 fields to copy, click on the ....
 
17.	Check Geographic Area Name, Estimate!!Total!!Total population and geoid. Click OK.
 
18.	Check the Discard records which could not be joined. This will eliminate any extra records in the population table. Click the ... button under joined layer to select the output file location and select Save to File....
 
19.	Name the output geopackage as california_total_population.gpkg . Click Run.
 
20.	Once the processing finishes, verify that the algorithm was successful if all 8057 feature(s) are joined. Click Close.
 
21.	You will see a new layer california_total_population loaded in the Layers panel. At this point, the fields from the CSV file are joined with the census tracts layer. Now that we have the population data in the census tracts layer, we can style it to create a visualization of population density distribution. Click the Open the Layer Styling Panel button.
 
22.	In the Layer Styling panel, select Graduated from the drop-down menu. As we are looking to create a population density map, we want to assign different color to each census tract feature based on the population density. We have the population in the Estimate!!Total!!Total population field, and the area field in ALAND. Click Expression button, to compute the percentage of total population in each cencus tract.
Catatan
When creating a thematic (choropleth) map such as this, it is important to normalize the values you are mapping. Mapping total counts per polygon is not correct. It is important to normalize the values dividing by the area. If you are displaying totals such as crime, you can normalize them by dividing by total population, thus mapping crime rate and not crime. Learn more
 
23.	Enter the following expression to calculate the population density. The area of the feature is given in square kilometers. We then convert it to square meters by multiplying with 1000000 and calculating the population density with the formula Population/Area. Preview the result and click OK.
1000000 * ("Estimate!!Total!!Total population"/"ALAND")
 
24.	In the Layer Styling Panel, click classify and enter the classes as 10.
 
25.	Click on the color ramp to choose the color ramp RdYlGn.
 
26.	The higher density concerns more so, let's assign green to lower density and red to high-density areas. Click on the color ramp and choose Invert Color Ramp.
 
27.	Now we have an excellent looking information visualization of population density in California. To make it better, let's make the border of each census tract transparent. Click on the Symbol tab.
 
28.	Click on Stroke color and click Transparent stroke.
 
29.	The bins can be adjusted, click on the Values this will popup a dialog to enter the upper and lower bound value.
 
30.	Once your satisfied close the Layer styling panel. We now have a nice looking information visualization of population density in California.
 
<int 2>

1.	Temukan file nybb_19a.zip di Browser QGIS dan perluas. Pilih  layer nybb_19a/nybb.shp dan seret ke kanvas. Ini adalah lapisan poligon yang mewakili batas-batas borough di kota New York.
 
2.	Selanjutnya, cari file V_SSS_SEGMENTRATING_1.zip dan perluas. Pilih  layer dot_V_SSS_SEGMENTRATING_1_20190129.shp dan tambahkan ke kanvas. Ini adalah lapisan garis dari semua jalan di kota.
 
3.	Mari kita periksa atribut yang tersedia untuk setiap fitur dari lapisan dot_V_SSS_SEGMENTRATING_1_20190129. Klik kanan dan pilih Buka Tabel Atribut.
 
4.	Anda akan melihat atribut yang disebut Rating_B yang  memiliki nilai dalam rentang 0-10 yang mewakili peringkat segmen jalan. Atribut RatingWord memiliki peringkat deskriptif. Kita dapat menggunakan  bidang Rating_B untuk menghitung peringkat rata-rata.
 
5.	Anda mungkin telah memperhatikan beberapa fitur memiliki peringkat NR. Ini adalah segmen yang tidak dinilai. Memasukkan mereka dalam analisis kami tidak akan benar. Sebelum kita melakukan penggabungan spasial, mari kita siapkan Filter untuk mengecualikan catatan ini. Klik kanan layer dot_V_SSS_SEGMENTRATING_1_20190129 dan pilih Filter.
 
6.	Di Penyusun Kueri, ketikkan ekspresi berikut untuk memilih semua rekaman yang tidak diberi peringkat NR. Anda juga dapat membangun ekspresi secara interaktif dengan mengklik Bidang, Operator dan memilih Nilai yang sesuai. Klik Oke.
"Kata Peringkat" != 'NR'
 
7.	Anda akan melihat layer dot_V_SSS_SEGMENTRATING_1_20190129 sekarang memiliki ikon filter yang menunjukkan bahwa ada filter aktif yang diterapkan pada layer ini. Sekarang kita dapat melakukan penggabungan spasial menggunakan layer ini. Pergi ke Pemrosesan ‣ Kotak Alat.
 
8.	Cari dan temukan algoritma Vector general ‣ Join attribute by location (summary). Klik dua kali untuk meluncurkannya.
 
9.	Dalam dialog Join attribute by location (summary), pilih nybb sebagai layer Input. Layer jalan dot_V_SSS_SEGMENTRATING_1_20190129 akan menjadi layer Join. Anda dapat membiarkan predikat Geometri ke Intersects default. Klik tombol ...  di samping Bidang untuk melaminasi.
 
Catatan
Tip untuk membantu Anda memilih input yang benar dan menggabungkan layer: Layer input adalah salah satu yang akan dimodifikasi akan atribut baru dalam gabungan spasial. Karena kita ingin bidang peringkat rata-rata ditambahkan ke layer borough, itu akan menjadi layer input.
10.	Pilih Rating_B dan klik OK.
 
11.	Demikian pula, klik tombol ...  di samping Ringkasan untuk menghitung.
 
12.	Pilih mean sebagai operator ringkasan dan klik OK. Sekarang kami siap untuk memulai pemrosesan. Klik Jalankan.
 
13.	Algoritma pemrosesan akan bekerja melalui fitur-fitur dan menerapkan gabungan spasial. Verifikasi bahwa pekerjaan pemrosesan berhasil dan klik Tutup.
 
14.	Kembali ke jendela utama QGIS, Anda akan melihat layer Joined layer baru ditambahkan ke kanvas. Buka tabel atribut untuk layer ini. Anda akan melihat kolom baru Rating_B_mean  ditambahkan ke layer borough input dengan peringkat rata-rata semua jalan yang berpotongan dengan fitur itu.
 
15.	Sekarang kita dapat melakukan operasi terbalik. Terkadang analisis Anda memerlukan mendapatkan atribut dari lapisan lain berdasarkan hubungan spasial tetapi tidak menghitung ringkasan apa pun. Kita dapat menggunakan atribut Join dengan algoritma lokasi untuk analisis tersebut. Tugasnya adalah menambahkan nama borough ke setiap fitur di lapisan jalan berdasarkan poligon borough mana yang bersinggungan dengannya. Sebelum kita menjalankan algoritma ini, mari kita hapus filter dari  layer dot_V_SSS_SEGMENTRATING_1_20190129. Klik ikon filter dan tekan tombol Hapus di Penyusun Kueri. Klik Oke.
 
16.	Giliran layer Joined di  panel Layers. Temukan Vector general ‣ Join attribute by location algorithm di Processing Toolbox dan klik dua kali untuk meluncurkannya.
 
17.	Pilih dot_V_SSS_SEGMENTRATING_1_20190129 sebagai layer Input dan nybb sebagai layer Join. Anda dapat membiarkan predikat Geometri ke Intersects default. Klik tombol ...  di samping Bidang untuk menambahkan dan memilih BoroName. Klik Oke.
 
18.	Segmen garis dapat melintasi batas wilayah, jadi kami memilih jenis Gabung sebagai  fitur terpisah Peti untuk setiap fitur yang terletak (satu-ke-banyak). Klik Jalankan.
 
19.	Setelah pemrosesan selesai, buka tabel atribut dari layer Joined yang baru ditambahkan. Anda akan melihat bahwa ada  atribut BoroName baru yang  ditambahkan ke setiap fitur jalan.

<int 3>


1.	Temukan file metro_stations_accessbility.zip di Browser QGIS dan perluas. Pilih  file metro_stations_accessbility.shp dan seret ke kanvas. Layer baru metro_stations_accessbility akan dimuat di  panel Layers.
 
2.	Lapisan data untuk bar dan pub dalam format CSV. Untuk memuatnya di QGIS, pergi ke Layer ‣ Add Layer ‣ Add Delimited Text Layer...    . ( Lihat  Mengimpor spreadsheet atau file CSV (QGIS3)  untuk detail lebih lanjut tentang mengimpor file CSV)
 
3.	Di | Manajer Sumber Data Dialog Teks Terbatas, telusuri dan pilih file Bars_and_pubs__with_patron_capacity.csv yang diunduh  sebagai Nama file. Bidang X dan kolom bidang Y harus dipilih secara otomatis untuk  koordinat x dan koordinat y  masing-masing. Klik Tambahkan.
 
4.	Anda akan melihat layer Bars_and_pubs__with_patron_capacity baru  ditambahkan ke  panel Layers. Kedua lapisan input berada di Geograhpic Coordinate Reference System (CRS) EPSG:4326 WGS84. Untuk melakukan analisis spasial, disarankan untuk menggunakan Projected Coordinate Reference System (CRS). Jadi kita sekarang akan memproyeksikan kembali kedua lapisan ke CRS regional yang sesuai yang meminimalkan distorsi dan memungkinkan kita untuk bekerja dalam satuan jarak seperti meter, bukan derajat. Pergi ke Pemrosesan ‣ Kotak Alat.
 
5.	Cari dan temukan alat layer Vector general ‣ Reproject. Klik dua kali untuk meluncurkannya.
 
6.	Pilih Bars_and_pubs__with_patron_capacity sebagai layer Input. Klik tombol Select CRS  di sebelah Target CRS.
 
7.	Saat memilih sistem koordinat yang diproyeksikan untuk analisis Anda, tempat pertama yang harus dicari adalah CRS regional untuk bidang yang diminati. Untuk Australia, Map Grid of Australia (MGA) 2020 adalah sistem grid berbasis UTM yang digunakan untuk pemetaan lokal dan regional. Melbourne termasuk dalam UTM Zone 55, sehingga kita dapat memilih GDA 2020 / MGA zone 55 EPSG:7855' CRS.
 
Catatan
Jika Anda tidak yakin dengan CRS lokal untuk wilayah tempat Anda bekerja, memilih CRS untuk zona UTM berdasarkan datum WGS84 adalah pilihan yang aman. Anda dapat mengetahui nomor zona UTM wilayah Anda menggunakan UTM Grid Zones of the World.
8.	Selanjutnya, klik tombol ...  di samping Reprojected dan pilih Save to GeoPackage.  Geopackage adalah data spasial format data terbuka yang direkomendasikan dan merupakan format pertukaran data default untuk QGIS3. Satu file GeoPackage .gpkg dapat berisi beberapa lapisan vektor dan raster.
 
9.	Beri nama geopackage sebagai spatialquery dan klik Simpan.
 
10.	Ketika diminta untuk nama layer, masukkan bars_and_pubs dan klik OK. Klik Run untuk memproyeksi ulang layer.
 
11.	Jendela akan beralih ke tab Log dan  Anda akan melihat algoritma berjalan dan membuat layer output baru bars_and_pubs.
 
12.	Sekarang kita akan memproyeksi ulang layer metro_stations_accessbility. Beralih kembali ke  tab Paramters di  jendela layer Reproject. Pilih metro_stations_accessbility sebagai layer Input. Pertahankan Target CRS yang sama. Selanjutnya, klik tombol ...  di samping Reprojected dan pilih Save to GeoPackage. Pilih file output yang  sama spatialquery (Ingat bahwa satu file geopackage dapat berisi beberapa layer, jadi kita akan menyimpan layer baru ke file geopackage yang sama). Masukkan metro_stations sebagai nama Layer. Klik Jalankan.
 
13.	Kembali ke jendela utama QGIS, Anda akan melihat 2 layer baru dimuat di  panel Layers: bars_and_pubs dan metro_stations. Anda dapat mematikan visibilitas untuk lapisan asli. Sekarang, kita siap untuk melakukan kueri spasial. Karena kami tertarik untuk memilih bar dan pub dalam jarak 500m dari stasiun metro, langkah pertama adalah membuat buffer di sekitar stasiun metro yang mewakili area pencarian kami. Cari dan temukan geometri Vektor ‣ Alat penyangga di Kotak Alat Pemrosesan dan klik dua kali untuk meluncurkannya.
 
14.	Dalam dialog Buffer, pilih metro_stations sebagai layer Input. Atur 500 meter sebagai Jarak. Simpan output ke  geopackage spatialquery yang sama  dan masukkan metro_stations_buffers sebagai nama Layer. Klik Jalankan.
 
15.	Anda akan melihat layer metro_stations_buffers baru  dimuat di  panel Layers. Sekarang kita dapat mengetahui titik mana dari  lapisan bars_and_pubs yang  termasuk dalam poligon dari lapisan metro_stations_buffers. Temukan pilihan Vektor ‣ Ekstrak berdasarkan Lokasi alat dari Kotak Alat Pemrosesan dan klik dua kali untuk meluncurkannya.
 
Catatan
Ekstrak berdasarkan lokasi akan membuat layer baru dengan fitur yang cocok dari kueri spasial. Jika Anda hanya ingin memilih fitur, gunakan  alat Pilih menurut lokasi.
16.	Dalam dialog Ekstrak menurut lokasi, pilih bars_and_pubs sebagai Ekstrak fitur dari. Periksa Intersect sebagai predikat geometri. Atur metro_stations_buffers sebagai Dengan membandingkan dengan fitur dari. Simpan output ke  geopackage spatialquery sebagai layer yang dipilih. Klik Jalankan.
 
17.	Setelah pemrosesan selesai, Anda akan melihat layer yang dipilih ditambahkan ke  panel Layers. Perhatikan bahwa lapisan ini hanya berisi titik-titik dari bars_and_pubs yang termasuk dalam poligon buffer.
 
18.	Analisis kami selesai. Anda mungkin memperhatikan bahwa poligon buffer terlihat berbentuk oval. Ini karena Project CRS kami masih diatur ke EPSG:4326 WGS84. Untuk memvisualisasikan hasil dengan lebih baik, Anda dapat pergi ke Project  ‣  Properties ‣ CRS dan pilih GDA 2020 / MGA zone 55 EPSG:7855 yang kami gunakan untuk analisis. Setelah diatur ke CRS ini, buffer akan muncul dalam bentuk yang benar.

<int 4>
1.	Temukan  file ne_10m_populated_places_simple.zip yang diunduh di panel Browser dan perluas. Seret  file ne_10m_populated_places_simple.shp ke kanvas.
 
2.	Anda akan melihat layer baru ne_10m_populated_places_simple dimuat di  panel Layers. Lapisan ini berisi titik-titik yang mewakili tempat-tempat berpenduduk. Sekarang kita akan memuat lapisan gempa. Layer ini hadir sebagai  file teks Tab Serepated Values (TSV). Untuk memuat file ini, klik  tombol Open Data Source Manager pada Data Source Toolbar. Anda juga dapat menggunakan  pintasan keyboard Ctrl + L.
 
3.	Dalam kotak dialog Manajer Sumber Data, pilih Teks yang Dibatasi.
 
4.	Klik tombol ...  tombol di sebelah Nama file  dan telusuri ke  file earthquakes-2021-11-25_13-39-30_+0530.tsv yang diunduh  . Tergantung pada sistem operasi, Anda mungkin tidak melihat file di direktori yang diunduh. Jika itu masalahnya, beralihlah ke Semua file (*; .)  dalam  dialog Pilih File Teks yang Dibatasi untuk Dibuka. Setelah dibuka, pilih Pembatas kustom di  bagian Format file, dan centang Tab. Di  bagian Definisi geometri,  pilih Koordinat titik. Secara default bidang  X dan  nilai bidang Y akan diisi secara otomatis dengan bidang yang sesuai dalam input. Dalam kasus kami, mereka adalah Bujur dan Lintang. Anda dapat membiarkan Geometry CRS ke EPSG:4326 - WGS 84 CRS default  . Jika file Anda berisi koordinat dalam CRS yang berbeda, Anda dapat memilih CRS yang sesuai di sini. Klik Tambahkan diikuti oleh Tutup.
 
5.	Perbesar dan jelajahi kedua himpunan data. Setiap titik merah mewakili lokasi kejadian gempa, dan setiap titik hijau mewakili lokasi tempat berpenduduk. Tujuan kami adalah untuk mengetahui titik terdekat dari lapisan tempat berpenduduk untuk masing-masing titik di lapisan gempa. Mari kita periksa tabel Atribut dari lapisan gempa bumi. Pilih layer dan klik ikon Open Attribute Table di Toolbar.
 
6.	Ada 2586 fitur, tetapi data berisi beberapa entri tanpa informasi lintang atau bujur. Kita harus menghapusnya sebelum melanjutkan lebih jauh. Tutup tabel atribut.
 
7.	Pergi ke Pemrosesan  ‣  Kotak Alat  ‣  Geometri vektor ‣ Hapus alat  geometri nol. Klik dua kali untuk membukanya.
 
8.	Dalam kotak dialog Hapus Geometri Null, pilih gempa bumi-2021-11-25_13-39-30_+0530 sebagai lapisan Input dan centang kotak Hapus juga geometri kosong. Klik Jalankan. Setelah pemrosesan selesai, klik Tutup.
 
9.	Layer baru Geometri non null akan ditambahkan ke  panel Layers. Untuk analisis kita akan menggunakan layer ini bukan layer asli. Hapus centang  layer earthquakes-2021-11-25_13-39-30_+0530 di  panel Layers untuk menyembunyikannya. Pilih  layer geometri Non null dan klik  tombol Open Attribute Table dari Attributes Toolbar.
 
10.	Anda akan melihat hitungan yang lebih rendah untuk fitur total karena semua baris dengan nilai lintang dan bujur kosong telah dihapus. Tutup tabel atribut.
 
11.	Sekarang saatnya untuk melakukan analisis tetangga terdekat. Cari dan temukan alat Pemrosesan  ‣  Kotak Alat  ‣  Analisis vektor ‣ Jarak ke hub  terdekat (baris ke hub). Klik dua kali untuk meluncurkannya.
 
Catatan
Kita juga dapat menambahkan layer titik sebagai output, gunakan alat Distance to nearest hub (points) untuk itu.
12.	Dalam  kotak dialog Jarak ke Hub Terdekat (Line to Hub), pilih Geometri non null sebagai lapisan Titik sumber. Pilih ne_10m_populated_places_simple sebagai layer Destination hubs. Pilih  nama  sebagai atribut nama lapisan Hub. Alat ini juga akan menghitung jarak garis lurus antara tempat berpenduduk dan gempa terdekat. Atur Kilometer sebagai unit Pengukuran. Klik pada ...  di Hub Distance dan klik Simpan ke File...  untuk menyimpan file sebagai earthquakes_with_nearest_city.gpkg . Klik Jalankan. Setelah pemrosesan selesai, klik Tutup.
 
13.	Kembali ke jendela utama QGIS, Anda akan melihat layer baris baru yang disebut earthquakes_with_nearest_city dimuat di  panel Layers. Lapisan ini memiliki fitur garis yang menghubungkan setiap titik gempa ke tempat berpenduduk terdekat. Pilih  layer earthquakes_with_nearest_city dan klik  ikon Open Attribute Tabel di Toolbar.
 
14.	Gulir ke kanan ke kolom terakhir, dan Anda akan melihat 2 atribut baru yang disebut HubName dan HubDist ditambahkan ke fitur gempa asli. Ini adalah nama jarak ke tetangga terdekat dari lapisan tempat berpenduduk.
 
<int 5>

1.	Buka zip dan ekstrak 2018_Gaz_ua_national.zip dan tl_2018_us_county.zip ke folder di komputer Anda. Buka  QGIS dan cari file us.tmax_nohads_ll_20190501_float.tif di Browser QGIS seret ke kanvas.
 
2.	Anda akan melihat layer raster baru us.tmax_nohads_ll_20190501_float dimuat di  panel Layers. Lapisan raster ini berisi suhu maksimum yang tercatat pada setiap piksel dalam derajat Celcius. Selanjutnya kita akan memuat file titik daerah perkotaan. File ini hadir sebagai file teks dalam format Tab Separated Values (TSV). Klik  tombol Buka Manajer Sumber Data  pada Toolbar Sumber Data.
 
3.	Beralih ke tab Teks Terbatas. Klik tombol ...  tombol di sebelah Nama file  dan tentukan jalur ke file teks yang Anda unduh. Di  bagian Format file, pilih Pembatas kustom dan centang Tab. Pilih INTPTLONG sebagai bidang X dan INTPTLAT sebagai bidang Y. Klik Tambahkan lalu Tutup.
 
4.	Layer titik baru 2018_Gaz_ua_national akan dimuat di  panel Layers. Sekarang kita siap untuk mengekstrak nilai dari layer raster pada titik-titik ini. Pergi ke Pemrosesan ‣ Kotak Alat.
 
5.	Cari dan temukan analisis Raster ‣ Contoh algoritma nilai raster. Klik dua kali untuk meluncurkannya.
 
6.	Pilih 2018_Gaz_ua_national sebagai layer titik input. Pilih us.tmax_nohads_ll_20190501_float sebagai Layer Raster untuk sampel. Perluas parameter Lanjutan dan masukkan tmax sebagai awalan kolom Output. Klik Jalankan. Setelah pemrosesan selesai, klik Tutup.
 
7.	Layer baru Sampled Points akan dimuat di  panel Layers. Pilih  alat Identifikasi di Toolbar Atribut dan klik pada titik mana pun. Anda akan melihat atribut yang ditampilkan di  panel Identifikasi Hasil. Anda akan melihat atribut baru yang disebut tmax_1 ditambahkan ke setiap fitur. Ini adalah nilai piksel dari layer raster yang diekstraksi di lokasi titik. 1  mewakili nomor pita raster. Jika layer raster memiliki beberapa band, Anda akan melihat beberapa kolom baru di layer output.
 
8.	Bagian pertama dari analisis kami sudah berakhir. Mari kita hapus layer yang tidak perlu. Tahan tombol Shift dan  pilih Titik Sampel dan  lapisan 2018_Gaz_ua_national. Klik kanan dan pilih Hapus untuk menghapusnya dari QGIS. Saat diminta untuk Hapus 2 entri legenda? , pilih OK.
 
9.	Sekarang kita akan menggunakan layer county untuk mengambil sampel raster dan menghitung suhu rata-rata untuk setiap county. Temukan file tl_2018_us_county.shp di Browser QGIS seret ke kanvas.
 
Catatan
Sebagian besar algoritma pemrosesan akan membaca layer input dan membuat layer baru. Tetapi algoritma Statistik Zonal berbeda. Ini memodifikasi layer input dan menambahkan atribut baru ke dalamnya. Itulah mengapa penting untuk meng-unzip file input terlebih dahulu. QGIS dapat memuat lapisan dari arsip zip secara langsung, tetapi tidak dapat memodifikasi lapisan zip. Algoritma pemrosesan akan gagal jika tidak dapat memperbarui lapisan input.
10.	Layer baru  tl_2018_us_county akan dimuat ke  panel Layers. Pergi ke Pemrosesan ‣ Kotak Alat.
 
11.	Cari dan temukan analisis Raster ‣ Algoritma statistik zona dan klik dua kali untuk meluncurkannya.
 
12.	Pilih us.tmax_nohads_ll_20190501_float sebagai layer  Raster dan tl_2018_us_county sebagai layer Vector yang berisi zona. Masukkan tmax_ sebagai awalan kolom Output. Klik tombol ...  di samping Statistik untuk menghitung.
 
13.	Pilih hanya nilai Mean dan klik OK.
 
14.	Klik Jalankan untuk memulai pemrosesan. Algoritma mungkin memerlukan waktu beberapa menit untuk menyelesaikannya. Klik Tutup.
 
15.	Seperti disebutkan sebelumnya, algoritma Zonal Statistics tidak membuat layer baru, tetapi memodifikasi layer zona. Klik kanan layer tl_2018_us_county, dan pilih Open Attribute Table.
 
16.	Anda akan melihat kolom baru yang disebut tmax_mean ditambahkan ke tabel atribut. Ini berisi nilai suhu rata-rata yang diekstraksi di atas poligon untuk setiap fitur. Ada beberapa nilai nol karena kabupaten-kabupaten itu (milik Alaska, Hawaii dan Puerto Riko) berada di luar batas lapisan raster.
 
<int 6>

1.	Buka zip semua file yang diunduh. Di Browser, cari folder yang berisi file batas WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygons.shp dan drag-and-drop ke kanvas QGIS.
 
2.	Sekarang cari ubin raster worldcover ESA_WorldCover_10m_2020_v100_N24_E093_Map.tif dan jatuhkan ke kanvas QGIS.
 
3.	Anda sekarang akan memiliki batas vektor dan lapisan raster landcover yang dimuat di  panel Layers. Mari kita klip raster landcover ke batas taman nasional. Pergi ke Pemrosesan ‣ Kotak Alat untuk membuka Kotak alat pemrosesan. Cari dan temukan GDAL  ‣ Ekstraksi raster ‣ Clip raster dengan algoritma lapisan topeng  . Klik dua kali untuk meluncurkannya.
 
4.	Dalam dialog Clip Raster by Mask Layer  , pilih  layer ESA_WorldCover_10m_2020_v100_N24_E093_Map sebagai layer Input dan  layer WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygons sebagai Mask Layer. Masukkan -9999 di Tetapkan nilai nodata tertentu ke bagian pita output.
 
5.	Sekarang buka bagian Parameter Lanjutan dan pilih Kompresi Tinggi di Profil. Sekarang di bawah Terpotong (topeng), klik pada ...  dan pilih Simpan Ke File... . Masukkan nama file sebagai worldcover_clipped.tif. Klik Jalankan.
 
6.	Sekarang layer worldcover_clipped akan dimuat di kanvas QGIS. Klik kanan layer ESA_WorldCover_10m_2020_v100_N24_E093_Map dan pilih Remove Layer...
 
7.	Kedua lapisan kita datang di Geographic CRS EPSG:4326. CRS ini memiliki satuan derajat dan tidak cocok untuk menghitung luas. Pertama-tama kita harus memproyeksi ulang layer ke Projected CRS. untuk analisis regional seperti ini, UTM adalah pilihan yang baik untuk CRS yang diproyeksikan. Kami akan memproyeksi ulang layer ke CRS untuk zona UTM lokal. Buka kotak alat Pemrosesan dan cari algoritma layer Vector general ‣ Reproject. Klik dua kali untuk meluncurkannya.
 
8.	Dalam dialog Reproject Layer,  pilih  layer WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygons sebagai layer Input, klik  tombol Select CRS  di bawah Target CRS.
 
9.	Bidang minat kami berada di Zona UTM 46N. Cari 46 N  dan pilih WGS 84 / UTM zone 46N CRS. 
 
Catatan
Untuk mengetahui zona UTM mana untuk wilayah Anda, lihat Apa Zona UTM saya di situs web.
10.	Di bagian Reprojecting, klik ...  dan pilih Simpan Ke File... . Masukkan nama sebagai nationalpark_reprojected.gpkg. Klik Jalankan.
 
11.	Sekarang layer nationalpark_reprojected akan dimuat di kanvas. Klik kanan  layer WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygons dan pilih Remove Layer...  untuk menghapusnya. Sekarang kita akan memproyeksi ulang layer raster. Di Kotak Alat Pemrosesan, cari dan temukan GDAL  ‣  Proyeksi raster ‣ Warp (proyek ulang)
 
12.	Dalam  dialog Warp (Reproject) pilih worldcover_clipped sebagai layer Input, pilih WGS 84 / UTM zone 46N CRS  di Target CRS. Buka Parameter Lanjutan dan pilih Kompresi Tinggi di Profil.
 
13.	Sekarang di bawah Reprojected, klik ...  dan pilih Simpan Ke File... . Masukkan nama sebagai worldcover_reprojected.tif. Klik Jalankan.
 
14.	Sekarang layer worldcover_reprojected akan dimuat di kanvas, lepaskan layer worldcover_clipped. Mari kita atur layer proyek ke zona UTM. Klik pada layer mana saja dan pilih  Layer CRS ‣ Set Project CRS dari Layer. 
 
15.	Sekarang proyek CRS akan diperbarui. Mari kita atur simbologi layer raster sesuai nama kelas dan warna himpunan data ESA WorldCover. Klik kanan pada layer worldcover_reprojected dan klik Properties...
 
16.	Dalam dialog Layer Properties, pilih Symbology. Anda dapat melihat warna Layer divisualisasikan dalam nada putih-hitam. Untuk memperbaikinya, klik pada Style  ‣ Load Style... . Telusuri dan pilih  file ESAWorldCover_ColorLegend.qml.
 
17.	Sekarang Anda dapat melihat warna simbol dan deskripsi kelas yang diperbarui. Klik Oke.
 
18.	Perluas layer worldcover_reprojected di  panel Layers untuk melihat legenda dengan deskripsi kelas yang benar.
 
19.	Sekarang mari kita hitung area untuk setiap kelas. Di kotak alat pemrosesan, cari dan temukan alat laporan nilai unik lapisan Raster. Klik dua kali untuk membukanya.
 
20.	Dalam dialog Raster Layer Unique Values Report, pilih layer Input sebagai worldcover_reprojected. Di bawah tabel Nilai unik klik ...   dan pilih Simpan ke File... . Masukkan nama sebagai class_areas.gpkg. Klik Jalankan.
 
21.	Sekarang layer class_areas akan ditambahkan ke  panel Layers. Klik kanan pada layer dan klik Open Attribute Table. Kolom m2 berisi area untuk setiap kelas dalam meter persegi.
 
22.	Mari kita ubah area menjadi kilometer persegi. Di Kotak Alat Pemrosesan, cari dan pilih Tabel vektor ‣ Kalkulator Bidang.
 
23.	Dalam dialog Kalkulator Bidang, pilih  lapisan class_areas di Lapisan Input. Masukkan Nama bidang sebagai area_sqkm. Di bidang Hasil ketik pilih Float. Di  jendela Ekspresi, masukkan ekspresi di bawah ini. Ini akan mengubah sqmt menjadi sqkm dan membulatkan hasilnya menjadi 2 tempat desimal. Di bawah klik Terhitung pada ...  dan pilih Simpan Ke File...  . Masukkan nama sebagai class_area_sqkm.gpkg. Klik Jalankan.
bulat("m2"/ 1e6, 2)
 
24.	Sekarang layer class_area_sqkm akan dimuat di kanvas. Buka tabel Atribut dan periksa  kolom area_sqkm yang baru ditambahkan  . Anda akan melihat bahwa  kolom Value berisi angka untuk setiap kelas. Untuk membuat hasilnya lebih mudah ditafsirkan, Mari tambahkan juga deskripsi untuk setiap nomor kelas. Deskripsi kelas tersedia di Panduan Pengguna Produk ESA.
 
25.	Buka Field Calculator, dan pilih layer class_areas_sqkm di Input Layer. Masukkan Nama bidang  sebagai landcover, di tipe bidang Hasil, pilih String. Di  jendela Ekspresi masukkan ekspresi di bawah ini. Ekspresi ini menggunakan  pernyataan CASE untuk menetapkan nilai berdasarkan beberapa kondisi. Di bawah klik Terhitung pada ...  dan pilih Simpan Ke File...  . Masukkan nama sebagai class_area_with_landcover.gpkg. Klik Jalankan.
PERKARA
KETIKA "nilai" = 10 MAKA 'Tutupan pohon'
KETIKA "nilai" = 20 MAKA 'Semak belukar'
KETIKA "nilai" = 30 MAKA 'Padang Rumput'
KETIKA "nilai" = 40 MAKA 'Cropland'
KETIKA "nilai" = 50 MAKA 'Built-up'
KETIKA "nilai" = 60 MAKA 'Vegetasi telanjang / jarang'
KETIKA "nilai" = 70 MAKA 'Salju dan Es'
KETIKA "nilai" = 80 MAKA 'Badan air permanen'
KETIKA "nilai" = 90 MAKA 'Lahan basah herba'
KETIKA "nilai" = 95 MAKA 'Lumut dan lumut'
KETIKA "nilai" = 100 MAKA 'Mangrove'
UJUNG
 
26.	Sekarang layer class_area_with_landcover akan dimuat di kanvas. Buka tabel Atribut. Kolom  landcover  akan berisi nama landcover terhadap setiap nilai landcover.
 
27.	Mari kita ekspor hasil ini sebagai file excel. Sebelum ekspor, kami juga akan mengatur tabel dan menghapus bidang yang tidak diinginkan. Di Kotak Alat Pemrosesan, cari dan pilih tabel Vektor ‣ Bidang refactor.
 
28.	Dalam dialog Refactor Fields, pilih  layer class_area_with_landcover di Layer Input. Pilih semua kolom kecuali area_sqkm dan landcover, lalu klik Hapus bidang yang dipilih.
 
29.	Anda juga bisa mengubah urutan bidang dalam tabel menggunakan tombol Pindahkan Bidang yang Dipilih. Setelah Anda selesai mengedit, klik pada ...  tombol di samping Refactored dan pilih Save To File... . Pilih File XLSX (*.xlsx) sebagai formatnya. Masukkan nama file sebagai park_area_by_landcover.xlsx dan klik Simpan. Dalam  dialog Refactor Fields, klik  Run  untuk menerapkan perubahan Anda.
 
30.	Hasilnya akan menjadi spreadheet dengan landcover dan  kolom area_sqkm.
 
<int 7>

1.	Pertama-tama kita akan memuat layer basemap dari OpenStreetMap dan kemudian mengimpor data CSV. Di  tab Browser, gulir ke bawah dan temukan  bagian Ubin XYZ.
 
2.	Perluas untuk melihat layer ubin OpenStreetMap. Seret dan lepas ke kanvas utama. Selanjutnya kita akan memuat file CSV. Klik  tombol Open Data Source Manager.
 
3.	Beralih ke tab Teks Dibatasi. Di sini kita akan mengimpor data kejahatan yang datang dalam file teks format CSV. Klik tombol ...  di sebelah Nama file  dan telusuri ke  file 2019-02-surrey-street.csv yang diunduh  . Bidang X dan bidang Y di  bagian Definisi Geometri akan diisi secara otomatis dengan  kolom Bujur dan Lintang  . Geometri CRS harus dibiarkan default EPSG: 4326 - definisi WGS 84. Pastikan data terlihat benar di  panel data Sampel dan klik Tambahkan, diikuti oleh Tutup.
 
4.	Anda akan melihat 2 layer - OpenStreetMap dan 2019-02-surrey-street dimuat di panel QGIS Layers. Klik kanan  layer 2019-02-surrey-street dan pilih Zoom to Layer.
 
5.	Anda akan melihat layer poin insiden kejahatan yang dilapis pada peta dasar OpenStreetMap. Zoom dan Pan untuk menjelajahi data. Datanya cukup padat dan sulit untuk mendapatkan gambaran di mana ada konsentrasi kejahatan yang tinggi. Di sinilah visualisasi peta panas akan berguna. Pilih layer 2019-02-surrey-street dan klik  tombol panel Open the Layer Styling.
 
6.	Pilih Heatmap sebagai perender di menu dropbox. Panel Layer Styling bersifat interaktif dan Anda dapat melihat efek perubahan Anda tercermin dalam kanvas segera. Layer sekarang akan ditampilkan dalam color-ramp skala abu-abu default.
 
7.	Peta panas biasanya perender menggunakan jalan warna kuning--ke-merah atau putih--ke-merah di mana konsentrasi titik yang lebih tinggi menghasilkan lebih banyak panas. Klik  menu tarik-turun Color ramp dan pilih Reds color-ramp.
 
8.	Selanjutnya Anda harus memilih Radius. Parameter ini menentukan lingkungan melingkar di sekitar setiap titik di mana titik itu akan memiliki pengaruh. Nilai ini akan sangat tergantung pada jenis data input Anda. Untuk data kita, mari kita asumsikan insiden kejahatan akan memiliki pengaruh hingga 5 Kilometer dari lokasi. Perhatikan bahwa proyek CRS saat ini diatur ke EPSG: 3857 di sudut kanan bawah. CRS ini memiliki satuan meter, jadi kita harus menentukan 5000 meter sebagai jari-jarinya. Parameter lain yang disembunyikan dari menu ini adalah bentuk Kernel. Ini adalah fungsi yang menentukan bagaimana pengaruh suatu titik harus tersebar di jari-jari yang diberikan. Perender Heatmap menggunakan  fungsi Quartic untuk perhitungan ini. Ada jenis kernel lain seperti Triangular, Uniform, Triweight dan Epanechnikov yang  dapat ditentukan ketika menggunakan metode pembuatan peta panas yang berbeda yang dijelaskan nanti dalam tutorial ini. Lihat posting ini untuk  penjelasan dan panduan yang baik untuk memilih radius dan bentuk kernel yang tepat.
 
9.	Visualisasi peta panas sudah siap. Kita dapat menyesuaikan Opacity dari heatmap di  bagian Layer Rendering di bagian bawah. Atur opacity ke 60% sehingga Anda dapat melihat basemap bersama dengan heatmap.
 
10.	Untuk banyak jenis analisis, hanya mempertimbangkan kepadatan titik sudah cukup baik. Namun terkadang, Anda mungkin ingin memberikan kepentingan yang berbeda untuk setiap poin. Kejahatan yang lebih kejam seharusnya memiliki pengaruh lebih besar pada peta panas keluaran daripada perampokan. Demikian pula, kadang-kadang suatu titik dapat mewakili beberapa pengamatan di satu lokasi yang perlu dipertanggungjawabkan dalam analisis. Untuk melakukan ini, Anda dapat menyediakan bidang bobot numerik opsional  yang menentukan nilai untuk setiap titik. Mari tambahkan bidang berat dan gunakan untuk meningkatkan peta panas. Klik kanan  layer 2019-02-surrey-street dan pilih Open Attribute Table.
 
11.	Anda akan melihat kolom teks yang disebut Jenis kejahatan  dalam data input yang menjelaskan jenis kejahatan. Kita dapat menggunakan ini untuk mengkategorikan berbagai jenis kejahatan dan menetapkan bobot yang lebih tinggi untuk kejahatan yang lebih kejam.
 
12.	Klik kalkulator bidang Buka.
 
13.	Sekarang kita akan memasukkan rumus yang menggunakan jenis Kejahatan dan menentukan nilai bobot. QGIS memiliki cara praktis untuk menambahkan bidang komputasi tersebut menggunakan Bidang Virtual. Bidang virtual disimpan dalam proyek QGIS dan tidak mengubah data sumber. Ini juga dihitung secara dinamis dan dapat digunakan di mana saja di QGIS seperti nilai atribut lainnya. Masukkan bobot sebagai nama bidang Output  dan atur jenis bidang Output ke Bilangan bulat (bilangan bulat). Masukkan ekspresi berikut di Editor ekspresi. Di sini kita menggunakan  pernyataan CASE untuk menetapkan nilai yang berbeda berdasarkan kondisi yang berbeda. Klik Oke.
PERKARA
KETIKA "Jenis kejahatan" SEPERTI 'Kekerasan%' MAKA 10
KETIKA "Jenis kejahatan" SEPERTI 'Kriminal%' MAKA 5
LAIN 1
UJUNG
 
14.	Atribut baru akan ditambahkan untuk setiap fitur dengan nilai bobot yang sesuai.
 
15.	Kembali ke panel Layer Styling, klik menu drop-down untuk Weight points by dan pilih bidang weight yang baru ditambahkan  .
 
16.	Anda akan melihat perubahan rendering peta panas untuk memperhitungkan parameter berat. Tutup panel Layer Styling.
 
17.	Jika Anda memerlukan visualisasi peta panas untuk disimpan sebagai lapisan raster permanen atau ingin menyesuaikan peta panas dengan opsi lanjutan seperti kernel atau radius dinamis yang berbeda, Anda dapat menggunakan Peta Panas (Estimasi Kerapatan Kernel) dari Kotak Alat Pemrosesan. Kami sekarang akan menggunakan algoritma ini. Pergi ke Pemrosesan ‣ Kotak Alat.
 
18.	Sebelum kita dapat membuat peta panas, kita perlu memproyeksikan ulang data sumber ke CRS yang diproyeksikan. Karena jarak memainkan peran penting dalam perhitungan peta panas, tidak benar menggunakan CRS geografis. Cari dan temukan algoritma layer Vector general ‣ Reproject.
 
19.	Dalam dialog Reproject layer, klik  tombol Select CRS  untuk Target CRS. Cari dan pilih EPSG:27700 OSGB 1936 / British National Grid CRS. CRS yang diproyeksikan ini adalah pilihan yang baik untuk data di Inggris. Klik Jalankan.
 
20.	Layer baru bernama Reprojected akan ditambahkan ke  panel Layers. Hapus centang kotak di sebelah  layer 2019-02-surrey-street lama  untuk menyembunyikannya.
 
21.	Cari dan temukan algoritma Interpolation ‣ Heatmap (Kernel Density Estimation). 
 
22.	Dalam dialog Heatmap (Kernel Density Estimation), kita akan menggunakan paramter yang sama seperti sebelumnya. Pilih Radius  sebagai 5000 meter dan Berat dari lapangan sebagai berat. Atur ukuran Pixel X dan ukuran Pixel Y menjadi 50 meter. Biarkan Kernel membentuk ke  nilai default Quartic. Klik Jalankan.
 
Catatan
Parameter Radius from field memungkinkan Anda menentukan radius pencarian dinamis untuk setiap titik. Ini dapat digunakan bersama dengan Weight from field untuk memiliki kontrol grainer halus tentang bagaimana pengaruh setiap titik menyebar.
23.	Setelah pemrosesan selesai, layer raster baru bernama OUTPUT akan dimuat. Visualisasi default jelek karena menggunakan  perender abu-abu Singleband. Klik  tombol Open the Layer Styling panel.
 
24.	Ubah render ke Singleband Pseudocolor dan pilih  ramp warna Reds. Layer sekarang terlihat seperti visualisasi heatmap yang telah kita buat sebelumnya.
 
Catatan
Perhatikan bahwa layer OUTPUT di  panel Layers memiliki legenda tetapi  layer 2019-02-surrey-street tidak. Masalah umum dengan menggunakan lapisan peta panas yang dibuat dengan perender Heatmap adalah kurangnya legenda. Katakanlah Anda ingin menggunakan peta panas di Tata Letak Cetak dan menambahkan legenda. Peta panas raster yang dibuat dengan metode algoritma pemrosesan Heatmap memungkinkan hal ini.

<int 8>

1.	Di Panel Browser QGIS, cari direktori tempat Anda menyimpan data yang diunduh. Perluas ne_10m_land.zip dan pilih  layer ne_10m_land.shp. Seret layer ke kanvas. Selanjutnya, cari  file ASAM_shp.zip. Perluas dan pilih  layer asam_data_download/ASAM_events.shp dan seret ke kanvas.
 
2.	Setelah lapisan dimuat, Anda dapat melihat titik-titik individual yang mewakili insiden lokasi pembajakan. Ada ribuan insiden dan sulit untuk menentukan dengan lebih banyak pembajakan. Daripada titik individual, cara yang lebih baik untuk memvisualisasikan data ini adalah melalui peta panas. Pilih layer ASAM_events dan klik  tombol Open the layer Styling Panel di  panel Layers. Klik  drop-down Simbol tunggal.
 
3.	Di drop-down pemilihan perender, pilih Perender peta panas. Selanjutnya, pilih  ramp warna Viridis dari  pemilih Color ramp.
 
4.	Sesuaikan nilai Radius ke 5.0. Di bagian bawah, perluas  bagian Layer Rendering dan sesuaikan Opacity menjadi 75.0%. Ini memberikan efek visual yang bagus dari hotspot dengan lapisan tanah di bawahnya.
 
5.	Sekarang mari kita animasikan data ini untuk menunjukkan peta tahunan insiden pembajakan. Klik kanan pada layer ASAM_event, dan pilih Properties.
 
6.	Dalam kotak dialog Layer properties, pilih  tab Temporal dan aktifkan dengan mengklik kotak centang..
 
7.	Data sumber berisi atribut dateofocc - mewakili tanggal di mana insiden itu terjadi. Ini adalah bidang yang akan digunakan untuk menentukan poin yang diberikan untuk setiap periode waktu. Pilih  Bidang Tunggal dengan Data/Waktu di  menu Tarik-turun Konfigurasi, dateofocc sebagai Bidang.
 
8.	Sekarang simbol jam akan muncul di sebelah nama layer. Klik pada Temporal Control Panel (ikon Jam) dari Map Navigation Toolbar.
 
9.	Klik pada Animated Temporal Navigation (ikon putar) untuk mengaktifkan kontrol animasi. Klik Atur ke Rentang Penuh (ikon refresh) di samping Rentang untuk secara otomatis mengatur rentang waktu agar sesuai dengan himpunan data.
 
10.	Sekarang Anda siap untuk melihat pratinjau animasi. Atur Langkah sebagai 1 Tahun lalu klik tombol Putar untuk memulai animasi.
 
Catatan
Jika animasi terlalu cepat, Anda dapat menyesuaikan frame rate dengan mengklik Temporal Settings (ikon roda gigi kuning) di sudut kanan atas panel Temporal Controller. Mengurangi frame rate (frame per detik) akan memperlambat animasi.
11.	Akan sangat membantu untuk juga menampilkan label yang menunjukkan kerangka waktu saat ini di peta. Kita bisa melakukannya menggunakan dekorasi Judul bawaan. Pergi ke Lihat  ‣  Dekorasi ‣ Label Judul.
 
12.	Klik kotak centang untuk mengaktifkannya dan klik tombol Sisipkan Ekspresi dan masukkan ekspresi berikut untuk menampilkan tahun. Di sini variabel @map_start_time berisi stempel waktu dari irisan waktu saat ini yang ditampilkan. Jadi kita dapat menggunakan stempel waktu itu dan memformatnya untuk menampilkan tahun kejadian. Lihat Dokumentasi QGIS untuk  detail tentang berbagai opsi pemformatan yang didukung untuk stempel waktu.
format_date(@map_start_time, 'yyyy')
 
13.	Pilih ukuran font sebagai 25,  atur warna bilah latar belakang sebagai Putih dan atur transparansi menjadi 50%.  Di Penempatan pilih Kanan Bawah. Sekarang klik Ok.
 
14.	Setelah parameter ditetapkan sesuai, tahun akan ditampilkan seperti yang ditunjukkan. Untuk mengekspor ini sebagai gambar dan mengonversinya sebagai GIF, pilih Ekspor Animasi (ikon simpan) di jendela kontrol Temporal.
 
15.	Klik pada ...  Direktori output untuk memilih direktori tempat gambar akan disimpan.
 
16.	Di bawah Extent pilih  layer Calculate from Layer ‣ ne_10_land. Klik Simpan.
 
17.	Setelah ekspor selesai, Anda akan melihat gambar PNG untuk setiap tahun (total 18 gambar) di direktori output.
 
18.	Sekarang mari kita buat GIF animasi dari gambar-gambar ini. Ada banyak pilihan untuk membuat animasi dari bingkai gambar individu. Saya suka ezgif untuk alat yang mudah dan online. Kunjungi situs dan klik Pilih File dan pilih semua file .png. Setelah dipilih, klik Unggah dan buat GIF!  kancing. Setelah dibuat, Anda dapat mengunduh GIF menggunakan Menyimpan  tombol.

<int 9>

1.	Browse ke file India-States.zip yang diunduh  di QGIS Browser. Perluas dan seret  file India-States.shp ke kanvas peta.
 
2.	Anda akan melihat layer India-States baru  dimuat di  panel Layers. Pergi ke Pemrosesan ‣ Kotak Alat.
 
3.	Kami akan attemp untuk menjalankan algoritma pemrosesan pada lapisan input untuk menunjukkan bagaimana geometri yang tidak valid dapat menyebabkan masalah selama operasi geoprocessing. Cari dan temukan Kartografi ‣ Algoritma  pewarnaan topologis. Klik dua kali untuk meluncurkannya.
 
4.	Dalam dialog Pewarnaan topologis, pilih India-States sebagai layer Input. Biarkan semua parameter lainnya tetap default dan klik Jalankan.
 
Catatan
Algoritma pewarnaan topologis mengimplementasikan algoritma untuk mewarnai peta sehingga tidak ada poligon yang berdekatan memiliki warna yang sama. Ini adalah teknik kartografi yang berguna dan Teorema Empat Warna menyatakan bahwa 4 warna sudah cukup untuk mencapai hasil ini. Ada versi teori grafik dari thorem ini yang disebut Teorema lima warna. Implementasi algoritma QGIS didasarkan pada grafik sehingga secara practive Anda akan melihat bahwa lapisan poligon yang kompleks seperti ini akan membutuhkan hingga 5 warna.
5.	Saat algoritma berjalan, Anda akan melihat peringatan yang ditampilkan di tab Log.  1 fitur di lapisan input memiliki geometri yang tidak valid dan dilewati selama pemrosesan. Pengaturan default untuk menangani geometri tidak valid di Kotak Alat Pemrosesan terletak di Pengaturan  ‣  Opsi  ‣  Pemrosesan  ‣  Umum ‣ Pemfilteran fitur tidak valid  dan diatur ke Lewati (abaikan) fitur dengan geometri yang tidak valid. Ini adalah pengaturan default yang bagus, tetapi jika input Anda besar, Anda mungkin melewatkan peringatan ini dan mungkin tidak tahu bahwa fitur input dilewati. Anda mungkin ingin mengubah nilai ke Hentikan eksekusi algoritma ketika geometri tidak valid.
 
6.	Kembali ke jendela utama QGIS, Anda akan melihat layer baru Berwarna ditambahkan ke  panel Layers. Perhatikan bahwa layer baru kehilangan status yang memiliki geometri tidak valid. Kita sekarang tahu bahwa poligon keadaan khusus ini memiliki geometri yang tidak valid tetapi kita tidak tahu apa penyebabnya. Kita dapat dengan mudah mengetahuinya. Cari dan temukan geometri Vektor ‣ Periksa algoritma validitas.
 
7.	Dalam dialog Periksa Validitas, pilih India-States sebagai lapisan Input. Pilih GEOS sebagai metode. Klik Jalankan.
 
8.	Saat algoritma selesai diproses, Anda akan melihat 3 layer baru di panel Layers - Output yang valid,  output tidak valid dan output Kesalahan. Output Layer Error berisi lokasi dan deskripsi kesalahan geometri. Klik kanan dan pilih Buka Tabel Atribut.
 
Catatan
Dokumentasi QGIS memiliki artikel rinci tentang Jenis pesan kesalahan dan artinya yang menjelaskan penyebab semua kesalahan.
9.	Anda akan melihat bahwa pesan kesalahannya adalah Ring self-intersection. Pilih baris dan klik  tombol Zoom map to selected features. Saat Anda memperbesar, Anda akan melihat akar penyebab kesalahan geometri.
 
10.	QGIS dilengkapi dengan algoritma built-in untuk memperbaiki kesalahan geometri secara otomatis. Cari dan temukan geometri Vektor ‣ Perbaiki algoritma geometri. Klik dua kali untuk menjalankannya.
 
11.	Dalam dialog Fix Geometries, pilih India-States sebagai layer Input dan klik Run.
 
12.	Layer baru Fixed Geometries akan ditambahkan ke  panel Layers. Pada titik ini, kesalahan geometri telah diperbaiki dan Anda dapat menjalankan algoritma pemrosesan apa pun pada lapisan ini tanpa masalah. Tetapi kita dapat melihat bahwa masih ada celah antara poligon yang berdekatan yang tidak terduga dan dapat menyebabkan kesalahan topologis di telepon. Kita dapat memperbaikinya juga dengan mengedit poligon. Klik  tombol Toggle Editing di Digitizing Toolbar. Pilih Vertex Tool dan dari drop-down pilih Vertex Tool (Current Layer).
 
13.	Ketika alat vertex aktif, klik pada vertex untuk memilihnya. Anda dapat menekan tombol Delete untuk  menghapus vertex atau menyeretnya untuk memindahkannya. Anda dapat memindahkan verteks sehingga tepi poligon sekarang menyentuh poligon yang berdekatan.
 
14.	Setelah selesai, klik tombol Toggle Editing lagi dan klik Save.
 
15.	Mari kita jalankan algoritma  pewarnaan Kartografi ‣ Topologis lagi.  
 
16.	Dalam dialog Pewarnaan Topologis, pastikan Anda memilih Geometri Tetap sebagai lapisan Input. Klik Jalankan.
 
17.	Anda akan melihat algoritma berjalan tanpa kesalahan dan layer Colored baru  akan ditambahkan ke  panel Layers. Perhatikan bahwa algoritma tidak mewarnai layer dengan sendirinya, tetapi bekerja dengan menambahkan kolom baru yang disebut color_id ke setiap poligon yang  dapat digunakan untuk menetapkan warna unik yang berbeda dari poligon yang berdekatan. Pilih  layer Colored dan klik  tombol Open the Layer Styling Panel.
 
18.	Pilih Perender yang dikategorikan dan kolom color_id  sebagai Nilai. Klik Klasifikasikan. Anda sekarang akan melihat peta berwarna sehingga poligon yang berdekatan memiliki warna yang berbeda.
 


 





