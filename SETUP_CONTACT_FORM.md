# Setup Contact Form — Langkah Lengkap

Aku sudah membuat UI contact form modal. Sekarang kamu perlu setup backend untuk menerima pesan. Berikut langkah-langkahnya:

## **Pilihan 1: EmailJS (RECOMMENDED - Paling Mudah) ⭐**

EmailJS adalah service gratis yang mengirim email langsung dari JavaScript tanpa perlu backend server.

### Step 1: Daftar di EmailJS
1. Buka https://www.emailjs.com/
2. Klik "Sign Up" (gratis)
3. Verifikasi email kamu
4. Login ke dashboard

### Step 2: Buat Email Service
1. Di dashboard, klik **"Email Services"** (sidebar kiri)
2. Klik **"Add Service"**
3. Pilih provider email:
   - **Gmail** (recommended)
   - Outlook/Yahoo/custom SMTP
4. Ikuti instruksi:
   - Untuk Gmail: Login, ijinkan akses
   - Kasih nama service (contoh: `gmail_service`)
5. Klik **"Create Service"**

### Step 3: Buat Email Template
1. Klik **"Email Templates"** (sidebar)
2. Klik **"Create New Template"**
3. Isi template:

```
Subject: Pesan Baru dari {{from_name}}

Dari: {{from_name}} ({{from_email}})
Pesan:
{{message}}
```

4. Klik **"Save Template"**
5. **Catat Template ID** (contoh: `template_abc123`)

### Step 4: Ambil Public Key
1. Klik **"Account"** (settings icon, top-right)
2. Klik **"API Keys"**
3. Copy **Public Key** (jangan Private Key!)

### Step 5: Update Script di HTML
Buka `index.html` dan ganti:

```javascript
// Sebelumnya:
emailjs.init('YOUR_PUBLIC_KEY');

// Ganti dengan:
emailjs.init('YOUR_PUBLIC_KEY_DARI_EMAILJS');
```

Dan ganti juga di form handler:

```javascript
// Sebelumnya:
const response = await emailjs.send('SERVICE_ID', 'TEMPLATE_ID', {

// Ganti dengan:
const response = await emailjs.send('gmail_service', 'template_abc123', {
```

**Selesai!** Sekarang contact form siap digunakan.

---

## **Pilihan 2: Formspree (Alternative)**

Service lain yang juga mudah setup.

### Step 1: Daftar
1. Buka https://formspree.io/
2. Sign up dengan email
3. Buat form baru

### Step 2: Dapatkan Form ID
Kamu akan dapat endpoint URL seperti: `https://formspree.io/f/YOUR_FORM_ID`

### Step 3: Update Script
Ganti form submission:

```javascript
contactForm.addEventListener('submit', async (e) => {
  e.preventDefault();
  
  const formData = new FormData(contactForm);
  
  const response = await fetch('https://formspree.io/f/YOUR_FORM_ID', {
    method: 'POST',
    body: formData,
    headers: {
      'Accept': 'application/json'
    }
  });
  
  if (response.ok) {
    alert('Pesan berhasil dikirim!');
    contactForm.reset();
    closeContactModal();
  }
});
```

---

## **Pilihan 3: Firebase Firestore (Untuk Data Storage)**

Jika mau menyimpan pesan di database cloud.

### Keuntungan:
- Data tersimpan di cloud
- Bisa lihat messages di Firebase Console
- Real-time updates

### Kekurangan:
- Tidak kirim email otomatis (harus setup Cloud Functions)
- Setup lebih kompleks

---

## **Rekomendasi untuk Portofolio Kamu**

**Gunakan EmailJS** karena:
✅ Paling mudah setup  
✅ Langsung kirim email ke inbox kamu  
✅ Gratis untuk hingga 200 email/bulan  
✅ Tidak perlu backend server  
✅ Cukup 5 menit setup

---

## **Testing**

Setelah setup:

1. Buka portfolio di browser
2. Klik tombol **Contact** di navbar
3. Isi form dan klik **Kirim Pesan**
4. Cek email inbox kamu (atau spam folder)

---

## **Troubleshooting**

### "Form tidak terkirim"
- Pastikan Public Key sudah benar
- Pastikan SERVICE_ID dan TEMPLATE_ID benar
- Cek browser console (F12 → Console) untuk error message

### "Email tidak masuk inbox"
- Cek spam/junk folder
- Verifikasi email service di EmailJS dashboard
- Cek quota (free tier: 200 email/bulan)

### "Tombol Contact tidak membuka modal"
- Cek browser console untuk JavaScript error
- Pastikan element ID benar (`contactModal`, `contactForm`, etc)

---

## **Cara Melihat Pesan di EmailJS**

1. Login ke EmailJS dashboard
2. Klik **"Email Logs"** (sidebar)
3. Lihat history semua email yang terkirim
4. Bisa reply langsung dari sini

---

Sudah siap? Pilih Pilihan 1 (EmailJS) dan ikuti step-step nya! 🚀
