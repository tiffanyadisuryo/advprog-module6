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



