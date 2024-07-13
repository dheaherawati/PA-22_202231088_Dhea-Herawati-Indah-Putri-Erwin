
# Penjelasan codingan

import cv2
import numpy as np
import matplotlib.pyplot as plt
Mengimpor library yang diperlukan: cv2 untuk OpenCV, numpy untuk manipulasi array, dan matplotlib.pyplot untuk visualisasi.

image_path = ('buah.jpg')// Menyimpan path ke gambar 'buah.jpg'. Pastikan path ini sesuai dengan lokasi sebenarnya dari file gambar.

original_image = cv2.imread(image_path)
original_image = cv2.cvtColor(original_image, cv2.COLOR_BGR2RGB)//Menggunakan cv2.cvtColor() untuk mengubah format warna gambar dari BGR ke RGB. Ini penting karena matplotlib mengharapkan format RGB untuk menampilkan gambar dengan benar.

// untuk menentukan rentang warna dalam skala RGB yang akan digunakan untuk melakukan segmentasi warna pada gambar.
lower_green = np.array([0, 100, 0], dtype=np.uint8)  # Lower bound untuk warna hijau dalam RGB
upper_green = np.array([80, 255, 80], dtype=np.uint8)  # Upper bound untuk warna hijau dalam RGB

//Rentang warna lower_yellow dan upper_yellow, digunakan untuk menentukan batas bawah dan batas atas untuk segmentasi warna kuning dalam skala RGB. 
lower_yellow = np.array([150, 120, 0], dtype=np.uint8)  # Lower bound untuk warna kuning dalam RGB
upper_yellow = np.array([255, 255, 80], dtype=np.uint8)  # Upper bound untuk warna kuning dalam RGB

//tampilkan maska yang dihasilkan menggunakan matplotlib untuk memverifikasi segmentasi warna daun.
mask_daun = cv2.inRange(original_image, lower_green, upper_green

//tampilkan maska yang dihasilkan menggunakan matplotlib untuk memverifikasi segmentasi warna buah.
mask_buah = cv2.inRange(original_image, lower_yellow, upper_yellow)


kernel = np.ones((5, 5), np.uint8)
mask_daun = cv2.morphologyEx(mask_daun, cv2.MORPH_OPEN, kernel)
mask_buah = cv2.morphologyEx(mask_buah, cv2.MORPH_OPEN, kernel)

//contours_daun: Variabel yang akan menyimpan daftar kontur dari objek daun yang terdeteksi dalam mask_daun.
contours_buah: Variabel yang akan menyimpan daftar kontur dari objek buah yang terdeteksi dalam mask_buah.
contours_daun, _ = cv2.findContours(mask_daun, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
contours_buah, _ = cv2.findContours(mask_buah, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

//np.zeros_like(original_image): Membuat array NumPy yang sama ukuran dan tipe datanya dengan original_image, tetapi semua elemennya diisi dengan nol. Ini menciptakan maska kosong yang siap untuk digunakan.
mask_segmented_daun = np.zeros_like(original_image)
mask_segmented_buah = np.zeros_like(original_image)

//cv2.drawContours(): Fungsi ini digunakan untuk menggambar kontur ke dalam gambar atau maska. Parameter pertama adalah gambar atau maska tempat kontur akan digambar, parameter kedua adalah daftar kontur yang akan digambar, parameter ketiga adalah indeks kontur yang ingin digambar (di sini -1 artinya semua kontur), dan parameter keempat adalah warna dan ketebalan kontur.
cv2.drawContours(mask_segmented_daun, contours_daun, -1, (255, 255, 255), thickness=cv2.FILLED)


cv2.drawContours(mask_segmented_buah, contours_buah, -1, (255, 255, 255), thickness=cv2.FILLED)

//cv2.bitwise_and(): Fungsi ini melakukan operasi bitwise AND antara original_image dan mask_segmented_daun atau mask_segmented_buah, menghasilkan gambar yang hanya mempertahankan piksel yang ada di lokasi yang diindikasikan oleh maska.

segmented_daun = cv2.bitwise_and(original_image, mask_segmented_daun)
segmented_buah = cv2.bitwise_and(original_image, mask_segmented_buah)

// Menampilkan hasil menggunakan matplotlib dalam satu output
fig, axs = plt.subplots(2, 2, figsize=(15, 10))//Buat subplot dengan ukuran 2x2

//Tampilkan gambar asli
axs[0, 0].imshow(original_image)
axs[0, 0].set_title('Gambar Asli')

//Tampilkan mask buah
axs[0, 1].imshow(mask_buah, cmap='gray')
axs[0, 1].set_title('Mask Buah')

//Tampilkan segmentasi buah
axs[1, 0].imshow(segmented_buah)
axs[1, 0].set_title('Segmentasi Buah')

//Tampilkan segmentasi daun
axs[0, 1].imshow(segmented_leaves)
axs[0, 1].set_title('Segmentasi Daun')

//Tampilkan maska menggunakan matplotlib
plt.tight_layout()
plt.show()