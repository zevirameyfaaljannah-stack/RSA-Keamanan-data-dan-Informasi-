# Implementasi Algoritma RSA Menggunakan Python

Program ini merupakan implementasi sederhana dari algoritma kriptografi **RSA (Rivest–Shamir–Adleman)** menggunakan bahasa pemrograman Python. Program ini melakukan proses pembuatan kunci, enkripsi pesan, dan dekripsi pesan.

---

# 1. Import Library

Pada awal program digunakan dua library Python yaitu `random` dan `math`.

- `random` digunakan untuk menghasilkan angka acak.
- `math` digunakan untuk operasi matematika seperti mencari nilai gcd (Greatest Common Divisor).

Contoh kode:

import random
import math

---

# 2. Fungsi Pengecekan Bilangan Prima

Fungsi `cek_prima()` digunakan untuk mengecek apakah suatu angka merupakan bilangan prima atau bukan.

Cara kerja fungsi ini adalah dengan melakukan pengecekan pembagian angka dari 2 sampai akar kuadrat dari angka tersebut. Jika angka habis dibagi oleh angka lain selain 1 dan dirinya sendiri, maka angka tersebut bukan bilangan prima.

Contoh kode:

def cek_prima(x):
    if x < 2:
        return False
    for i in range(2, int(x ** 0.5) + 1):
        if x % i == 0:
            return False
    return True

---

# 3. Fungsi Mengambil Bilangan Prima Acak

Fungsi `ambil_prima()` digunakan untuk menghasilkan bilangan prima secara acak.

Program akan mengambil angka acak antara 100 sampai 300. Angka tersebut kemudian dicek menggunakan fungsi `cek_prima()`. Jika angka tersebut merupakan bilangan prima, maka angka tersebut akan digunakan sebagai nilai p atau q.

Contoh kode:

def ambil_prima():
    while True:
        angka = random.randrange(100, 300)
        if cek_prima(angka):
            return angka

---

# 4. Fungsi Modular Inverse

Fungsi `invers_modulo()` digunakan untuk mencari nilai invers modulo yang diperlukan dalam perhitungan kunci privat RSA.

Nilai ini digunakan untuk mencari nilai **d** sehingga memenuhi persamaan:

d × e ≡ 1 mod φ(n)

Contoh kode:

def invers_modulo(a, m):
    m0 = m
    y = 0
    x = 1

    if m == 1:
        return 0

    while a > 1:
        q = a // m
        t = m

        m = a % m
        a = t
        t = y

        y = x - q * y
        x = t

    if x < 0:
        x += m0

    return x

---

# 5. Menentukan Bilangan Prima p dan q

Program akan mengambil dua bilangan prima secara acak menggunakan fungsi `ambil_prima()`.

Jika nilai p dan q sama, maka program akan mengambil ulang nilai q sampai berbeda.

Contoh kode:

p = ambil_prima()
q = ambil_prima()

while p == q:
    q = ambil_prima()

---

# 6. Menghitung Nilai n dan φ(n)

Setelah mendapatkan bilangan prima p dan q, program menghitung nilai:

n = p × q  
φ(n) = (p − 1) × (q − 1)

Nilai n digunakan sebagai bagian dari public key dan private key.

Contoh kode:

n = p * q
phi = (p - 1) * (q - 1)

---

# 7. Menentukan Nilai e

Nilai e dipilih secara acak dengan syarat harus relatif prima dengan φ(n). Program akan mengecek menggunakan fungsi `gcd` dari library math.

Contoh kode:

e = random.randrange(2, phi)

while math.gcd(e, phi) != 1:
    e = random.randrange(2, phi)

---

# 8. Menghitung Nilai d (Private Key)

Nilai d dihitung menggunakan fungsi `invers_modulo()` yang merupakan invers dari e terhadap φ(n).

Contoh kode:

d = invers_modulo(e, phi)

---

# 9. Menampilkan Kunci RSA

Program kemudian menampilkan nilai p, q, n, φ(n), public key, dan private key.

Contoh kode:

print("Bilangan Prima p :", p)
print("Bilangan Prima q :", q)
print("Nilai n :", n)
print("Nilai phi :", phi)
print("Public Key :", (e, n))
print("Private Key :", (d, n))

---

# 10. Proses Enkripsi

Pesan yang akan dienkripsi harus berupa angka dan lebih kecil dari n.

Rumus enkripsi RSA:

C = M^e mod n

Contoh kode:

pesan = 15
cipher = pow(pesan, e, n)

print("\nPesan Asli :", pesan)
print("Hasil Enkripsi :", cipher)

---

# 11. Proses Dekripsi

Ciphertext yang dihasilkan akan didekripsi menggunakan private key dengan rumus:

M = C^d mod n

Contoh kode:

hasil_dekripsi = pow(cipher, d, n)

print("Hasil Dekripsi :", hasil_dekripsi)

---

# Kesimpulan

Program ini merupakan implementasi sederhana dari algoritma RSA yang menunjukkan bagaimana sistem kriptografi kunci publik bekerja.

Tahapan utama dalam algoritma RSA meliputi:

1. Membuat dua bilangan prima p dan q
2. Menghitung nilai n dan φ(n)
3. Menentukan public key (e,n)
4. Menghitung private key (d,n)
5. Melakukan proses enkripsi pesan
6. Melakukan proses dekripsi pesan

RSA banyak digunakan dalam sistem keamanan digital seperti enkripsi data, komunikasi aman, dan sistem autentikasi.
