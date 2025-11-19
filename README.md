# 11_Buton_Kontrollu_LED_Yakma_PULL-UP (GPIO Input)

Bu proje, **STM32F407-Discovery** kartı üzerinde harici bir buton (`PA0`) kullanarak LED desenlerini değiştiren bir uygulamadır.

Bu depo ile birlikte ilk kez **GPIO Giriş (Input)** okuma işlemleri ve **Dahili Pull-Up Direnci** kullanımı devreye girmektedir.

---

### 🎯 Proje Senaryosu

Kod, sonsuz döngü içinde `PA0` pinine bağlı butonun durumunu sürekli kontrol eder (**Polling** yöntemi) ve duruma göre iki farklı deseni uygular:

1.  **Varsayılan Durum (Butona Basılı DEĞİL):**
    * **Okunan Değer:** `1` (SET) - (Dahili Pull-Up sayesinde 3.3V).
    * **Eylem:** Tek indeksli LED'ler yanar.
    * **Görsel Sonuç:** `PA2` ve `PA4` LED'leri AÇIK, diğerleri KAPALI.

2.  **Aktif Durum (Butona BASILDI):**
    * **Okunan Değer:** `0` (RESET) - (Buton pini Toprağa/GND'ye çeker).
    * **Eylem:** Çift indeksli LED'ler yanar.
    * **Görsel Sonuç:** `PA1` ve `PA3` LED'leri AÇIK, diğerleri KAPALI.

---

### 🔌 Pull-Up Direnci ve Mantığı Nedir?

Elektronikte bir giriş pini boşta bırakılırsa (butona basılmadığında), ortamdaki gürültüden etkilenerek rastgele değerler alabilir (Floating/Yüzen Durum). Bunu engellemek için pini varsayılan olarak 3.3V seviyesinde tutmak gerekir.

* **Internal Pull-Up:** Bu projede harici bir direnç lehimlemek yerine, STM32 mikrodenetleyicisinin içinde bulunan **dahili direnç** yazılımla aktif edilmiştir.
* **Harici direnç** kullanılmak istenseydi. Aşağıdaki görselde hem PULL-UP hem de PULL-DOWN bağlantı görülmektedir.

*	DONANIMSAL PULL-UP;
PULL-UP için 10k ohm direnç kullanıldı.
LED direnci 220 ohm kullanıldı.
 
<img width="560" height="496" alt="image" src="https://github.com/user-attachments/assets/cb9cdf8d-af96-437e-9dd0-a1cd8c987171" />

*	DONANIMSAL PULL-DOWN;
PULL-DOWN için 10k ohm direnç kullanıldı.
LED direnci 220 ohm kullanıldı.

<img width="454" height="415" alt="image" src="https://github.com/user-attachments/assets/4f681b8e-db6c-4fe8-aee1-932fef17776b" />

---

### ⚙️ STM32CubeIDE ile Pull-Up Ayarı

Projenin `.ioc` dosyasında buton pinini yapılandırırken şu adımlar izlenmiştir:

1.  `PA0` pini **`GPIO_Input`** olarak seçilir.
2.  Sol menüden `System Core > GPIO` sekmesine gidilir.
3.  `PA0` seçilir ve **"GPIO Pull-up/Pull-down"** ayarı **`Pull-up`** yapılır.

<img width="843" height="644" alt="image" src="https://github.com/user-attachments/assets/48b67254-9e9d-4e19-99a7-65d42794f563" />


---

### 🛠️ Gerekli Donanım

* **1x** STM32F407-Discovery Geliştirme Kartı
* **4x** LED (Tercih edilen renklerde)
* **4x** 220 Ohm Direnç (LED koruması için)
* **1x** Push-Button (Buton)
* **1x** Breadboard
* **Jumper Kablolar** (Erkek-Erkek ve Erkek-Dişi)

---

### 🔌 Devre Şeması

**Dikkat:** Pull-Up mantığı kullanıldığı için butonun bir ucu pine, diğer ucu **GND** hattına bağlanmalıdır. (VCC/3.3V veya 5V hattına bağlamayın!)

| Bileşen | STM32 Pini | Bağlantı Detayı |
| :--- | :--- | :--- |
| **Buton** | `PA0` | Butonun diğer bacağı -> **GND** |
| **LED 1** | `PA1` | Anot -> Pin, Katot -> Direnç -> GND |
| **LED 2** | `PA2` | Anot -> Pin, Katot -> Direnç -> GND |
| **LED 3** | `PA3` | Anot -> Pin, Katot -> Direnç -> GND |
| **LED 4** | `PA4` | Anot -> Pin, Katot -> Direnç -> GND |

<img width="566" height="477" alt="image" src="https://github.com/user-attachments/assets/35b7fcef-e301-4943-abd7-5aa8d290d2aa" />

---

### 💻 Kod Bloğu

<img width="933" height="557" alt="image" src="https://github.com/user-attachments/assets/65e4f369-0f19-48e9-bbcd-1d1630a46b16" />

---

### 🚀 Nasıl Kullanılır?

1.  Bu depoyu klonlayın (`git clone ...`).
2.  STM32CubeIDE yazılımını açın.
3.  `File > Open Projects from File System...` seçeneği ile proje klasörünü seçin.
4.  Proje içindeki `.ioc` dosyasını açarak pin yapılandırmasını inceleyebilirsiniz.
5.  Derleyin (Build) ve ST-Link V2 üzerinden kartınıza yükleyin (Run).
