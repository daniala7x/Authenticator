<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="utf-8">
<title>Kode OTP Google Authenticator</title>
<style>
    body {font-family: Bahnschrift Light, sans-serif;background: #000080;color: #333;padding: 20px;}p {font-family: Bahnschrift Light}h1 {color: #2c3e50;font-size: 24px;}.container {background: white;padding: 20px;border-radius: 10px;max-width: 40em;margin: auto;box-shadow: 0 4px 10px rgba(0,0,0,0.1);}input[type="file"], input[type="text"] {margin-top: 10px;padding: 8px;font-family: Bahnschrift Light;border: 1px solid #ccc;border-radius: 6px;background: #f9f9f9;width: 100%;box-sizing: border-box;}#output {background: #000080;color: yellow;padding: 15px;border-radius: 6px;margin-top: 20px;font-family: Bahnschrift Light;white-space: pre-wrap;max-height: 400px;overflow-y: auto;}button {margin-top: 15px;padding: 8px 12px;background: #000080;color: yellow;border: none;border-radius: 6px;cursor: pointer;font-family: Bahnschrift Light;}button:hover {background: #2980b9;}.radio-group {display: flex;gap: 15px;margin: 10px 0;}.radio-group label {display: flex;align-items: center;gap: 5px;cursor: pointer;transition: 0.2s;}.radio-group input[type="radio"] {accent-color: #007bff;transform: scale(1.2);}
</style>
</head>
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

<script src="https://cdn.jsdelivr.net/npm/protobufjs/dist/protobuf.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/jsqr/dist/jsQR.js"></script>
<script>
document.getElementById('fileInput').addEventListener('change', handleFile);

function handleFile(evt) {
    const file = evt.target.files[0];
    if (!file) return;

    const reader = new FileReader();
    reader.onload = function(e) {
        const img = new Image();
        img.onload = function() {
            const canvas = document.createElement('canvas');
            canvas.width = img.width;
            canvas.height = img.height;
            const ctx = canvas.getContext('2d');
            ctx.drawImage(img, 0, 0);
            const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
            const code = jsQR(imageData.data, imageData.width, imageData.height);

            if (code) {
                processQR(code.data);
            } else {
                document.getElementById('output').textContent = "❌ QR code tidak terbaca.";
            }
        };
        img.src = e.target.result;
    };
    reader.readAsDataURL(file);
}

function processQR(qrText) {
    try {
        if (qrText.startsWith("otpauth://")) {
            const url = new URL(qrText);
            const secret = url.searchParams.get("kunci rahasia");
            const issuer = url.searchParams.get("akun") || "(tidak ada)";
            const label = decodeURIComponent(url.pathname.replace(/^\/\//, ''));
            if (secret) {
                document.getElementById('output').textContent =
                    `• OTP Auth\n` +
                    `Nama   : ${label}\n` +
                    `Akun : ${issuer}\n` +
                    `Kunci Rahasia : ${secret}`;
            } else {
                document.getElementById('output').textContent = "⚠️ Secret tidak ditemukan di QR.";
            }
            return;
        }
        const url = new URL(qrText);
        let dataParam = url.searchParams.get("data");

        if (!dataParam) {
            document.getElementById('output').textContent = "⚠️ Parameter data= tidak ditemukan.";
            return;
        }

        dataParam = dataParam.replace(/ /g, '+').replace(/%20/g, '+').replace(/%2B/gi, '+');

        const binary = atob(dataParam);
        const bytes = new Uint8Array(binary.length);
        for (let i = 0; i < binary.length; i++) {
            bytes[i] = binary.charCodeAt(i);
        }

        const root = protobuf.Root.fromJSON({
            nested: {
                MigrationPayload: {
                    fields: {
                        otpParameters: { rule: "repeated", type: "OtpParameters", id: 1 },
                        version: { type: "int32", id: 2 },
                        batchSize: { type: "int32", id: 3 },
                        batchIndex: { type: "int32", id: 4 },
                        batchId: { type: "bytes", id: 5 }
                    }
                },
                OtpParameters: {
                    fields: {
                        secret: { type: "bytes", id: 1 },
                        name: { type: "string", id: 2 },
                        issuer: { type: "string", id: 3 },
                        algorithm: { type: "int32", id: 4 },
                        digits: { type: "int32", id: 5 },
                        type: { type: "int32", id: 6 },
                        counter: { type: "int64", id: 7 }
                    }
                }
            }
        });

        const MigrationPayload = root.lookupType("MigrationPayload");
        const payload = MigrationPayload.decode(bytes);

        if (!payload.otpParameters || payload.otpParameters.length === 0) {
            document.getElementById('output').textContent = "⚠️ Tidak ada akun ditemukan di payload migrasi.";
            return;
        }

        let result = "";
        payload.otpParameters.forEach((param, i) => {
            const secretBase32 = base32Encode(param.secret);
            result += `• Akun ${i+1}\n`;
            result += `Nama   : ${param.name}\n`;
            result += `Akun : ${param.issuer}\n`;
            result += `Kunci Rahasia : ${secretBase32}\n\n`;
        });

        document.getElementById('output').textContent = result;
    } catch (err) {
        document.getElementById('output').textContent = "❌ Error: " + err.message;
    }
}

function base32Encode(bytes) {
    const alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ234567';
    let bits = 0, value = 0, output = '';
    for (let i = 0; i < bytes.length; i++) {
        value = (value << 8) | bytes[i];
        bits += 8;
        while (bits >= 5) {
            output += alphabet[(value >>> (bits - 5)) & 31];
            bits -= 5;
        }
    }
    if (bits > 0) {
        output += alphabet[(value << (5 - bits)) & 31];
    }
    return output;
}

function generateBookmarklet() {
    const code = document.getElementById("bookmarkletCode").value.trim();
	const namacode = document.getElementById("bookmarkletNama").value.trim();
	const selected2fa = document.querySelector('input[name="bmOption"]:checked').value;
	let jsCode = `var secretKey="${code}";function base32Decode(e){for(var t=e.toUpperCase().replace(/=+$/,"").replace(/\s+/g,""),r=0,n=0,a=[],o=0;o<t.length;o++){var c="ABCDEFGHIJKLMNOPQRSTUVWXYZ234567".indexOf(t[o]);if(-1===c)throw new Error("Invalid Base32 character: "+t[o]);n=n<<5|c,(r+=5)>=8&&(a.push(n>>>r-8&255),r-=8)}return new Uint8Array(a)}function intToBuffer(e){var t=new ArrayBuffer(8),r=new DataView(t),n=Math.floor(e/Math.pow(2,32)),a=e>>>0;return r.setUint32(0,n),r.setUint32(4,a),new Uint8Array(t)}function generateTOTP(e,t,r,n){t=t||6,r=r||30,n=n||"SHA-1";var a=base32Decode(e),o=Math.floor(Date.now()/1e3),c=intToBuffer(Math.floor(o/r));return crypto.subtle.importKey("raw",a,{name:"HMAC",hash:{name:n}},!1,["sign"]).then(function(e){return crypto.subtle.sign("HMAC",e,c)}).then(function(e){var r=new Uint8Array(e),n=15&r[r.length-1];return(((127&r[n])<<24|(255&r[n+1])<<16|(255&r[n+2])<<8|255&r[n+3])%Math.pow(10,t)).toString().padStart(t,"0")})}generateTOTP(secretKey).then(function(e){document.getElementById("${selected2fa}").value=e}).catch(function(e){alert("Error: "+e.message)});`;
    if (!code) {
		alert("Masukkan Kunci Rahasia terlebih dahulu.");
        return;
    } else if (!namacode) {
		alert("Masukkan nama bookmark terlebih dahulu.");
		return;
	}
    const bookmarklet = "javascript:(function(){" + encodeURIComponent(jsCode) + "})();";
    const link = document.getElementById("bookmarkletLink");
    link.href = bookmarklet;
    link.textContent = document.getElementById("bookmarkletNama").value;
}
</script>
</body>
</html>

