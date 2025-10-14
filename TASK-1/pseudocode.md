BASLA

// 1. Değişkenlerin Tanımlanması
TANIMLA SAYI: pinDenemeSayisi = 0
TANIMLA SAYI: hesapBakiyesi = 5000
TANIMLA SAYI: gunlukLimit = 7500
TANIMLA SAYI: bugunCekilenTutar = 1000
TANIMLA SAYI: cekilecekTutar
TANIMLA METIN: girilenPIN
TANIMLA METIN: dogruPIN = "1234"
TANIMLA METIN: devamCevabi
TANIMLA MANTIKSAL: pinDogrulandi = YANLIS

YAZ "Lütfen kartınızı takınız."

// 2. PIN Doğrulama Döngüsü (Sadeleştirilmiş Yapı)
DÖNGÜ (pinDenemeSayisi < 3)
YAZ "Lütfen 4 haneli PIN kodunuzu giriniz:"
OKU girilenPIN

`EĞER (girilenPIN == dogruPIN) ISE`
    `pinDogrulandi = DOGRU`
    `CIK // PIN doğruysa döngüden çık.`
`DEGILSE`
    `pinDenemeSayisi = pinDenemeSayisi + 1`
    `YAZ "Hatalı PIN girdiniz. Kalan deneme hakkınız: ", (3 - pinDenemeSayisi)`
`EĞER SONU`
DÖNGÜ SONU

// 3. Ana İşleme Geçiş Kontrolü
EĞER (pinDogrulandi == DOGRU) ISE
// 4. Ana İşlem Döngüsü (Başka işlem yapmak için)
DÖNGÜ (DOĞRU)
YAZ "Hoş geldiniz. Çekmek istediğiniz tutarı giriniz:"
OKU cekilecekTutar

    `// 5. Tutar Kontrolleri`
    `EĞER (cekilecekTutar % 20 != 0) ISE`
        `YAZ "Hata: Girdiğiniz tutar 20 TL ve katları olmalıdır."`
    `DEGILSE EĞER (cekilecekTutar > hesapBakiyesi) ISE`
        `YAZ "Hata: Hesabınızda yeterli bakiye bulunmamaktadır."`
    `DEGILSE EĞER (cekilecekTutar + bugunCekilenTutar > gunlukLimit) ISE`
        `YAZ "Hata: Bu işlemle günlük para çekme limitinizi aşacaksınız."`
    `DEGILSE`
        `// 6. İşlemin Gerçekleştirilmesi`
        `hesapBakiyesi = hesapBakiyesi - cekilecekTutar`
        `bugunCekilenTutar = bugunCekilenTutar + cekilecekTutar`
        `YAZ "İşleminiz gerçekleştiriliyor..."`
        `YAZ "Lütfen paranızı ve fişinizi alınız."`
        `YAZ "İşlem başarılı. Kalan bakiyeniz: ", hesapBakiyesi, " TL"`
    `EĞER SONU`

    `// 7. Başka İşlem Sorgulama`
    `YAZ "Başka bir işlem yapmak ister misiniz? (E/H)"`
    `OKU devamCevabi`

    `EĞER (devamCevabi == "H" VEYA devamCevabi == "h") ISE`
        `CIK // Ana işlem döngüsünden çık.`
    `EĞER SONU`
`DÖNGÜ SONU`

`YAZ "İyi günler dileriz. Lütfen kartınızı almayı unutmayınız."`
DEGILSE
YAZ "3 kez hatalı deneme yaptığınız için kartınız bloke olmuştur."
EĞER SONU

BITIR
