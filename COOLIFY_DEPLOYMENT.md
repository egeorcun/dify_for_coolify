# Dify Coolify Deployment Guide

Bu rehber, Dify'ı Coolify platformunda nasıl deploy edeceğinizi adım adım açıklar.

## Gereksinimler

- Coolify kurulu bir sunucu
- En az 4GB RAM
- En az 2 CPU core
- En az 20GB disk alanı

## Deployment Adımları

### 1. Projeyi Hazırlama

Coolify için özel olarak hazırlanmış dosyaları kullanacağız:
- `docker-compose.coolify.yml` - Coolify için optimize edilmiş Docker Compose dosyası
- `.env.coolify.example` - Basitleştirilmiş environment variables

### 2. Coolify'da Yeni Proje Oluşturma

1. Coolify dashboard'unuza giriş yapın
2. "New Project" butonuna tıklayın
3. Proje adını girin (örn: "dify")
4. "Create Project" butonuna tıklayın

### 3. Docker Compose Service Ekleme

1. Yeni oluşturulan projenin içinde "Add Service" butonuna tıklayın
2. "Docker Compose" seçeneğini seçin
3. Service adını girin (örn: "dify-app")

### 4. Docker Compose Dosyasını Yükleme

1. `docker-compose.coolify.yml` dosyasının içeriğini kopyalayın
2. Coolify'da "Docker Compose" alanına yapıştırın

### 5. Environment Variables Ayarlama

`.env.coolify.example` dosyasını referans alarak aşağıdaki değişkenleri ayarlayın:

#### Zorunlu Değişkenler:

```bash
# Database
DB_PASSWORD=güçlü_veritabanı_şifreniz

# Redis
REDIS_PASSWORD=güçlü_redis_şifreniz

# Application Security
SECRET_KEY=güçlü_secret_key_buraya
# Oluşturmak için: openssl rand -base64 42

# Vector Database
WEAVIATE_API_KEY=güçlü_weaviate_api_key

# Sandbox
SANDBOX_API_KEY=güçlü_sandbox_api_key

# Plugin
PLUGIN_DIFY_INNER_API_KEY=güçlü_plugin_api_key
```

#### Opsiyonel Değişkenler:

```bash
# URL Configuration (kendi domain'inizi kullanıyorsanız)
CONSOLE_API_URL=https://api.yourdomain.com
CONSOLE_WEB_URL=https://console.yourdomain.com
APP_API_URL=https://app-api.yourdomain.com
APP_WEB_URL=https://app.yourdomain.com

# Admin şifresi (ilk kurulum için)
INIT_PASSWORD=admin_şifreniz
```

### 6. Domain Ayarları

1. Coolify'da service ayarlarına gidin
2. "Domains" sekmesine tıklayın
3. Ana domain'inizi ekleyin (örn: `dify.yourdomain.com`)
4. SSL sertifikası otomatik olarak oluşturulacak

### 7. Port Ayarları

Coolify otomatik olarak portları yönetecek, ancak manuel ayar gerekirse:
- Web Frontend: Port 3000
- API: Port 5001
- Database: Port 5432 (internal)
- Redis: Port 6379 (internal)
- Weaviate: Port 8080 (internal)
- Sandbox: Port 8194 (internal)

### 8. Deployment

1. Tüm ayarları kontrol edin
2. "Deploy" butonuna tıklayın
3. Deployment loglarını takip edin

### 9. İlk Kurulum

1. Deployment tamamlandıktan sonra domain'inize gidin
2. İlk admin kullanıcısını oluşturun
3. Dify'ı kullanmaya başlayın!

## Güvenlik Önerileri

### API Keys Oluşturma

Güçlü API key'ler oluşturmak için:

```bash
# Secret Key için
openssl rand -base64 42

# Diğer API key'ler için
openssl rand -hex 32
```

### Firewall Ayarları

- Sadece gerekli portları açık tutun
- Database ve Redis portlarını external erişime kapatın
- SSL/TLS kullanın

## Troubleshooting

### Yaygın Sorunlar

1. **Database bağlantı hatası**
   - DB_PASSWORD'ün doğru ayarlandığından emin olun
   - Database service'inin çalıştığını kontrol edin

2. **Redis bağlantı hatası**
   - REDIS_PASSWORD'ün doğru ayarlandığından emin olun
   - Redis service'inin çalıştığını kontrol edin

3. **Web arayüzü açılmıyor**
   - Domain ayarlarını kontrol edin
   - SSL sertifikasının oluştuğunu kontrol edin

4. **API erişim sorunu**
   - CORS ayarlarını kontrol edin
   - API URL'lerinin doğru ayarlandığından emin olun

### Log Kontrolü

Coolify dashboard'unda her service için logları kontrol edebilirsiniz:
1. Service'e tıklayın
2. "Logs" sekmesine gidin
3. Real-time logları görüntüleyin

## Backup ve Maintenance

### Backup

Önemli veriler için backup alın:
- PostgreSQL database
- Redis data
- Weaviate vector data
- Application storage

### Güncelleme

1. Yeni image versiyonlarını docker-compose.coolify.yml'de güncelleyin
2. Coolify'da "Redeploy" butonuna tıklayın

## Performans Optimizasyonu

### Resource Limits

Coolify'da her service için resource limit'leri ayarlayın:
- API: 1GB RAM, 1 CPU
- Worker: 1GB RAM, 1 CPU
- Database: 2GB RAM, 1 CPU
- Redis: 512MB RAM, 0.5 CPU

### Scaling

Yüksek trafik için:
- Worker service'ini scale edin
- Database connection pool'unu artırın
- Redis memory'sini artırın

## Destek

Sorun yaşarsanız:
1. Bu dokümandaki troubleshooting bölümünü kontrol edin
2. Coolify loglarını inceleyin
3. Dify community'sine başvurun

## Notlar

- Bu konfigürasyon production kullanımı için optimize edilmiştir
- Tüm sensitive bilgileri environment variables olarak ayarlayın
- Düzenli backup almayı unutmayın
- Güvenlik güncellemelerini takip edin