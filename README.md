# youtube_bilgi
#yotube videosudan başlık ve beğeni, izlenme verilerini çeken python projesi 
## Gereksinimler
#Bu projeyi çalıştırmak için aşağıdaki kütüphane gereklidir:
#pip install yt-dlp

import yt_dlp
from datetime import datetime

def video_bilgisi_getir(url):
    try:
        # yt-dlp seçenekleri
        ydl_opts = {
            'quiet': True,  # Kayıtlar ve işlemlerle ilgili çıkışı engeller
            'no_warnings': True,  # Uyarıları engeller
            'force_generic_extractor': True,  # Genel çıkarıcıyı zorlar
        }
        
        # yt-dlp ile video bilgilerini alıyoruz
        with yt_dlp.YoutubeDL(ydl_opts) as ydl:
            # Video bilgilerini çıkarıyoruz, download=False olduğu için indirme yapmaz
            info = ydl.extract_info(url, download=False)

            # Yayınlanma tarihini formatlamak için datetime kullanıyoruz
            upload_date = datetime.strptime(info['upload_date'], '%Y%m%d').strftime('%d-%m-%Y')

            # Video bilgilerini ekrana yazdırıyoruz
            print("\n--- YouTube Video Bilgileri ---")
            print(f"Başlık: {info['title']}")  # Video başlığı
            print(f"Kanal: {info['uploader']}")  # Kanal adı
            print(f"İzlenme Sayısı: {info['view_count']:,}")  # İzlenme sayısı
            print(f"Yayınlanma Tarihi: {upload_date}")  # Yayınlanma tarihi
            print(f"Açıklama (ilk 300 karakter):\n{info['description'][:300]}...\n")  # Açıklamanın ilk 300 karakteri
    except Exception as e:
        # Eğer hata oluşursa, hata mesajını yazdırıyoruz
        print("Bir hata oluştu:", e)

# Kullanıcıdan YouTube video linkini alıyoruz
link = input("YouTube video linkini buraya yapıştır: ")
video_bilgisi_getir(link)  # Fonksiyonu çağırıyoruz
