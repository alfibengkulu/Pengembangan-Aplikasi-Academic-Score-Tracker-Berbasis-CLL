# Pengembangan-Aplikasi-Academic-Score-Tracker-Berbasis-CLL
Pengembangan Aplikasi Academic Score Tracker Berbasis CLL suatu proses perancangan, pembuatan, dan penyempurnaan aplikasi yang berfungsi untuk mencatat, mengelola, serta menghitung hasil akademik mahasiswa, yang dioperasikan menggunakan Command Line Language (CLL) 
import "fmt" // Memanggil kotak perkakas "fmt" untuk urusan Input/Output (Cetak ke layar & Baca keyboard)

// --- DEFINISI STRUKTUR DATA (BLUEPRINT) ---
// Membuat tipe data baru bernama 'DataNilai'
// Ini seperti formulir kosong yang punya kolom Nama, SKS, Nilai, dll.
type DataNilai struct {
	Nama  string  // Menyimpan nama mata kuliah (Teks)
	SKS   int     // Menyimpan jumlah SKS (Angka bulat)
	Nilai float64 // Menyimpan nilai angka asli (Desimal, misal 85.5)
	Huruf string  // Menyimpan Indeks Huruf (A, B, C...)
	Bobot float64 // Menyimpan Bobot Nilai (4.0, 3.0...)
}

func main() {
	// --- PERSIAPAN WADAH DATA ---
	// Membuat 'Slice' kosong. Ini seperti binder yang awalnya kosong,
	// tapi bisa diisi kertas (append) sebanyak-banyaknya nanti.
	var transkrip []DataNilai

	// Membuat 'Array' tetap berisi 7 nama mata kuliah.
	// Kita pakai ':=' (Short Declaration) biar cepat.
	daftarMatkul := [7]string{
		"Algoritma Pemrograman",
		"Matematika Diskrit",
		"Pengantar TIK",
		"Kalkulus I",
		"Bahasa Inggris",
		"Dasar Logika",
		"Pendidikan Karakter",
	}

	fmt.Println("     ACADEMIC SCORE TRACKER     ")

	// --- LOOP UTAMA (PERULANGAN INPUT) ---
	// 'for' tanpa kondisi artinya perulangan abadi (Infinite Loop).
	// Program akan terus berputar di sini sampai ketemu perintah 'break'.
	for {
		fmt.Println("\n=== PILIH MATA KULIAH ===")

		// Menampilkan daftar menu dari Array 'daftarMatkul'
		// 'range' bertugas membongkar isi array satu per satu.
		// 'i' adalah indeks (0,1,2...), 'mk' adalah isinya (teks nama matkul).
		for i, mk := range daftarMatkul {
			fmt.Printf("%d. %s\n", i+1, mk) // i+1 supaya nomor urut mulai dari 1
		}

		// Menyiapkan variabel sementara untuk menampung inputan user saat ini
		var nomor int
		var sks int
		var nilai float64
		var nama string

		// --- INPUT NOMOR ---
		fmt.Print("\nMasukkan Nomor Matkul (1-7): ")
		fmt.Scan(&nomor) // Membaca angka dari keyboard dan menyimpannya ke variabel 'nomor'

		// --- VALIDASI NOMOR ---
		// Jika nomor kurang dari 1 ATAU (||) lebih dari 7, maka salah.
		if nomor < 1 || nomor > 7 {
			fmt.Println("⚠️  Nomor salah! Harap pilih 1-7.")
			continue // 'continue' artinya: Jangan lanjut ke bawah, tapi lompat kembali ke AWAL loop (Pilih Matkul lagi)
		}

		// Mengambil nama matkul dari daftar berdasarkan nomor yang dipilih.
		// Dikurang 1 (nomor-1) karena urutan array komputer dimulai dari 0.
		nama = daftarMatkul[nomor-1]
		fmt.Printf(">> Anda memilih: %s\n", nama)

		// --- INPUT SKS ---
		fmt.Print("Masukkan Jumlah SKS: ")
		fmt.Scan(&sks)

		// --- INPUT NILAI (DENGAN LOOPING KHUSUS) ---
		// Kita bikin loop lagi di dalam, khusus untuk memastikan nilai yang diinput benar (0-100).
		for {
			fmt.Print("Masukkan Nilai (0-100): ")
			fmt.Scan(&nilai)

			// Cek apakah nilai valid (antara 0 sampai 100)?
			if nilai >= 0 && nilai <= 100 {
				break // Jika benar, 'break' (keluar dari loop kecil ini saja), lanjut ke proses berikutnya.
			}

			// Jika kode sampai sini, berarti nilai salah. Loop akan mengulang tanya nilai.
			fmt.Println("Nilai tidak valid! Harap masukkan angka 0-100.")
		}

		// --- LOGIKA KONVERSI NILAI ---
		// Menyiapkan variabel kosong untuk hasil konversi
		huruf := ""
		bobot := 0.0

		// Cek kondisi nilai menggunakan if-else bertingkat
		if nilai >= 80 {
			huruf = "A"
			bobot = 4.0
		} else if nilai >= 70 {
			huruf = "B"
			bobot = 3.0
		} else if nilai >= 60 {
			huruf = "C"
			bobot = 2.0
		} else if nilai >= 50 {
			huruf = "D"
			bobot = 1.0
		} else { // Jika nilai di bawah 50
			huruf = "E"
			bobot = 0.0
		}

		// --- MENYIMPAN DATA (APPEND) ---
		// Kita bungkus data (nama, sks, nilai, huruf, bobot) ke dalam struct 'DataNilai'.
		// Lalu kita masukkan struct itu ke dalam slice 'transkrip'.
		transkrip = append(transkrip, DataNilai{
			Nama: nama, SKS: sks, Nilai: nilai, Huruf: huruf, Bobot: bobot,
		})

		// --- KONFIRMASI LANJUT/STOP ---
		var lagi string
		fmt.Print("\nInput mata kuliah lain? (y/n): ")
		fmt.Scan(&lagi)

		// Cek jawaban user.
		// Jika BUKAN "y" (kecil) DAN BUKAN "Y" (besar)...
		if lagi != "y" && lagi != "Y" {
			break // ...Maka STOP loop utama (Program selesai input). Lanjut ke bagian bawah.
		}
	} // <-- Ini tutup kurung kurawal loop utama (Infinite Loop)

	// --- BAGIAN OUTPUT / LAPORAN ---
	fmt.Println("\n===============================")
	fmt.Println("      KARTU HASIL STUDI        ")
	fmt.Println("===============================")

	// Variabel akumulator (penampung total)
	totalSKS := 0
	totalBobot := 0.0

	// Loop untuk menampilkan isi transkrip satu per satu
	for i, mk := range transkrip {
		// Mencetak baris tabel.
		// %-25s artinya: Cetak string dengan lebar kolom 25 karakter (biar rapi rata kiri).
		// %.0f artinya: Cetak float tanpa angka di belakang koma (bulat).
		fmt.Printf("%d. %-25s | Nilai: %s (%.0f) | SKS: %d\n",
			i+1, mk.Nama, mk.Huruf, mk.Nilai, mk.SKS)

		// Hitung total berjalan
		totalSKS += mk.SKS                        // Tambahkan SKS matkul ini ke Total SKS
		totalBobot += mk.Bobot * float64(mk.SKS)  // Tambahkan (Bobot x SKS) ke Total Bobot
	}

	fmt.Println("-------------------------------")
	
	// Cek dulu apakah totalSKS lebih dari 0 untuk menghindari pembagian dengan nol (Error)
	if totalSKS > 0 {
		// Rumus IPK: Total Nilai Mutu dibagi Total SKS
		ipk := totalBobot / float64(totalSKS)
		
		fmt.Printf("Total SKS yang diambil : %d\n", totalSKS)
		// %.2f artinya: Cetak float dengan 2 angka di belakang koma (misal 3.50)
		fmt.Printf("IPK Semester ini       : %.2f\n", ipk)
	}
	fmt.Println("===============================")
}














