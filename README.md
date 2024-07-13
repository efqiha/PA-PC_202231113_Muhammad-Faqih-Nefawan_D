# PA-PC_202231113_Muhammad-Faqih-Nefawan_D
Project UAS Pengolahan Citra 2024 
---
Pada project ini, saya Muhammad Faqih Nefawan dengan NIM 202231113 akan menjelaskan pengerjaan proyek UAS yang dikerjakan terkait dengan deteksi warna dan segmentasi pada gambar RGB dengan Python.

## Pendahuluan
### Latar Belakang
Pengolahan citra digital terus berkembang dengan cepat berkat banyak aplikasi praktis dalam berbagai bidang. Dalam pengolahan gambar, deteksi warna memungkinkan komputer untuk mengenali dan membedakan objek berdasarkan warnanya. Untuk segmentasi gambar medis dan pengenalan objek dalam gambar dan video, teknik ini sangat bermanfaat. Selain dalam bidang pengolahan gambar, deteksi warna memiliki banyak aplikasi lain. Analisis kualitas produk, kontrol kualitas industri, robotika, dan navigasi termasuk dalam kategori ini. Pada proyek ini, akan diterapkan teknik deteksi warna dan segmentasi pada gambar berwarna RGB dengan menggunakan library OpenCV dan bahasa pemrograman Python.

### Tinjauan Pustaka
Thresholding dan segmentasi warna digunakan untuk mendeteksi warna pada proyek ini. Thresholding mengklasifikasikan pixel berdasarkan intensitas warna. Teknik ini sangat berguna untuk memisahkan objek dari background berdasarkan warna yang telah didefinisikan.

### Tujuan
- Mempelajari dan menerapkan metode deteksi warna pada gambar RGB menggunakan library OpenCV.
- Menggunakan teknik thresholding untuk segmentasi warna.
- Memahami dan menganalisis hasil deteksi warna menggunakan visualisasi.
- Mendokumentasikan langkah-langkah proyek dan hasilnya.

## Langkah Pengerjaan
### Deteksi Warna pada Citra
#### Membaca Citra
Langkah pertama yang dilakukan adalah membaca citra RGB dari file menggunakan fungsi `cv2.imread`. Citra yang digunakan dalam proyek ini adalah file "Sawo_kecil_uwu.jpg".

```python
import cv2
from matplotlib import pyplot as plt
import numpy as np
```
``` python
img = cv2.imread("Sawo_kecil_uwu.jpg")
```

#### Konversi Warna
Citra yang dibaca dikonversi dari format BGR ke format RGB untuk visualisasi yang benar menggunakan fungsi `cv2.cvtColor`.

```python
rgb_img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
```

- `cv2.COLOR_BGR2RGB`: Parameter ini digunakan untuk mengubah warna gambar dari BGR yang merupakan default warna dari OpenCV ke RGB untuk mendapatkan warna gambar asli.

#### Konversi ke Ruang Warna HSV
Citra kemudian dikonversi ke ruang warna HSV untuk memudahkan proses thresholding berdasarkan warna.

```python
hsv_img = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
```

- `cv2.COLOR_BGR2HSV`: Parameter ini digunakan untuk mengubah warna gambar dari BGR ke HSV (Hue, Saturation, Value), yang memisahkan informasi warna (Hue) dari informasi intensitas (Value), membuatnya lebih mudah untuk thresholding berdasarkan warna.

#### Thresholding untuk Deteksi Buah
Range warna untuk buah didefinisikan dalam ruang warna HSV. Thresholding dilakukan untuk mendapatkan mask buah.

```python
lower_sawo = np.array([5, 100, 20])
upper_sawo = np.array([25, 255, 255])

mask_sawo = cv2.inRange(hsv_img, lower_sawo, upper_sawo)
```

- `lower_sawo`: Batas bawah untuk warna buah dalam ruang warna HSV. Pada contoh ini, nilai `[5, 100, 20]` menunjukkan bahwa Hue minimal adalah 5, Saturation minimal adalah 100, dan Value minimal adalah 20.
- `upper_sawo`: Batas atas untuk warna buah dalam ruang warna HSV. Pada contoh ini, nilai `[25, 255, 255]` menunjukkan bahwa Hue maksimal adalah 25, Saturation maksimal adalah 255, dan Value maksimal adalah 255.

Fungsi `cv2.inRange` digunakan untuk melakukan thresholding, di mana semua pixel yang berada dalam range warna yang ditentukan akan diset ke nilai maksimal (255), sementara yang di luar range akan diset ke nilai minimal (0).

#### Segmentasi Buah
Segmentasi buah dilakukan dengan operasi bitwise-AND antara mask buah dan citra asli.

```python
segment_sawo = cv2.bitwise_and(rgb_img, rgb_img, mask=mask_sawo)
```

- `rgb_img`: Gambar asli dalam format RGB.
- `mask_sawo`: Mask hasil thresholding yang menunjukkan area buah.

#### Thresholding untuk Deteksi Daun
Range warna untuk daun didefinisikan dalam ruang warna HSV. Thresholding dilakukan untuk mendapatkan mask pada daun.

```python
lower_daun = np.array([30, 40, 40])
upper_daun = np.array([80, 255, 255])

mask_daun = cv2.inRange(hsv_img, lower_daun, upper_daun)
```

- `lower_daun`: Batas bawah untuk warna daun dalam ruang warna HSV. Pada kasus ini, nilai `[30, 40, 40]` menunjukkan bahwa Hue minimal adalah 30, Saturation minimal adalah 40, dan Value minimal adalah 40.
- `upper_daun`: Batas atas untuk warna daun dalam ruang warna HSV. Pada kasus ini, nilai `[80, 255, 255]` menunjukkan bahwa Hue maksimal adalah 80, Saturation maksimal adalah 255, dan Value maksimal adalah 255.

#### Segmentasi Daun
Segmentasi daun dilakukan dengan operasi bitwise-AND antara mask daun dan citra asli.

```python
segment_daun = cv2.bitwise_and(rgb_img, rgb_img, mask=mask_daun)
```

- `rgb_img`: Gambar asli dalam format RGB.
- `mask_daun`: Mask hasil thresholding yang menunjukkan area daun.

#### Visualisasi Hasil
Hasil segmentasi ditampilkan menggunakan library `matplotlib`.

```python
fig, axs = plt.subplots(2,2, figsize=(12,8))

axs[0, 0].imshow(rgb_img)
axs[0, 0].set_title('Gambar Asli')

axs[0, 1].imshow(mask_sawo, cmap='gray')
axs[0, 1].set_title('Mask Sawo')

axs[1, 0].imshow(segment_sawo, cmap='gray')
axs[1, 0].set_title('Segmentasi Sawo')

axs[1, 1].imshow(segment_daun, cmap='gray')
axs[1, 1].set_title('Segmentasi Daun')

plt.show()
```
Hasil yang didapatkan, Mask pada buah sawo tidak penuh ditampilkan karena adanya perbedaan nilai pixel, dan hanya dapat ditampilkan beberapa bagian saja. Sementara pada daun, dapat didapatkan seluruh daun karena tidak terdapat perbedaan yang siginifikan pada nilai pikselnya, namun juga didapatkan gambar di background yang berwarna hampir sama, yang merupakan gambar rumput.

## Penutup
### Kesimpulan
Dengan menggunakan library OpenCV dan Python untuk mencapai tujuan deteksi warna dan segmentasi pada gambar RGB dalam proyek ini. Teknik thresholding terbukti efektif dalam mendeteksi dan memisahkan objek berdasarkan warna. Dengan menggunakan visualisasi, hasil segmentasi dapat ditampilkan dengan cukup baik.

### Daftar Pustaka
    Goenawan, A. D., Rachman, M. B. A., & Pulungan, M. P. (2022). Identifikasi warna pada objek citra digital secara real time menggunakan pengolahan model warna HSV. Jurnal JURTIE, 4(1), 68-74. 
---
