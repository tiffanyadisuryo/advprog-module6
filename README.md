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
membaca setiap isi dari hello.html dan melakukan ekstraksi ke bentuk string
<br><br>

```rust
let response = format!("{status_line}\r\nContent-Length:{length}\r\n\r\n{contents}");
```
membentuk respon http dengan format, status http, panjang content, dan isi content itu sendiri dari kode di atasnya.
<br><br>

![ss_comimt_2](https://github.com/tiffanyadisuryo/advprog-module6/assets/119838581/c6c6130a-86eb-444e-a77e-ac9145fce970)

Sehingga pada akhirnya memunculkan tulisan dari file hello.html pada link tersebut seperti gambar di atas.
<br><br>

## Commit 3 Reflection Notes
```rust
let request_line = buf_reader.lines().next().unwrap().unwrap();
```
line ini bertujuan untuk mendapat request line agar nantinya dapat diidentifikasi apakah ditemukan atau tidak alamatnya.
<br><br>

```rust
    let (status_line, filename) = if request_line == "GET / HTTP/1.1" {
        ("HTTP/1.1 200 OK", "hello.html")
    } else {
        ("HTTP/1.1 404 NOT FOUND", "404.html")
    };
```
setelah didapat request line dari kode di atas, maka langsung diidentifikasi masuk ke yang sukses atau not found, dari situ dilanjutkan kepada file html-nya masing-masing.
<br><br>


### Commit 4 Reflection Notes
```rust
thread::sleep(Duration::from_secs(10))
```
line tersebut menyebabkan tiap kali masuk route /sleep akan delay selama 10 detik sebelum response terkirim. Ini dilakukan untuk melihat bagaimana system dapat handle respon yang lambat.
<br><br>
