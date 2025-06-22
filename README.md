<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplikasi Absen Siswa</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        label {
            display: block;
            margin-top: 10px;
        }
        #fotoPreview {
            margin-top: 10px;
            max-width: 300px;
            max-height: 300px;
        }
    </style>
</head>
<body>
    <h1>Form Absen Siswa</h1>
    <form id="absenForm">
        <label for="nama">Nama Lengkap:</label>
        <input type="text" id="nama" required><br>

        <label for="kelas">Kelas:</label>
        <input type="text" id="kelas"><br>

        <label for="status">Status Absen PKL:</label>
        <select id="status" required>
            <option value="Masuk">Masuk</option>
            <option value="Pulang">Pulang</option>
        </select><br>

        <label for="foto">Kamera:</label>
        <input type="file" id="foto" accept="image/*" capture="environment" required><br>
        <img id="fotoPreview" src="" alt="Preview Foto" style="display:none;"/><br>

        <p id="tanggalWaktu"></p>
        <p id="lokasi"></p>

        <button type="submit">Kirim Absen</button>
        <button type="button" id="resetButton">Reset</button>
    </form>

    <script>
        let lokasi = "";

        // Fungsi untuk memperbarui tanggal dan waktu
        function updateTanggalWaktu() {
            const now = new Date();
            const tanggalWaktu = now.toLocaleString('id-ID', { timeZone: 'Asia/Jakarta' });
            document.getElementById("tanggalWaktu").innerText = "Tanggal dan Waktu: " + tanggalWaktu;
        }

        // Event listener untuk pengambilan foto
        document.getElementById("foto").addEventListener("change", function(event) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    document.getElementById("fotoPreview").src = e.target.result;
                    document.getElementById("fotoPreview").style.display = "block";
                    updateTanggalWaktu();
                    getLocation(); // Ambil lokasi otomatis setelah foto diambil
                };
                reader.readAsDataURL(file);
            }
        });

        // Fungsi untuk mengambil lokasi
        function getLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(function(position) {
                    lokasi = "Latitude: " + position.coords.latitude + ", Longitude: " + position.coords.longitude;
                    document.getElementById("lokasi").innerText = lokasi;
                }, function(error) {
                    console.error("Error getting location: ", error);
                });
            } else {
                alert("Geolocation tidak didukung oleh browser ini.");
            }
        }

        // Event listener untuk pengiriman formulir
        document.getElementById("absenForm").onsubmit = function(event) {
            event.preventDefault();
            const nama = document.getElementById("nama").value;
            const kelas = document.getElementById("kelas").value;
            const status = document.getElementById("status").value;
            const foto = document.getElementById("foto").files[0];

            // Simpan data ke Google Sheets (akan dibahas di bawah)
            console.log("Nama:", nama);
            console.log("Kelas:", kelas);
            console.log("Status:", status);
            console.log("Foto:", foto);
            console.log("Lokasi:", lokasi);
            // Tambahkan kode untuk mengupload foto dan menyimpan data ke Google Sheets di sini
        };

        // Event listener untuk tombol reset
        document.getElementById("resetButton").onclick = function() {
            document.getElementById("absenForm").reset();
            document.getElementById("fotoPreview").style.display = "none";
            document.getElementById("tanggalWaktu").innerText = "";
            document.getElementById("lokasi").innerText = "";
        };
    </script>
</body>
</html>
