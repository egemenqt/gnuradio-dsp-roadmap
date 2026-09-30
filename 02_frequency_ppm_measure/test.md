# Proje 02 — Test (Frekans Hatası / ppm)

Formüller için `README.md` sonundaki **Formüller** bölümüne bak. Cevap anahtarı en altta.

## Sorular

**1.** `f_ölçülen = 446.30014 MHz`, `f_nominal = 446.30000 MHz`. Δf kaç Hz'dir?
- a) +14 Hz  b) +140 Hz  c) +1400 Hz  d) −140 Hz

**2.** Yukarıdaki hata kaç ppm'dir? (`ppm = Δf / f_nominal × 10⁶`)
- a) ≈ 0.031  b) ≈ 0.31  c) ≈ 3.1  d) ≈ 31

**3.** B210 TCXO ±2 ppm ise 446.3 MHz'de en fazla kaç Hz sapma beklenir?
- a) ±89 Hz  b) ±446 Hz  c) ≈ ±893 Hz  d) ≈ ±8930 Hz

**4.** `samp_rate = 100 kHz`, `N_FFT = 8192`. RBW kaç Hz'dir?
- a) ≈ 3 Hz  b) ≈ 12.2 Hz  c) ≈ 24.4 Hz  d) ≈ 244 Hz

**5.** Kanal 446.300 MHz, offset 10 kHz aşağı. LO frekansı nedir?
- a) 446.310 MHz  b) 446.300 MHz  c) 446.290 MHz  d) 446.280 MHz

**6.** Bu LO ile gerçek sinyal baseband'de nerede görünür?
- a) −10 kHz  b) 0 Hz  c) +10 kHz  d) +20 kHz

**7.** IQ image hangi frekansta görünür?
- a) 446.280 MHz  b) 446.290 MHz  c) 446.300 MHz  d) 446.310 MHz

**8.** Gerçek sinyal −24 dB, image −57 dB. Image rejection kaç dB'dir?
- a) 24 dB  b) 33 dB  c) 57 dB  d) 81 dB

**9.** Taşıyıcı ±400 Hz geziniyor. Bu belirsizlik yaklaşık kaç ppm'dir (446.3 MHz'de)?
- a) ±0.09  b) ±0.9  c) ±9  d) ±90

**10.** Ölçülen ppm neden "saf telsiz hatası" değildir?
- a) FFT penceresi yüzünden
- b) B210 ve telsiz osilatörlerinin birleşik hatasıdır; GPSDO gibi kalibre referans olmadan ayrılamaz
- c) ppm sadece USB ile ölçülür
- d) Image rejection'ı etkiler

## Cevap anahtarı

1-b · 2-b (140/446.3e6·1e6 ≈ 0.31) · 3-c (2×446.3 ≈ 893 Hz) · 4-b (100000/8192 ≈ 12.2) · 5-c (f_kanal − f_offset) · 6-c (f_bb = f_sinyal − f_LO = +10 kHz) · 7-a (f_LO − f_bb) · 8-b (−24 − (−57) = 33 dB) · 9-b (400/446.3 ≈ 0.9) · 10-b
