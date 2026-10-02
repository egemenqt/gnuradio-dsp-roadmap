# Proje 04 — Test (DC Ofset ve IQ Dengesizliği)

Formüller için `README.md` sonundaki **Formüller** bölümüne bak. Cevap anahtarı en altta.

## Sorular

**1.** Düzeltme kapalıyken PTT kapalı, constellation bulutunun merkezi ≈ −0.0013 − j0.0020. DC ofset gücü kaç dBFS'tir?
- a) ≈ −26 dBFS  b) ≈ −42 dBFS  c) ≈ −52 dBFS  d) ≈ −66 dBFS

**2.** LO = 446.28 MHz, sinyal 446.300 MHz'de. IQ image hangi frekansta görünür?
- a) 446.260 MHz  b) 446.280 MHz  c) 446.300 MHz  d) 446.320 MHz

**3.** Sinyal tepesi −18 dB, image tepesi −57 dB. Image rejection kaç dB'dir?
- a) 18 dB  b) 57 dB  c) 75 dB  d) 39 dB

**4.** Constellation'dan Q/I = 0.98 okunuyor (faz hatası 0 kabul). IRR yaklaşık kaçtır?
- a) ≈ 26 dB  b) ≈ 40 dB  c) ≈ 46 dB  d) ≈ 95 dB

**5.** 39 dB'lik IRR tamamen genlik dengesizliğinden geliyorsa I ve Q arasındaki genlik farkı yaklaşık ne kadardır?
- a) %0.01  b) %0.2  c) %2.2  d) %22

**6.** Düzeltmeler `auto` iken kaydırıcı oynatılıp geri getirilince image önce belirgin (~30 dB), ~20 s sonra kayboluyor. En iyi açıklama nedir?
- a) IQ düzeltmesi bir takip (tracking) döngüsüdür; her yeniden ayarlamada sıfırdan yakınsar
- b) Telsiz ısındıkça image azalır
- c) FFT penceresi zamanla değişir
- d) DC ofset image'ı maskeler

**7.** Merkez tam sinyalin üstündeyken (sinyal ~DC) ve DC düzeltmesi `auto` iken constellation bozuluyor. Neden?
- a) IQ image sinyalin üstüne biniyor
- b) Kuantizasyon adımı büyüyor
- c) Gain çok yüksek
- d) DC takip döngüsü, DC'ye çok yakın yavaş dönen taşıyıcıyı "DC" sanıp çıkarmaya çalışıyor

**8.** `samp_rate = 100 kHz`, sinyal baseband'de +20 kHz. Time Sink'te döngü başına kaç örnek düşer?
- a) 2  b) 5  c) 20  d) 100

**9.** Sinyal DC'deyken I ve Q, periyodu ~4.5 ms olan sinüsler çiziyor. Taşıyıcı LO'dan ne kadar uzakta?
- a) ≈ 222 Hz  b) ≈ 45 Hz  c) ≈ 2.2 kHz  d) ≈ 4.5 kHz

**10.** 446.309 MHz'deki küçük tepe, merkez 446.30'dan 446.28'e kaydırılınca mutlak frekansta yerinde kaldı. Bu tepe nedir?
- a) Sinyalin IQ image'ı
- b) DC ofset
- c) Havadan gelen, alıcı dışı (dış kaynaklı) gerçek bir sinyal
- d) LO kaynaklı bir spur

## Cevap anahtarı

1-c (10·log10(0.0013² + 0.0020²) ≈ −52.4 dBFS) · 2-a (2·446.28 − 446.300 = 446.260) · 3-d (−18 − (−57) = 39 dB) · 4-b (20·log10(1.98/0.02) ≈ 39.9 dB) · 5-c (g = (89.1 − 1)/(89.1 + 1) ≈ 0.978 → %2.2) · 6-a · 7-d · 8-b (100k/20k = 5) · 9-a (1/4.5 ms ≈ 222 Hz) · 10-c (LO kaynaklı olsa merkezle birlikte hareket ederdi)
