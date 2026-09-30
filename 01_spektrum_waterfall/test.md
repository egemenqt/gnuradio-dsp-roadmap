# Proje 01 — Test (Spektrum + Waterfall)

Formüller için `README.md` sonundaki **Formüller** bölümüne bak. Cevap anahtarı en altta.

## Sorular

**1.** `samp_rate = 1 MHz`, `N_FFT = 4096`. RBW kaç Hz'dir?
- a) ≈ 122 Hz  b) ≈ 244 Hz  c) ≈ 488 Hz  d) ≈ 1000 Hz

**2.** `samp_rate = 1 MHz` iken ekranda görülebilen bant (merkeze göre) nedir?
- a) ±250 kHz  b) ±500 kHz  c) ±1 MHz  d) ±2 MHz

**3.** Time Sink'te 60 000 örnek, `samp_rate = 1 MHz`. Ekrandaki zaman penceresi kaç ms'dir?
- a) 6 ms  b) 30 ms  c) 60 ms  d) 600 ms

**4.** 4 W'lık telsizin çıkış gücü kaç dBm'dir? (`P_dBm = 10·log10(P / 1 mW)`)
- a) 6 dBm  b) 26 dBm  c) 36 dBm  d) 40 dBm

**5.** FFT boyutu 4096'dan 1024'e düşürülürse bin başına gürültü tabanı yaklaşık ne kadar değişir?
- a) 3 dB düşer  b) 6 dB yükselir  c) 12 dB yükselir  d) Değişmez

**6.** Ekrandaki tepe−taban farkı neden gerçek SNR değildir?
- a) FFT işlem kazancı yüzünden tepe/taban farkı şişer; sinyal ve gürültü aynı bant genişliğinde ölçülmemiştir
- b) Waterfall renk haritası yüzünden
- c) Gürültü hiç ölçülemez
- d) SNR sadece dBm ile tanımlanır

**7.** Gain 20 → 48 dB yapılınca ~200 kHz'lik geniş tümsek ve 1.0'a yapışık |IQ| görülüyor. Neden?
- a) Sinyal gerçekten genişledi
- b) ADC doygunluğu (kırpma) spektrumu yayar
- c) Waterfall hızı arttı
- d) DC ofset büyüdü

**8.** DMR çerçevesi 2 TDMA slotundan oluşur, her slot 30 ms. Çerçeve süresi kaç ms'dir?
- a) 15 ms  b) 30 ms  c) 60 ms  d) 120 ms

**9.** Merkezdeki tepenin DC ofset mi gerçek sinyal mi olduğunu ayırt etmek için ne yapılır?
- a) FFT'yi büyütmek  b) PTT'yi bırakmak veya merkez frekansı kaydırmak  c) gain'i 0 yapmak  d) samp_rate'i düşürmek

**10.** Nyquist koşulu 12.5 kHz'lik bir kanal için en az hangi örnekleme hızını gerektirir?
- a) 6.25 kHz  b) 12.5 kHz  c) 25 kHz  d) 50 kHz

## Cevap anahtarı

1-b (1e6/4096 ≈ 244 Hz) · 2-b (±fs/2) · 3-c (60000/1e6 = 60 ms) · 4-c (10·log10(4000) ≈ 36 dBm) · 5-b (10·log10(4) ≈ 6 dB) · 6-a · 7-b · 8-c (2×30 ms) · 9-b (PTT'yi bırak: gerçek sinyal kaybolur, DC kalır; frekansı kaydır: gerçek sinyal kanalında kalır) · 10-c (fs ≥ 2B)
