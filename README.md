### BirFatura'daki ikinci 20 günlük stajımda, monday.com'a benzer, hataların gerçek zamanlı takip edilebileceği bir web site projesi geliştirmem istendi ve bu projeyi staj sürecim tamamlandığında kullanıma hazır bir hale getirdim. Bu projeyi 8 ay boyunca, aktif bir şekilde şirket içerisinde kullandık. 8 ay sonunda proje yerini Jira'ya bıraktı. (2018) ###
---
**İÇİNDEKİLER**

**1.  PROJE TAKİP SİSTEMİ**

    1.  Amaç

    2.  Kullanılan Araçlar

    3.  Gereksinimlerin Belirlenmesi

    4.  Kuruluşun Kullandığı Proje Takip Sisteminin Yapısı

    5.  Proje Takip Sisteminin Veri Tabanı Yapısı

    6.  Uygulama

        1.  Proje takip sistemi projesinin oluşturulması

        2.  Proje takip sisteminin kullanıcı ara yüzü stili ve navigation bar

        3.  Proje takip sisteminin tablo yapısı

        4.  Web Api mantığı ile verilerin gönderilmesi, Ajax ile elde edilmesi
            ve cache’lenmesi

        5.  Tabloların oluşturulması

        6.  Tablo içeriklerinin oluşturulması

        7.  Filtreleme

        8.  Tabloların küçültülmesi

        9.  SignalR kitaplığı ile tabloya veri eklenmesi, verilerin düzenlenmesi
            ve arşivlenmesi

        10. Proje takip sistemi konuşmalarının yapısı

        11. Proje takip sisteminin kişisel web sayfaları

        12. Proje takip sisteminin timeline yapısı

        13. Proje takip sisteminin e-posta gönderme sistemi

        14. Proje takip sisteminin yayımlanması

    7.  Öneriler

**2.  KAYNAKLAR**

**3.  EK – 1. Terimler**


1.  **PROJE TAKİP SİSTEMİ**

    1.  **Amaç**

Bana verilen bu görevde, benden, kuruluşun monday.com web sitesinde her ay 25
dolar ücret ödeyerek kullandıkları sistemin benzer işlevlerine sahip, Microsoft
.Net ortamının sağladığı teknolojileri kullanarak ASP.NET Web Uygulamasında
(.Net Framework) Web Forms ve Web API ile bir web sitesi tasarlamam istendi.

Ücret ödeyerek kullandıkları bu sistem [2], kuruluşta çalışan herkesin:

-   Geliştirdikleri projeler için müşterilerin geri bildirimlerini, isteklerini
    takip edebilmelerini

-   Projeler ile ilgili yapılacak herhangi bir işi kuruluşta çalışan kişilere
    atayabilmelerini, bu iş ile ilgili bir timeline (zaman çizgisi)
    oluşturabilmelerini ve mail ile görev atanan kişilerin bilgilendirilmesini

-   Gelecekte yapmayı düşündükleri proje geliştirme ile ilgili herhangi bir
    fikri ekleyerek görebilmelerini

-   Geçmişte, projeler ile ilgili yaptıkları değişiklikleri, geliştirmeleri
    takip edebilmelerini

-   Tüm bunlarla ilgili, kişilerin sistem içerisinde konuşmalar (yazışmalar)
    yapabilmelerini ve konuşma yaptıkları konu içerisinde yeni bir konuşma
    eklendiğinde, konuşmaya dahil olan kişilerin yeni eklenen konuşma ile ilgili
    mail ile bilgilendirilmesini

-   Ayrıca, tüm bunlarla ilgili öncelik veya durum ataması yapabilmelerini

sağlayarak, hem proje geliştirme takibi, hem de proje hata takibi yapmalarını
kolaylaştırmaktadır.

Geliştirdiğim bu proje takip sistemi ile kuruluşun halihazırda kullandığı sistem
yerine kendi sistemini kullanarak kuruluşun ihtiyaçlarına göre, sistem üzerinde
değişiklikler yaparak, daha özelleştirilebilir bir proje takip sistemine
geçmesi, halihazırda kullandığı sistem için kuruluşun harcadığı maliyetin
azaltılması, kuruluşun geleceğe dair proje geliştirme fikirlerinin üçüncü
kişilerce erişilebilirliğinin engellenmeye çalışılması ve kuruluşun ihtiyaçları
doğrultusunda verilerin arşivlenebilmesi hedeflendi.

1.  **Kullanılan Araçlar**

Bu uygulamayı geliştirirken aşağıdaki programlama dillerini, araçları ve
kitaplıkları kullandım:

*Programlama Dilleri*: C\#, JavaScript, HTML, CSS, SQL, JSON

*Geliştirme Ortamları*: Visual Studio 2017, Microsoft SQL Server Management
Studio (MS SQL Server)

*Kitaplıklar*: SignalR, System.LINQ, Strathweb.CacheOutput.WebApi2, jQuery,
Bootstrap, Metronic, DayPilot, CKEditor, Moment

*Diğer Araçlar*: Chrome web tarayıcısı, Excel, FileZilla, Turhost hosting
servisi, html-color-codes.info

Staj yaptığım kuruluş, geliştirdiği çözümleri için Microsoft Azure Bulut
Sistemi’ni kullandığı için, benden, proje takip sistemini ASP.NET uygulaması
olarak geliştirmem istendi. Dolayısıyla bu projeyi geliştirirken Visual Studio
geliştirme ortamını kullandım.

Projenin web sunucusu tarafını C\#, istemci (client) tarafını HTML, CSS ve
JavaScript programlama dillerini kullanarak geliştirdim.

Verileri yönetmek için Microsoft SQL Management Studio geliştirme ortamı ile SQL
programlama dilini kullandım. Projenin veri tabanını MS SQL Server’da
oluşturdum.

Web sunucusu ile veri tabanı arasında gerçekleşecek veri ekleme, silme ve
güncelleme gibi işlemleri yönetmek için LINQ kitaplığından yararlandım.

Web sunucusu ile istemci arasında gerçekleşecek veri alış veriş yükünü veri
tabanında hafifletebilmek amacıyla istemciye gönderilen bazı bilgilerin sunucu
tarafında cache’lenmesini sağlamak için Strathweb.CacheOutput kitaplığını
kullandım.

Proje takip sistemini kullanacak kişilerin, sistemi kullandıkları an yapacakları
veri ekleme, silme ve güncelleme gibi işlemleri gerçekleştirdikleri anda,
sistemi kullanan diğer kişilerin de sayfayı yenilemeye ihtiyaç duymadan veriler
üzerinde gerçekleşen değişiklikleri anlık olarak görebilmelerini sağlamak
amacıyla SignalR kitaplığını kullandım. SignalR kitaplığını kullanarak,
geliştirdiğim proje takip sistemini WebSocket protokolünden yararlanarak gerçek
zamanlı bir ASP.NET uygulaması haline getirdim.

Sistemin kullanıcı ara yüzü tasarımını Bootstrap, JQuery ve kuruluşun
halihazırda kullandığı Metronic kitaplıklarını kullanarak yaptım.

Geliştirdiğim proje takip sisteminde, kullanıcıların timeline
oluşturabilmelerini sağlamak için JavaScript programlama dili ile yazılmış olan
DayPilot kitaplığından yararlandım.

Kullanıcıların konuşmalar panelinde konuşmalarına resim, video, tablo ve çeşitli
stillerde metin ekleyebilmeleri için CKEditor kitaplığını kullandım.

Bazı tarihsel işlemler için Moment.js kitaplığını kullandım [3].

Kuruluşun kullandığı sistemden verileri elde edebilmem için, bu verileri
monday.com web sitesi .xlsx dosyası olarak dışarı aktarmama izin verdiğinden
Excel yazılımını kullandım.

Kullanıcı ara yüzündeki görselliği arttırmak için tablo başlıkları, durum ve
öncelik sütunlarındaki renk skalasını belirlemek amacıyla aldığım ekran
görüntüsünden renklerin neler olduğunu imaj dosyasından tespit etmeye yarayan
html-color-codes.info web sitesinin sağladığı *“Colors From Image”* aracını
kullandım [4].

Projeyi web sunucusunda yayınlayabilmem için staj yaptığım kuruluşun kullandığı
Turhost hosting servisinde yeni bir FTP hesabı oluşturup, bu hesap aracılığıyla
FileZilla yazılımını kullanarak, geliştirdiğim proje takip sistemini kuruluşta
çalışanların erişebileceği hale getirdim.

1.  **Gereksinimlerin Belirlenmesi**

Öncelikle, bana rehberlik yapan bilgisayar mühendisi nasıl bir sistem
istediğini, sistemden beklentilerini anlattı. Geliştireceğim sistemin sahip
olmasını istediği özellikleri:

-   monday.com web sitesinde kullandıkları *“Proje Takip”* sayfası için veri
    girişinin yapılabileceği bir form alanı olmalı. Durum, öncelik ve kimde
    alanlarının her biri HTML elementi olan select ile seçilebilmeli. Girilen
    veriler, bir tablo yapısında gösterilmeli.

![](https://2.bp.blogspot.com/-fUdx_Oxr-Y4/XM7uw4fJUHI/AAAAAAAAC7A/N2oZLDl4b_Ayg4GpHBDa0ghSgApVqpFuQCLcBGAs/s1600/e90d3030f543420b48732aebb3e98740.png)

Şekil 3.1. Kuruluşun kullandığı proje takip sisteminin Proje Takip web
sayfasından bir ekran görüntüsü

-   Kullandıkları sistemde *“DevOps”* sayfasındaki gibi bir timeline’ın olması
    yerine, yine bu sayfada verileri girmek için oluşturulacak form da başlangıç
    ve bitiş tarihleri datepicker aracılığı ile kaydedilmeli.

![](https://4.bp.blogspot.com/-s0H2tmdLBaQ/XM7uqcFhwrI/AAAAAAAAC5s/m-anw3VLKVgDMB3OgBw31menXG13LciUwCLcBGAs/s1600/6ce87a9f2a490f923a30ad659fd6a927.png)

Şekil 3.2. Kuruluşun kullandığı proje takip sisteminin DevOps web sayfasından
bir ekran görüntüsü

-   Her sayfada, filtrelemenin yapılabileceği bir arama alanı olmalı.

-   Herhangi bir kişiye görev ataması yapıldığında mail ile o kişi, verilen
    görev hakkında bilgilendirilmeli.

-   Web sunucusu ile veri tabanı arasındaki tüm işlemler, LINQ kitaplığı
    kullanılarak yapılmalı.

olarak belirtti.

Ayrıca, geliştireceğim sistemin kullanıcı ara yüzü tasarımı için herhangi bir
beklentisinin olmadığını belirtti. Tablolar arasında satırların sürükle bırak
şeklinde taşınabilir olmasını, tablonun uzun olması durumunda çok da kullanışlı
olmadığını düşündüğü için istemedi. Bunun yerine verilerin eklendiği formda,
select elementi ile verinin tablosunun değiştirilmesini istedi. Sistemi
kullanacak kişilerin kullanıcı kaydı ve giriş işlemlerini, ASP.NET projesini ilk
başta oluştururken kimlik doğrulama seçeneğini bireysel kullanıcı hesapları
olarak seçtiğim zaman, ASP.NET’in gerekli veri tabanı tablosu, sınıf ve aspx
dosyalarını oluşturacağını belirtti.

Proje takip sistemini geliştirmeye başlamadan önce, sistemi nasıl geliştirmeye
başlayacağıma karar verebilmem ve proje ihtiyaçlarını belirleyemem için
kuruluşun monday.com web sitesinde kullandığı sistemi incelemeye başladım.
İncelemelerim sonucunda, bana rehberlik yapan bilgisayar mühendisinin
beklentilerinin aksine, geliştireceğim sistemde timeline’ın olması ve veri
değişikliklerinin sayfayı yenilemeden, herkes tarafından anlık olarak
görülebilmesi gibi özelliklerin de olması gerektiği sonucuna vardım.

Bana rehberlik yapan bilgisayar mühendisi, her ne kadar projede bu özelliklerin
bulunmasına dair herhangi bir beklentisinin olmadığını belirtmiş olsa da, bu
özelliklerin olmadığı bir projenin basit bir web sayfası olmaktan öteye
geçemeyeceğini ve kullanışlı olmayacağını birkaç kez belirtmemden sonra, bu
özelliklerin de projeye dahil edilmesini kabul etti.

Sonuç olarak, bana rehberlik yapan bilgisayar mühendisinin isteklerine ek
olarak, projeyi geliştirebilmem için gerekli gereksinimleri:

-   Timeline oluşturabileceğim ve bu timeline üzerinde değişiklikler
    yapabileceğim bir kitaplık

-   Sistemde gerçekleşen veri değişikliklerini anlık olarak, sistemi o anda
    kullanan kişilerin web sayfasını tamamen yenilemeleri yerine, başka türlü
    gösterebileceğim bir yöntem

-   Sistemin kullanıcı ara yüzünün kullanışlı olmasını sağlayabileceğim bir web
    sayfası tasarım şablonu

-   Konuşmalar için zengin metin editörü özelliği olan bir kitaplık

bulmam olarak belirledim.

Daha sonrasında bunlara ek olarak projeyi geliştirmeye başladıktan sonra, staj
yaptığım kuruluşta, veri tabanından kullanıcıya gönderilen (hiç veya uzun süre
değişmeyen) bilgilerin kullanıcı tarayıcısında veya sunucuda cache’lenmesi ile
performansın arttırılabileceğini öğrenmem sonrasında gereksinimlere:

-   Hiç veya uzun süre değişmeyen verileri veri tabanının yükünü hafifletebilmek
    için hem kullanıcı tarafında hem de sunucu tarafında verileri
    cache’leyebileceğim bir yöntem

bulmayı da ekledim.

1.  **Kuruluşun Kullandığı Proje Takip Sisteminin Yapısı**

Geliştireceğim proje takip sisteminin sahip olması gerektiği özellikler için
gereksinimleri belirlememin ardından, kuruluşun kullandığı sistemin nasıl bir
yapıda olduğunu tespit edebilmem için monday.com’daki sistemin yapısını
inceledim.

Sistem, toplamda 5 bölümden oluşmaktadır:

*Proje Takip:* Bu bölüm, 5 tablodan oluşmaktadır. Her bir tablonun yapısı aynı
olmakla birlikte, sadece tablo isimleri farklılık göstermektedir.

![](https://1.bp.blogspot.com/-Fp1qZTbdLYw/XM7upC0s5rI/AAAAAAAAC5g/GP3lJ7W_JU8iA653lo822HbaJ9BAfArSwCLcBGAs/s1600/5eb6f5ad177434fa69d735dbf8799828.png)

Şekil 3.3. Proje Takip web sayfasındaki tablolar

Tabloların sütunları konu, müşteri e-postası, ekleyen, durum, öncelik, kimde ve
eklenme tarihi olmak üzere 7 sütundan oluşmaktadır. Konu, müşteri e-postası,
ekleyen, kimde ve eklenme tarihi de dahil olmak üzere bu sütun değerleri string
türünde değerler almaktadır. Kullanıcı, eklenme tarihini bile manuel olarak
girmek zorundadır. Durum sütunu 9 değer almaktayken, öncelik sütunu 5 değer
almaktadır.

![](https://3.bp.blogspot.com/-buc7aKA4qXQ/XM7ukhfMAlI/AAAAAAAAC40/wc1zlxJG5EYO5-nQw0uS9Pd61dbA4Z7wACLcBGAs/s1600/1cada5ce49d4de962b55461b95ec718d.png)

Şekil 3.4. Proje Takip durum sütunu değerleri

![](https://3.bp.blogspot.com/-ltX9GW0rr0g/XM7usW3SXnI/AAAAAAAAC6E/hG1GSVYbx0E22xVkIn7uhwbnrUfsXe2wgCLcBGAs/s1600/9ad924e657a18b890c5c6575dd0150a3.png)

Şekil 3.5. Proje Takip öncelik sütunu değerleri

*DevOps:* Bu bölüm, 2 tablodan oluşmaktadır. Her bir tablonun yapısı aynı
olmakla birlikte, sadece tablo isimleri değişiklik göstermektedir.

![](https://2.bp.blogspot.com/-5rFzoGEnpC0/XM7urW_fDtI/AAAAAAAAC54/PxWVykUiLCUMOvoMPsh684VJS26tR8qeACLcBGAs/s1600/9658bd93fd7e475ea3d930f2897a87ca.png)

Şekil 3.6. DevOps web sayfasındaki tablolar

Tabloların sütunları konu, kimde, durum, öncelik, tahmini süre, eklenme tarihi
ve timeline olmak üzere 7 sütundan oluşmaktadır. Konu, kimde, tahmini süre ve
eklenme tarihi de dahil olmak üzere bu sütun değerleri string türünde değerler
almaktadır. Proje Takip bölümde olduğu gibi bu bölümde de eklenme tarihinin
yanında ayrıca, tahmini süre timeline’dan elde edilmesi gerekirken, yine bu
sütun değeri manuel olarak girilmek zorundadır. Durum sütunu 8 değer alırken,
öncelik sütunu 5 değer almaktadır.

Öncelik sütununun aldığı değerler Proje Takip bölümünde olduğu gibi değişmezken,
durum sütununun aldığı değerler DevOps bölümünde değişiklik göstermektedir.

![https://1.bp.blogspot.com/-NTboyvSRm0o/XM7uvrP75lI/AAAAAAAAC6w/-hUX0IUaWFw_2GqAWr39m_BE7o4QuwE8ACLcBGAs/s1600/d99e16bc04a43251f2179766754683c1.png)

Şekil 3.7. DevOps durum sütunu değerleri

Timeline sütunundaki herhangi bir değere tıklanmasıyla web sayfasının en başında
timeline bölümü gösterilmektedir.

*Kuruluşta Çalışanların Kişisel Olarak Kullandıkları Bölümler:* Kişisel olarak
kullanılan bölümlerden 3 tane bulunmaktadır.

İlk kişisel bölümde; yapılacaklar ve yapılmışlar olmak üzere iki tablodan
oluşmaktadır. Tabloların her biri konu, eklenme tarihi, işlem adı ve ödeme id
olmak üzere 4 sütundan oluşmaktadır. Konu, eklenme tarihi ve ödeme id string
türünde değer alırken, işlem adı select elementinden “Düzeltme Genel”, “Fite
Kontör Yüklenecek”, “Stuck” ve “” (boş) olmak üzere 4 değer almaktadır.

İkinci kişisel bölümde; yapılacaklar tablosu yer almaktadır. Bu tablo konu,
durum ve timeline sütunlarından oluşmaktadır. Konu sütunu string türünde değer
alırken, durum sütunu “Bilgi Bekleniyor”, “Yapıldı”, “Acil Yapılacak” ve “”
(boş) olmak üzere 4 farklı değer almaktadır.

Son olarak üçüncü kişisel bölümde monitoring - azure - api izleme, marketing,
uygulama, web sitesi adlarında 4 tablo bulunmaktadır. Her bir tablo konu ve
durum sütunlarından oluşmaktadır. Konu sütunu string türünde değer alırken,
durum sütunu “Done”, “Working on it”, “Stuck”, “Must Start”, “” (boş) olmak
üzere 5 farklı değer almaktadır.

*Timeline:* Timeline özelliğine sahip her tablonun timeline sütunundaki
değerlere tıklandığında, web sayfasının en başında timeline bölümü
gösterilmektedir. Timeline bölümünün sahip olduğu özellikler:

-   Konunun başlangıç ve bitiş tarihleri atanmamışsa, o konu timeline’da konunun
    eklenme tarihinde gösterilmektedir.

-   Konunun sağ veya sol köşelerinden uzatılıp, kısaltılarak konunun başlangıç
    ve bitiş tarihleri ayarlanabilmektedir.

-   Konunun süresini değiştirmeden tarihler arasında taşınabilmektedir.

-   Konu kime atanmışsa, timeline üzerinde başka bir kişiye taşınamamaktadır.

-   Timeline üzerinde yapılan herhangi bir değişiklik, web sayfasını yenilemeye
    gerek kalmadan ilgili tablo satırı üzerinde değişiklik yapabilmektedir.

![](https://1.bp.blogspot.com/-tOqNsu0yZjA/XM7uq3U4qCI/AAAAAAAAC50/dMQsu3V4wmQSkyQU22AcfvBo1dEyn5nMwCLcBGAs/s1600/79a7c8bd137d070c7220055f41587edc.png)

Şekil 3.8. Timeline bölümünden bir ekran görüntüsü

*Konuşmalar:* Tüm bölümlerdeki tabloların tamamında, her konu sütunundaki
değerleri tıklandığında konuşmaların görüntülenebilip, eklenebildiği yeni bir
alan görüntülenmektedir. Konuşmalar resim, video, tablo, liste, farklı stillerde
metin gibi çeşitli türlerde veriler alabilmektedir.

Herhangi bir konuya yeni bir konuşma eklenirse, o konuşmaya katılan herkese yeni
konuşma eklendiğine dair bilgilendirme e-postası gönderilmektedir.

![](https://3.bp.blogspot.com/-CTAsPuxjADk/XM7un6zGFTI/AAAAAAAAC5Q/ZRMJJrtnASM8COsMCzhVPoXysXzv_1OtQCLcBGAs/s1600/36f44f1e414352008c85ae3071472b3b.png)

Şekil 3.9. Konuşmalar bölümünden bir ekran görüntüsü

1.  **Proje Takip Sisteminin Veri Tabanı Yapısı**

Rehberlik yapan bilgisayar mühendisi, geliştireceğim sistemin veri tabanının
sadece 3 tablodan oluşmasını istedi. Sistemde oluşturacağım 5 ayrı bölümün tablo
verilerini de tek bir tabloda tutmamı istedi. Tablolar arasında veri bütünlüğünü
sağlayan ilişkilerin bulunmamasını istedi. Neden olarak; sunucu ile veri tabanı
arasındaki veri işlemlerini LINQ kitaplığı ile yapacağım için, veri tabanından
talep edeceğim bir verinin eğer ki başka bir tablo ile ilişkisi varsa, o tabloda
ilişkili olduğu verileri de çağıracak olması ve bunun sisteme gereksiz bir yük
bindirecek olması şeklinde belirtti. Ayrıca hiçbir verinin silinmemesini, bu
verilerin arşiv olarak saklanmasını da istedi.

ASP.NET kimlik doğrulayıcısı, veri tabanında 5 adet tablo oluşturdu. Sistemi
geliştirirken bu 5 tablodan sadece, AspNetUsers tablosunda işlemler yaptım.

![](https://2.bp.blogspot.com/-2vduJJZC8cc/XM7uvKz8RdI/AAAAAAAAC6s/UQQgFE5xhKAaIqfy9JGuevzbH_7K5YaCwCLcBGAs/s1600/d71d878cdfb333792d2bf9b83fbd24c3.png)

Şekil 3.10. ASP.NET kimlik doğrulayıcısının veri tabanında oluşturduğu tablolar

Veri tabanında oluşturduğum tablolarda; HataTakipDiger adlı veri tabanı
tablosunda geliştirdiğim sistemin tablo isimlerini, tabloların sütun
başlıklarını, durum ve öncelik sütunlarının değerlerini ve tüm bunların arka
plan renklerini, HataTakipKonusmalar adlı veri tabanı tablosunda kullanıcıların
yaptığı konuşmalarla ilgili verileri, HataTakipMain adlı veri tabanı tablosunda
geliştirdiğim sistemdeki tabloların satır verilerini sakladım.

![](https://2.bp.blogspot.com/-z4zoK5jrTZY/XM7uqgaNZQI/AAAAAAAAC5w/EnsQz4nu1yMVA-GbwhV1QeXCRnriKpKVgCLcBGAs/s1600/6fe38b6d5aae4a687ee42f34b0dcce08.png)

Şekil 3.11. Proje takip sisteminin veri tabanı yapısı

ASP.NET kimlik doğrulayıcısının oluşturduğu AspNetUsers veri tabanı tablosuna
eklediğim Silindi niteliği ile sakladığı 0 ya da 1 değerine göre kullanıcının
sisteme erişme durumunu, Sayfa niteliği ile kullanıcının kişisel web sayfasını
tanımlamış oldum.

HataTakipDiger veri tabanı tablosuna eklediğim Etiket niteliği geliştirdiğim
sistemin tablo ismi, sütun başlıkları, durum ve öncelik değerlerini saklarken,
Tur niteliği verdiğim integer değerine göre verilerin tablo ismi, sütun başlığı,
durum değeri, öncelik değeri ayrımını yapmasını sağladım. Bunun yanında, hem bu
tabloda hem de HataTakipMain veri tabanı tablosunda Sayfa niteliği ile her
bölümün farklı farklı tablo isimleri, sütun başlıkları, durum değerleri
olmasından dolayı, bunların hangi sayfanın tablo verisi olduğu ayrımını yaptım.
Renk niteliğinde ise sistemdeki tablolarda görselliği arttırmak için verilerin
arka plan renklerini kaydettim.

Veri tabanı tablolarına eklediğim Silindi niteliği ile sistemi kullanan
kişilerin silmek istediği verileri veri tabanından silmek yerine, bu niteliğin
alacağı 0 ve 1 değerlerine göre verileri sistemde kullanıcının görmesini
engellemiş oldum.

1.  **Uygulama**

Projeyi geliştirmeye başlamadan önce gereksinimleri belirledikten sonra,
gereksinimleri nasıl yerine getireceğimi araştırdım.

Projeye gerçek zamanlı bir web sitesi haline getirebilmek için, Angular hakkında
araştırma yaptım. Angular’ın sadece istemci bazlı gerçek zamanlı uygulama
yaptığını öğrendim. Yani, sunucuya herhangi bir gerçek zamanlı uygulama özelliği
sağlamıyor. Bunu gerçekleştiren unsurun Node.js olduğunu öğrendim. Rehberlik
yapan bilgisayar mühendisine, sunucu tarafında Node.js kullanarak projeyi
geliştirebilir miyim diye sorduğumda olumsuz yanıt aldım. Bende araştırmalarıma
devam ettim. SignalR kitaplığının bunu gerçekleştirebileceğini öğrendim. SignalR
kitaplığını kullanarak, ASP.NET de sohbet uygulamalarının yapıldığı örneklerle
karşılaştım [5]. Bunları inceleyerek SignalR kitaplığının kullanımını öğrendim.

1.  **Proje takip sistemi projesinin oluşturulması**

Daha sonrasında projeye başlamaya karar verdim. Visual Studio da ASP.NET Web
Uygulaması (.NET Framework) olarak Web Forms ve Web API projesi oluşturdum.
ASP.NET in kendi kullanıcı giriş ve kayıt sistemini kullanabilmek için kimlik
doğrulamayı bireysel kullanıcı hesapları olarak seçtim.

![](https://3.bp.blogspot.com/-9dM_7V5If-M/XM7unAgl5RI/AAAAAAAAC5M/CeNrDGJtaFknrr8Ytk1-RvkKXrrdUFaEQCLcBGAs/s1600/33146df83cefd19a695316b031ebb240.png)

Şekil 3.12. Proje takip sistemi projesinin oluşturulması

SignalR ve LINQ kitaplıklarını kullanabilmek için, bu kitaplıkları Nuget paketi
olarak projeye dahil ettim.

ASP.NET in kimlik doğrulayıcısı için gerekli veri tabanı tablolarını
oluşturabilmesi için, öncelikle projeyi derledim ve web sayfasında ilk kullanıcı
kaydını yaptığımda kimlik doğrulayıcısı tarafından veri tabanı da dahil olmak
üzere veri tabanı tabloları oluşturuldu. Sunucu ile veri tabanı arasındaki
bağlantıyı sağlayan Web.config dosyasındaki connectionStrings elementi
oluşturuldu.

\<connectionStrings\>

>   \<add name="DefaultConnection" connectionString="Data
>   Source=(LocalDb)\\MSSQLLocalDB;AttachDbFilename=\&quot;\|DataDirectory\|\\aspnet-Proje
>   Takip-20180526053256.mdf\&quot;;Initial Catalog=\&quot;aspnet-Proje
>   Takip-20180526053256\&quot;;Integrated Security=True"
>   providerName="System.Data.SqlClient" /\>

\</connectionStrings\>

Microsoft SQL Server Management Studio ile sunucu adını (localdb)\\MSSQLLocalDB
girerek veri tabanlarını lokalde kontrol ettiğimde, gerçekten de veri tabanı ve
tablolarının oluşturulduğunu gördüm.

![](https://1.bp.blogspot.com/-1yL8ryp_3oo/XM7ulVQUaSI/AAAAAAAAC5A/KKrZ6yjzpeA3gXDuCX1TF0RBLitSRh_jgCLcBGAs/s1600/21c0417f439dbfc403678ba12ae507d8.png)

Şekil 3.13. ASP.NET kimlik doğrulayıcısının oluşturduğu veri tabanı ve tabloları

AspNetUsers veri tabanı tablosunun içeriğine baktığımda, web sayfasında
kaydettiğim kullanıcının veri tabanında da oluşturulduğunu gördüm. Sadece
AspNetUsers veri tabanı tablosunu kullanacağım.

![](https://4.bp.blogspot.com/-FXBpDZiA-dE/XM7ulx4lvoI/AAAAAAAAC5E/EVb_cmTN20E75sweXig9561gaYAvImU-ACLcBGAs/s1600/27465b6d87878a405ea6fdb42249a03e.png)

Şekil 3.14. İlk kullanıcı kaydından sonra AspNetUsers veri tabanı tablosunun
içeriği

AspNetUsers veri tabanı tablosuna Name, Surname, Silindi ve Sayfa niteliklerini
ekledim.

1.  **Proje takip sisteminin kullanıcı ara yüzü stili ve navigation bar**

Projenin web sayfalarının kullanıcı ara yüzü tasarımı için rehberlik yapan
bilgisayar mühendisi kendi projeleri için kullandıkları Metronic kitaplığını
bana verdi. Bu kitaplığı Scripts klasörüne ekledim. Metronic kitaplığını
incelediğimde, buradaki navigation bar, form bileşenlerini ve tablo stillerini
kullanmaya karar verdim.

![](https://2.bp.blogspot.com/-Azeg92KQVkQ/XM7uwFhfcKI/AAAAAAAAC64/_8oOEDXG-KIjOI55sGdvEY6CqQ_u8n4RwCLcBGAs/s1600/e063c7d67822a5959269973f72686757.png)

Şekil 3.15. Metronic kitaplığı kullanıcı ara yüzü bileşenlerinden ekran
görüntüsü

Sunucu ile veri tabanı arasındaki veri işlemlerini yönetmeyi kolaylaştıran LINQ
kitaplığının sınıfını Model klasöründe LINQ to SQL Classes olarak dbml (Database
Markup Language) türünde dosyayı oluşturdum. Visual Studio’nun Sunucu Gezgini
penceresinden sunucu olarak lokale bağlandım. Veri tabanından AspNetUsers veri
tabanı tablosunu dataset dosyama sürükle bırak şeklinde taşıdım.

Projeye, HataTakip adında Web Formu olarak bir aspx sayfası ekledim. Bu web
sayfasına Metronic kitaplığının navigation bar, JavaScript ve stil kodlarını
ekledim. Elementlerin daha iyi görünmesi için stillerini düzenledim. Ayrıca
içerikte filtreleme yapabilmek için kullanılacak bir input elementi de ekledim.

Kullanıcı adının sağ üstte görüntülenmesi için sınıf dosyasında:

protected string EkleyenIsmi;

protected void Page_Load(object sender, EventArgs e)

{

>   using (HataTakipDatasetDataContext context = new
>   HataTakipDatasetDataContext())

>   {

>   string Ekleyen = HttpContext.Current.User.Identity.Name.ToString();

>   EkleyenIsmi = context.AspNetUsers.Where(q =\> q.UserName ==
>   Ekleyen).First().Name;

>   }

>   }

HataTakip.aspx dosyasında EkleyenIsmi değerini alabilmek için:

...

\<span class="username username-hide-on-mobile"\>\<%=EkleyenIsmi %\>\</span\>

kodlarını yazdım.

![](https://3.bp.blogspot.com/-7iLNteGjSu0/XM7urRW-LzI/AAAAAAAAC58/34czriB5J_AFwrgAULlc_mHNfhshM6-JACLcBGAs/s1600/89000366db466139fdea10587f32b245.png)

Şekil 3.16. Proje takip sisteminin navigation bar yapısı

HataTakip sayfasının sınıfına, eğer bu sayfayı görüntülemeye çalışan kişi
kullanıcı girişi yapmamışsa, kullanıcı girişi sayfasına yönlendiren kodları
Page_Load metodunda ekledim.

if (!((HttpContext.Current.User != null) &&
HttpContext.Current.User.Identity.IsAuthenticated))

{

>   Response.Redirect("Account/Login.aspx");

}

Kullanıcının çıkış yapabilmesi için Çıkış Yap butonuna tıklayınca tetiklenecek
kod:

protected void CikisYap_Click(object sender, EventArgs e)

{

>   Context.GetOwinContext().Authentication.SignOut(DefaultAuthenticationTypes.ApplicationCookie);

>   Response.Redirect("Account/Login.aspx");

}

\<asp:LinkButton ID="CikisYap" runat="server" OnClick="CikisYap_Click"\>

>   \<i class="icon-key"\>\</i\>Çıkış Yap

\</asp:LinkButton\>

1.  **Proje takip sisteminin tablo yapısı**

Daha sonrasında, proje takip sistemi için HataTakipMain, HataTakipKonusmalar ve
HataTakipDiger adlarında proje için gerekli veri tabanı tablolarını oluşturdum.
Önceden belirttiğim gibi, HataTakipDiger veri tabanı tablosunun yapısına göre
verileri ekledim.

![](https://3.bp.blogspot.com/-VD5_05j9czE/XM7uoGgqhKI/AAAAAAAAC5U/mBovmlpTl5U3gNiM0Y_qo6GmoDuDKpuxgCLcBGAs/s1600/4bd5bdc231b2c7d1b83bafa950ebff3a.png)

Şekil 3.17. HataTakipDiger veri tabanı tablosunun yapısı

Id niteliğini primary key ve identity özelliğini “yes” yaptım. Toplamda 59 satır
veri eklemiş oldum.

![](https://4.bp.blogspot.com/-cpFE2MoXNas/XM7utDmJdyI/AAAAAAAAC6Q/88lGwkxX0u8JqvCbEKeLnETSI9lOHGATACLcBGAs/s1600/afaff4f4287fdc1361c61727b9c64c7d.png)

Şekil 3.18. HataTakipDiger veri tabanı tablosunun içeriği

Proje, Web API mantığı ile çalışacağı için istemci, sunucu aracılığıyla veri
tabanından veriyi alıp, bu verilerle tüm işlemler (tabloların oluşturulması,
filtreleme gibi) JavaScript ile istemcide gerçekleştirilecektir. Bu sayede
verinin sunucuda yorumlanmış halde gönderilmesi yerine JSON türünde alınan bu
ham veri istemcide yorumlanarak, sunucunun yükü istemciye aktarılmış olur. Bu
amaçla Scripts klasöründe, istemcide tabloları oluşturacağım Tablo ve
konuşmaları oluşturacağım Konusmalar adında iki adet JavaScript dosyası
oluşturdum.

1.  **Web Api mantığı ile verilerin gönderilmesi, Ajax ile elde edilmesi ve
    cache’lenmesi**

İstemcinin verileri alabilmesi için, Controllers klasöründe DataController
adında sınıf oluşturdum. Bu sınıfta tabloların oluşması için gereken tablo
bilgileri ve içeriği verilerinin GET metotlarını yazdım. Aynı zamanda kullanıcı
girişi yapılmadan bu verilerin clien’a gönderilmesini [Authorize] ile
engelledim. Tablo adlarının, sütun değerlerinin ve içeriklerin istemciye
gönderilmesi için aşağıdaki koda benzer şekilde kodladım:

[Authorize]

[AcceptVerbs("GET")]

public List\<HataTakipDiger\> \_htTable(int sayfa)

{

>   using (HataTakipDatasetDataContext context = new
>   HataTakipDatasetDataContext())

>   {

>   return context.HataTakipDiger.Where(q =\> q.Tur == 4 && (q.Sayfa == 0 \|\|
>   q.Sayfa == sayfa)).ToList();

>   }

}

Örneğin bu kodda, veri tabanı tablosunda Tur değeri 4 ve Sayfa değeri 0 veya
sayfa parametresinden gelen değere göre tablo isimlerini ve arka plan renklerini
LINQ kitaplığını kullanarak geriye döndüren bir metot olarak yazdım. LINQ
kitaplığının veri işlemleri için yazım şekli, SQL programlama diline benzediği
için kullanımında herhangi bir zorluk yaşamadım [7]. LINQ yazım şeklini lambda
ile kullanmayı tercih ettim.

Verilerin sunucuda cache’lenerek, veri tabanı yükünü hafifletmek için internette
yaptığım araştırmalar sonucunda [8], Strathweb.CacheOutput kitaplığı ile
cache’lenecek verilerin her bir API metodunun üzerine:

[CacheOutput(ServerTimeSpan = 3600)]

ekleyerek, 60 dakika boyunca verinin cache’lenmesini sağladım.

Projenin istemciye gönderilecek verilerle ilgili sunucu tarafında controller
metotlarını yazdıktan sonra, JavaSctipt dosyasında istemcide bu verileri
alabilecek Ajax metodunu yazdım.

const DOMAIN = "http://" + window.location.hostname + "/api/data/";

const URLLER = ["_htTable?sayfa=" + SAYFA, "_htColumn?sayfa=" + SAYFA,
"_htMain?sayfa=" + SAYFA, "_htOther?sayfa=" + SAYFA, "_htKullanici",
"_htKonusmalar"];

const LISTELER = ["listTable", "listColumn", "listMain", "listOther",
"listUser", "listKonusmalar"];

// Verileri al

function AjaxVerileriGetir(url, listeler, liste, asenkron = true) {

>   AjaxHatasi:

>   \$.ajax({

>   type: 'GET',

>   url: url,

>   dataType: 'json',

>   async: asenkron,

>   success: function (data) {

>   listeler[liste] = data;

>   },

>   error: function () {

>   continue AjaxHatasi;

>   }

>   });

}

Bu Ajax metodunda, Web API url adresleri ve liste parametre olarak verilip,
sunucunun gönderdiği veriler list yapsına dönüştürülüyor. Bu metot asenkron
olarak çalıştığı için web sayfasının yüklenmesinden bağımsız olarak
çalışmaktadır.

1.  **Tabloların oluşturulması**

Verileri istemcide elde ettikten sonra, bunları anlamlandırabilmek için tablonun
iskeletini oluşturan metodu yazdım:

// Tabloları oluştur

function TabloOlustur(listTable, listColumn, indeks, WIDTH) {

>   let kod = ""

>   \+ "\<div class='portlet box' style='background-color: "

>   \+ listTable[indeks].Renk + "'\>"

>   \+ "\<div class='portlet-title'\>"

>   \+ "\<div class='caption'\>"

>   \+ listTable[indeks].Etiket

>   \+ "\</div\>"

>   \+ "\</div\>"

>   \+ "\<div class='portlet-body' id='MyTable" + listTable[indeks].Id

>   \+ "'\>"

>   \+ "\<div class='table-scrollable' id='TablomUst"

>   \+ listTable[indeks].Id + "'\>"

>   \+ "\<table class='table table-bordered table-hover' style='width:100%'\>"

>   \+ "\<thead\>"

>   \+ "\<tr\>";

>   // Thead sütun düzeni

>   for (let j = 0; j \< Object.keys(listColumn).length; j++) {

>   if (WIDTH[j] !== "0%") {

>   kod += "\<th style='display:inline-block; width:"

>   \+ WIDTH[j] + "; overflow:hidden; white-space:nowrap; text-overflow:initial'
>   title='"

>   \+ listColumn[j].Etiket + "'\>"

>   \+ listColumn[j].Etiket + "\</th\>";

>   }

>   }

>   kod += "\</tr\>"

>   \+ "\</thead\>"

>   \+ "\<tbody id='MyTBody" + listTable[indeks].Id + "'\>"

>   \+ "\</tbody\>"

>   \+ "\</table\>"

>   \+ "\</div\>\</div\>\</div\>";

>   return kod;

}

![](https://2.bp.blogspot.com/-AEMWvSh61iY/XM7ut2dmIMI/AAAAAAAAC6c/ddg19Xv2JRwrc8acHFPAS6yto4qwh1CcACLcBGAs/s1600/c84a40996c33644a467dc82acae3daf2.png)

Şekil 3.19. Tabloların oluşturulması

Bu metot da, tabloları oluşturdum ve stillerini belirledim. Kuruluşun kullandığı
sisteme benzer şekilde sütunları oluşturdum. Bunun yanında, düzenleme ve silme
butonlarını koymak için fazladan iki sütun daha oluşturdum. Metodun döndürdüğü
stringi JQuery kitaplığını kullanarak \<div id="Tablolar"\>\</div\> elementinin
içerisine:

for (let i = 0; i \< Object.keys(listTable).length; i++) {

\$("\#Tablolar").append(TabloOlustur(listTable, listColumn, i, WIDTH));

}

kodlarını kullanarak dinamik olarak append ettim [7].

1.  **Tablo içeriklerinin oluşturulması**

Daha sonra, bu tabloların içerisini doldurmak için, veri tabanında HataTakipMain
veri tabanı tablosuna veri eklemem gerekti.

![](https://4.bp.blogspot.com/-RpYMXJQ7gwY/XM7uviPKREI/AAAAAAAAC60/GAT9KLpwv6MIK7zzZkDRJhEp2MPPsuIvACLcBGAs/s1600/d9d1c31093d8a0e6b33a4aef557ebff3.png)

Şekil 3.20. HataTakipMain veri tabanı tablosunun yapısı

Bunun için kuruluşun kullandığı monday.com web sitesine girip, buradan excel
dosyası olarak verileri elde ettim. Bu verileri veri tabanına ekledim.

![](https://4.bp.blogspot.com/-xPwB3iNMwrY/XM7up78tenI/AAAAAAAAC5o/CqGhw60o3VEShiw971fbnL4X16pmWRP4ACLcBGAs/s1600/6c174cb4f4be007acc8da08f245eef9f.png)

Şekil 3.21. HataTakipMain veri tabanı tablosunun içeriği

Son hali ile 183 satır koddan oluşan satır oluşturma metodunu yazdım. Bu metot
da string olarak döndürdüğü HTML kodunu tabloları oluşturan metodun oluşturduğu
\#MyTBody elementine append ettim:

for (let i = 0; i \< Object.keys(listMain).length; i++) {

>   \$("\#MyTBody" + listMain[i].TabloId).append(SatirOlustur(listMain,
>   listOther, listUser, i, WIDTHKEYS, SAYFA));

}

Konu sütununun içeriğinin tablo satırından kaymaması ve taşmaması amacıyla fazla
gelen kısmını gizlemek için şu stil yapısını kullandım [9]:

kod += "\<td style='"

>   ...

>   \+ "display:inline-block;"

>   \+ "overflow:hidden;"

>   \+ "text-overflow:ellipsis;"

>   \+ "white-space:nowrap;"

>   ...

>   \+ '\>

![](https://4.bp.blogspot.com/-ztVa23a5OA4/XM7uxtJTNKI/AAAAAAAAC7M/La1HdW24I-kmySnaxpedrxN1KynSyvl5wCLcBGAs/s1600/f8c56ac0ef048ead55e16a21e54f7c7d.png)

Şekil 3.22. Tablo içeriklerinin doldurulması

1.  **Filtreleme**

Tabloların içeriğini doldurduktan sonra, internette filtreleme ile ilgili
yaptığım araştırmalar sonucunda, bununla ilgili sorulan bir soruya verilmiş ilk
cevabı baz alarak [10], filtreleme metodunu yazdım:

// Tablo içeriği filtreleme

function Filtrele(event) {

>   let aranan = event.target.value.toLocaleLowerCase('tr-TR');

>   for (let i = 0; i \< Object.keys(listTable).length; i++) {

>   let tablolar = listTable[i];

>   let rows = document.querySelector("\#MyTBody"

>   \+ tablolar["Id"]).rows;

>   for (let j = 0; j \< rows.length; j++) {

>   let arananBulundu = false;

>   let obj1 = rows[j].cells;

>   for (let k = 0; k \< obj1.length; k++) {

>   if (obj1[k].textContent.toLocaleLowerCase('tr-TR').indexOf(aranan) \> -1) {

>   arananBulundu = true;

>   break;

>   }

>   }

>   if (arananBulundu) {

>   rows[j].style.display = "";

>   }

>   else {

>   rows[j].style.display = "none";

>   }

>   }

>   }

}

PageOnLoad metoduna filtreleme inputunun id’sini yazarak her tuşa basmayı
bıraktıktan sonra Filtrele metodunun çalışmasını sağladım:

document.querySelector('\#Text10').addEventListener('keyup', Filtrele, false);

Bu metot ile kullanıcı, tablonun her bir satırındaki içerik için filtreleme
yapabilir.

![](https://3.bp.blogspot.com/-sNrSNO4MPcE/XM7us10EaiI/AAAAAAAAC6M/DzyYvA0I_4oM6VCL6M-oSRA0tfXBx547wCLcBGAs/s1600/a969ba30791509b33ff6aa266e87da36.png)

Şekil 3.23. Proje takip sisteminin içerik filtreleme yapısı

1.  **Tabloların küçültülmesi**

Tabloların küçültülmesi için, tablo içeriklerinin gizlenmesini sağlayan JQuery
kitaplığının sağladığı efekti kullanarak kullanıcının proje takip sistemi
kullanım deneyimini arttırmayı amaçladım [11]:

function gizleGoster(element, id) {

>   \$("\#" + element).toggleClass('fa-arrow-down');

>   \$(id).slideToggle(200);

}

![](https://4.bp.blogspot.com/-6A7-CNT7RtE/XM7uuQVDU8I/AAAAAAAAC6g/BDXdajFV2DkTNDbPS5vLQX8CwD3qzkvfACLcBGAs/s1600/ca71277fe3984a369b2d0634944df00b.png)

Şekil 3.24. Tablo içeriklerinin gizlenmesi

1.  **SignalR kitaplığı ile tabloya veri eklenmesi, verilerin düzenlenmesi ve
    arşivlenmesi**

Daha sonra, tabloya veri eklemek ve mevcut veriyi düzenlemek için öncelikle,
aspx sayfasında bir form oluşturdum:

\<div id="formum"\>

>   \<div class="row"\>

>   \<div class="col-md-12"\>

>   \<span\>Konu :\</span\>

>   \<input type="text" class="form-control" placeholder="Konu" id="Text1" /\>

>   \</div\>

>   \</div\>

>   \<div class="row"\>

>   \<div class="col-md-3"\>

>   \<span\>Müşteri :\</span\>

>   \<input type="text" class="form-control" placeholder="Müşteri" id="Text2"
>   /\>

>   \</div\>

>   \<div class="col-md-2"\>

>   \<span\>Kimde :\</span\>

>   \<asp:DropDownList ID="DropDownList1" runat="server"
>   CssClass="form-control"\>\</asp:DropDownList\>

>   \</div\>

>   \<div class="col-md-2"\>

>   \<span\>Durum :\</span\>

>   \<asp:DropDownList ID="DropDownList2" runat="server"
>   CssClass="form-control"\>\</asp:DropDownList\>

>   \</div\>

>   \<div class="col-md-2"\>

>   \<span\>Öncelik :\</span\>

>   \<asp:DropDownList ID="DropDownList3" runat="server"
>   CssClass="form-control"\>\</asp:DropDownList\>

>   \</div\>

>   \<div class="col-md-2" id="TabloSec" style="display: none"\>

>   \<span\>Tablo :\</span\>

>   \<asp:DropDownList ID="DropDownList4" runat="server"
>   CssClass="form-control"\>\</asp:DropDownList\>

>   \</div\>

>   \<div class="col-md-1" style="margin-bottom: 35px;"\>

>   \<span style="color: \#fff"\>\&nbsp;\</span\>

>   \<input id="Button" type="button" value="Ekle" class="btn green"
>   style="width: 100%" name="" onclick="return Ekle()" /\>

>   \</div\>

>   \</div\>

>   \</div\>

![](https://4.bp.blogspot.com/-h1AKix8rLmc/XM7uw1UfoXI/AAAAAAAAC7E/6Ok0lJ9R0QQ9jZmIzYbI8yaumll6sf_qQCLcBGAs/s1600/ed78cf6fb13f4dc98355decce5ddacd0.png)

Şekil 3.25. Veri eklemek ve düzenlemek için kullanılan form yapısı

Bu form ile sunucuya gönderilecek veri HataTakipMain veri tabanı tablosunun
yapısına uygun olarak sunucuda düzenlenip, veri tabanına eklenecek. Projenin bu
kısmında SignalR kitaplığını kullanarak form verilerini alıp, veri tabanı
işlemlerinden sonra o anda proje takip sistemini kullanan kişilere değişen veya
eklenen verilerin gösteriminin sağlanması gerekiyor.

SignalR kitaplığını çalıştırabilmek için Startup.cs dosyasının Configuration
metoduna:

app.MapSignalR();

kodunu ekledim [12]. Projede Hubs adında klasör oluşturdum ve bunun içerisine
sunucu ile istemciler arasında iletişimi sağlayacak olan SignaR Hub Class olarak
HataTakipHub sınıf dosyasını ekledim. Models klasörüne veri tabanı işlemlerini
yapacağım HataTakipDatabase sınıf dosyasını ekledim.

JavaScript’te SignalR metodu tetiklenecek ve sunucuda ilgili metot
çalıştırılacak, sunucuda tetiklenen bu metot yine o an sistemi kullanan tüm
kullanıcıların JavaScript’te başka bir metodunu tetikleyecek ve son olarak
tetiklenen metot manipüle edilen verileri anlamlandırıp, tabloda değişiklik
yapacak. Bu şekilde proje takip sistemine gerçek zamanlı uygulama özelliğini
kazanacak. İstemci ile sunucu arasında gerçekleşen tüm bu işlemler WebSocket
protokolünün sağladığı haberleşme altyapısı ile gerçekleşiyor.

Sunucuda HataTakipDatabase sınıfına veri ekleme, düzenleme ve arşivleme ile
ilgili metotları yazdım:

public void VeriEkle(int tabloId, string konu, string musteri, string ekleyenId,
string kimdeId, int durumId, int oncelikId, DateTime eklenmeTarihi, int sayfa)

{

>   using (HataTakipDatasetDataContext context = new
>   HataTakipDatasetDataContext())

>   {

>   int kullaniciAktif = context.AspNetUsers.Where(q =\> q.Id ==
>   ekleyenId).Select(q =\> q.Silindi).First();

>   HataTakipMain hataTakip = new HataTakipMain()

>   {

>   TabloId = tabloId,

>   Konu = konu.Trim(),

>   Musteri = musteri.Trim(),

>   EkleyenId = ekleyenId,

>   KimdeId = kimdeId,

>   DurumId = durumId,

>   OncelikId = oncelikId,

>   EklenmeTarihi = eklenmeTarihi.ToString("dd.MM.yy HH:mm"),

>   Sayfa = sayfa

>   };

>   context.HataTakipMain.InsertOnSubmit(hataTakip);

>   context.SubmitChanges();

>   }

>   }

VeriEkle metodunda, parametre olarak gelen verileri LINQ kitaplığını kullanarak
veri tabanında eklenmesini sağladım.

public void VeriDuzenle(int id, int tabloId, string konu, string musteri, string
ekleyenId, string kimdeId, int durumId, int oncelikId, int sayfa)

{

>   using (HataTakipDatasetDataContext context = new
>   HataTakipDatasetDataContext())

>   {

>   int kullaniciAktif = context.AspNetUsers.Where(q =\> q.Id ==
>   ekleyenId).Select(q =\> q.Silindi).First();

>   context.HataTakipMain.Where(q =\> q.Id == id).ToList().ForEach(q =\>

>   {

>   q.TabloId = tabloId;

>   .Konu = konu.Trim();

>   q.Musteri = musteri.Trim();

>   q.KimdeId = kimdeId;

>   q.DurumId = durumId;

>   q.OncelikId = oncelikId;

>   });

>   context.SubmitChanges();

>   }

}

VeriDuzenle metodunda, parametre olarak gelen id ve verileri LINQ kitaplığını
kullanarak veri tabanında id değerine göre ilgili veri satırının güncellenmesini
sağladım.

public void Arsivle(string ekleyenId, int id)

{

>   using (HataTakipDatasetDataContext context = new
>   HataTakipDatasetDataContext())

>   {

>   int kullaniciAktif = context.AspNetUsers.Where(q =\> q.Id ==
>   ekleyenId).Select(q =\> q.Silindi).First();

>   context.HataTakipMain.Where(q =\> q.Id == id).ToList().ForEach(q =\>
>   q.Silindi = 1);

>   context.SubmitChanges();

>   context.HataTakipKonusmalar.Where(q =\> q.Id == id && q.EkleyenId ==
>   ekleyenId).ToList().ForEach(q =\> q.Silindi = 1);

>   context.SubmitChanges();

>   }

}

Arsivle metodunda, parametre olarak gelen id’yi LINQ kitaplığını kullanarak veri
tabanında id değerine göre ilgili veri satırının Silindi niteliğinin değerini 1
yaparak, kullanıcıların bu veri satırını artık görememesini sağladım.

Daha sonra, internette yaptığım araştırmalar sonucunda SignalR ile ilgili
örnekleri baz alarak [13], sunucuda HataTakipHub sınıfına veri ekleme, düzenle
ve arşivleme işlemlerini tetikleyecek olan metotları yazdım:

public void SendNotifications(string id, string tabloId, string konu, string
musteri, string ekleyenId, string kimdeId, string durumId, string oncelikId,
Boolean ekleniyor, string sayfa)

{

>   using (HataTakipDatasetDataContext context = new
>   HataTakipDatasetDataContext())

>   {

>   Boolean yeriDegisiyor = false;

>   DateTime tarih = DateTime.Now;

>   HataTakipDatabase db = new HataTakipDatabase();

>   List\<HataTakipMain\> sonListe;

>   if (ekleniyor)

>   {

>   db.VeriEkle(Convert.ToInt32(id), konu, musteri, ekleyenId, kimdeId,
>   Convert.ToInt32(durumId), Convert.ToInt32(oncelikId), tarih,
>   Convert.ToInt32(sayfa));

>   sonListe = context.HataTakipMain.OrderByDescending(q =\> q.Id).ToList();

>   Clients.All.receiveEkle(sonListe, ekleniyor, yeriDegisiyor);

>   }

>   else

>   {

>   string enSonYeri = context.HataTakipMain.Where(q =\> q.Id ==
>   Convert.ToInt32(id)).First().TabloId.ToString();

>   if (enSonYeri != tabloId)

>   {

>   yeriDegisiyor = true;

>   }

>   db.VeriDuzenle(Convert.ToInt32(id), Convert.ToInt32(tabloId), konu, musteri,
>   ekleyenId, kimdeId, Convert.ToInt32(durumId), Convert.ToInt32(oncelikId),
>   Convert.ToInt32(sayfa));

>   using (HataTakipDatasetDataContext contextGuncel = new
>   HataTakipDatasetDataContext())

>   {

>   sonListe = contextGuncel.HataTakipMain.Where(q =\> q.Id ==
>   Convert.ToInt32(id)).ToList();

>   Clients.All.receiveEkle(sonListe, ekleniyor, yeriDegisiyor);

>   }

>   }

>   }

}

SendNotifications metodu JavaScript ile tetiklenecek ve bu metot, istemciden
aldığı verilere göre VeriEkle veya VeriDuzenle metotlarını çalıştırarak, veri
tabanı işlemleri gerçekleştirilecek. Daha sonrasında:

Clients.All.receiveEkle(sonListe, ekleniyor, yeriDegisiyor);

metodu çalıştırılarak, o anda sistemi kullanan tüm kullanıcılarda JavaScript
dosyasında aynı isimde proxy.client.receiveEkle metodu çalıştırılacak:

let proxy = \$.connection.hataTakipHub;

// Eklenen - Güncellenen Kaydı Herkeste Göster

proxy.client.receiveEkle = function (listSonEklenen, eklendiMi, yeriDegistiMi) {

>   let kod = SatirOlustur(listSonEklenen, listOther, listUser, 0, WIDTHKEYS,
>   SAYFA);

>   let obj1 = listSonEklenen[0];

>   // Ekle

>   if (eklendiMi) {

>   \$(kod).prependTo("\#MyTBody" + obj1["TabloId"]);

>   }

>   // Güncelle

>   else {

>   // Tablosu değiştiyse

>   if (yeriDegistiMi) {

>   document.getElementById("MyRow-"

>   \+ listSonEklenen[0].Id).remove();

>   let degistirildi = false;

>   let rows = document.querySelector("\#MyTBody"

>   \+ obj1["TabloId"]).rows;

>   if (rows.length !== 0) {

>   for (let i = 0; i \< rows.length; i++) {

>   let ara = rows[i].outerHTML.match(/MyRow-\\d+/g);

>   let id = parseInt(ara[0].replace("MyRow-", ""));

>   if (id \< parseInt(listSonEklenen[0].Id)) {

>   \$("\#MyTBody"

>   \+ obj1["TabloId"] + " tr:eq("

>   \+ i + ")").before(kod);

>   }

>   degistirildi = true;

>   break;

>   }

>   }

>   if (!degistirildi) {

>   \$("\#MyTBody" + obj1["TabloId"]

>   \+ " tr:eq(" + (rows.length - 1)

>   \+ ")").after(kod);

>   }

>   } else {

>   \$(kod).prependTo("\#MyTBody" + obj1["TabloId"]);

>   }

>   }

>   // Tablosu değişmediyse

>   else {

>   \$("\#MyRow-" + listSonEklenen[0].Id).replaceWith(kod);

>   }

>   konuEkleniyor = true;

};

Herkeste çalıştırılan bu metot ile manipüle edilen veriyi kullanarak, eğer
tabloya ekleme yapılmışsa ilgili tablonun en üstüne HTML tablo satırı append
ediliyor, eğer düzenleme yapılmışsa ilgili tablo satırı silinip, tablodaki yeri
değişmeden yeni tablo satırı ekleniyor.

Form da Ekle veya Düzenle butonuna tıklayınca tüm bu işlemlerin gerçekleşmesi
için JavaScript’te Ekle() metodunu yazdım.

function Ekle() {

>   let Id;

>   let Mesaj;

>   if (konuEkleniyor) {

>   Id = document.getElementById("Button").name;

>   Mesaj = 'Kayıt eklensin mi?';

>   } else {

>   Id = guncellenenId;

>   Mesaj = 'Kayıt güncellensin mi?';

>   }

>   if (confirm(Mesaj)) {

>   let proxy = \$.connection.hataTakipHub;

>   proxy.server.sendNotifications(

>   Id,

>   GetValue("MainContent_DropDownList4", 1),

>   GetValue("Text1"),

>   GetValue("Text2"),

>   KULLANICIID,

>   GetValue("MainContent_DropDownList1"),

>   GetValue("MainContent_DropDownList2", 1),

>   GetValue("MainContent_DropDownList3", 1),

>   konuEkleniyor,

>   SAYFA);

>   \$.connection.hub.start();

>   \$("\#" + sonTiklanan).toggleClass('fa-minus');

>   \$("\#formum").slideToggle(200);

>   }

}

Kullanıcının veri silmek istediğinde sunucuda çalıştırılacak metotlar:

public void GeriBildirimSil(string hataTakipId, string ekleyenId, Boolean
konuMu)

{

>   using (HataTakipDatasetDataContext context = new
>   HataTakipDatasetDataContext())

>   {

>   HataTakipDatabase db = new HataTakipDatabase();

>   db.Arsivle(ekleyenId, Convert.ToInt32(hataTakipId), konuMu);

>   }

>   if (konuMu)

>   {

>   Clients.All.receiveSil(hataTakipId, konuMu);

>   }

>   else

>   {

>   using (HataTakipDatasetDataContext context = new
>   HataTakipDatasetDataContext())

>   {

>   Clients.All.receiveSil(hataTakipId, konuMu,
>   context.HataTakipKonusmalar.Where(q =\> q.Id ==
>   Convert.ToInt32(hataTakipId)).Select(q =\> q.HataTakipId));

>   }

>   }

}

Veri silme işleminin herkeste çalıştırılacak JavaScript kodunu yazdım:

proxy.client.receiveSil = function (Id, konuMu, HTId) {

>   if (konuMu) {

>   document.getElementById("MyRow-" + Id).remove();

>   } else {

>   \$("\#KonusmalarEdit").hide();

>   \$("\#KonusmalarEdit").prependTo("body");

>   let sayac = parseInt(\$("\#KonusmaSayac-"

>   \+ HTId).text()) - 1;

>   if (sayac === 0) {

>   document.getElementById("Konusma-"

>   \+ HTId).style.opacity = "0.3";

>   }

>   \$("\#KonusmaSayac-" + HTId).text(sayac);

>   try {

>   document.getElementById("MyConversation-"

>   \+ Id).remove();

>   } catch (e) { }

>   }

>   konuEkleniyor = true;

>   konusmaEkleniyor = true;

};

Sil butonuna tıklayınca, SignalR metodunu tetikleyecek metot:

function Arsivle(Id, konuMu = true) {

>   if (confirm('Kayıt silinsin mi?')) {

>   let proxy = \$.connection.hataTakipHub;

>   proxy.server.geriBildirimSil(document.getElementById(Id).name, KULLANICIID,
>   konuMu);

>   \$.connection.hub.start();

>   }

}

Bu şekilde, proje takip sisteminin veri ekleme, düzenleme ve silme işlemlerini
projeye gerçek zamanlı uygulama özelliği kazandırarak gerçekleştirmiş oldum.

1.  **Proje takip sistemi konuşmalarının yapısı**

Daha sonra, konular içerisinde konuşmaların yapıldığı yapıya geçtim.

![](https://4.bp.blogspot.com/-KEa9OqzfYcw/XM7uyC3qkiI/AAAAAAAAC7Q/3iO6hDHc5V07XPSxFSfKBCbf_YIw2bkHACLcBGAs/s1600/fc55229dcf038a55c60fab47db5ad1ba.png)

Şekil 3.26. HataTakipKonusmalar veri tabanı tablosunun yapısı

HataTakip aspx sayfasında kullanıcıların konuşmaların görüntüleyebileceği div
elementi alanı ve konuşmaların yapısını oluşturdum. Konuşmalar için
kullanıcıların esnek bir yazım sağlaması, resim, video, tablo, çeşitli stillerde
yazı ekleyebilmesi için CKEditor kitaplığını kullandım. Kullanımı için CKEditor
dokümantasyonlarını inceledim [14].

\<div id="KonusmalarIcerik"\>

>   \<div\>\<h4\>\<a href="javascript:KonusmalarGizleGoster(true)"
>   id="KonusmalarKapat"\>\<span class="glyphicon
>   glyphicon-remove"\>\</span\>\</a\>

>   \<span id="KonusmalarBaslik"\>\</span\>\</h4\>

>   \</div\>

>   \<div id="KonusmalarCKEditorButton"\>

>   \<input type="text" class="form-control" placeholder="Güncelleme yaz..."
>   id="TextCKEditor"/\>

>   \</div\>

>   \<div id="KonusmalarCKEditor" style="display: none"\>

>   \<div class="row" style="margin-bottom: 10px"\>

>   \<div class="col-md-4"\>

>   \<input id="ButtonCKEditorKapat" type="button" value="Kapat" class="btn
>   green" style="width: 100%" name="" onclick="return
>   CKEditorGosterGizle(true)"/\>

>   \</div\>

>   \</div\>

>   \<textarea name="editor1" id="editor1"\>\</textarea\>

>   \<script\>

>   CKEDITOR.replace('editor1', {

>   language: 'tr',

>   });

>   \</script\>

>   \<div class="col-md-12" style="margin: 10px 0 10px 0;" id="KonusmalarAlt"\>

>   \<div class="col-md-6"\>

>   \<span\>\&nbsp;\</span\>

>   \<input id="ButtonKonusmalar" type="button" value="Ekle" class="btn green"
>   style="width: 100%" name="" onclick="return KonusmaEkle()"/\>

>   \</div\>

>   \</div\>

>   \</div\>

>   \<div id="KonusmalarContainer"\>\</div\>

\</div\>

![](https://4.bp.blogspot.com/-FUaMfB5JFQU/XM7uksjkIHI/AAAAAAAAC44/yowSd_5wDSElXTcSFAhyqyhGfkaNAfgFQCLcBGAs/s1600/1247320e523fd4edd8886bd84c80eb69.png)

Şekil 3.27. Proje takip sisteminin konuşmalar sistemi

Konuya özel yapılmış konuşmaların verisini alabilmek için DataController
sınıfında şu metodu yazdım:

[Authorize]

[AcceptVerbs("GET")]

public List\<HataTakipKonusmalar\> \_htKonusmalar(int id)

{

>   using (HataTakipDatasetDataContext context = new
>   HataTakipDatasetDataContext())

>   {

>   return context.HataTakipKonusmalar.Where(q =\> q.HataTakipId == id &&
>   q.Silindi == 0).OrderByDescending(q =\> q.Id).ToList();

>   }

}

id parametresine konu değerini verip, o konuda yapılmış konuşmaları elde ettim.
İstemciye gelen konuşmalar verisini yorumlanarak, konuşmaların gösterilmesini
sağlamak için Konusmalar JavaScript dosyasında HTML stringini döndürecek şu
metodu yazdım:

function KonusmaSatirEkle(listKonusma, indeks) {

>   let kod = "";

>   let objKonusma = listKonusma[indeks];

>   let IsimSoyisim = "";

>   let DurumEtiket = "";

>   let DurumRenk = "";

>   for (let i = 0; i \< listOther.length; i++) {

>   let objDurum = listOther[i];

>   if (objDurum.Id === objKonusma.DurumId) {

>   DurumEtiket = objDurum.Etiket;

>   DurumRenk = objDurum.Renk;

>   break;

>   }

>   }

>   // Kullanıcı veritabanında kayıtlıysa, yaptığı konuşmaları göster

>   for (let i = 0; i \< listUser.length; i++) {

>   let objKullanici = listUser[i];

>   if (objKullanici.Id === objKonusma.EkleyenId) {

>   IsimSoyisim = objKullanici.Isim + " " + objKullanici.Soyisim;

>   kod += "\<div class='row' id='MyConversation-" + objKonusma.Id + "'\>\<div
>   class='KonusmaTumu' style='padding:10px 20px;'\>"

>   \+ "\<div class='col-md-12' style='padding:5px; border:1px solid \#ababab;
>   border-radius:5px'\>"

>   \+ "\<div class='row'\>\<div class='col-md-12' id='KonusmaEditle-" +
>   objKonusma.Id + "'\>";

>   // Giriş yapan kullanıcının, yaptığı konuşmaları düzenleyebildiği layout

>   if (KULLANICIID === objKonusma.EkleyenId) {

>   kod += "\<a href='javascript:KonusmalarMenuGizleGoster(" + objKonusma.Id +
>   ")' style='color:\#444; float:right; margin-left:5px; border:1px solid
>   \#c4c4c4; border-radius:50%; padding:0 5px;'\>"

>   \+ "\<i class='fa fa-sort-desc' style='position:relative;
>   top:-3px'\>\</i\>\</a\>";

>   }

>   kod += "\<span style='color:\#ababab; float:right'\>"

>   \+ objKonusma.EklenmeTarihi + "\</span\>"

>   \+ "\<h5 style='float:left'\>\<a style='color:\#0287C0'\>"

>   \+ escapeHtml(IsimSoyisim) + "\</a\>\</h5\>"

>   \+ "\</div\>\</div\>"

>   \+ "\<div class='row'\>\<div class='col-md-12'\>\<div
>   class='KonusmalarMesaj' style='text-align:justify; width:95%;
>   word-break:break-all; overflow:hidden;'\>"

>   \+ objKonusma.Mesaj + "\</div\>\</div\>\</div\>";

>   if (DurumEtiket !== "") {

>   kod += "\<div class='row'\>\<div style='float:right; margin-right:20px;
>   background-color:" + DurumRenk + "; padding:5px 10px; border:1px solid " +
>   DurumRenk + "; border-radius:5px; color:\#fff;'\>" + DurumEtiket +
>   "\</div\>\</div\>";

>   }

>   kod += "\</div\>\</div\>\</div\>";

>   break;

>   }

>   }

>   return kod;

}

Bu metot da, Web API ile sunucudan aldığım konuşma verilerini, konuşmayı yapan
kişinin ad ve soyadını, konuşma içeriğini, varsa durum bilgisini bir arada
göstererek, \$('\#KonusmalarContainer').html(kod); kodu ile konuşmaların
gösterildiği div elementinin içerisine gönderdim.

Konuşmalar sisteminde de SignalR kitaplığını kullandım. Böylece projenin bu
kısmına da gerçek zamanlı uygulama özelliğini kazandırdım.

Bu kısımda da tablolara veri ekleyip, arşivlemek ve düzenlemek için kullandığım
metotlara benzer şekilde, metotlar yazarak konuşma ekleme, arşivleme ve
düzenleme işlemlerini gerçekleştirdim. Konuşma düzenleme ve arşivleme
işlemlerini gizlenebilen div elementinde ki li elementlerine tıklanarak çağrılan
metotlarla gerçekleştirdim.

![](https://1.bp.blogspot.com/--8e7UMLfe8g/XM7ur9NHsQI/AAAAAAAAC6A/ESxkRX70dzUVkWQPQaBHXc3sdK9oU4b3gCLcBGAs/s1600/9a2ec5f0b31b1148c7372670d0a3e88a.png)

Şekil 3.28. Proje takip sisteminde eklenen bir konuşmanın ekran görüntüsü

Konuşma bölümünü jQuery kitaplığının sağladığı animasyon özelliklerini
kullanarak, bu bölümü gösterilip gizlenebilir hale getirdim [15].

function KonusmalarGizleGoster(kapat = false) {

>   if (document.getElementById("KonusmalarLayout").style.display === "none") {

>   \$("\#KonusmalarLayout").animate({ width: 'toggle' }, 500);

>   \$("\#KonusmalarIcerik").css({ width: 500 });

>   } else {

>   CKEditorGosterGizle(true);

>   }

>   if (kapat) {

>   \$("\#KonusmalarLayout").animate({ width: 'toggle' }, 500);

>   CKEditorGosterGizle(true);

>   }

}

Tabloların konu sütununda her bir konunun en sağında, o konuda yapılan
konuşmaların sayısını gösteren bir div elementi oluşturdum. Bu konuşmaların
sayısını alabilmek için DataController sınıfında şu metodu yazdım:

[Authorize]

[AcceptVerbs("GET")]

public List\<KonusmaSayisi\> \_htKonusmalar()

{

>   using (HataTakipDatasetDataContext context = new
>   HataTakipDatasetDataContext())

>   {

>   var konusmaSayilari = from q in context.HataTakipKonusmalar

>   where q.Silindi == 0

>   group q by q.HataTakipId into qGroup

>   select new KonusmaSayisi

>   {

>   Id = qGroup.Key,

>   Count = qGroup.Count(),

>   };

>   return new List\<KonusmaSayisi\>(konusmaSayilari);

>   }

}

![](https://2.bp.blogspot.com/-U7WrBXNAuoU/XM7utcepIZI/AAAAAAAAC6U/gN-yzu8SEj4UWegVPh-UEtJ_X5_E9juKgCLcBGAs/s1600/b2b6f876a6251f41580224a9288916b7.png)

Şekil 3.29. Konuşma sayılarının tablonun konu satırında gösterilmesi

Konuşma sayısı verilerini tablo satırlarını oluşturduğum metotta kullanarak, her
bir konuda yapılmış konuşmaların sayısını gösterdim.

1.  **Proje takip sisteminin kişisel web sayfaları**

Tablo oluşturmak, içeriklerini doldurmak, tablo verilerini eklemek, düzenlemek
ve arşivlemek, konuşmaların gösterilmesi, konuşma eklemek, düzenlemek ve
arşivlemek için kullandığım metotları değiştirmeden, sadece bu metotların aldığı
parametreler üzerinde değişiklikler yaparak proje takip sisteminin diğer kişisel
web sayfalarını oluşturdum.

![3_41](https://3.bp.blogspot.com/-rxtxKFEz3sU/XM7uxT87y2I/AAAAAAAAC7I/917nvaKfTlcUncns9HVaLMLxJbZR4QcDgCLcBGAs/s1600/f29dceebfc09071e0206d58f82419466.png)

Şekil 3.30. Proje takip sisteminin 1. kişisel web sayfasından ekran görüntüsü

![3_42](https://3.bp.blogspot.com/-fFwYqE3C2v0/XM7uk92hGSI/AAAAAAAAC48/FRp2r1m-uiEFcbjSL32_tC5OIEBVpWqwgCLcBGAs/s1600/0ad57ff1205cec1f9bfa05774c4865fe.png)

Şekil 3.31. Proje takip sisteminin 2. kişisel web sayfasından ekran görüntüsü

![3_43](https://1.bp.blogspot.com/-vL7PydET8Gw/XM7uo9JnU3I/AAAAAAAAC5c/wsOTv40n9mELKbYOP_6ytxMu36L8mRICACLcBGAs/s1600/5a41665d35348997e713f23f6aae65c3.png)

Şekil 3.32. Proje takip sisteminin 3. kişisel web sayfasından ekran görüntüsü

1.  **Proje takip sisteminin timeline yapısı**

Diğer web sayfaların içeriğini de benzer şekilde oluşturduktan sonra, DevOps
sayfasına timeline eklemek için internette araştırma yaptım. Araştırmalarım
sonucunda işime yarayabilecek her bir timeline kitaplığının ücret karşılığında
satıldığını gördüm. En uygun kitaplık olarak DayPilot’un iki ay boyunca ücretsiz
kullanmayı sağlayan JavaScript kitaplığını tercih ettim. Bu kitaplığı
kullanabilmek için DayPilot Scheduler dokümantasyonlarını inceledim [16].

![](https://3.bp.blogspot.com/-bMfgxbhB9-k/XM7upph-XiI/AAAAAAAAC5k/MudpI5OyQroTyOFoklkJY27Q_VU3aQzyACLcBGAs/s1600/646598c06ac15472dcaeacd19f36e9df.png)

Şekil 3.33. DevOps web sayfasından bir ekran görüntüsü

Timeline için Timeline adında yeni bir JavaScript dosyası oluşturdum.
Oluşturacağım timeline web sayfasının en üstünde gösterilecek. Kullanıcılar
timeline’ı kullanarak verilen görevin başlangıç ve bitiş tarihlerini
belirleyebilecek, böylece kaç gün süreceğini görebilecek ve geçmişe dönük
yapılan işleri takvim üzerinde görebilecek.

Timeline’da da SignalR kitaplığını kullandım. Böylece timeline’a da gerçek
zamanlı uygulama özelliği kazandırdım.

DayPilot Scheduler dokümantasyonlarını inceledim ve DevOps web sayfasında
timeline oluşturacak şu metodu yazdım:

let dp;

function Timeline(id, listUser, listMain, listTable, herkesiGoster = true) {

>   let simdikiZaman = new Date();

>   dp = new DayPilot.Scheduler(id);

>   dp.startDate = "2017-11-01";

>   dp.days = 1095;

>   dp.scale = "Day";

>   dp.locale = "tr-tr";

>   dp.timeHeaders = [

>   { groupBy: "Month", format: "MMMM yyyy" },

>   { groupBy: "Cell", format: "d" }

>   ];

>   dp.eventHeight = 35;

>   // Kişileri oluştur

>   let baslikListesi = [];

>   for (let i = 0; i \< listUser.length; i++) {

>   if (herkesiGoster == false) {

>   if (listUser[i].Sayfa == SAYFA) {

>   baslikListesi.push({ name: (listUser[i].Isim + " " +
>   listUser[i].Soyisim).trim(), id: listUser[i].Id });

>   break;

>   }

>   } else {

>   baslikListesi.push({ name: (listUser[i].Isim + " " +
>   listUser[i].Soyisim).trim(), id: listUser[i].Id });

>   }

>   }

>   dp.resources = baslikListesi;

>   dp.heightSpec = "Max";

>   dp.height = 500;

>   // Olayları oluştur

>   dp.events.list = []; // Olay listesi

>   for (let i = 0; i \< listMain.length; i++) {

>   dp.events.list.push(OlayOlustur(listMain, i));

>   }

>   dp.eventMovingStartEndEnabled = true;

>   dp.eventResizingStartEndEnabled = true;

>   dp.timeRangeSelectingStartEndEnabled = true;

>   // Herhangi bir olayı diğer kişilere taşımayı engelle

>   dp.onEventMoving = function (args) {

>   for (let i = 0; i \< listUser.length; i++) {

>   for (let j = 0; j \< listUser.length; j++) {

>   if (i !== j) {

>   if (args.e.resource() === listUser[i].Id && args.resource ===
>   listUser[j].Id) {

>   args.left.enabled = false;

>   args.allowed = false;

>   }

>   }

>   }

>   }

>   };

>   // Olayı günler arasında taşıma

>   dp.onEventMoved = function (args) {

>   TarihleriGuncelle(args);

>   };

>   // Olay başlangıç - bitiş günlerini değiştirme

>   dp.onEventResized = function (args) {

>   TarihleriGuncelle(args);

>   };

>   dp.init();

}

Timeline’da olayları oluşturabilmek için şu metodu yazdım:

function OlayOlustur(listMain, indeks) {

>   let tarihler = listMain[indeks].Musteri.split("-");

>   let baslangicTarihi = tarihler[0].split(".");

>   let basTGun = baslangicTarihi[0];

>   let basTAy = baslangicTarihi[1];

>   let basTYil = baslangicTarihi[2].split(" ")[0];

>   let bitisTarihi = tarihler[1].split(".");

>   let bitTGun = bitisTarihi[0];

>   let bitTAy = bitisTarihi[1];

>   let bitTYil = bitisTarihi[2].split(" ")[0];

>   let renk = BarColor(listMain[indeks].TabloId);

>   let tarih = new Date("20" + bitTYil + "-" + bitTAy + "-" + bitTGun +
>   "T10:00:00");

>   tarih.setDate(tarih.getDate() - 1);

>   return {

>   start: new DayPilot.Date("20" + basTYil + "-" + basTAy + "-" + basTGun +
>   "T00:00:00"),

>   end: new DayPilot.Date("20" + bitTYil + "-" + bitTAy + "-" + bitTGun +
>   "T00:00:00"),

>   id: listMain[indeks].Id,

>   resource: listMain[indeks].KimdeId,

>   text: listMain[indeks].Konu,

>   bubbleHtml: listMain[indeks].Konu,

>   barColor: renk,

>   barBackColor: renk

>   };

}

Kullanıcı timeline üzerinde her hangi bir olayın zamanını değiştirir veya olayı
taşırsa SignalR çalıştırılarak, o anda proje takip sistemini kullanan diğer
kullanıcıların da veri değişikliğini görüntüleyebilmesi için tetiklenecek şu
metodu yazdım:

// Olayı taşıma ya da gününü değiştirme sonucunda çağırılan hub

function TarihleriGuncelle(args) {

>   let id = args.e.data.id;

>   let baslangicTarihi = args.e.data.start.value.split("T")[0].split("-");

>   let bitisTarihi = args.e.data.end.value.split("T")[0].split("-");

>   let basTYil = baslangicTarihi[0].slice(2);

>   let basTAy = baslangicTarihi[1];

>   let basTGun = baslangicTarihi[2];

>   let bitTYil = bitisTarihi[0];

>   let bitTAy = bitisTarihi[1];

>   let bitTGun = bitisTarihi[2];

>   let proxy = \$.connection.hataTakipHub;

>   proxy.server.timelineTarihGuncelle(

>   id,

>   KULLANICIID,

>   basTGun + "." + basTAy + "." + basTYil + " 00:00",

>   bitTGun + "." + bitTAy + "." + bitTYil.slice(2) + " 00:00");

>   \$.connection.hub.start();

}

Bu değişikliklerin veri tabanında da etkili olabilmesi için HataTakipHub
sınıfında şu metodu yazdım:

public void TimelineTarihGuncelle(string id, string ekleyenId, string
baslangicTarihi, string bitisTarihi)

{

>   HataTakipDatabase db = new HataTakipDatabase();

>   db.TimelineTarihGuncelle(ekleyenId, Convert.ToInt32(id), baslangicTarihi,
>   bitisTarihi);

>   using (HataTakipDatasetDataContext context = new
>   HataTakipDatasetDataContext())

>   {

>   List\<HataTakipMain\> sonListe = context.HataTakipMain.Where(q =\> q.Id ==
>   Convert.ToInt32(id)).ToList();

>   Clients.All.receiveEkle(sonListe, false, false);

>   }

}

![](https://4.bp.blogspot.com/-SRPxhpNk8Zw/XM7uwKLOe2I/AAAAAAAAC68/RsSwwbz5vn08nOha4ACC1oacLnIH5PCZwCLcBGAs/s1600/e68a3450e3aecf26b20c6ddf8ea87498.png)

Şekil 3.34. Proje takip sisteminin timeline yapısı

Görevi atanmamış konular ise, kullanıcısı olmayan alanda gösterilmektedir.
Timeline başlangıç ve bitiş tarihlerini HataTakipMain veri tabanı tablosunun
Musteri niteliğinde sakladım.

![](https://4.bp.blogspot.com/-_wBraPEQnlk/XM7usosjlWI/AAAAAAAAC6I/S0az1pr5be8KW4zxmXsxe8rpBJ5ItlCtwCLcBGAs/s1600/9bbb50cbfc697aae186967c8c4b78a00.png)

Şekil 3.35. HataTakipMain veri tabanı tablosunun timeline için Musteri
niteliğinin içeriği

1.  **Proje takip sisteminin e-posta gönderme sistemi**

En son olarak görev atanan kişilere ve herhangi bir konuda konuşması bulunup o
konuya yeni bir konuşma eklendiğinde diğer kişilere e-posta gönderilecek metodu
yazdım [17]:

private void EmailGonder(string aliciAdi, string alici, string ekleyenId, string
konu, int tur)

{

>   string ekleyen = "";

>   string ekleyenEmail = "";

>   using (HataTakipDatasetDataContext context = new
>   HataTakipDatasetDataContext())

>   {

>   ekleyen += context.AspNetUsers.Where(q =\> q.Id == ekleyenId).Select(q =\>
>   q.Name).First() + " ";

>   ekleyen += context.AspNetUsers.Where(q =\> q.Id == ekleyenId).Select(q =\>
>   q.Surname).First();

>   ekleyenEmail += context.AspNetUsers.Where(q =\> q.Id == ekleyenId).Select(q
>   =\> q.Email).First();

>   }

>   if (alici != "" && alici != ekleyenEmail)

>   {

>   string emailSmtp = "smtp.yandex.com.tr";

>   int emailSmtpPort = 587;

>   string email = "noreply\@sanpark.com";

>   string emailSifre = "ŞİFRE";

>   string sablonBasi = "\<div\>\<table cellpadding='0' cellspacing='0'
>   border='0' align='center' bgcolor='\#f9f9f9'
>   style='background-color:\#f9f9f9; border-collapse:collapse;
>   line-height:100%; margin:0; padding:0;
>   width:100%'\>\<tbody\>\<tr\>\<td\>\<table style='border-collapse:collapse;
>   margin:auto; max-width:635px; min-width:320px;
>   width:100%'\>\<tbody\>\<tr\>\<td valign='top'\>\<table cellpadding='0'
>   cellspacing='0' border='0' style='border-collapse:collapse; color:\#c0c0c0;
>   font-size:13px; line-height:26px; margin:0 auto 26px;
>   width:100%'\>\</table\>\</td\>\</tr\>\<tr\>\<td valign='top' style='padding:
>   0 20px'\>"

>   \+ "\<table cellpadding='0' cellspacing='0' border='0' align='center'
>   style='border-radius:3px; background-clip:padding-box;
>   border-collapse:collapse; color:\#545454; font-size:13px; line-height:20px;
>   margin:0 auto; width:100%'\>"

>   \+ "\<tbody\>\<tr\>\<td valign='top'\>\<table cellpadding='0'
>   cellspacing='0' border='0' style='border:none; border-collapse:separate;
>   font-size:1px; height:2px; line-height:3px; width:100%'\>"

>   \+ "\<tbody\>\<tr\>\<td valign='top' bgcolor='\#0071b2'
>   style='background-color:\#0071b2; border:none;
>   width:100%'\>\&nbsp;\</td\>\</tr\>\</tbody\>\</table\>"

>   \+ "\<table cellpadding='0' cellspacing='0' border='0'
>   style='background-clip:padding-box; border-collapse:collapse;
>   border-color:\#dddddd; border-radius:0 0 3px 3px; border-style:solid solid
>   none; border-width:0 1px 1px; width:100%'\>"

>   \+ "\<tbody\>\<tr\>\<td bgcolor='white' style='background-clip:padding-box;
>   background-color:white; border-radius:0 0 3px 3px; color:\#525252;
>   font-size:15px; line-height:22px; overflow:hidden; padding:40px 40px
>   30px'\>"

>   \+ "\<div align='left' style='margin-bottom:16px; text-align:left'\>"

>   \+ "\<img data-imagetype='External'
>   src='http://www.cyberparkict.com/images/uyeler/291220141711126.jpg'
>   style='margin:17px 0; max-width:40%'\>\<p style='font-size:25px;'\>Hata
>   Takip\</p\>\</div\>"

>   \+ "\<p align='left' style='line-height:1.5; margin:0 0 17px; padding-top:0;
>   text-align: left'\>";

>   string sablonSonu =
>   "\</p\>\</td\>\</tr\>\</tbody\>\</table\>\</td\>\</tr\>\</tbody\>\</table\>\</td\>\</tr\>\</tbody\>\</table\>\</td\>\</tr\>\<tr\>\<tdvalign='top'
>   height='20'\>\</td\>\</tr\>\</tbody\>\</table\>\</div\>";

>   string baslik;

>   string mesaj;

>   if (tur == 0)

>   {

>   baslik = "Yeni bir görevin var.";

>   mesaj = sablonBasi + "Merhaba " + aliciAdi

>   \+ "\<br/\>\<p\>" + ekleyen + " size yeni bir görev verdi.\</p\>\<p\>" +
>   konu + "\</p\>" + sablonSonu;

>   }

>   else

>   {

>   baslik = "Yeni bir konuşman var.";

>   mesaj = sablonBasi + "Merhaba " + aliciAdi

>   \+ "\<br/\>\<p\>" + ekleyen + " yeni bir konuşma ekledi.\</p\>\<p\>" + konu
>   + "\</p\>" + sablonSonu;

>   }

>   using (MailMessage msg = new MailMessage(email, alici))

>   {

>   msg.Subject = baslik;

>   msg.Body = mesaj;

>   msg.IsBodyHtml = true;

>   using (SmtpClient client = new SmtpClient(emailSmtp, emailSmtpPort))

>   {

>   client.Credentials = new NetworkCredential(email, emailSifre);

>   client.EnableSsl = true;

>   client.Send(msg);

>   }

>   }

>   }

}

![](https://4.bp.blogspot.com/-a6Lrzhbn0kU/XM7uu97oK7I/AAAAAAAAC6o/26N1hUhvyrEAhId9sfA4DRXxE6stGIfpwCLcBGAs/s1600/cc0c06b818dd33dce74760544291d400.png)

Şekil 3.36. DevOps web sayfasında yapılan bir görev ataması

![](https://4.bp.blogspot.com/-S-ZeEUr2g3k/XM7uuxsA7XI/AAAAAAAAC6k/x9ygU9pvJ6MtHPcWOPHYQ26HSgnnQmhpwCLcBGAs/s1600/cac6520e1f5237daed3e945085572fb6.png)

Şekil 3.37. Proje takip sisteminde görev atamasından sonra gönderilen e-posta
içeriği

1.  **Proje takip sisteminin yayımlanması**

Staj sürecimin sonuna geldiğim için proje takip sistemi projesini geliştirmeyi
bitirmek durumda kaldım. Rehberlik yapan bilgisayar mühendisinin isteği
doğrultusunda, kuruluşta çalışan kişilerin geliştirdiğim sistemi kullanabilmesi
için, bu projeyi kuruluşun kendi domain’ine aktarmamı istedi. Bana verdiği
kullanıcı e-posta ve şifresi ile Turhost hosting şirketine giriş yaptım ve
burada yeni bir FTP hesabı oluşturdum.

FileZilla yazılımını kullanarak bu FTP hesabı ile domain’e bağlandım.

Geliştirdiğim projenin sadece gerekli dosyalarını sunucuya aktarmak için Visual
Studio’da Proje’ye, ardından Yayımla Proje Takip’e bastım.

![](https://1.bp.blogspot.com/-MLAqUnV2924/XM7uoOE9gEI/AAAAAAAAC5Y/zBSh2BVpV98bn9aJPlHTwS2UoNlF9XSgwCLcBGAs/s1600/4d0daa6de3cf901871f2e47905f45b3b.png)

Şekil 3.38. Proje takip sisteminin yayımlanması

Buradan C:\\publish\\takip klasörüne gerekli dosyaları publish yaptım. Bu
dosyaları da FileZilla yazılımı ile sunucuya kopyaladım.

Lokaldeki veri tabanını da Turhost’a aktarmak için, öncelikle Turhost’ta yeni
bir veri tabanı oluşturdum. Ardından, MSSQL Server Management Studio da veri
tabanı üzerinde sağ tıkladım. Buradan Tasks ve Generate Scripts bastım. Karşıma
çıkan sihirbaz penceresini tamamladıktan sonra, lokaldeki veri tabanının sql
dosyasını elde ettim [18].

![](https://3.bp.blogspot.com/-ysOWBCrLaMA/XM7ut-J9OsI/AAAAAAAAC6Y/Bpy8S-AD6r09wR8_GB78IbIpFjQJPQqvQCLcBGAs/s1600/c0eaa73bf0d53414ae10a45efbf5834a.png)

Şekil 3.39. Proje takip sistemi veri tabanının sql dosyası

Bu sql dosyasını Turhost hosting web sitesinde oluşturduğum veri tabanına import
ettim.

![](https://4.bp.blogspot.com/-L75aiP3CYrU/XM7umXyYa9I/AAAAAAAAC5I/5-dzy-crDtYZXopCjmJ97AI_erHua1mZACLcBGAs/s1600/2e3c1d9d92b77e771534686976183f73.png)

Şekil 3.40. Turhost veri tabanı sunucusu

1.  **Öneriler**

Staj süresinin kısıtlı olması ve ASP.NET ve web teknolojileri alanında
tecrübemin yeterli olmaması nedenleriyle, geliştirdiğim bu projenin birçok
eksiklikleri var ve bu projeyi kullandıkça ortaya başka eksiklikler, yapılması
gerekenler çıkacaktır. Benimde projeyi tamamlamamla birlikte kısa sürede
edindiğim izlenimlere göre bir TODO (yapılacaklar) listesi oluşturdum. Eğer ki
daha fazla zamanım olmuş olsaydı, bu listede yazdıklarımı tamamlamak isterdim:

-   Tablo ve satır oluşturma fonksiyonları daha fazla genelleştirilebilir.

-   Element gizleme - gösterme, temizleme fonksiyonları sadeleştirilmeli.

-   Uzun süre değişmeyen veriler istemcide de cache'lenmeli.

-   Aspx sayfalarının tümü master page'de olmalı.

-   Veri tabanına gönderilen boş veriler null olarak kaydedilmeli.

-   Tablolarda veri ekleme ve güncelleme formu yerine bu işlemler hücre bazlı
    yapılabilir.

-   Timeline’a silme seçeneği eklenmeli.

-   Konuşmalar için görüntülendi sayısı oluşturulmalı.

-   Ekleme, güncelleme ve silme gibi işlemlerden önce kullanıcıya gösterilen
    onay diyaloğunun görseli daha iyi olabilir.

-   Gelişmiş filtreleme yapılmalı.

**KAYNAKLAR**

1.  İnternet: Sanpark IT & Design Co., Ltd.
    <https://www.linkedin.com/company/sanpark-it-%26-design-co.-ltd./?originalSubdomain=tr>

2.  İnternet: HataTakip, <https://sanpark.monday.com>

3.  İnternet: Moment.js Documentation, <https://momentjs.com/docs/>

4.  İnternet: Colors From Image,
    <https://html-color-codes.info/colors-from-image/>

5.  İnternet: Ansari A., “SignalR Chat App With ASP.NET WebForm And BootStrap -
    Part One”,
    <https://www.c-sharpcorner.com/article/signalr-chat-app-with-asp-net-webform-and-bootstrap/>
    (2018).

6.  İnternet: How to Use Lambda in LINQ Select Statement,
    <https://stackoverflow.com/questions/15577890/how-to-use-lambda-in-linq-select-statement>

7.  İnternet: ASP.NET Web API CacheOutput,
    <https://github.com/filipw/Strathweb.CacheOutput>.

8.  İnternet: Create \<div\> and Append \<div\> Dynamically,
    <https://stackoverflow.com/questions/14004117/create-div-and-append-div-dynamically>

9.  İnternet: Coyier C., Fixed Table Layouts,
    <https://css-tricks.com/fixing-tables-long-strings/> (2014).

10. İnternet: Filtering Table Multiple Columns,
    <https://stackoverflow.com/questions/43622127/filtering-table-multiple-columns>

11. İnternet: jQuery Effects - Sliding,
    <https://www.w3schools.com/jquery/jquery_slide.asp>

12. İnternet: Fletcher P., Teebken T., Tutorial: Getting Started with SignalR 2
    and MVC 5,
    <https://docs.microsoft.com/en-us/aspnet/signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc>
    (2014).

13. İnternet: Kumar R., Broadcast SQL Data Using SignalR in ASP.Net,
    <https://www.c-sharpcorner.com/UploadFile/raj1979/broadcast-sql-data-using-signalr-in-Asp-Net/>
    (2014).

14. İnternet: CKEditor Quick Start Guide,
    <https://docs.ckeditor.com/ckeditor4/latest/guide/dev_installation.html>

15. İnternet: Slide Right to Left?,
    <https://stackoverflow.com/questions/596608/slide-right-to-left>

16. İnternet: Scheduler, <https://doc.daypilot.org/scheduler/>

17. İnternet: Usta S., Yandex Üzerinden Mail Göndermek,
    <http://sametusta.work/yandex-uzerinden-mail-gondermek/> (2015).

18. İnternet: Exporting Data From SQL Server Into A Script File,
    <https://success.scribesoft.com/s/article/Exporting-Data-From-SQL-Server-Into-A-Script-File>
    (2016).

19. İnternet: Microsoft .Net Nedir?
    <https://www.teknologweb.com/microsoft-net-nedir> (2013).

20. İnternet: Tümleşik Geliştirme Ortamı.
    <https://tr.wikipedia.org/wiki/Tümleşik_geliştirme_ortamı>

21. İnternet: JSON'a Giriş. <https://www.json.org/json-tr.html>

22. İnternet: WebSocket Nedir? <https://kodcu.com/2016/11/websocket-nedir-2/>
    (2016).

23. İnternet: Web API. <https://en.wikipedia.org/wiki/Web_API>

24. İnternet: Uzaktan Yordam Çağrısı,
    <https://tr.wikipedia.org/wiki/Uzaktan_yordam_%C3%A7a%C4%9Fr%C4%B1s%C4%B1>

25. İnternet: Microsoft Visual Studio.
    <https://en.wikipedia.org/wiki/Microsoft_Visual_Studio>

26. İnternet: SQL Server Management Studio,
    <https://en.wikipedia.org/wiki/SQL_Server_Management_Studio>

27. İnternet: FileZilla Nedir? Ne İşe Yarar?,
    <http://www.bilisimcafe.net/filezilla-nedir-ne-ise-yarar/> (2014).

28. İnternet: AJAX (Programlama).
    <https://tr.wikipedia.org/wiki/AJAX_(programlama)>

29. İnternet: Fletcher P., Introduction to SignalR,
    <https://docs.microsoft.com/tr-tr/aspnet/signalr/overview/getting-started/introduction-to-signalr>
    (2014).

30. İnternet: Demiryürek E., LINQ Teknolojisi,
    <https://www.kadinyazilimci.com/linq-teknolojisi/> (2014).

31. İnternet: jQuery, <https://tr.wikipedia.org/wiki/JQuery>

32. İnternet: Bootstrap (Önyüz Çatısı),
    <https://tr.wikipedia.org/wiki/Bootstrap_(%C3%B6ny%C3%BCz_%C3%A7at%C4%B1s%C4%B1)>

33. İnternet: HTML5 Scheduler Component, <https://www.daypilot.org/>

**EK – 1. Terimler**

*.Net Ortamı*: Microsoft şirketinin, programlama dilinden ve çalıştırılacak
sistemden bağımsız olarak uygulama geliştirmeyi amaçlayan platformudur. Pek çok
programlama dili ile uygulama geliştirmeye imkân sağlayan bir ortamdır. Uygulama
geliştirecekler için arka planda çalışan bir araçtır. Platformun desteklediği
programlama dillerinden birisi ile Visual Studio’yu kullanarak programlar veya
web uygulamaları geliştirilir [19].

*Integrated Development Environment (IDE)*: IDE, yazılımcıların hızlı ve rahat
bir şekilde program geliştirebilmesini amaçlayan, içerisinde barındırdığı
geliştirme sürecini organize edebilen birçok araçlarla birlikte geliştirme
sürecinin verimli kullanılmasına katkıda bulunan araçların tamamını içerisinde
barındıran yazılım türüdür. Programlama diline göre sözdizimi renklendirmesi
yapabilen, kod dosyalarının hiyerarşik olarak görülebilmesi amacıyla hazırlanmış
gerçek zamanlı, tümleşik bir derleyici, yorumlayıcı ve hata ayıklayıcı [20].

*JavaScript Object Notation (JSON)*: JSON, veri değişim formatıdır. İnsanların
okuyup yazabilmesi ve makinaların tarayıp, oluşturabilmesi kolaydır. JSON,
tamamen programlama dillerinden bağımsızdır, ancak C türevi dillere (C, C++,
C\#, Java, JavaScript, Perl, Python), yazılış bakımından çok benzeyen bir veri
tanımlama formatıdır. Bu özellikler, JSON'u veri alışverişi için ideal hale
getirir [21].

*WebSocket*: Http istek/cevap protokolüdür. Sunucuda bir değişiklik olduğunda,
sunucu bunu istemciye bildiremez. Bu değişikliği algılayabilmek için WebSocket
gibi yapılar kullanılır. WebSocket tek bir TCP bağlantısı üzerinden çift yönlü
ve full duplex mesajlaşmayı sağlar. İstemciden istek gelmesine gerek kalmadan
sunucudaki değişiklikler istemciye iletilebilir hale gelir. Http protokolüne
uygun olmayan gerçek zamanlı web uygulamalarındaki karmaşık yapının
basitleştirilmesini sağlar. İstemci ve sunucu arasında kurulan kalıcı bağlantı
sayesinde her iki taraf birbirine veri gönderebilir hale gelir [22].

*Web API (Web Application Programming Interface)*: Web API, bir web sunucusu
veya bir web tarayıcısı için uygulama programlama ara yüzüdür [23].

*RPC (Remote Procedure Call)*: Uzak yordam çağrısı, bir diğer adres uzayı
üzerinde süreçler arası iletişim teknolojisidir. Arka planda birçok işlemi
gerçekleştiren bir servistir. Temelde istemci ve sunucu arasında yapılan
işlemlerin iletişimi için tasarlanmıştır [24].

*Visual Studio*: Visual Studio, Microsoft tarafından geliştirilen bir tümleşik
geliştirme ortamıdır (IDE). Microsoft Windows, .NET Framework tarafından
desteklenen tüm platformlar için yönetilen kod ile birlikte yerel kod ve Windows
Forms uygulamaları, web siteleri, web uygulamaları ve web servisleri (Web API)
ile birlikte konsol ve grafiksel kullanıcı ara yüzü (GUI) uygulamaları
geliştirmek için kullanılır. Hata ayıklayıcısı, hem kaynak hem de makine
seviyesinde çalışır. GUI uygulamaları, web, sınıf ve veri tabanı şema
tasarımcıları oluşturmak için form tasarımcısı içerir. Visual Studio, çeşitli
programlama dillerini ve şablonları destekler. Bunlar; C, C++, C++/CLI, Visual
Basic .Net, C\#, F\#, JavaScript, TypeScript, XML, XSLT, HTML ve CSS’dir. Ayrıca
Python, Ruby, Node.js gibi dilleri de eklentiler aracılığıyla destekler [25].

*Microsoft SQL Server Management Studio*: SQL Server Management Studio (SSMS),
SQL Server için tüm bileşenleri yapılandırmak ve yönetmek için kullanılan bir
yazılım uygulamasıdır. Bu araç, hem sunucu editörleri hem de nesne ve sunucu
özellikleriyle çalışan grafik araçlarını içerir [26].

*FileZilla*: Uzak sunucu bağlantı protokolünü sağlayan bir yazılımdır. Basit ara
yüzü sayesinde hosting, reseller, vps ya da birçok farklı parçalanmış sunucu
tipine bağlantı yapmaya olanak tanır [27].

*Ajax (Asynchronous JavaScript and XML)*: Web sayfalarında JavaScript ve
XMLHttpRequest kullanımı ile etkileşimli uygulamalar oluşturmaya yarayan
tekniktir. En yaygın kullanım alanı, sayfayı yeniden yüklemeye gerek
kalmaksızın, sayfada görünür değişiklikler yapmaktır. XMLHttpRequest
kullanılarak birden fazla bağımsız işlem yapılabilir [28].

*SignalR*: ASP.NET uygulamalarını gerçek zamanlı web işlevselliği ekleme
işlemini basitleştiren bir ASP.NET kitaplığıdır. Bir kullanıcının yeni verileri
görmek için her zaman web sayfasını yenilemesi yerine SignalR kitaplığının
sunduğu özellikler ile eşzamanlı olarak değişen verilerin kullanıcıların
görüntüleyebilmesini sağlar. JavaScript işlevleri ile .NET kodundan çağrılan
sunucudan istemciye uzaktan yordam çağrısı (RPC) oluşturmak için basit bir API
sağlar [29].

*LINQ (Language Integrated Query)*: LINQ, nesneler üzerinde bulunan ilişkisel
veriyi hızlı bir şekilde sorgulamak için kullanılan bir sorgulama aracıdır. LINQ
to SQL ise LINQ altyapısının SQL veri tabanı üzerine uyarlanmış halidir [30].

*Strathweb.CacheOutput*: CacheOutput, sunucu tarafında veriyi önbelleğe alma
işlemlerini üstlenen ve uygun istemci tarafı (yanıt) başlıklarını (header)
ayarlayan bir kitaplıktır [7].

*jQuery*: 2006 yılında geliştirilmeye başlanmış ve gelişimi sürdürülen bir açık
kaynak ve en popüler çapraz-platform JavaScript kitaplığıdır. Yoğun olarak
animasyonlarda kullanılır. Flash'ın alternatifi olarak kullanılan bu teknoloji
ile Flash galeri, tab menü, sayfa geçişleri gibi birçok işlem yapılmaktadır
[31].

*Bootstrap*: Twitter Bootstrap olarak da bilinen açık kaynak kodlu, web
sayfaları veya uygulamaları geliştirmek için kullanılabilecek araçlar bütünü ve
önyüz çatısıdır. Web sayfaları veya uygulamalarında kullanılabilecek, HTML ve
CSS tabanlı tasarım şablonlarını içerir. Bu şablonlar form, navigasyon çubuğu,
buton gibi ara yüz bileşenleri oluşturmakta kullanılabilmektedir [32].

*Metronic*: En çok kullanılan HTML/CSS/Angular/Bootstrap yönetim tema şablon
kitaplığından bir tanesidir.

*DayPilot*: DayPilot HTML5 zamanlayıcısı (scheduler), birden fazla kaynağın bir
zaman çizelgesinde görüntülenmesini sağlayan bir kitaplıktır. Hücre boyutu
ayarlanabilir (dakikalar ve yıllar). Sürükle ve bırak Ajax işlemlerini (taşıma,
yeniden boyutlandırma), kaynakların hiyerarşisini (ağaç görünümü), dinamik olay
yüklemesini (kaydırma yaparken), görüntü dışa aktarmayı (SVG, PNG, JPEG) ve
diğer gelişmiş özellikleri destekler [33].

*CKEditor*: Kullanımı kolay, ücretsiz, açık kaynak, hızlı ve güvenli bir
zenginleştirilmiş metin editörüdür.

*Moment*: Moment sadece tarih işlemler için oluşturulmuş açık kaynak bir
JavaScript kitaplığıdır.
