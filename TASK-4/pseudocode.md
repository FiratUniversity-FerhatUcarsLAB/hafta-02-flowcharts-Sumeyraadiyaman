BASLA

// --- Öğrenci Girişi ---
PRINT "Ders Kayıt Sistemine Hoşgeldiniz"
INPUT ogr_no
INPUT sifre

IF OGRENCI_DOGRULA(ogr_no, sifre) == FALSE THEN
    PRINT "Geçersiz öğrenci numarası veya şifre!"
    BITIR
ENDIF

PRINT "Giriş başarılı."
ogr_gano = GANO_GETIR(ogr_no)
toplam_kredi = 0
secilen_dersler = []

// --- Ders Listesini Görüntüleme ---
ders_listesi = DERS_LISTESI_GETIR(ogr_no)
FOR ders IN ders_listesi DO
    PRINT ders.AD + " (" + ders.KREDI + " kredi)"
ENDFOR

// --- Ders Seçim Döngüsü ---
DEVAM = "E"
WHILE DEVAM == "E"
    PRINT "1 - Ders Ekle"
    PRINT "2 - Ders Çıkar"
    PRINT "3 - Kayıt Onayla"
    INPUT islem

    SWITCH islem

        CASE 1:
            INPUT ders_kodu
            ders = DERS_BUL(ders_kodu)

            // --- Kontroller ---
            IF ders.KONTENJAN <= ders.MEVCUT THEN
                PRINT "Kontenjan dolu!"
                BREAK
            ENDIF

            IF ONKOSUL_TAMAMLANDI_MI(ogr_no, ders) == FALSE THEN
                PRINT "Ön koşul dersini tamamlamadınız!"
                BREAK
            ENDIF

            IF ZAMAN_CAKISMASI_VAR_MI(secilen_dersler, ders) == TRUE THEN
                PRINT "Bu ders zaman olarak başka bir dersle çakışıyor!"
                BREAK
            ENDIF

            IF (toplam_kredi + ders.KREDI) > 35 THEN
                PRINT "Kredi limiti aşıldı (35 kredi sınırı)!"
                BREAK
            ENDIF

            // --- Tüm kontroller geçtiyse ---
            secilen_dersler.EKLE(ders)
            toplam_kredi = toplam_kredi + ders.KREDI
            PRINT ders.AD + " başarıyla eklendi."
            BREAK


        CASE 2:
            INPUT ders_kodu
            IF DERS_LISTEDE_VAR_MI(secilen_dersler, ders_kodu) == FALSE THEN
                PRINT "Bu dersi seçmediniz!"
                BREAK
            ENDIF
            ders = DERS_BUL(ders_kodu)
            secilen_dersler.CIKAR(ders)
            toplam_kredi = toplam_kredi - ders.KREDI
            PRINT ders.AD + " dersiniz çıkarıldı."
            BREAK


        CASE 3:
            PRINT "Kayıt özeti:"
            FOR d IN secilen_dersler DO
                PRINT d.AD + " (" + d.KREDI + " kredi)"
            ENDFOR
            PRINT "Toplam kredi: " + toplam_kredi

            // --- Danışman Onayı ---
            IF ogr_gano < 2.5 THEN
                PRINT "Kayıt danışman onayına gönderildi (GANO < 2.5)."
            ELSE
                PRINT "Kayıt otomatik olarak onaylandı."
            ENDIF

            PRINT "Kayıt işlemi tamamlandı."
            DEVAM = "H"
            BREAK

        DEFAULT:
            PRINT "Geçersiz işlem!"

    ENDSWITCH

    IF DEVAM != "H" THEN
        PRINT "Başka işlem yapmak ister misiniz? (E/H)"
        INPUT DEVAM
    ENDIF

ENDWHILE

PRINT "İyi günler!"
BITIR
