Diberikan sebuah binary bernama:

```bash
pemeriksa_kredensial
```

Challenge meminta kita menemukan flag yang tersembunyi di dalam program.

Petunjuk:

> Semakin banyak lapisan transformasi, semakin penting urutan pembalikannya.

Petunjuk ini mengarah pada adanya beberapa tahap encoding/transformasi yang harus dibalik satu per satu.

---

## 1. Identifikasi File

Cek tipe file:

```bash
file pemeriksa_kredensial
```

Output:

```text
ELF 64-bit LSB executable
```

Binary Linux biasa.

---

## 2. Enumerasi String

Daripada langsung buka Ghidra sambil berharap keajaiban turun dari langit, cek dulu string yang ada:

```bash
strings pemeriksa_kredensial
```

Ditemukan string menarik:

```text
fWQza2M0cmNfbjE0aGNfNDYzczRiXzNzcjN2M3J7Z2FsZg==
```

String ini sangat mencurigakan karena formatnya mirip Base64.

---

## 3. Decode Base64

Gunakan:

```bash
echo "fWQza2M0cmNfbjE0aGNfNDYzczRiXzNzcjN2M3J7Z2FsZg==" | base64 -d
```

Output:

```text
}d3kc4rc_n14hc_463s4b_3sr3v3r{galf
```

Masih belum terlihat seperti flag.

Biasanya flag formatnya:

```text
flag{...}
```

Sedangkan hasil ini justru:

```text
}....{galf
```

Nah ini sudah mencurigakan.

---

## 4. Reverse String

Balik urutan karakter.

Python:

```python
s = "}d3kc4rc_n14hc_463s4b_3sr3v3r{galf"
print(s[::-1])
```

Output:

```text
flag{r3v3rs3_b4s364_ch41n_cr4ck3d}
```

---

## 5. Verifikasi

Format flag valid:

```text
flag{...}
```

Isi flag juga sesuai tema challenge:

```text
r3v3rs3_b4s364_ch41n_cr4ck3d
```

yang berarti:

```text
reverse base64 chain cracked
```

Sangat cocok dengan petunjuk tentang membalik urutan transformasi.

---

# Alur Transformasi

Kemungkinan proses yang dilakukan pembuat challenge:

```text
flag{r3v3rs3_b4s364_ch41n_cr4ck3d}
        ↓
dibalik (reverse)
        ↓
}d3kc4rc_n14hc_463s4b_3sr3v3r{galf
        ↓
Base64 encode
        ↓
fWQza2M0cmNfbjE0aGNfNDYzczRiXzNzcjN2M3J7Z2FsZg==
```

Untuk mendapatkan flag:

```text
Base64 decode
        ↓
reverse string
        ↓
flag
```

Urutannya harus dibalik dari proses pembuatannya.

---

# Flag

```text
flag{r3v3rs3_b4s364_ch41n_cr4ck3d}
```
