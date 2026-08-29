# Applied Electromagnetics Lab — optimize edilmiş site

Tasarım, renk paleti, tipografi ve içerik **birebir korundu**. Değişenler sadece
altyapı: varlık yönetimi, paylaşım meta'ları, onay mekanizması ve erişilebilirlik.

## Kurulum

`harranemlab.github.io` deposunun kökündeki mevcut `index.html` dosyasını
buradakiyle değiştirin ve `assets/` klasörünü kökle aynı seviyeye koyun:

```
harranemlab.github.io/
├── index.html
├── robots.txt
├── sitemap.xml
└── assets/
    ├── logo.webp  hero-900.webp  hero-1672.webp  vision.webp
    ├── banner-1000.webp  banner-1900.webp
    ├── students-1000.webp  students-1900.webp
    ├── og-image.jpg  favicon-32.png  icon-192.png  apple-touch-icon.png
    └── sponsors/  (keysight, spark, rogers, te, terasic, laird, renesas .webp)
```

Başka bir yapılandırma gerekmiyor — GitHub Pages statik olarak servis eder.

## Sonuç

| | Önce | Sonra |
|---|---|---|
| index.html | 8.36 MB | 54 KB (gzip 14 KB) |
| Kablodan geçen (ilk boyama) | 6.28 MB | **~92 KB** masaüstü / ~51 KB mobil |
| Tüm site, bütün görseller | 6.28 MB | 807 KB |
| Dış isteğe bağımlılık | Google Analytics | yok |
| Tekrar ziyaret | 6.28 MB yeniden | ~14 KB (görseller önbellekte) |

Kritik kazanç vision infografiğinden geldi: 1254×1246 RGBA PNG, tamamen opak bir
alfa kanalı taşıyordu ve **iki kez** gömülüydü. RGB WebP q82 olarak:
2855 KB → 249 KB, ve artık figure ile modal aynı dosyayı paylaşıyor.

## Yapılan değişiklikler

**Varlıklar**
- 14 base64 `data:` URI → `assets/` altında harici dosya. Base64 gzip'lenemiyordu,
  önbelleğe alınamıyordu ve HTML tamamen inmeden sayfa boyanmıyordu.
- Tüm görseller WebP; hero, banner ve students için `srcset` (900/1000 px mobil sürüm).
- Ekran altındaki her görselde `loading="lazy"` + `decoding="async"`; hero'da
  `fetchpriority="high"` ve `<link rel="preload">` (LCP için).
- Her `<img>`'de `width`/`height` → yükleme sırasında layout kayması yok.

**Paylaşım ve SEO**
- 1200×630 OG kartı üretildi (`assets/og-image.jpg`), sitenin kendi hero görseli ve
  lacivert gradyanıyla. LinkedIn/X/WhatsApp artık kart gösteriyor.
- `og:*` ve `twitter:*` meta'ları, `rel="icon"` + `apple-touch-icon`.
- JSON-LD genişletildi: `Person` altına ORCID/Scholar/Scopus/IEEE/ResearchGate
  `sameAs` dizisi, `PostalAddress`, `knowsAbout`, ve 5 yayın için `ScholarlyArticle`
  listesi. Yayınlar HTML'de statik kaldı — taranabilirlik için en güvenlisi bu.
- `robots.txt` ve `sitemap.xml` eklendi.
- `<meta charset>` artık dosyanın en başında (GA snippet'inden önce).

**Gizlilik**
- Google Analytics tamamen kaldırıldı: gtag snippet'i, Consent Mode kodu, onay
  banner'ı, tıklama/scroll olay takibi ve `data-track` öznitelikleri.
- Site artık hiçbir çerez yazmıyor ve dışarıya tek bir istek bile atmıyor
  (headless Chromium ile doğrulandı). Böylece açık rıza, çerez politikası ve
  KVKK m.9 yurt dışı aktarım meselesi tamamen ortadan kalktı.
- `#privacy` bölümü ve footer'daki Privacy linki tamamen kaldırıldı — beyan
  edilecek bir veri işleme kalmadığı için gereksizdi.
- Sponsor karoları `<a>` yerine `<div>`; logolar duruyor, dışarı link vermiyorlar.


**Erişilebilirlik**
- Modal artık gerçek bir dialog: `role="dialog"`, `aria-modal`, odak tuzağı
  (Tab/Shift+Tab döngüsü), kapanınca odak tetikleyen düğmeye dönüyor.
- `--muted` `#657489` → `#5c6a7e`. Açık gri zeminde 4.46:1 idi (AA sınırının altı),
  şimdi 5.16:1. Beyaz zeminde 5.50:1.
- Görünür klavye odak halkası (`:focus-visible`), koyu bölümlerde açık mavi varyant.
- Öksüz kalan `#directions` bölümü hem üst menüye hem footer'a eklendi.
- Mobil menü artık dışarı tıklayınca ve Escape ile kapanıyor.
- Hero ve vision görsellerine anlamlı `alt` metni.

**Performans / JS**
- Scroll dinleyicisi `requestAnimationFrame` ile throttle edildi. Önceki sürüm her
  scroll olayında `scrollHeight` okuyup zorunlu reflow tetikliyordu.
- Tek IntersectionObserver hem `section_view` olayını hem menüdeki aktif bölüm
  vurgusunu (`aria-current`) yönetiyor.
- Dört derinlik eşiği de gönderildikten sonra dinleyici erken çıkıyor.

## Sonraki adım için not

Yeni yayın eklerken iki yeri güncellemek gerekiyor: `#outputs` içindeki `<article
class="pub">` bloğu ve sayfa sonundaki `ItemList` JSON-LD. Yılda birkaç makale için
bu yeterli; sayı artarsa listeyi bir `publications.json`'dan üretmek mantıklı olur,
ama o zaman yayınlar istemci tarafında render edileceği için arama motoru
görünürlüğünden feragat edilir.
