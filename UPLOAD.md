# GitHub'a yükleme

Depoda dosya yapısı **birebir şöyle** olmalı:

```
harranemlab.github.io/
├── index.html
├── .nojekyll
├── robots.txt
├── sitemap.xml
└── assets/
    ├── logo.webp
    ├── hero-900.webp
    ├── hero-1672.webp
    ├── vision.webp
    ├── banner-1000.webp
    ├── banner-1900.webp
    ├── students-1000.webp
    ├── students-1900.webp
    ├── og-image.jpg
    ├── favicon-32.png
    ├── icon-192.png
    ├── apple-touch-icon.png
    └── sponsors/
        ├── keysight.webp
        ├── spark.webp
        ├── rogers.webp
        ├── te.webp
        ├── terasic.webp
        ├── laird.webp
        └── renesas.webp
```

`assets` klasörü `index.html` ile **aynı seviyede** olacak, içine girmeyecek.

## Yol 1 — Tarayıcıdan (git kurmanıza gerek yok)

1. Zip'i bilgisayarınızda bir klasöre açın.
2. Depoda **Add file → Upload files**.
3. Açtığınız klasörün **içindekileri** (index.html, assets klasörü, diğerleri)
   tarayıcı penceresine **sürükleyip bırakın**.
   - "choose your files" linkiyle dosya seçici kullanmayın: seçici klasör kabul
     etmez, dosyaları tek tek alıp hepsini köke atar. Klasör yapısı ancak
     sürükle-bırak ile korunur.
   - Dış klasörün kendisini değil, içindekileri sürükleyin.
4. Yükleme bitince dosya listesinde `assets/` satırını görmelisiniz. Görmüyorsanız
   yapı bozulmuştur — 3. adımı tekrarlayın.
5. **Commit changes**.

## Yol 2 — Komut satırından

```bash
git clone https://github.com/harranemlab/harranemlab.github.io.git
cd harranemlab.github.io

# zip'ten çıkan dosyaları buraya kopyalayın, sonra:
git add -A
git commit -m "Optimize assets, drop analytics"
git push
```

## Kontrol

Push'tan 1-2 dakika sonra:

- `https://github.com/harranemlab/harranemlab.github.io/tree/main/assets`
  açılıyor mu? Açılmıyorsa klasör yüklenmemiş.
- `https://harranemlab.github.io/assets/logo.webp` doğrudan görseli veriyor mu?
- Site açıkken F12 → Network → sayfayı yenileyin. Kırmızı 404 satırı varsa
  istenen yol ile depodaki yol uyuşmuyor demektir.

## Sık yapılan hatalar

- **Sadece index.html değiştirildi.** Eski sürümde görseller base64 olarak HTML'in
  içindeydi, harici dosya yoktu. Yeni index.html `assets/` klasörünü arıyor;
  klasör yoksa bütün görseller kırılır. İkisi birlikte yüklenmeli.
- **Büyük/küçük harf.** GitHub Pages harfe duyarlıdır. `Assets/` ile `assets/`
  farklı yollardır; `Logo.webp` ile `logo.webp` de öyle.
- **Dosya seçiciyle yükleme.** Klasör yapısını düzleştirir, her şey köke düşer.
- **Önbellek.** Site hâlâ eski görünüyorsa Ctrl+Shift+R (Mac'te Cmd+Shift+R).
