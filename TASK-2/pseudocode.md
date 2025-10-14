Başla

// Kullanıcı Girişi
EĞER kullanıcı_girişi == başarılı İSE
    Devam Et
DEĞİLSE
    Giriş ekranına yönlendir
    BİTİR

// Ürün Kategorileri Arasında Gezinme
DÖNGÜ kategori IN ürün_kategorileri
    kategori_görüntüle
    ürün_seç

    // Ürün Sepete Ekleme
    EĞER ürün_seçildi İSE
        // Stok Kontrolü
        EĞER ürün.stok > 0 İSE
            sepete_ekle(ürün)
        DEĞİLSE
            stok_yetersiz_mesajı_göster
        SON
    SON
SON

// Sepeti Görüntüleme ve Düzenleme
DÖNGÜ ürün IN sepet
    ürün_bilgisi_göster
    EĞER kullanıcı_düzenleme_yapmak_istiyor İSE
        düzenleme_yap(ürün)
    SON
SON

// İndirim Kodu Uygulama
EĞER kullanıcı_indirim_kodu_girdi İSE
    EĞER indirim_kodu_geçerli İSE
        indirimi_uygula(sepet)
    DEĞİLSE
        geçersiz_kod_mesajı_göster
    SON
SON

// Minimum Tutar Kontrolü
EĞER sepet.toplam_tutar >= 50 İSE
    Devam Et
DEĞİLSE
    minimum_tutar_uyarısı_göster
    BİTİR
SON

// Kargo Ücreti Hesaplama
EĞER sepet.toplam_tutar >= 200 İSE
    kargo_ucreti = 0
DEĞİLSE
    kargo_ucreti = 25
SON

// Ödeme Yöntemi Seçimi
EĞER kullanıcı_odeme_yontemi_seçti İSE
    ödeme_yöntemi = kullanıcı_seçimi
DEĞİLSE
    ödeme_yöntemi_seçim_uyarısı_göster
    BİTİR
SON

// Sipariş Onayı
siparişi_onayla(sepet, ödeme_yöntemi, kargo_ucreti)
sipariş_başarılı_mesajı_göster

BİTİR
