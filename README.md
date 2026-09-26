# GNU Radio Öğrenme Yol Haritası

> Proje tabanlı, günde bir küçük proje mantığıyla. Her adımın sonunda çalışan bir
> `.grc` flowgraph + kısa not bırak. Amaç: DSP temellerini GNU Radio üzerinden
> sezgisel olarak oturtmak ve kendi cihazlarını / broadcast sinyallerini çözebilmek.

Son güncelleme: 2026-09-26

---

## 0. Şu ana kadar yapılanlar (mevcut durum)

Bunlar `gnu_radio_lessons/` ve `gnuradio/` klasörlerinde zaten var:

- Lesson 1–8 (temel bloklar, sinyal üretimi, GRC arayüzü)
- FM modülasyon + transmit (`FM_mod_transmit.grc`)
- AM demodülasyon (`am_demodulation.grc`)
- Wideband FM alıcı (`wbfm_receive2.grc`)
- Spectrum analyzer (`spectrum_analyzer.py`, `spectrum_analyzer_for_mouse.grc`)
- USRP (B205mini) ve LimeSDR entegrasyonu (`usrp.grc`, `limesdr.grc`)
- Kendi kablosuz cihazını inceleme (mouse sinyali yakalama/analiz)

> Yani "GRC arayüzü + temel akış" fazını geçtin. Aşağısı bunun üstüne kuruluyor.

---

## ★ Ana seri: USRP B210 + 2× Hytera PD565 (sırayla)

Donanım: B210 (70 MHz–6 GHz, 2×2), PD565 (DMR Tier II + analog FM, UHF 4 W / düşük 1 W).
Flowgraph'lar: `~/Desktop/gnuradio/pd565/pXX_*.grc`

**Güvenlik:** B210 RX maksimum girişi ≈ -15 dBm. 4 W (36 dBm) telsiz 1 m'de ≈ +10 dBm verir
→ B210 zarar görebilir. Telsiz **düşük güçte**, B210 girişinde **30 dB attenüatör** veya
anten yok / birkaç metre mesafe. Asla doğrudan kablo bağlama. Yalnızca lisanslı/izinli kanallar.
B210 TX sadece kablolu loopback + attenüatör.

### Aşama A — Gözlem (analog mod)
| # | Proje | DSP kavramı | ✔ |
|---|-------|-------------|---|
| 01 | Spektrum + waterfall + genlik; PTT'ye bas, sinyali gör | IQ, FFT, samp_rate, RBW | ☐ |
| 02 | Frekans hatasını ölç (Hz ve ppm), FFT boyutunu değiştir | Frekans çözünürlüğü, pencereleme | ☐ |
| 03 | Sinyal gücü ve gürültü tabanı → SNR; gain'in etkisi | dBFS, gürültü tabanı, doygunluk | ☐ |
| 04 | DC ofset / IQ dengesizliği: kanal tam merkezde vs 100 kHz kaydırılmış | DC spike, image, tuning offset | ☐ |

### Aşama B — Analog FM alıcı
| # | Proje | DSP kavramı | ✔ |
|---|-------|-------------|---|
| 05 | Frequency Xlating FIR ile kanal seç + decimation | Karıştırma, FIR, decimation | ☐ |
| 06 | Quadrature Demod ile NBFM → hoparlör | FM demod, gain = fs/(2π·dev) | ☐ |
| 07 | Kanal filtresi genişliğini oynat, sesi karşılaştır | Bant genişliği ↔ SNR ödünleşimi | ☐ |
| 08 | Power squelch + AGC | Güç kestirimi, eşik, AGC döngüsü | ☐ |
| 09 | FM sapmasını (deviation) ölç, 12.5 kHz kanala sığıyor mu | Carson kuralı, dolu bant genişliği | ☐ |
| 10 | CTCSS alt tonunu tespit et (67–250 Hz) | Dar bant filtre, Goertzel | ☐ |
| 11 | Pre/de-emphasis; ses spektrumunu incele | IIR filtre, frekans yanıtı | ☐ |

### Aşama C — Kayıt ve offline DSP (numpy)
| # | Proje | DSP kavramı | ✔ |
|---|-------|-------------|---|
| 12 | IQ'yu dosyaya kaydet (SigMF meta ile) | complex64 formatı, metadata | ☐ |
| 13 | Aynı NBFM alıcıyı numpy/scipy ile yaz | FFT, lfilter, np.angle türevi | ☐ |
| 14 | Spektrogram + PTT başlangıç/bitiş tespiti | STFT, zaman-frekans | ☐ |
| 15 | TX açılış rampası (attack time) ölçümü | Zarf, yükselme süresi | ☐ |

### Aşama D — DMR fiziksel katman (kendi telsizlerin, şifresiz)
| # | Proje | DSP kavramı | ✔ |
|---|-------|-------------|---|
| 16 | DMR sinyalini waterfall/genlikte gör: 30 ms TDMA burst'leri | TDMA, zaman slotu | ☐ |
| 17 | FM demod çıkışının histogramı → 4 seviye (4FSK) | Çok seviyeli FSK | ☐ |
| 18 | Sembol hızını kestir (4800 baud) | Otokorelasyon, spektral çizgi | ☐ |
| 19 | Sembol zamanlama geri kazanımı (Symbol Sync) + göz diyagramı | Timing recovery, RRC | ☐ |
| 20 | Dibitleri çıkar, ETSI TS 102 361-1 senkron dizisini bul | Korelasyon, frame sync | ☐ |
| 21 | Burst yapısını çöz: sync / slot type / payload sınırları | Çerçeve yapısı | ☐ |
| 22 | Analog vs DMR: aynı mesafede kalite / bant kullanımı karşılaştırması | Spektral verim | ☐ |

### Aşama E — B210 TX (yalnızca kablolu loopback)
| # | Proje | DSP kavramı | ✔ |
|---|-------|-------------|---|
| 23 | TX→30+ dB attenüatör→RX; tek ton gönder, al | TX zinciri, interpolasyon | ☐ |
| 24 | Kendi NBFM modülatörün → loopback → Proje 06 alıcısı | FM mod, uçtan uca | ☐ |
| 25 | 2-FSK ile metin gönder/al | Bit→sembol, eşik karar | ☐ |
| 26 | 4FSK modem (DMR benzeri) | Çok seviyeli mod, RRC | ☐ |
| 27 | BPSK/QPSK + Costas loop + BER ölçümü | Taşıyıcı senkron, BER vs SNR | ☐ |
| 28 | Basit paket: preamble + başlık + CRC + payload | Stream tags, CRC | ☐ |
| 29 | Kendi OOT bloğun (ör. Goertzel/CTCSS dedektörü) | gr_modtool, C++/Python blok | ☐ |
| 30 | Bitirme: kendi 4FSK paket modemin, raporlu | Hepsi | ☐ |

Not: DMR ses çözümü (AMBE+2 vokoder) kapsam dışı; odak fiziksel katman ve çerçeve yapısı.

---

## Faz 1 — DSP temellerini sağlamlaştır (1–2 hafta)

Hedef: Blokların *neden* çalıştığını anlamak. GRC'de kullandığın her bloğun
arkasındaki matematiği görselleştir.

| Gün | Proje | Kazanım |
|-----|-------|---------|
| 1 | Sinyal üret (sine/square/noise) → QT Time + Frequency Sink yan yana | Zaman ↔ frekans ilişkisi |
| 2 | Throttle vs gerçek örnekleme hızı — CPU %'sini gözle | Sample rate sezgisi |
| 3 | Low-pass / high-pass / band-pass filtre; transition width oynat | FIR filtre parametreleri |
| 4 | Decimation & interpolation (Rational Resampler) | Örnekleme hızı dönüşümü |
| 5 | Complex ↔ Float ↔ Mag/Phase blokları | IQ verisinin anlamı |
| 6 | FFT'yi elle: Stream to Vector → FFT → Log Power → Vector to Stream | Spectrum sink'in içi |
| 7 | Kendi notunu yaz: "IQ nedir, neden karmaşık sayı?" | Kavramsal özet |

Referans: `notes/RLE_PR_117_XIV.pdf`, GNU Radio Wiki "Guided Tutorials".

---

## Faz 2 — Analog modülasyon derinleşme (1 hafta)

Zaten AM/FM yaptın; şimdi parametreleri bilinçli seç.

- AM: modülasyon indeksi (m) değiştir, over-modulation'ı spektrumda gör
- DSB vs SSB (Hilbert transform ile SSB üret)
- FM: deviation & Carson bant genişliği hesabı, de-emphasis filtresi
- **Proje:** Kendi ürettiğin FM sinyalini file sink'e yaz → tekrar oku → demodüle et
  (loopback, donanımsız test). RF yayını yerine dosya üzerinden çalış.

---

## Faz 3 — Dijital modülasyonun temeli (2 hafta)

Burası 5G/telekom yolunun asıl kapısı.

| Konu | Proje |
|------|-------|
| Symbol / bit / baud | Random Source → Constellation | 
| BPSK / QPSK | Constellation Modulator + Scatter (QT Constellation Sink) |
| Pulse shaping | Root-Raised-Cosine filtre, göz diyagramı (Eye) |
| Kanal etkisi | Channel Model bloğu: gürültü + faz kayması ekle |
| Senkronizasyon | Symbol Sync, Costas Loop ile taşıyıcı geri kazanımı |
| **Bitiş projesi** | Dosya → QPSK mod → kanal → demod → dosya, hatasız geri al |

Referans: GNU Radio "Digital Modulation" tutorial serisi.

---

## Faz 4 — Gerçek sinyal alma (donanımlı, hobbyist) (2 hafta)

Kendi SDR'ınla (RTL-SDR / USRP / LimeSDR) *legal* ve açık yayınları çöz:

- Broadcast FM radyo (stereo pilot, RDS decode)
- NOAA / hava durumu APT görüntüsü (varsa)
- ADS-B (uçak konumları, 1090 MHz — açık ve popüler)
- Kendi cihazların: mouse/klavye 2.4G, 433 MHz uzaktan kumanda (senin cihazın)

> İlke: Yalnızca açık yayınlar veya sana ait cihazlar. Kayıt/analiz için önce
> file sink'e yaz, sonra offline çalış — donanımı meşgul etmeden tekrar tekrar dene.

---

## Faz 5 — Akış kontrolü, tagging, mesajlaşma (1 hafta)

- Stream Tags: burst başlangıcı işaretleme
- Message passing (PMT), Message Debug bloğu
- Tag Debug ile paket sınırlarını gör
- **Proje:** Basit bir paket yapısı (preamble + payload) üret, tag'le, çöz

---

## Faz 6 — Out-of-Tree (OOT) blok yazma (2 hafta)

GRC'nin dışına çıkıp kendi bloklarını yaz — burası "kullanıcı"dan "geliştirici"ye geçiş.

1. `gr_modtool` ile yeni modül oluştur
2. Python bloğu: kendi DSP fonksiyonunu yaz (ör. custom AGC)
3. Embedded Python Block (GRC içinde hızlı prototip)
4. C++ bloğu: performans kritik bir blok (ör. custom FIR)
5. YAML ile GRC arayüzüne ekle, unit test yaz
6. **Proje:** Kendi bloğunu GRC flowgraph'ında kullan

Referans: GNU Radio "Creating C++ OOT with gr-modtool".

---

## Faz 7 — 5G / hücresel yönüne köprü (araştırma) (sürekli)

SDR temelin oturunca telekom stack'ine bak:

- OFDM'i GNU Radio'da kur: IFFT → CP ekle → kanal → FFT → geri al
- `srsRAN_4G` (zaten Desktop'ta var) ile lab ortamında LTE stack incele
- gr-lte / gr-gsm gibi açık projelerin flowgraph'larını **okuyarak** öğren
- 5G NR fiziksel katman: SS/PBCH, PRACH kavramları (teori önce)

> Bu faz araştırma ağırlıklı — kendi laboratuvar/izole ortamında, yetkili
> kapsamda çalış. Teori + açık kaynak kod okuma ile ilerle.

---

## Çalışma disiplini

- **Günde 1 küçük çıktı:** çalışan `.grc` + 3 satır not (ne öğrendim, ne takıldım)
- Her flowgraph'ı `file sink` ile kaydet → offline tekrar analiz et
- Takıldığın bloğu Faz 1'e geri dönüp izole test et
- Haftalık: o haftanın notlarını tek bir özet dosyasında topla

## Faydalı referanslar

- GNU Radio Wiki — Tutorials & Guided Tutorials
- `notes/UHD and USRP User Manual.pdf` (donanım)
- `notes/RLE_PR_117_XIV.pdf` (DSP teori)
- Kaynak kod: kurulu GNU Radio örnekleri (`gr-*` modülleri)
