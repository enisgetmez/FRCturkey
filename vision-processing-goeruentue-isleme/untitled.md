---
description: İçerik Hazırlanıyor..
---

# Görüntü İşleme Kaynakları

### Vision Processing Stratejileri

Vision Processing kullanmak, robotunuzun alandaki öğelere duyarlı olmasını ve daha fazla özerk olmasını sağlamanın harika bir yoludur. Genellikle FRC oyunlarında, topların veya diğer oyun parçalarının otonom olarak hedeflenmesi veya sahadaki yerlere gitmesi için bonus puanlar vardır. Vision Processing, bu sorunların çoğunu çözmenin harika bir yoludur.Eğer meydan okuyan özerk bir kodunuz varsa, o zaman insan sürücülerine yardım etmek için teleop döneminde de kullanılabilir.

Görme işlemi için ve vizyon programının çalışması gereken bileşenleri seçmek için birçok seçenek vardır. WPILib ve ilgili araçlar bir dizi seçeneği destekler ve ekiplere ne yapacağına karar vermek için çok fazla esneklik sağlar. Bu makalede, mevcut olan birçok seçenek ile ilgili bazı bilgiler verilecektir.

![](../.gitbook/assets/image%20%2859%29.png)

#### OpenCV Görüntü İşleme Kütüphanesi

OpenCV, akademi ve endüstri genelinde yaygın olarak kullanılan açık kaynaklı bir görüntü işleme kütüphanesidir. GPU hızlandırılmış işlem sağlayan bir desteğe sahip, C ++, Java ve Python da dahil olmak üzere birçok dil için uygun kütüphanelere sahiptir. Ayrıca birçok web sitesi, kitap, video ve eğitim kursları ile belgelenmiştir, böylece nasıl kullanılacağını öğrenmenize yardımcı olacak birçok kaynak bulunmaktadır. Bu nedenlerle ve daha fazlası için WPILib'in C ++ ve Java sürümleriyle kullanım için OpenCV'yi kabul ettik. 2017'den başlayarak WPILib ile birlikte gelen OpenCV kütüphaneleri, video yakalama, işleme ve vision için kütüphanede destek ve vision algoritmalarınızı oluşturmanıza yardımcı olacak araçlar hazırladık. OpenCV hakkında daha fazla bilgi için bkz. [Http://opencv.org.](Http://opencv.org)

**RoboRIO üzerinde vision kodu**

![](../.gitbook/assets/image%20%2864%29.png)

Vision programı, genel robot programının sadece bir parçası olduğu için programlama oldukça basittir. Program elle veya GRIP tarafından C ++ veya Java ile yazılabilir. Dezavantajı, robot programı ile aynı işlemci üzerinde çalışan vision kodunun performans sorunları yaşayabilmesidir. Bu, robot ve vision programınızın gereksinimlerine bağlı olarak değerlendirmeniz gereken bir şeydir.

Vision kodu, robot kodunun kullandığı sonuçları üretir. Bir görüş dizisinden değer alan robot kodu yazarken senkronizasyon sorunlarına dikkat edin. GRIP tarafından üretilen kod ve WPILib'deki VisionRunner sınıfı bunu daha kolay hale getirecektir.

Video akışı kamera operatörü arayüzünü kullanarak SmartDashboard'a gönderilebilir, böylece operatörler kameranın gördüklerini görebilir. Buna ek olarak, OpenCV komutlarını kullanarak görüntülere ek açıklamalar ekleyebilirsiniz, böylece kontrol panelinde hedefler veya diğer ilginç nesneler tanımlanabilir.

**DS bilgisayarında vision kodu**

![](../.gitbook/assets/image%20%2832%29.png)

Görüntü, işlem yapmak için Driver Station dizüstü bilgisayarına geri akışa alınmıştır. Classmate dizüstü bilgisayarlar vision işlemede roboRIO'dan daha hızlıdır fakat üzerinde çalışan gerçek zamanlı programlara sahip değildir. GRIP, dizüstü bilgisayarında doğrudan NetworkTables kullanarak robota gönderilen sonuçlarla birlikte çalıştırılabilir. Alternatif olarak, kendi vision programınızı seçtiğiniz bir dili kullanarak yazabilirsiniz. Yerel bir NetworkTables uygulaması ve OpenCV bağlantısı çok iyi olduğundan Python iyi bir seçimdir.

Görüntüler işlendikten sonra, hedef konum, mesafe veya ihtiyacınız olan herhangi bir şey gibi anahtar değerler, NetworkTables ile robota geri gönderilebilir. Gecikme sizler için bir sorun olabilir çünkü dizüstü bilgisayardaki görüntülerde gecikmeler ve sonuçların geri gelmesi biraz zaman alabilir.

Görüntü akışı SmartDashboard veya GRIP'de görüntülenebilir.

**Bir işlemci üzerinde vision kodu**

![](../.gitbook/assets/image%20%2834%29.png)

Raspberry PI veya Kangaroo gibi iş süreçleri, görüntü kodunun desteklenmesi için idealdir. Avantajı, tam hız çalıştırabilmeleri ve robot programına müdahale etmemeleridir. Bu durumda, kamera büyük olasılıkla işlemciye\(Raspberry pi vb.\) veya \(ethernet kamera kullanıyorsanız\) robotun üzerindeki bir ethernet girişine bağlanır. Program, herhangi bir dilde yazılabilir, ancak OpenCV ve NetworkTable'lar için basit bağlantılardan dolayı Python iyi bir seçimdir. Bazı takımlar ve deneyimsiz programcılar için çok karmaşık olabilse de, Nvidia Jetson gibi işlemciler en yüksek hız ve en yüksek çözünürlük için yüksek performans sağlayabilmesi için kullanılabilir.

Veri, ağ işlemcisi üzerindeki vision programından, NetworkTables  veya seri bağlantı üzerinden özel bir protokol kullanılarak robota gönderilebilir.

#### Kamera seçenekleri

WPILib tarafından desteklenen bir dizi kamera seçeneği vardır. Kameralar, nasıl çalıştığını belirleyebilecek bir dizi parametreye sahiptir. Örneğin, kare hızı ve görüntü çözünürlüğü, alınan görüntülerin kalitesini etkiler, ancak çok yüksek etkili işlem süresi , robot programınızda ciddi etkiye sahip olabilecek bant genişliğinin oluşmasını sağlar.

2017'de yeni olan, C ++ ve Java'da mevcut olan ve robota bağlı kameralarla arayüz sağlayan bir CameraServer sınıfı var. Bir Kaynak nesnesi aracılığıyla yerel işlem için görüntüler alır ve orada görüntülemek veya işlem yapmak için akışınızı sürücü istasyonunuza gönderir. Video akışı CameraServer tarafından işlenir.

WPILib ile kameraların kullanımı ile ilgili detaylar bir sonraki bölümde detaylandırılmıştır.

### Görüntü alın ve işleyin: CameraServer sınıfı

**Kavramlar**

Genellikle FRC'de kullanılan kameralar \(Axis kamera gibi USB ve Ethernet kameraları\) nispeten sınırlı çalışma modları sunar. Genel olarak, tek bir çözünürlük ve kare hızında yalnızca tek bir görüntü çıktısı \(tipik olarak JPG gibi sıkıştırılmış biçimde bir RGB görüntüsü \) sağlarlar. USB kameralar, sadece birden fazla uygulama kameranıza aynı anda erişemeyeceği için sınırlıdır.

2017 CameraServer çoklu kameraları destekler. Bir kamera bağlantısı kesildiğinde otomatik olarak yeniden bağlanma gibi ayrıntıları ele alır ve aynı zamanda kameranın sunduğu görüntüleri çoklu "istemciler" haline getirir \(örn. Hem robot kodunuz hem de gösterge panosu kameranıza aynı anda bağlanabilir\).

#### Kamera isimleri

CameraServer'daki her kamera benzersiz bir şekilde adlandırılmalıdır. Bu ayrıca, Panodaki kamera için görünen addır. CameraServer startAutomaticCapture \(\) ve addAxisCamera \(\) işlevlerinin bazı türevleri otomatik olarak kamerayı \(ör. "USB Camera 0" veya "Axis Camera"\) adlandırır veya kameraya daha açıklayıcı bir ad verebilirsiniz \(örn. "Intake Cam"\). Tek gereksinim, her kameranın kendine özgü bir adı olmasıdır.

#### USB kamera ile ilgili notlar

#### CPU kullanımı

CameraServer ile CPU Kullanımı, gerektiğinde sadece sıkıştırma ve açma işlemlerini gerçekleştirerek , herhangi bir istemci bağlı olmadığında akışı otomatik olarak devre dışı bırakarak CPU kullanımını en aza indirecek şekilde tasarlanmıştır.

CPU kullanımını en aza indirmek için, gösterge paneli çözünürlüğü kamerayla aynı çözünürlüğe ayarlanmalıdır; Bu, CameraServer'ın görüntüyü sıkıştırmasını ve yeniden sıkıştırmasını sağlamaz, bunun yerine resim gösterge panelinden alınan JPEG görüntüsünü doğrudan panoya iletebilir. Kontrol panelindeki çözünürlüğü değiştirmenin \* kamera çözünürlüğünü değiştirmediğini unutmayın. kamera çözünürlüğünü değiştirmek, NetworkTables üzerinden veya robot kodunuzda \(kamera nesnesinde setResolution öğesini çağırarak\) yapılabilir.

#### USB Bant Genişliği

Roborio, USB arabirimleri üzerinden tek seferde çok fazla veri iletebilir ve alabilir. Kamera görüntüleri çok fazla veri gerektirebilir ve bu yüzden bant genişliği limitini aşmak nispeten kolaydır. Bir USB bant genişliği hatasının en yaygın nedeni, özellikle birden çok kamera bağlı olduğunda, JPEG olmayan bir video modunun seçilmesi veya çok yüksek çözünürlükte çalışmasıdır.

#### Mimari

2017 için CameraServer iki katmandan, yüksek seviyeli WPILib CameraServer sınıfından ve düşük seviyeli cscore kitaplığından oluşmaktadır.

#### CameraServer sınıfı

CameraServer sınıfı \(WPILib'in bir parçası\) robot kodunuza kamera eklemek için yüksek düzeyde bir arabirim sağlar. Ayrıca kameralar ve kamera sunucuları hakkındaki bilgileri NetworkTable'lara yayınlamaktan sorumludur, böylece LabView Dashboard ve SmartDashboard gibi Sürücü İstasyonu kontrol panelleri kameraları listeleyebilir ve akışlarının nerede bulunduğunu belirleyebilir. Oluşturulan tüm kameraların ve sunucuların bir veritabanını korumak için tek bir desen kullanır.

CameraServer'daki bazı temel işlevler şunlardır:



* startAutomaticCapture \(\): Bir USB kamera \(ör. Microsoft LifeCam\) ekleyin ve bunun için bir sunucu başlatır, böylece panodan görüntülenebilir.
* addAxisCamera \(\): Bir axis kamerası eklemek için kullanılır. Robot kodunuzda Axis kameranın görüntülerini işlemiyor olsanız bile, bu işlevi Axis kamerasının Pano'nun açılır menü listesinde görünmesi için kullanabilirsiniz. Aynı zamanda bir sunucu başlatır, böylece sürücü istasyonunuz USB üzerinden roboRio'ya bağlandığında axis akışı hala görüntülenebilir \(hem Axis kameranız hem de iki robot radyo Ethernet portuna bağlı roboRio varsa yarışmada kullanışlıdır\).
* getVideo \(\): Bir kameraya OpenCV erişimi edinin. Bu, roboRio'da \(robot kodunuzda\) görüntü işleme için kameradan görüntüler almanızı sağlar. 
* putVideo \(\): OpenCV görüntülerini besleyebileceğiniz bir sunucu başlatın. Bu, özel işlenmiş ve / veya açıklamalı görüntüleri panoya aktarmanıza olanak tanır.

#### Cscore Kütüphanesi

Cscore kütüphanesi alt seviye uygulamasına aşağıdakileri sağlar:

* USB ve HTTP \(örneğin, axis kamera\) kameralarından görüntüler alın
* Kamera ayarlarını değiştir \(ör. Kontrast ve parlaklık\)
* Kamera video modlarını değiştir \(piksel formatı, çözünürlük ve kare hızı\)
* Bir web sunucusu olarak hareket edin ve görüntüleri standart bir M-JPEG akışı olarak sunun
* Görüntü işleme için görüntüleri OpenCV "Mat" nesnelerine dönüştürün

