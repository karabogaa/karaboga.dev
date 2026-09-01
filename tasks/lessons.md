# Dersler

## Headless Chrome ile görsel doğrulama

Bu repoda tasarım değişikliğini ekran görüntüsüyle doğrularken üç kez aynı
tuzağa düşüldü: **görüntüdeki kusur sayfada değil, ölçüm yönteminde çıktı.**
Bir sonraki seferde önce yöntemi doğrula, sonra kodu suçla.

- `--window-size` genişliği **500px'in altına inmiyor** (hem `--headless` hem
  `--headless=new`). 390 istersen 500 alırsın, görüntü de kırpılır — bu
  "yatay taşma" gibi görünür ama değildir. Gerçek dar viewport için sayfayı
  sabit genişlikli bir `<iframe>` içinde render et; iframe kendi layout
  viewport'unu alır.
- Taşmayı gözle değil ölçerek doğrula:
  `document.documentElement.scrollWidth` vs `clientWidth`.
- `scroll-behavior: smooth` + fragment (`#about`) headless'ta yarım kalıyor;
  ekran görüntüsü boş kare yakalayabiliyor. Ölçüm için `scrollBehavior='auto'`.
- `--dump-dom` piksel üretmez: CSS transition'ları ilerlemez, IntersectionObserver
  düzensiz ateşlenir. Bu modda `getComputedStyle(el).opacity` geçiş ortasında
  donuk kalır — sınıfın uygulanıp uygulanmadığına bak, hesaplanan değere değil.
  Nihai görünümü sadece gerçek `--screenshot` kanıtlar.

## Reveal animasyonları

Scroll-reveal dekorasyondur; altında sakladığı içerik sayfanın amacıdır.
`opacity: 0` varsayılanını asenkron bir observer'a bağlarken mutlaka zaman
aşımlı bir güvenlik ağı bırak — observer ateşlenmezse içerik kalıcı olarak
görünmez kalır ve bu sessiz bir hatadır.
