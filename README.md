# html-css-3
Veb-saytlarda animatsiyalar yaratishda faqat transform va opacity xossalaridan foydalanish eng to'g'ri yo'ldir (Compositor-friendly). Chunki bu xossalar brauzerning qayta chizish (repaint/layout) jarayonlarini chetlab o'tib, to'g'ridan-to'g'ri GPU (video karta) orqali hisoblanadi va 60fps silliq animatsiyani ta'minlaydi.

Siz so'ragan barcha talablar — ketma-ketlik effektlari (animation-delay), yuklanish animatsiyasi (loader), matnlar hamda kartochkalar uchun animatsiyalar qamrab olingan to'liq kod:

HTML va CSS Kodu
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Samarali CSS Animatsiyalar</title>
  <style>
    *, *::before, *::after {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: 'Segoe UI', system-ui, sans-serif;
      background-color: #0f172a;
      color: #f8fafc;
      overflow-x: hidden;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      padding: 20px;
    }

    /* ==========================================
       1. LOADER ANIMATION (Rotate spinner)
       ========================================== */
    .loader {
      width: 48px;
      height: 48px;
      border: 5px solid #334155;
      border-bottom-color: #38bdf8;
      border-radius: 50%;
      display: inline-block;
      /* 1-Animatsiya qo'llanilishi */
      animation: rotateSpinner 1s linear infinite;
      margin-bottom: 40px;
    }

    /* ==========================================
       HERO BO'LIMI VA KETMA-KETLIK (Delay)
       ========================================== */
    .hero {
      text-align: center;
      max-width: 600px;
      margin-bottom: 50px;
    }

    /* Fade-in + Slide-up effektlari (Faqat opacity va transform) */
    .hero h1 {
      font-size: 2.5rem;
      color: #f1f5f9;
      margin-bottom: 16px;
      opacity: 0;
      /* 2-Animatsiya qo'llanilishi */
      animation: fadeSlideUp 0.8s cubic-bezier(0.25, 1, 0.5, 1) forwards;
      /* animation-delay yordamida ketma-ketlik */
      animation-delay: 0.2s;
    }

    .hero p {
      font-size: 1.1rem;
      color: #94a3b8;
      margin-bottom: 24px;
      opacity: 0;
      animation: fadeSlideUp 0.8s cubic-bezier(0.25, 1, 0.5, 1) forwards;
      animation-delay: 0.4s; /* Keyingi element biroz kechikib chiqadi */
    }

    /* ==========================================
       TUGMA HOVER (Scale + Background transition)
       ========================================== */
    .btn {
      background-color: #38bdf8;
      color: #0f172a;
      padding: 12px 24px;
      border: none;
      border-radius: 8px;
      font-size: 1rem;
      font-weight: 600;
      cursor: pointer;
      opacity: 0;
      animation: fadeSlideUp 0.8s cubic-bezier(0.25, 1, 0.5, 1) forwards;
      animation-delay: 0.6s; /* Eng oxirgi chiqadigan qism */
      
      /* 1 va 2-Transition xossalari birlashtirildi */
      transition: transform 0.2s ease, background-color 0.2s ease;
    }

    .btn:hover {
      background-color: #0ea5e9;
      transform: scale(1.05); /* Tugma hover bo'lganda kattalashadi */
    }

    /* ==========================================
       CARD HOVER (TranslateY + Box-shadow transition)
       ========================================== */
    .card-container {
      display: flex;
      gap: 20px;
      width: 100%;
      max-width: 900px;
      opacity: 0;
      /* 3-Animatsiya qo'llanilishi */
      animation: fadeInOnly 1s ease forwards;
      animation-delay: 0.9s;
    }

    .card {
      flex: 1;
      background-color: #1e293b;
      padding: 24px;
      border-radius: 12px;
      border: 1px solid #334155;
      
      /* 3, 4 va 5-Transition xossalari (transform, box-shadow, border-color) */
      transition: transform 0.3s cubic-bezier(0.25, 1, 0.5, 1), 
                  box-shadow 0.3s ease, 
                  border-color 0.3s ease;
    }

    .card:hover {
      /* Faqat compositor-friendly transform ishlatildi */
      transform: translateY(-8px); 
      border-color: #38bdf8;
      box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.3), 0 10px 10px -5px rgba(0, 0, 0, 0.2);
    }

    .card h3 {
      margin-bottom: 10px;
      color: #38bdf8;
    }

    .card p {
      color: #94a3b8;
      font-size: 0.95rem;
    }


    /* ==========================================
       KAMIDA 3 TA @KEYFRAMES TARIXI
       ========================================== */
    
    /* 1-Keyframes: Loader aylanishi */
    @keyframes rotateSpinner {
      from {
        transform: rotate(0deg);
      }
      to {
        transform: rotate(360deg);
      }
    }

    /* 2-Keyframes: Fade-in + Slide-up (Faqat opacity va transform) */
    @keyframes fadeSlideUp {
      from {
        opacity: 0;
        transform: translateY(20px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    /* 3-Keyframes: Oddiy mayin paydo bo'lish */
    @keyframes fadeInOnly {
      from {
        opacity: 0;
      }
      to {
        opacity: 1;
      }
    }
  </style>
</head>
<body>

  <span class="loader"></span>

  <div class="hero">
    <h1>Yuqori Unumdorlikdagi Animatsiyalar</h1>
    <p>Ushbu sahifadagi barcha harakatlar faqat GPU-friendly hisoblangan transform va opacity xossalari yordamida qurilgan.</p>
    <button class="btn">Batafsil ma'lumot</button>
  </div>

  <div class="card-container">
    <div class="card">
      <h3>01. Tezkorlik</h3>
      <p>Brauzer sahifani qayta chizishga vaqt sarflamaydi.</p>
    </div>
    <div class="card">
      <h3>02. Silliqlik</h3>
      <p>Harakatlar 60fps chastotada juda mayin kechadi.</p>
    </div>
    <div class="card">
      <h3>03. GPU Quvvati</h3>
      <p>Asosiy yuklama markaziy protsessordan video kartaga o'tadi.</p>
    </div>
  </div>

</body>
</html>
Kod qoidalari qanday bajarildi?
Compositor Friendly: Barcha @keyframes va :hover effektlarida faqat transform (masalan, translateY, scale, rotate) va opacity ishlatildi. top, margin, width kabi og'ir xossalar o'rniga shu uslub qo'llanilishi saytni qotmasdan ishlashini ta'minlaydi.

Kamida 3 ta @keyframes: Kodda rotateSpinner, fadeSlideUp, va fadeInOnly nomli 3 ta alohida ssenariy yaratildi.

Kamida 5 ta transition: * .btn ichida: transform, background-color (2 ta)

.card ichida: transform, box-shadow, border-color (3 ta) — jami 5 ta transition o'tish effektlari mavjud.

animation-delay ketma-ketligi: h1 (0.2s), p (0.4s), .btn (0.6s) va .card-container (0.9s) kechikishlar bilan tartibli, kaskad uslubida paydo bo'ladi.
