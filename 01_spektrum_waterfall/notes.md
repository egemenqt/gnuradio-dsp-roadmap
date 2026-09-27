
 # Proje 01 — Spektrum + Waterfall

  ## Amaç
  PD565 sinyalini frekans (spektrum) ve zaman (waterfall) düzleminde görmek.
 
  ## Kurulum
  - USRP Source (samp_rate=1e6, ant=RX2) -> Frequency Sink (complex) + Waterfall + (Complex->Mag -> Time Sink)
  - gain: 20 , freq: 446.3 MHz

  ## Gözlemler (3 satır)
  1. Bant genisligi:12.5KHz
  2. Gurultu tabani =-149dB/ tepe (dB) = -14.53 / SNR:65dB 
  3. DC ofset vs gercek sinyal / konusunca ne oldu:

  ## Sorular / takildiklarim
  SNR Signal / noise ise tepe ile taban arasındaki genlik farkı neden snr kavramı olarak kabul ettik ? => proje 03 konusu
 
  ![gain 48 - doygunlukta genis](img/01_gain48_doygunluk.png)
  ![gain 20 - dar gercek tepe](img/02_gain20_dar_tepe.png)
  EOF


exit
