<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="utf-8">
<body>
<div class="container">
	<h1> 🔑 Kode OTP Google Authenticator </h1>
	<div style="display:flex; justify-content:space-between; align-items:center;">
	  <p style="margin:0;">Donasi : <a style="text-decoration:none" href="https://s.id/ajak_nongkrong" target="_blank">Klik disini buat donasi 😁</a></p>
	</div>
    <hr>
    <h3 style="margin-bottom: 5px">🔑 Pembuat Bookmark</h3>
	<p style="margin-top:0">Fitur ini akan menghasilkan Bookmark kode OTP saat anda memasukkan kode kunci rahasia yang ada. Kode OTP akan langsung terisi saat anda meng-klik Bookmark yang akan dibuat disini sesuai dengan yang anda pilih, baik itu akun Dapodik (SP Datadik/ PTK datadik, akun SDN / verval, dan akun ASN Digital</p>
    <input type="text" id="bookmarkletCode" placeholder="Kunci Rahasia">
	<input type="text" id="bookmarkletNama" placeholder="Buat Nama Bookmark">
	<div class="radio-group">
		<p style="font-family: Bahnschrift Light, sans-serif;margin:0">Pilih Penggunaan OTP :</p>
		<label><input type="radio" name="bmOption" value="kode2fa" checked> Dapodik</label>
		<label><input type="radio" name="bmOption" value="totp_code"> SDM/Verval</label>
		<label><input type="radio" name="bmOption" value="otp"> ASN Digital</label>
	</div>
    <button onclick="generateBookmarklet()">Buat Bookmark</button>
    <p style="margin: 24px 0;font-family: Bahnschrift Light, sans-serif">Bookmark anda : <a id="bookmarkletLink" href="#" target="_blank"></a></p>
	<hr>
    <h3 style="margin-bottom: 5px">🔑 Pembaca Kunci Rahasia QR Code</h3>
    <p style="margin-top:0">Fitur ini digunakan jika anda belum mengetahui kunci rahasia dalam QR Code anda. jika anda telah mengetahui kunci rahahasianya, abaikan saja fitur ini.</p>
    <p style="margin-top:0"> Ikuti langkah-langkah berikut untuk menampilkan kunci rahasia pada QR code OTP anda : </p>
    <p style="margin-top:0"> 1. Buka Google Authenticator anda</p>
    <p style="margin-top:0"> 2. Pilih garis tiga (≡) untuk menampilkan menu tabulasi</p>
    <p style="margin-top:0"> 3. Piih "Transfer Kode"</p>
    <p style="margin-top:0"> 4. Pilih "Ekspor kode"</p>
    <p style="margin-top:0"> 5. Pilih akun yang akan anda Ekspor</p>
    <p style="margin-top:0"> 6. Akan terlihat QR Code anda, Screenshot lalu kembali (jangan pilih berikutnya)</p>
    <p style="margin-top:0"> 7. Upload hasil Screenshot itu disini</p>
    <input type="file" id="fileInput" accept="image/*">
    <pre id="output">Upload QR Google Authenticator</pre>
</div>
</body>
</html>

