EYLÜL BOT - HEROKU WORKER DEPLOYMENT

Bu dosyaları projenin kök dizinine kopyala:
- heroku.yml
- Dockerfile.heroku

Gerekli Heroku Config Vars:
- TELEGRAM_BOT_TOKEN = BotFather tokenı
- OWNER_ID = Telegram kullanıcı ID'n
- LLM_PROVIDER = openai (veya kullandığın sağlayıcı)
- LLM_MODEL = sağlayıcının desteklediği model adı
- LLM_API_KEY = kullandığın yapay zekâ sağlayıcısının API anahtarı

Container stack ile dağıtım:
1. Heroku uygulamanı oluştur.
2. Proje kök dizininde:
   heroku stack:set container -a UYGULAMA_ADI
3. Heroku Dashboard > Settings > Config Vars bölümünden yukarıdaki değişkenleri ekle.
4. Git deposuna heroku.yml ve Dockerfile.heroku dosyalarını ekleyip gönder:
   git add heroku.yml Dockerfile.heroku
   git commit -m "Add Heroku worker deployment"
   git push heroku main
5. Worker dyno'yu aç:
   heroku ps:scale worker=1 -a UYGULAMA_ADI
6. Logları kontrol et:
   heroku logs --tail -a UYGULAMA_ADI

ÖNEMLİ:
- Bu uygulama Telegram long-polling botudur; web dyno değil worker dyno olarak çalışır.
- Heroku dyno dosya sistemi kalıcı değildir. Varsayılan SQLite veritabanı (data/db/vusan.db), yeniden başlatma veya yeni dağıtımda kaybolabilir. Kalıcı veri gerekiyorsa veritabanı için kalıcı bir harici depolama/uygun veritabanı çözümü gerekir.
- TELEGRAM_BOT_TOKEN ve LLM_API_KEY gibi gizli değerleri GitHub'a veya dosyalara yazma; yalnızca Heroku Config Vars üzerinden ekle.
