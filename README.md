# 🚀 Yerel Yapay Zeka Kurulum Rehberi: Open WebUI + Ollama (GPU Destekli)

Bu rehber; verilerin tamamen sizin kontrolünüzde kaldığı, internete ihtiyaç duymayan, GPU destekli ve yüksek performanslı bir yapay zeka istasyonunu Docker üzerinde nasıl kuracağınızı adım adım anlatır.

## 📌 Neden Yerel Kurulum?
- **Gizlilik:** Verileriniz yerel diskte kalır, bulut servislerine gönderilmez.
- **Sıfır Maliyet:** Ücretli aboneliklere veya token limitlerine takılmazsınız.
- **Donanım Gücü:** Ekran kartınızın (GPU) tüm potansiyelini kullanarak en ağır modelleri bile akıcı çalıştırırsınız.

---

## 🛠️ 0. Ön Hazırlıklar
Kuruluma başlamadan önce sisteminizde şu bileşenlerin kurulu olması gerekmektedir:
1.  **NVIDIA Güncel Sürücüler:** Ekran kartınızın en son sürücülerini yükleyin.
2.  **Docker Desktop:** Ayarlardan "Use the WSL 2 based engine" seçeneğinin aktif olduğundan emin olun.
3.  **NVIDIA Container Toolkit:** Docker'ın GPU birimlerine erişebilmesi için bu bileşen **kritiktir**. [Resmi Kurulum Sayfası](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

---

## 🏗️ 1. Adım: Arayüz (Open WebUI) Kurulumu
Kullanıcı paneli olarak **Open WebUI**'ın CUDA (GPU) destekli sürümünü ayağa kaldırıyoruz. PowerShell terminaline şu komutu yapıştırın:

```powershell
docker run -d -p 3000:8080 --gpus all --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:cuda
```

---

## 🧠 2. Adım: Model Motoru (Ollama) Kurulumu
Modelleri çalıştırmak için **Ollama** motorunu GPU desteğiyle kuruyoruz:

```powershell
docker run -d --gpus=all -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

---

## 📥 3. Adım: Modelleri Yükleme
Motor hazır olduktan sonra, popüler modelleri Ollama içerisine çekiyoruz:

**Google Gemma 3 (Çok Modlu ve Görsel Analiz Yetenekli):**
```powershell
docker exec -it ollama ollama pull gemma3
```

**DeepSeek-R1 (Akıl Yürütme ve Teknik Problem Çözme Odaklı):**
```powershell
docker exec -it ollama ollama pull deepseek-r1:8b
```

---

## 🔗 4. Adım: Yapılandırma
1.  Tarayıcınızdan `http://localhost:3000` adresine gidin.
2.  Profil simgesine tıklayıp **Settings > Connections** sekmesine geçin.
3.  Ollama API URL alanına `http://host.docker.internal:11434` yazıp doğrulayın.
4.  Bağlantı sağlandığında üst menüden dilediğiniz modeli seçip kullanmaya başlayabilirsiniz.

---

## ⚠️ Karşılaşılan Hatalar ve Çözümleri

### 🛑 Hata: "Container name /ollama is already in use"
Sistemde aynı isimde eski bir konteyner kalmış olabilir. Şu komutla temizleyin:
```powershell
docker rm -f ollama
```

### 🛑 Hata: "could not select device driver"
NVIDIA Container Toolkit kurulu değildir veya Docker sistemi görmüyordur. Kurulumu kontrol edip Docker Desktop'ı yeniden başlatın.

### 🛑 Hata: Mermaid.js Diyagram Hataları
Modeller diyagram kodlarını yazarken bazen sözdizimi hatası yapabilir. Modele "Kodun sonundaki parantezleri kapat ve Mermaid formatına uygun düzelt" komutu vermeniz yeterlidir.

---

## 💡 Genel Kullanım Alanları
- **Metin Üretimi:** Makale, e-posta veya rapor taslakları hazırlama.
- **Kod Yazımı:** Yazılım geliştirme süreçlerinde asistanlık ve hata ayıklama.
- **Özetleme:** Uzun dökümanları veya toplantı notlarını saniyeler içinde analiz etme.
- **Görsel Analiz:** Gemma 3 ile resim veya ekran görüntüsü üzerinden bilgi alma.
