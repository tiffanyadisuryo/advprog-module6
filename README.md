# Tutorial 6

## Commit 1 Reflection notes
```rust
fn handle_connection(mut stream: TcpStream) { 
```
mendefinisikan function handle_connection yang argumennya reference mutable dari TcpStream. 
<br><br>

```rust
    let buf_reader = BufReader::new(&mut stream);
```
membuat object BufReader yang melakukan wrap pada reference mutable TcpStream di atas. 
<br><br>

```rust
    let http_request: Vec<_> = buf_reader
        .lines()
        .map(|result| result.unwrap())
        .take_while(|line| !line.is_empty())
        .collect();
```
line ini membaca setiap line pada buf_reader. 
- ```.lines()``` mengembalikan tiap line di buf_reader
- ```.map(|result| result.unwrap())``` memetakan ekstraksi String value dari result, yang didapat dari hasil pengambilan line di atasnya.
- ```.take_while(|line| !line.is_empty())``` for loop dalam bentuk while, sehingga saat line belum kosong maka akan terus mengiterasi.
- ```.collect();``` mengumpulkan semua HTTP requestnya dan dibuat menjadi ```Vec<_>``` 
<br><br>

```rust
    println!("Request: {:#?}", http_request);
```
print hasil http_request 
<br><br>

## Commit 2 Reflection Notes
```rust
let contents = fs::read_to_string("hello.html").unwrap();
```
Kode ini membaca setiap isi dari hello.html dan melakukan ekstraksi ke bentuk string. Contents yang akan ditampilkan pada kode dibawah merupakan isi dari hello.html ini.
<br><br>

```rust
let response = format!("{status_line}\r\nContent-Length:{length}\r\n\r\n{contents}");
```
Kode ini membentuk respon http dengan format, status http, panjang content, dan isi content itu sendiri dari kode di atasnya. status_line didefinisikan di atasnya dan jika ditandai sukses maka akan masuk ke hello.html. 
<br><br>

![ss_comimt_2](https://github.com/tiffanyadisuryo/advprog-module6/assets/119838581/c6c6130a-86eb-444e-a77e-ac9145fce970)

Sehingga pada akhirnya memunculkan tulisan dari file hello.html pada link tersebut seperti gambar di atas. Ini berarti status_line menandakan sukses ditemukannya route.
<br><br>

## Commit 3 Reflection Notes
```rust
let request_line = buf_reader.lines().next().unwrap().unwrap();
```
line ini bertujuan untuk mendapat request line agar nantinya dapat diidentifikasi apakah ditemukan atau tidak alamat route-nya.
<br><br>

```rust
    let (status_line, filename) = if request_line == "GET / HTTP/1.1" {
        ("HTTP/1.1 200 OK", "hello.html")
    } else {
        ("HTTP/1.1 404 NOT FOUND", "404.html")
    };
```
setelah didapat request line dari kode di atas, maka langsung diidentifikasi masuk ke yang sukses atau not found, dari situ dilanjutkan kepada file html-nya masing-masing. Pada kasus di atas jika route nya ditemukan atau sukses maka akan masuk ke hello.html yang sudah dibuat sebelumnya. Sementara, jika request_line-nya menunjukkan bahwa tidak ditemukan route maka akan masuk ke 404.html yang sudah dibuat. Ini dilakukan biar kodenya tidak terlalu banyak perulangan. 
<br><br>

![ss_comimt_3](https://github.com/tiffanyadisuryo/advprog-module6/assets/119838581/9824fa51-5aa6-4055-b658-1857ff5f66bd)
Maka hasilnya akan menjadi seperti berikut.
<br><br>

## Commit 4 Reflection Notes
```rust
thread::sleep(Duration::from_secs(10))
```
line tersebut menyebabkan tiap kali masuk route /sleep akan delay selama 10 detik sebelum response terkirim. Ini dikarenakan cuma ada 1 thread dan satu-satunya thread itu diberhentikan bselama 10 detik. Maka tentunya user cuma bisa mengandalkan 1 thread itu. Ini dilakukan untuk melihat bagaimana system dapat handle respon yang lambat. Tentunya dalam kasus ini terlihat bahwa saat satu thread ini sleep, maka tidak ada pilihan lainnya. Makanya sebaiknya menggunakan concurrency agar lebih efisien.
<br><br>

## Commit 5 Reflection Notes
Threadpool adalah kumpulan thread yang telah dibuat sebelumnya. Ini dilakukan agar saat dibutuhkan thread yang banyak, tinggal menggunakan yang sudah ada di threadpool. Pembuatan thread pool dimulai dari kode ```ThreadPool::new(size)``` dimana size menandakan jumlah worker threads yang diinginkan pada pool. Channel  ```mpsc::channel``` dibuat untuk menjalin komunikasi antar main thread dengan worker thread yang lain. Receiver end dari channel di-wrapped dengan ```Arc<Mutex<mpsc::Receiver<Job>>>``` agar concurrent dapat dilakukan antar main thread dan worker thread. 'Job'yang akan dilakukan oleh thread dikirim ke sender dari channel menggunakan ```sender.as_ref().unwrap().send(job)```. Worker akan terus mencoba mengambil job dengan loop. Saat job dilakukan dengan sukses maka akan print message. 

## Commit Bonus Reflection notes
Saya mengimplementasikan kode yang mirip pada 13-3. Jadi build-nya mengembangkan bagian input-output. Jika sebelumnya dengan new, jika tidak ditemukan thread maka akan panic. Sekarang, saat thread tidak dibuat maka tidak akan panic, dan akan memunculkan error. Saya menggunakan PoolCreationError. Isi dari PoolCreationError adalah string errornya.