// Başlangıç Değişkenleri
Sistem_Aktif ← TRUE
Ev_Sahibi_Evde ← FALSE
Alarm_Durum ← "KAPALI"
Alarm_Seviye ← 0

// Sonsuz Döngü (Sistem sürekli çalışır)
WHILE (Sistem_Aktif = TRUE) DO

    // Sensör verilerini sürekli oku
    Hareket ← Oku_Hareket_Sensoru()
    Kapi ← Oku_Kapi_Sensoru()
    Pencere ← Oku_Pencere_Sensoru()
    Kamera ← FALSE   // Başlangıçta pasif

    // Eğer sistem aktifse sensörleri değerlendir
    IF (Sistem_Aktif = TRUE) THEN

        // Herhangi bir sensör tehdit algıladı mı?
        IF (Hareket = TRUE OR Kapi = TRUE OR Pencere = TRUE) THEN

            // Kamera aktivasyonu
            Kamera ← TRUE
            Kamera_Kaydi_Baslat()
            Yazdir("Kamera aktif edildi, görüntü kaydediliyor...")

            // Yanlış alarm kontrolü
            IF (Ev_Sahibi_Evde = TRUE) THEN
                Yazdir("Ev sahibi evde, yanlış alarm olabilir.")
                Alarm_Seviye ← 1   // Düşük seviye uyarı
            ELSE
                // Gerçek tehdit: Sensör tipine göre alarm seviyesi belirle
                IF (Kapi = TRUE OR Pencere = TRUE) THEN
                    Alarm_Seviye ← 3   // Yüksek alarm (giriş ihlali)
                ELSE IF (Hareket = TRUE) THEN
                    Alarm_Seviye ← 2   // Orta seviye alarm (hareket tespiti)
                ENDIF
            ENDIF

            // Alarm işlemleri
            IF (Alarm_Seviye > 0) THEN
                Alarm_Durum ← "AKTİF"
                Sesli_Alarm_Calistir(Alarm_Seviye)
                Yazdir("Alarm aktif! Seviye: " + Alarm_Seviye)

                // Bildirim gönder
                Gonder_SMS("Güvenlik Uyarısı! Seviye: " + Alarm_Seviye)
                Gonder_Email("Ev Güvenlik Sistemi Uyarısı", "Seviye: " + Alarm_Seviye)
                Gonder_AppBildirim("Alarm Aktif – Seviye " + Alarm_Seviye)

                // Kullanıcı alarm sıfırlama komutu verene kadar bekle
                Bekle(5 saniye)
                Alarm_Sifirlama ← Oku_Alarm_Sifirlama_Durumu()

                IF (Alarm_Sifirlama = TRUE) THEN
                    Alarm_Durum ← "KAPALI"
                    Alarm_Seviye ← 0
                    Kamera_Kaydi_Durdur()
                    Yazdir("Alarm sıfırlandı, sistem izleme moduna döndü.")
                ELSE
                    Yazdir("Alarm aktif kalıyor, yeniden kontrol ediliyor...")
                ENDIF
            ENDIF

        ELSE
            // Hiçbir tehdit yoksa
            IF (Alarm_Durum = "KAPALI") THEN
                Yazdir("Sistem normal şekilde izliyor...")
            ELSE
                Yazdir("Alarm hâlâ aktif, sıfırlama bekleniyor...")
            ENDIF
        ENDIF

    ELSE
        Yazdir("Sistem pasif modda. İzleme yapılmıyor.")
        Bekle(10 saniye)
    ENDIF

    // Döngü gecikmesi (enerji tasarrufu ve sensör okuma aralığı)
    Bekle(2 saniye)

ENDWHILE
