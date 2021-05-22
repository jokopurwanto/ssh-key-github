# SSH Key GitHub
![SSH](https://user-images.githubusercontent.com/32213421/119216965-2a103a00-bb01-11eb-8e15-e626a62ee96c.JPG)

Pengenalan sedikit mengenai SSH, **SSH (Secure Shell Protocol)** adalah protokol jaringan yang digunakan untuk berkomunikasi data melalui jalur yang aman. SSH digunakan sebagai pengganti dari telnet karena lebih aman. Perbedaan mendasar antara SSH dan telnet yaitu dari pengiriman data informasi, pada telnet data informasi yang dikirim tidak di enkripsi sehingga rawan untuk di hack sedangkan pada SSH data informasi yang dikirim akan di enkripsi sehingga terjamin kerahasian datanya.

## Setup SSH Key

![SSH Key](https://user-images.githubusercontent.com/32213421/119216966-2bd9fd80-bb01-11eb-9f9b-ee21974a0953.JPG)

Pada kasus ini kita akan menggunakan SSH untuk keperluan `push` ke `repository` github tanpa perlu lagi menginputkan username & password (login). Hal ini bisa dilakukan dengan menggunakan SSH Key sehingga proses otentikasi akun berjalan secara otomatis. SSH Key adalah kunci pasangan dua arah (`private key` dan `public key`) yang di enkripsi dan saling berhubungan, bisa di ibaratkan `private key` sebagai kunci dan `public key` sebegai gembok. 

Adapun proses setup SSH Key sebagai berikut:
1.	Generate SSH key (`private key` dan `public key`).
2.	Add `private key` ke `ssh-agent`.
3.	Copy `public key` ke github.
4.	Tes koneksi SSH key dari lokal PC ke github.

## Generate SSH Key

Pertama generate SSH key dengan menggunakan perintah berikut di terminal.
```
# ssh-keygen -t rsa
```
Kemudian pada `Enter file in which to save the key` masukan id SSH key pada contoh di sini menggunakan `id` **github_jokopurwanto**, pada `passpharse` dapat dikosongkan namun jika diisi maka pada saat menambahkan `private key` ke `ssh-agent` perlu memasukan kembali `passpharse` yang telah dibuat.

![Generate SSH Key](https://user-images.githubusercontent.com/32213421/119176597-80479380-ba95-11eb-9d5b-f1d90b66ec2f.png)

Pada direktori `~/.ssh/` akan terbuat dua file key baru yaitu **github_jokopurwanto** (`private key`) dan **github_jokopurwanto.pub** (`public key`).

![List SSH Key](https://user-images.githubusercontent.com/32213421/119176601-8178c080-ba95-11eb-978c-44145a2f114a.png)

## Add Private Key
Jalankan `ssh-agent` dengan menggunakan perintah berikut:
```
# eval $(ssh-agent -s) 
```

![SSH Agent](https://user-images.githubusercontent.com/32213421/119176602-82115700-ba95-11eb-9291-05d88fe2ca2e.png)

Setelah `ssh-agent` berjalan maka langkah berikutnya kita tambahkan SSH `private key` ke `ssh-agent` dengan menggunakan perintah berikut:
```
# ssh-add github_jokopurwanto
```

![SSH Add](https://user-images.githubusercontent.com/32213421/119176603-82a9ed80-ba95-11eb-961a-db5bdf03b2f0.png)

## Copy Public Key
Selanjutnya copy content data pada file **github_jokopurwanto.pub** (`public key`) dengan menggunakan perintah berikut:
```
# cat github_jokopurwanto.pub 
```
Lalu copy semua teks yang ditampilkan.

![Public Key](https://user-images.githubusercontent.com/32213421/119176604-82a9ed80-ba95-11eb-87d9-947c39e95b37.png)

Kemudian buka akun github lalu masuk ke menu **Settings => SSH and GPG Keys**. Klik **New SSH key**, lalu pada bagian:
- **Title**: Dapat di isi dengan label untuk SSH key.
- **Key**: Dapat di isi dengan mem-paste data pada file **github_jokopurwanto.pub** (`public key`) yang sebelumnya telah dibuat dan dicopy.

![GitHub Settings](https://user-images.githubusercontent.com/32213421/119176607-83428400-ba95-11eb-8b10-c1d1070f692d.png)

![GitHub SSH & GPG](https://user-images.githubusercontent.com/32213421/119176610-83db1a80-ba95-11eb-82e7-f13d5b77dae3.png)

![GitHub Add SSH key](https://user-images.githubusercontent.com/32213421/119176611-83db1a80-ba95-11eb-8a84-addc2db0f615.png)

## Tes Koneksi SSH key
Selanjutnya melalukan tes koneksi SSH key dari lokal PC ke github dengan menggunakan perintah berikut:
```
# ssh -T git@github.com 
```
Jika berhasl maka akan muncul tampilan teks berikut:

`Hi jokopurwanto! You've successfully authenticated, but GitHub does not provide shell access.`

![Tes Koneksi SSH key](https://user-images.githubusercontent.com/32213421/119176612-8473b100-ba95-11eb-9270-01c4565bd681.png)

Maka sekarang setiap kali melakukan push menggunakan SSH, kita tidak perlu lagi memasukan username & password karena proses otentikasi akun sudah dilakukan menggunakan SSH key.
