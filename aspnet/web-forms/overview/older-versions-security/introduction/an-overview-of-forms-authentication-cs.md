---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
title: Form kimlik doğrulaması (C#) genel bakış | Microsoft Docs
author: rick-anderson
description: Özel yollar oluşturma
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: de2d65b9-aadc-42ba-abe1-4e87e66521a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 1f64384d403f3cf81ffa3327a81b635bc71e2b44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="an-overview-of-forms-authentication-c"></a>Form kimlik doğrulaması (C#) genel bakış
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_cs.pdf)

> Bu öğreticide biz yalnızca tartışma uygulamasına açılır; Özellikle, form kimlik doğrulaması uygulanmasına arar. Üyelik ve roller için basit form kimlik doğrulamasını taşımak gibi Bu öğreticide oluşturma başlangıç web uygulaması sonraki öğreticilerde üzerinde oluşturulacak devam eder.
> 
> Lütfen bu konu hakkında daha fazla bilgi için bu videoyu bakın: [kullanarak temel form kimlik doğrulamasını ASP.NET](# "using-basic-forms-authentication-in-aspnet").


## <a name="introduction"></a>Giriş

İçinde [önceki öğretici](security-basics-and-asp-net-support-cs.md) biz ASP.NET tarafından sağlanan çeşitli kimlik doğrulama, yetkilendirme ve kullanıcı hesabı seçenekleri ele alınan. Bu öğreticide biz yalnızca tartışma uygulamasına açılır; Özellikle, form kimlik doğrulaması uygulanmasına arar. Üyelik ve roller için basit form kimlik doğrulamasını taşımak gibi Bu öğreticide oluşturma başlangıç web uygulaması sonraki öğreticilerde üzerinde oluşturulacak devam eder.

Bu öğretici, forms kimlik doğrulaması iş akışı, önceki öğreticide biz işlemdeki bağlı bir konuya ilişkin ayrıntılı bir bakış ile başlar. ASP.NET Web sitesi aracılığıyla form kimlik doğrulaması kavramlarını gösteri için oluşturacağız. Ardından, biz form kimlik doğrulaması kullanmak için basit oturum açma sayfası oluşturun ve, kodda, bir kullanıcının kimliği doğrulanır ve bu durumda, kullanıcı adı ile günlüğe olup olmadığını belirlemek nasıl görmek için siteyi yapılandırır.

Kimlik doğrulama iş akışı, bir web uygulamasında etkinleştirme ve oturum açma ve kapatma sayfaları oluşturma, kullanıcı hesaplarını destekleyen ve bir web sayfası üzerinden kullanıcıların kimliğini doğrulayan bir ASP.NET uygulaması oluşturmanın tüm önemli adımlar verilmiştir formlarını anlama. Bu – nedeniyle ve bu öğreticileri birbirine - derleme nedeniyle son projelerinde forms kimlik doğrulaması deneyimini yapılandırma zaten olsa bile sonrakine geçmeden önce bu öğreticide tam çalışmanıza olanak ı önerin.

## <a name="understanding-the-forms-authentication-workflow"></a>Form kimlik doğrulama iş akışı anlama

ASP.NET çalışma zamanı bir ASP.NET sayfasının veya ASP.NET Web hizmeti gibi bir ASP.NET kaynağı için bir isteği işlerken isteğin olayların sayısı, yaşam döngüsü sırasında başlatır. İstek olanları istek doğrulanır ve yetkilendirme, bir işlenmeyen özel durum ve benzeri söz konusu olduğunda gerçekleşen bir olay tetiklenir çok başlangıç ve çok sonunda başlatılan olayları vardır. Olayların tam listesini görmek için bkz [HttpApplication nesnesinin olayları](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*HTTP modülleri* kodu isteği yaşam döngüsü belirli bir olaya yanıt yürütüldüğünde yönetilen sınıflarıdır. ASP.NET birkaç önemli görevleri arka planda gerçekleştirmek HTTP Modülleri ile birlikte gelir. Bizim tartışmaya yakından ilgili olan iki yerleşik HTTP modülleri şunlardır:

- **[`FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)** – genellikle kullanıcının tanımlama bilgilerini koleksiyona dahil forms kimlik doğrulaması bileti inceleyerek kullanıcının kimliğini doğrular. Bir form kimlik doğrulama anahtarı varsa, anonim bir kullanıcıdır.
- **[`UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)** – Geçerli kullanıcı istenen URL erişmek için yetkili olup olmadığını belirler. Bu modül, uygulamanın yapılandırma dosyalarında belirtilen yetkilendirme kuralları danışmanlık tarafından yetkilisi belirler. ASP.NET de içeren [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) istenen dosyaları ACL'ler danışmanlık tarafından yetkilisi belirleyen.

`FormsAuthenticationModule` Öncesinde kullanıcı kimlik doğrulama girişiminde `UrlAuthorizationModule` (ve `FileAuthorizationModule`) yürütme. İstekte bulunan kullanıcının istenen kaynağa erişme yetkisi yok, yetkilendirme modülü istek sonlandırır ve döndüren bir [HTTP 401 Yetkisiz](http://www.checkupdown.com/status/E401.html) durumu. Windows kimlik doğrulaması senaryolarda HTTP 401 durum tarayıcıya döndürülür. Bu durum kodu tarayıcının kullanıcıdan kimlik bilgilerini kalıcı bir iletişim kutusu üzerinden neden olur. FormsAuthenticationModule bu durum algılar ve bunun yerine kullanıcının oturum açma sayfasına yeniden yönlendirmek için değiştirdiği için form kimlik doğrulaması ile ancak, HTTP 401 yetkilendirilmedi durum hiçbir zaman tarayıcıya gönderilen (aracılığıyla bir [HTTP 302 yeniden yönlendirme](http://www.checkupdown.com/status/E302.html) durum).

Kullanıcının kimlik bilgilerinin geçerli olduğundan ve bu durumda, forms kimlik doğrulaması bileti oluşturun ve kullanıcının sayfasına yeniden yönlendirmek için bunlar ziyaret denediğiniz, oturum açma sayfasının sorumluluk belirlemektir. Kimlik doğrulaması bileti Web sayfalarında sonraki istekleri dahil olduğu `FormsAuthenticationModule` kullanıcıyı tanımlamak için kullanır.


![Form kimlik doğrulama iş akışı](an-overview-of-forms-authentication-cs/_static/image1.png)

**Şekil 1**: form kimlik doğrulama iş akışı


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Sayfa ziyareti arasında kimlik doğrulaması bileti anımsama

Site gezinirken kullanıcı olarak oturum açmış kalmayacak şekilde oturum açtıktan sonra forms kimlik doğrulaması bileti geri her isteğin web sunucusunda gönderilmesi gerekir. Bu genellikle kullanıcının tanımlama bilgileri koleksiyonu içinde kimlik doğrulaması bileti koyarak gerçekleştirilir. [Tanımlama bilgilerini](http://en.wikipedia.org/wiki/HTTP_cookie) , kullanıcının bilgisayarda bulunabilir ve HTTP üst bilgileri her istekte tanımlama bilgisi oluşturulan Web sitesine iletilen küçük metin dosyalarıdır. Bu nedenle, forms kimlik doğrulaması bileti oluşturulur ve tarayıcının tanımlama bilgilerinde depolanan sonra site sonraki her ziyaret böylece kullanıcı tanımlama isteği ile birlikte kimlik doğrulaması bileti gönderir.

Bir tanımlama bilgisi tarih ve saate, tarayıcı tanımlama bilgisi atar tarihlerinden yönüdür. Form kimlik doğrulaması tanımlama bilgisinin süresi dolduğunda, kullanıcı artık doğrulanabilen ve bu nedenle anonim olur. Kullanıcının ortak terminal durumundan ziyaret ederken tarayıcısında kapattıktan sonra süresi dolacak şekilde kendi kimlik doğrulama bileti istedikleri yüksektir. Evden ziyaret ederken, aynı kullanıcı kimlik doğrulaması bileti sahip böylece tarayıcı yeniden başlatmaları arasındaki anımsanacağını ancak isteyebilirsiniz yeniden oturum siteyi ziyaret ettikleri her zaman. Bu genellikle bir "Beni anımsa" biçiminde bir kullanıcı tarafından oturum açma sayfasında onay kutusu kararı verilir. Adım 3'te oturum açma sayfasını bir "Beni anımsa" onay kutusu uygulamak nasıl inceleyeceğiz. Aşağıdaki öğreticide ayrıntılı kimlik doğrulama bileti zaman aşımı ayarları giderir.

> [!NOTE]
> Web sitesine oturum açmak için kullanılan kullanıcı aracısı tanımlama bilgilerini desteklemeyebilir mümkündür. Böyle bir durumda, ASP.NET cookieless form kimlik doğrulama biletlerini kullanabilirsiniz. Bu modda, kimlik doğrulaması bileti URL'ye kodlanır. Cookieless kimlik doğrulama biletlerini kullanıldığında ve nasıl bunlar oluşturulur ve sonraki öğreticide yönetilen ele alacağız.


### <a name="the-scope-of-forms-authentication"></a>Form kimlik doğrulaması kapsamı

`FormsAuthenticationModule` Olan yönetilen ASP.NET çalışma zamanı parçası olan kod. Microsoft'un 7 sürümü önce [Internet Information Services (IIS)](https://www.iis.net/) web sunucusuna IIS HTTP ardışık düzen ve ASP.NET zamanının ardışık düzen arasında farklı bir engel oldu. Kısacası, IIS 6 ve önceki sürümlerinde, `FormsAuthenticationModule` bir istek IIS ASP.NET çalışma yetkisi aktarıldığında yalnızca yürütür. .Aspx, .asmx veya .ashx uzantı içeren bir sayfa istendiğinde varsayılan olarak, IIS statik içeriğin kendisini – HTML sayfaları ve CSS ve görüntü dosyaları – ve ASP.NET çalışma zamanı isteklerine yalnızca ellerini gibi işler.

IIS 7, ancak, tümleşik IIS ve ASP.NET için işlem hatları sağlar. Bazı yapılandırma ayarlarıyla IIS 7 için FormsAuthenticationModule çağırmak için Kurulum *tüm* istekleri. Ayrıca, IIS 7 ile tüm dosya türlerini URL yetkilendirme kuralları tanımlayabilirsiniz. Daha fazla bilgi için bkz: [değişiklikleri arasında IIS6 ve IIS7 güvenlik](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [bilgisayarınızı Web platformu güvenlik](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements), ve [anlama IIS7 URL yetkilendirmesi](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Yazıyı short, IIS 7'den önceki sürümlerde, yalnızca form kimlik doğrulaması ASP.NET çalışma zamanı tarafından işlenen kaynakları korumak için de kullanabilirsiniz. Benzer şekilde, URL yetkilendirme kuralları yalnızca ASP.NET çalışma zamanı tarafından işlenen kaynaklarına uygulanır. Ancak IIS 7 ile IIS HTTP ardışık düzen, böylece tüm isteklere bu işlevselliği genişletme içine FormsAuthenticationModule ve UrlAuthorizationModule tümleştirmek mümkündür.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>1. adım: Bu öğretici seri için bir ASP.NET Web sitesi oluşturma

Geniş olası hedef kitle ulaşabilmeniz için Microsoft'un ücretsiz Visual Studio 2008 sürümüyle oluşturmakta bu seri ASP.NET Web sitesi oluşturulacak [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Biz gerçekleştireceksiniz `SqlMembershipProvider` kullanıcı deposunda bir [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) veritabanı. Visual Studio 2005 veya Visual Studio 2008 veya SQL Server farklı bir sürümünü kullanıyorsanız, endişelenmeyin - adımlar neredeyse aynı olacaktır ve önemsiz olmayan farklılıkları gösterilecektir.

> [!NOTE]
> Her öğreticide kullanılan demo web uygulaması, bir yükleme olarak kullanılabilir. Bu indirilebilir bir uygulama, .NET Framework sürüm 3.5 için hedeflenen Visual Web Developer 2008 ile oluşturuldu. Uygulama için .NET 3.5 hedeflenen olduğundan, Web.config dosyasında, 3.5 özgü ek yapılandırma öğeleri içeriyor. Yazıyı henüz sonra indirilebilir web uygulaması bilgisayarınızda yüklü .NET 3.5 yüklemek varsa kısa ilk 3.5 özgü biçimlendirme Web.config dosyasından kaldırma olmadan çalışmaz.


Form kimlik doğrulaması yapılandırabilmeniz için önce ilk ASP.NET Web sitesi ihtiyacımız var. Yeni bir dosya sistemi tabanlı ASP.NET Web sitesi oluşturmaya başlayın. Bunu başarmak için Visual Web Developer başlatın ve dosya menüsüne gidin ve yeni Web sitesi iletişim kutusunu görüntüleme yeni Web sitesi seçin. ASP.NET Web sitesi şablonunu seçin, dosya sistemine konum aşağı açılan listesi ayarlamak, web sitesi yerleştirmek için bir klasör seçin ve dil C# ayarlayın. Bu yeni bir web sitesi Default.aspx ASP.NET sayfa ile bir uygulama oluşturacaksınız\_veri klasörü ve bir Web.config dosyası.

> [!NOTE]
> Visual Studio Proje yönetimi iki modlarını destekler: Web sitesi projeleri ve Web Uygulama projeleri. Web sitesi projeleri proje dosyası, Web Uygulama projeleri, Visual Studio .NET 2002/2003 proje mimarisi taklit – bir proje dosyası içerir ve / bin klasörüne yerleştirilir tek bir derleme halinde projenin kaynak kodu derleme ancak yoksundur. Web uygulama projesi modeli Service Pack 1'yeniden olsa da visual Studio 2005 başlangıçta yalnızca desteklenen Web sitesi, projeleri; Visual Studio 2008 her iki proje modelleri sunar. Ancak, Visual Web Developer 2005 ve 2008 sürümlerinde, yalnızca Web sitesi projeleri destekler. Web sitesi proje modeli kullanacaklardır. Express olmayan sürüm kullanıyorsanız ve kullanmak istediğiniz [Web uygulama projesi modeli](https://msdn.microsoft.com/library/aa730880%28vs.80%29.aspx) bunun yerine, bunu ancak olabileceğini bazı tutarsızlıklar ekranınızı ve karşı gerçekleştirmeniz gereken adımlar gördükleri arasında unutmayın çekinmeyin gösterilen ekran görüntüleri ve bu öğreticileri sağlanan yönergeleri.


[![Yeni bir dosya sistemi tabanlı Web sitesi oluşturma](an-overview-of-forms-authentication-cs/_static/image3.png)](an-overview-of-forms-authentication-cs/_static/image2.png)

**Şekil 2**: New File System-Based Web sitesi oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-forms-authentication-cs/_static/image4.png))


### <a name="adding-a-master-page"></a>Bir ana sayfa ekleme

Ardından, yeni bir ana sayfa Site.master adlı kök dizininde site ekleyin. [Ana sayfalar](https://msdn.microsoft.com/library/wtxbf3hh.aspx) ASP.NET sayfaları için uygulanabilir bir site genelinde şablonu tanımlamak bir sayfa Geliştirici etkinleştirin. Ana sayfalar ana avantajı dolayısıyla güncelleştirmek veya sitenin Düzen ince ayar kolaylaşır sitenin genel görünümünü tek bir konumda tanımlanabilir ' dir.


[![Bir ana sayfa eklemek Site.master Web sitesine adlı](an-overview-of-forms-authentication-cs/_static/image6.png)](an-overview-of-forms-authentication-cs/_static/image5.png)

**Şekil 3**: Web sitesine bir ana sayfa adlı Site.master ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-forms-authentication-cs/_static/image7.png))


Site genelinde sayfa düzeni burada ana sayfasında tanımlayın. Tasarım görünümünü kullanın ve ihtiyacınız ne olursa olsun düzeni veya Web denetimleri ekleme ya da el ile kaynak görünümü biçimlendirme el ile ekleyebilirsiniz. My ana sayfanın düzenini kullanılan düzeni taklit etmek üzere yapılandırılmış my *[, ASP.NET 2.0 verilerle çalışma](../../data-access/index.md)* öğretici serisi (bkz. Şekil 4). Ana sayfayı kullanan [geçişli stil sayfası](http://www.w3schools.com/css/default.asp) konumlandırma ve stilleri (hangi Bu öğreticinin ilişkili indirme işlemine dahildir) Style.css dosyasında tanımlanan CSS ayarlarla için. Aşağıda gösterilen biçimlendirmeden bildiremez olsa da, CSS kuralları tanımlanan şekilde gezinti &lt;div&gt;ait içerik soldaki bölmede görünür ve sabit genişlik 200 piksel sahip olmasını mutlak olarak konumlandırılmış.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample1.aspx)]

Bir ana sayfa statik sayfa düzeni ve ana sayfayı kullanan ASP.NET sayfaları tarafından düzenlenebilir bölgeler tanımlar. Bu içerik düzenlenebilir bölgeleri belirtilir `ContentPlaceHolder` içinde içeriği görülebilir denetim &lt;div&gt;. Tek bir ana sayfamızı sahip `ContentPlaceHolder` (MainContent), ancak ana sayfanın birden çok ContentPlaceHolders için sahip olabilir.

Yukarıda girilen biçimlendirme ile Tasarım görünümüne geçiş yapmak ana sayfa düzeni gösterilir. Bu ana sayfa kullanan tüm ASP.NET sayfaları için biçimlendirme belirtme olanağı ile bu Tekdüzen düzeni olacaktır `MainContent` bölge.


[![Tasarım görünümü görüntülendiğinde ana sayfa,](an-overview-of-forms-authentication-cs/_static/image9.png)](an-overview-of-forms-authentication-cs/_static/image8.png)

**Şekil 4**: ana sayfa, ne zaman görüntülenebilir aracılığıyla tasarım görüntüleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-forms-authentication-cs/_static/image10.png))


### <a name="creating-content-pages"></a>İçerik sayfaları oluşturma

Bu noktada Default.aspx sayfasında bizim Web sitesinde sahip olduğumuz ancak yeni oluşturduğumuz ana sayfa kullanmaz. Henüz herhangi bir içerik sayfasının içermiyorsa, bir ana sayfa kullanmak için bir web sayfasının bildirim temelli biçimlendirme işlemek mümkün olmakla birlikte yalnızca sayfayı silin ve yeniden kullanmak için ana sayfa belirtme projeye eklemek daha kolay olur. Bu nedenle, projeden Default.aspx silerek başlatın.

Ardından, Çözüm Gezgini'nde proje adına sağ tıklayın ve Default.aspx adlı yeni bir Web formu eklemek için seçin. Bu süre, "ana sayfa seç" onay kutusunu işaretleyin ve Site.master ana sayfa listeden seçin.


[![Bir ana sayfa seçmek seçerek yeni bir Default.aspx sayfa ekleyin](an-overview-of-forms-authentication-cs/_static/image12.png)](an-overview-of-forms-authentication-cs/_static/image11.png)

**Şekil 5**: bir yeni Default.aspx sayfasında bir ana sayfa seçmek için seçerek eklemek ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-forms-authentication-cs/_static/image13.png))


![Site.master ana sayfa kullanın](an-overview-of-forms-authentication-cs/_static/image14.png)

**Şekil 6**: Site.master ana sayfa kullanın


> [!NOTE]
> Web uygulama projesi modeli kullanıyorsanız, yeni öğe Ekle iletişim kutusunda bir "ana sayfa seç" onay kutusu içermez. Bunun yerine, "Web içerik formu." türünde bir öğe eklemeniz gerekir "Web içerik Form" seçeneğini seçerek ve Ekle düğmesine sonra Visual Studio aynı seçin asıl görüntüler Şekil 6'da gösterilen iletişim kutusu.


Yalnızca yeni Default.aspx sayfanın bildirim temelli biçimlendirme içeren bir @Page asıl yolunu belirtme yönergesi için ana sayfa MainContent ContentPlaceHolder sayfa dosyası ve içerik denetimi.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample2.aspx)]

Şu an için Default.aspx boş bırakın. Biz buna içeriği eklemek için daha sonra Bu öğreticide döndürür.

> [!NOTE]
> Ana sayfamızı menü veya diğer bir gezinti arabirimi için bir bölüm içerir. Sonraki öğreticide böyle bir arabirim oluşturacağız.

## <a name="step-2-enabling-forms-authentication"></a>2. adım: Form kimlik doğrulamasını etkinleştirme

Oluşturulan ASP.NET Web sitesi ile bizim sonraki form kimlik doğrulamasını etkinleştirmek için bir görevdir. Uygulamanın kimlik doğrulaması yapılandırma aracılığıyla belirtilen [ `<authentication>` öğesi](https://msdn.microsoft.com/library/532aee0e.aspx) Web.config dosyasında. `<authentication>` Öğe uygulama tarafından kullanılan kimlik doğrulama modeli belirten modu adlı tek bir öznitelik içeriyor. Bu öznitelik aşağıdaki dört değerden biri olabilir:

- **Windows** – önceki öğreticide, bir uygulamayı Windows kimlik doğrulaması kullandığında açıklandığı gibi ziyaretçi kimlik doğrulaması web sunucusunun sorumluluğundadır ve bu genellikle temel, Özet veya tümleşik Windows gerçekleştirilir kimlik doğrulaması.
- **Forms**– bir web sayfasında form aracılığıyla authenticated users.
- **Passport**– kullanıcılar Microsoft Passport ağı kullanarak doğrulaması.
- **Hiçbiri**– hiçbir kimlik doğrulama modeli kullanılır; tüm ziyaretçiler anonim olarak yapılır.

Varsayılan olarak, ASP.NET uygulamaları Windows kimlik doğrulaması kullanın. Form kimlik doğrulaması için kimlik doğrulama türünü değiştirmek için daha sonra değiştirmek ihtiyacımız `<authentication>` formlara öğenin mod özniteliği.

Projenizi bir Web.config dosyası henüz yoksa bir şimdi göre Çözüm Gezgini'nde proje adına sağ tıklayarak, yeni öğe Ekle seçme ve bir Web yapılandırma dosyası ekleme ekleyin.


[![Projenizi Web.config henüz içermiyorsa, şimdi ekleyin](an-overview-of-forms-authentication-cs/_static/image16.png)](an-overview-of-forms-authentication-cs/_static/image15.png)

**Şekil 7**: varsa bilgisayarınızı proje mu değil henüz dahil Web.config, Ekle, şimdi ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-forms-authentication-cs/_static/image17.png))


Ardından, bulun `<authentication>` öğesi ve kullanmak için form kimlik doğrulamasını güncelleştirme. Bu değişiklikten sonra Web.config dosyanızın biçimlendirme aşağıdakine benzer görünmelidir:

[!code-xml[Main](an-overview-of-forms-authentication-cs/samples/sample3.xml)]

> [!NOTE]
> Web.config bir XML dosyası olduğundan, büyük/küçük harf önemlidir. "F" büyük ile formlara, mod özniteliği ayarladığınızdan emin olun. "Forms" gibi farklı bir kasa kullanırsanız, bir tarayıcı aracılığıyla sitesini ziyaret ederken bir yapılandırma hatası alırsınız.


`<authentication>` Öğesi isteğe bağlı olarak içerebilir bir `<forms>` forms kimlik doğrulaması özgü ayarları içeren alt öğesi. Şimdilik, varsayılan formlar kimlik doğrulama ayarlarını yalnızca kullanalım. Biz inceleyeceksiniz `<forms>` sonraki öğreticide daha ayrıntılı alt öğesi.

## <a name="step-3-building-the-login-page"></a>3. adım: oturum açma sayfası oluşturma

Form kimlik doğrulamasını desteklemek için bir oturum açma sayfası, Web sitesi gerekir. "Anlama Forms kimlik doğrulaması iş akışı" bölümünde açıklandığı gibi `FormsAuthenticationModule` otomatik olarak kullanıcı oturum açma sayfasına yeniden yönlendir olmadıkları bir sayfaya erişmeye çalışırsanız görüntülemeye yetkili. Anonim kullanıcılar için oturum açma sayfasına bir bağlantı görüntüler ASP.NET Web denetimleri vardır. Bu "Oturum açma sayfasının URL'sini nedir?" sorusunu begs

Varsayılan olarak, forms kimlik doğrulaması sistem Login.aspx adlı için oturum açma sayfasına bekler ve web uygulamasının kök dizininde eklenir. Farklı bir oturum açma sayfası URL'si kullanmak istiyorsanız, bunu Web.config dosyasında belirterek yapabilirsiniz. Sonraki öğreticide bunu nasıl göreceğiz.

Oturum açma sayfasında üç sorumlulukları vardır:

1. Ziyaretçi kimlik bilgilerini girmesini sağlayan bir arabirim sağlar.
2. Gönderilen kimlik bilgileri geçerli olup olmadığını belirleyin.
3. "Kullanıcı forms kimlik doğrulaması bileti oluşturarak oturum".

### <a name="creating-the-login-pages-user-interface"></a>Oturum açma sayfasının kullanıcı arabirimi oluşturma

İlk görevle başlayalım. Sitenin kök dizinine Login.aspx adlı yeni bir ASP.NET sayfa ekleyin ve Site.master ana sayfa ile ilişkilendirin.


[![Yeni bir ASP.NET sayfası Ekle Login.aspx adlı](an-overview-of-forms-authentication-cs/_static/image19.png)](an-overview-of-forms-authentication-cs/_static/image18.png)

**Şekil 8**: adlı yeni bir ASP.NET sayfası Login.aspx ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-forms-authentication-cs/_static/image20.png))


İki metin kutuları – bir kullanıcı adı için bir form göndermek için bir düğmeyi ve parolalarını – için tipik oturum açma sayfası arabirimini oluşur. Web siteleri görmemeleri işaretlenirse, sonuçta elde edilen kimlik doğrulaması bileti tarayıcı yeniden başlatmaları arasındaki devam ediyorsa, bir "Beni anımsa" bir onay kutusu içerir.

İki metin kutuları Login.aspx ve kümesi ekleme kendi `ID` kullanıcı adı ve parola özelliklerine sırasıyla. Parolanın ayrıca ayarlayın `TextMode` parola özelliğine. Sonra bir onay kutusu denetim ayarı ekleyin, `ID` RememberMe özelliğine ve kendi `Text` "Beni anımsa" özelliğine. Oturum Aç düğmesini adlı bir düğme ekleme, `Text` özelliği, "Login" ayarlanır. Ve son olarak, bir etiket Web denetimi ekleyin ve ayarlayın, `ID` InvalidCredentialsMessage, özelliğine kendi `Text` özelliği "kullanıcı adı veya parola geçersiz. Lütfen yeniden deneyin. ", kendi `ForeColor` kırmızı, özelliğine ve onun `Visible` özelliğini false olarak ayarlayın.

Sayfanızın tanımlayıcı sözdizimi aşağıdaki gibi ve bu noktada, ekranınızın Şekil 9'ekran şuna benzemelidir:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample4.aspx)]


[![Oturum açma sayfasını iki metin kutuları, bir onay kutusu, bir düğmeyi ve etiket içerir](an-overview-of-forms-authentication-cs/_static/image22.png)](an-overview-of-forms-authentication-cs/_static/image21.png)

**Şekil 9**: oturum açma sayfasını içeren iki metin kutuları, bir onay kutusu, bir düğmeyi ve etiket ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-forms-authentication-cs/_static/image23.png))


Son olarak, bir olay işleyicisi için Oturum Aç düğmesini'nın tıklatın oluşturun olay. Tasarımcısı'ndan bu olay işleyicisi oluşturmak için düğme denetimi çift tıklatın.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Sağlanan kimlik bilgileri geçerli olup olmadığını belirleme

Artık görev 2 düğmenin Click uygulamak ihtiyacımız olay işleyicisi – sağlanan kimlik bilgilerinin geçerli olup olmadığını belirleme. Bunu yapmak için size sağlanan kimlik bilgileri bilinen bir kimlik bilgileriyle eşleşmesi olup olmadığını belirleyebilmeniz için için tüm kullanıcıların kimlik bilgilerini tutan bir kullanıcı deposu olması gerekir.

ASP.NET 2.0 önce geliştiricilerin kendi her iki kullanıcı depoları uygulanması ve depolama karşı sağlanan kimlik bilgilerini doğrulamak için kod yazma sorumlu. Çoğu geliştirici kullanıcı deposunda kullanıcı adı, parola, e-posta, LastLoginDate ve diğerleri gibi sütunlarla kullanıcılar adlı bir tablo oluşturma bir veritabanında, uygulamanız. Bu tablo daha sonra kullanıcı hesabı her bir kayıt gerekir. Kullanıcının sağlanan kimlik bilgileri doğrulanıyor eşleşen bir kullanıcı adı için veritabanını sorgulama ve veritabanında Parola Sağlanan parola corresponded sağlama içerir.

ASP.NET 2.0 ile geliştiriciler üyelik sağlayıcılardan biri kullanıcı deposunda yönetmek için kullanmanız gerekir. Bu öğretici serisinde biz kullanıcı deposu için bir SQL Server veritabanı kullanan SqlMembershipProvider kullanacak. SqlMembershipProvider kullanırken tabloları, görünümleri ve saklı yordamlar sağlayıcı tarafından beklenen içeren bir belirli veritabanı şeması uygulamanız gerekir. Bu şemada uygulamak nasıl inceleyeceğiz ***SQL Server üyelik şema oluşturma*** Öğreticisi. Kullanıcının kimlik bilgileri doğrulanıyor yerinde üyelik sağlayıcısı ile çağırmak kadar kolaydır [üyelik sınıfı](https://msdn.microsoft.com/library/system.web.security.membership.aspx)'s [ValidateUser (*kullanıcıadı*, *parola*) yöntem](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx), gösteren bir Boole değeri döndürür olup olmadığını geçerliliğini *kullanıcıadı* ve *parola* birleşimi. Biz henüz SqlMembershipProvider'ın kullanıcı deposu uygulanmadı olarak görmekten üyelik sınıfının ValidateUser yöntemi şu anda kullanamazsınız.

(Biz SqlMembershipProvider uygulandıktan sonra eski olur) kendi özel kullanıcı veritabanı tablosu oluşturmak için zaman yerine sabit kodlu atalım yerine geçerli kimlik bilgileri oturum açma içinde kendisi sayfa. Oturum Aç düğmesini 's olay işleyicisi'ı tıklatın ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample5.cs)]

Gördüğünüz gibi üç geçerli bir kullanıcı hesapları – Scott, Jisun ve Sam – vardır ve üç aynı parolayı ("parola") sahip. Geçerli bir kullanıcı adı ve parola eşleşme araması kullanıcılar ve parolalar diziler üzerinden kodu döngüsü. Kullanıcı adı ve parola geçerliyse, kullanıcı oturum açma ve ardından uygun sayfasına yeniden yönlendirmek ihtiyacımız. Kimlik bilgileri geçersiz olduğunda, biz InvalidCredentialsMessage etiketini görüntüler.

Kullanıcı geçerli kimlik bilgilerini girdiğinde, ı, ardından "uygun sayfaya." yönlendirilir bahsedilen Uygun sayfaya ancak nedir? Bir kullanıcı bir sayfayı görüntüleme yetkiniz yok ziyaret ettiğinde FormsAuthenticationModule otomatik olarak bunları oturum açma sayfasına yönlendirir, geri çağırma. Bunu yaparken, sorgu dizesi ReturnUrl parametresi aracılığıyla istenen URL içerir. Diğer bir deyişle, ProtectedPage.aspx ziyaret etmek bir kullanıcı çalıştı ve bunlar Bunu yapmak için yetkileri yok, FormsAuthenticationModule bunları yeniden yönlendirme:

Login.aspx?ReturnUrl=ProtectedPage.aspx

Başarıyla oturum açtıktan sonra kullanıcı geri ProtectedPage.aspx yönlendirilmeniz gerekir. Alternatif olarak, kullanıcılar kendi volition üzerinde oturum açma sayfasını ziyaret edebilirsiniz. Bu durumda, kullanıcı oturum sonra bunlar için kök klasör Default.aspx sayfasında gönderilmelidir.

### <a name="logging-in-the-user"></a>Kullanıcı günlüğü

Sağlanan kimlik bilgilerinin geçerli olduğu varsayımıyla oluşturmamız forms kimlik doğrulaması bileti böylece siteye kullanıcı oturum gerekir. [FormsAuthentication sınıfı](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) içinde [System.Web.Security ad alanı](https://msdn.microsoft.com/library/system.web.security.aspx) kimlik doğrulama sistemi günlük giriş ve çıkışı formlar üzerinden kullanıcılara oturum açma için çeşitli yöntemler sağlar. FormsAuthentication sınıfında birkaç yöntem olmasına karşın, biz juncture en bu ilgilendiğiniz üç şunlardır:

- [GetAuthCookie(*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) – creates a forms authentication ticket for the supplied name *username*. Ardından, bu yöntem oluşturur ve kimlik doğrulaması bileti içeriğini tutan HttpCookie nesne döndürür. Varsa *persistCookie* true, kalıcı bir tanımlama bilgisi oluşturulur.
- [SetAuthCookie(*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) – calls the GetAuthCookie(*username*, *persistCookie*) method to generate the forms authentication cookie. Bu yöntem, ardından (tanımlama bilgisi tabanlı form kimlik doğrulaması; kullanılmıyorsa, cookieless bilet mantığı işler bir iç sınıf bu yöntemi çağırır varsayılarak) tanımlama bilgileri koleksiyonu GetAuthCookie tarafından döndürülen tanımlama bilgisi ekler.
- [RedirectFromLoginPage(*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) – this method calls SetAuthCookie(*username*, *persistCookie*), and then redirects the user to the appropriate page.

Tanımlama bilgisi için tanımlama bilgileri koleksiyonu yazmadan önce kimlik doğrulaması bileti değiştirmeniz gerektiğinde GetAuthCookie kullanışlıdır. Forms kimlik doğrulaması bileti oluşturun ve tanımlama bilgileri koleksiyona eklemek istediğiniz, ancak kullanıcı uygun sayfasına yeniden yönlendir istemediğiniz SetAuthCookie yararlıdır. Belki de oturum açma sayfasında kalmalarını ya da diğer bazı sayfasına göndermek istiyorsunuz.

Kullanıcı oturum ve bunları uygun sayfasına yeniden yönlendirmek istiyoruz beri RedirectFromLoginPage kullanalım. Oturum Aç düğmesini'nın tıklatın güncelleştirme olay işleyicisi, aşağıdaki kod satırını ile iki açıklamalı Yapılacaklar satırları değiştirme:

FormsAuthentication.RedirectFromLoginPage(UserName.Text, RememberMe.Checked);

Forms kimlik doğrulaması bileti oluşturulurken kullanıcıadı TextBox'ın metin özelliğini forms kimlik doğrulaması bileti için kullanırız *kullanıcıadı* parametre ve kullandığınız Beni anımsa onay kutusunu işaretli durumu  *persistCookie* parametresi.

Oturum açma sayfasını test etmek için bir tarayıcıda ziyaret edin. Bir kullanıcı adı "Nope" ve "yanlış" bir parola gibi geçersiz kimlik bilgileri girerek başlayın. Oturum açma düğmesi tıklatıldığında geri gönderimin ortaya çıkar ve InvalidCredentialsMessage etiketi görüntülenir.


[![InvalidCredentialsMessage etiketi görüntülenen girme geçersiz kimlik bilgilerini belirtin](an-overview-of-forms-authentication-cs/_static/image25.png)](an-overview-of-forms-authentication-cs/_static/image24.png)

**Şekil 10**: InvalidCredentialsMessage etiketidir görüntülenen zaman girme geçersiz kimlik bilgileri ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-forms-authentication-cs/_static/image26.png))


Ardından, geçerli kimlik bilgilerini girin ve oturum açma düğmesine tıklayın. Geri gönderme forms kimlik doğrulaması bileti oluştuğunda bu zaman oluşturulur ve otomatik olarak geri Default.aspx yönlendirilir. Vardır, ancak hiçbir görsel ipuçları, oturum açmış olduğunu belirtmek için bu noktada, Web sitesine oturum. Program aracılığıyla bir kullanıcı olup olmadığını belirlemek nasıl göreceğiz 4. adımda ya da sayfasını ziyaret kullanıcıyı tanımlamak nasıl yanı sıra günlüğe kaydedilir.

Adım 5 ' Web sitesi dışında bir kullanıcı oturum teknikleri inceler.

### <a name="securing-the-login-page"></a>Oturum açma sayfasına güvenliğini sağlama

Kullanıcı kendi kimlik bilgilerini girer ve oturum açma sayfası formunun gönderir – kendi parola gibi – kimlik bilgilerini de web sunucusu için Internet üzerinden iletilirken *düz metin*. Ağ trafiği algılaması korsan kullanıcı adı ve parola görebilirsiniz anlamına gelir. Bunu önlemek için bunu kullanarak ağ trafiğini şifrelemek için önemlidir [Güvenli Yuva Katmanı (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Bu, kimlik bilgilerini (yanı sıra tüm sayfanın HTML biçimlendirmesi) web sunucusu tarafından alınana kadar kullanıcılar tarayıcı bırakın andan şifrelenmiş güvence altına alır.

Web sitenizi hassas bilgiler içermediği sürece yalnızca burada kullanıcının parolasını aksi düz metin kablo üzerinden gönderilecek SSL oturum açma sayfasında ve diğer sayfalarda kullanmanız gerekecektir. Varsayılan olarak, hem şifrelenmiş ve (üzerinde oynanmasını önlemek için) dijital olarak imzalı olduğundan, forms kimlik doğrulaması bileti güvenliğini sağlama hakkında endişelenmeniz gerekmez. Forms kimlik doğrulaması bileti güvenliği hakkında daha kapsamlı bir tartışma aşağıdaki öğreticide sunulur.

> [!NOTE]
> Birçok finansal ve tıbbi Web sitesi üzerinde SSL kullanmak üzere yapılandırılmış *tüm* sayfaları için erişilebilir kimliği doğrulanmış kullanıcılara. Forms kimlik doğrulaması sistem forms kimlik doğrulaması bileti yalnızca güvenli bir bağlantı üzerinden aktarılan yapılandırabilirsiniz Web oluşturuluyorsa. Çeşitli forms kimlik doğrulaması yapılandırma seçeneklerini sonraki öğreticide görüneceğini  *[Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular](forms-authentication-configuration-and-advanced-topics-cs.md)*.


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Adım 4: Kimliği doğrulanmış ziyaretçileri algılama ve kimliklerini belirleme

Bu noktada biz form kimlik doğrulaması etkin ve bir ilkel oturum açma sayfası oluşturuldu, ancak nasıl biz bir kullanıcı kimliği doğrulanmış veya anonim olup olmadığını belirleyebilirsiniz incelemek henüz. Bazı senaryolarda biz farklı veri veya kimliği doğrulanmış veya anonim kullanıcı sayfasını olup ziyaret bağlı olarak bilgileri görüntülemek isteyebilirsiniz. Ayrıca, biz görmemeleri kimliği doğrulanmış kullanıcının kimliğinin bilinmesi gerekir.

Şimdi bu teknikler göstermek için varolan Default.aspx sayfasında kullanmasıdır. Default.aspx içinde iki Panel denetimleri, bir adlandırılmış AuthenticatedMessagePanel ve başka bir adlandırılmış AnonymousMessagePanel ekleyin. İlk panelinde WelcomeBackMessage adlı bir etiket denetimi ekleyin. İkinci panelinde köprü denetim ekleme, kendi "Oturum Aç" Text özelliğine ve kendi NavigateUrl özelliğine Ayarla "~ / Login.aspx". Bu noktada Default.aspx için bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample6.aspx)]

Büyük olasılıkla artık tahmin gibi buradaki yalnızca AuthenticatedMessagePanel kimliği doğrulanmış ziyaretçileri ve yalnızca Anonim ziyaretçileri AnonymousMessagePanel görüntülemektir. Bunu gerçekleştirmek için kullanıcının veya oturum açtığı bağlı olarak bu panoları görünür özelliklerini ayarlamak ihtiyacımız.

[Request.IsAuthenticated özelliği](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) istek kimliği doğrulanmış olup olmadığını gösteren bir Boole değeri döndürür. Aşağıdaki kod sayfasına girin\_yük olay işleyici kodu:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample7.cs)]

Bu kodu yerinde bir tarayıcıdan Default.aspx ziyaret edin. Oturum açmak henüz varsayılarak, oturum açma sayfasına bir bağlantı göreceksiniz (bkz. Şekil 11). Bu bağlantıyı tıklatın ve siteye oturum açın. Adım 3'te gördüğümüz gibi kimlik bilgilerinizi girdikten sonra için Default.aspx döndürülür, ancak bu kez sayfa "Hoş Geldiniz geri!" gösterir. (bkz. Şekil 12) iletisi.


![Ziyaret eden anonim olarak, bir günlük bağlantısını görüntülendiğinde](an-overview-of-forms-authentication-cs/_static/image27.png)

**Şekil 11**: zaman ziyaret anonim olarak, bir günlük bağlantısını görüntülenir


![Kimliği doğrulanmış kullanıcılara gösterilir](an-overview-of-forms-authentication-cs/_static/image28.png)

**Şekil 12**: kimliği doğrulanmış kullanıcılar "yeniden Hoş Geldiniz!" gösterilir İleti


Şu anda oturum açmış kullanıcının kimliğini aracılığıyla belirleriz [HttpContext nesnesi](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)'s [kullanıcı özelliği](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx). HttpContext nesne geçerli istek hakkındaki bilgileri temsil eder ve yanıt, isteğin ve oturumu olarak ortak gibi ASP.NET nesnelerin giriş diğerleriyle birlikte durumda. Kullanıcı özelliği geçerli HTTP isteği ve uygulayan güvenlik bağlamını temsil eder [IPrincipal arabirimi](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

Kullanıcı özelliği FormsAuthenticationModule tarafından ayarlanır. Özellikle, FormsAuthenticationModule gelen istekte bir form kimlik doğrulama anahtarının bulduğunda, yeni bir GenericPrincipal nesnesi oluşturur ve kullanıcı özelliğine atar.

Asıl nesneler (örneğin, GenericPrincipal) kullanıcının kimliğini ve ait oldukları rolleri hakkında bilgi sağlar. IPrincipal arabirimi iki üyeleri tanımlar:

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) – asıl belirtilen role ait olup olmadığını gösteren bir Boole değeri döndüren bir yöntem.
- [Kimlik](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) – uygulayan bir nesne döndüren bir özelliği [IIdentity arabirimi](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). Üç özellik IIdentity arabirimi tanımlar: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx), ve [adı](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Aşağıdaki kodu kullanarak geçerli ziyaretçi adını belirleriz:

currentUsersName dize User.Identity.Name; =

Forms kimlik doğrulaması, kullanma, bir [FormsIdentity nesne](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) GenericPrincipal'ın kimliği özelliği için oluşturulur. FormsIdentity sınıfı "Form" dizesini AuthenticationType özelliğine ve kendi IsAuthenticated özelliği true her zaman döndürür. Name özelliği, forms kimlik doğrulaması bileti oluşturulurken belirtilen kullanıcı adını döndürür. Bu üç özelliğe ek olarak, temel kimlik doğrulaması bileti erişimi FormsIdentity içerir, [bilet özelliği](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). Ticket özelliğine türünde bir nesne döndürür [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), sona erme, IsPersistent, IssueDate, ad vb. gibi özelliklere sahiptir.

İşte çıkardığınız gereken en önemli nokta *kullanıcıadı* FormsAuthentication.GetAuthCookie belirtilen parametresi (*kullanıcıadı*, *persistCookie*), FormsAuthentication.SetAuthCookie (*kullanıcıadı*, *persistCookie*) ve FormsAuthentication.RedirectFromLoginPage (*kullanıcıadı*, *persistCookie*) yöntemleri User.Identity.Name tarafından döndürülen aynı değerdir. Ayrıca, bu yöntemleri ile oluşturulan kimlik doğrulaması bileti FormsIdentity nesnesine User.Identity atama ve Ticket özelliğine erişme mevcuttur:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample8.cs)]

Şimdi Default.aspx daha kişiselleştirilmiş bir ileti girin. Sayfa güncelleştirmesi\_WelcomeBackMessage etiketin metin özelliğini dize atanan böylece olay işleyicisi yük "tekrar, Hoş Geldiniz *kullanıcıadı*!"

WelcomeBackMessage.Text = "Yeniden Hoş Geldiniz" + User.Identity.Name + "!";

Şekil 13 bu değişikliği etkisini (Scott kullanıcı olarak oturum açarken) gösterir.


![Hoş Geldiniz iletisi şu anda oturum açmış kullanıcının adını içerir](an-overview-of-forms-authentication-cs/_static/image29.png)

**Şekil 13**: Hoş Geldiniz iletisi şu anda oturum açmış kullanıcının adını içerir


### <a name="using-the-loginview-and-loginname-controls"></a>LoginView ve LoginName denetimleri kullanma

Kimliği doğrulanmış ve anonim kullanıcılar için farklı içerik görüntüleme ortak bir gereksinimdir; Bu nedenle şu anda oturum açmış kullanıcının adını görüntüleme. Bu nedenle, Şekil 13'te, ancak tek satırlık bir kod yazmak zorunda kalmadan gösterilen aynı işlevselliği sağlayan iki Web denetimleri ASP.NET içerir.

[LoginView denetimi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) kimliği doğrulanmış ve anonim kullanıcılar için farklı verileri görüntülemek kolaylaştıran şablon temelli bir Web denetimi. (Link) iki önceden tanımlanmış şablonlar içerir:

- Anonymous – bu şablona eklenen işaretleme yalnızca Anonim ziyaretçilerine görüntülenir.
- LoggedInTemplate – bu şablonun işaretleme yalnızca kimliği doğrulanmış kullanıcılara gösterilir.

LoginView denetimi bizim sitenin ana sayfaya, Site.master ekleyelim. Yalnızca LoginView denetimi ekleme, yerine yine de her iki yeni ContentPlaceHolder denetiminin ekleyelim ve bu yeni ContentPlaceHolder LoginView denetimine yerleştirin. Bu karara stratejinin kısa süre içinde görünür olur.

> [!NOTE]
> Anonymous ve LoggedInTemplate ek olarak, LoginView denetimi role özgü şablonları ekleyebilirsiniz. Role özgü şablonları biçimlendirme için belirli bir role ait kullanıcılar gösterin. Sonraki öğreticide LoginView denetimi rol tabanlı özellikleri inceleyeceğiz.


Gezinti içinde ana sayfasına LoginContent adlı bir ContentPlaceHolder eklemeye başlayın &lt;div&gt; öğesi. Sonuçta elde edilen biçimlendirme yerleştirme Araç Kutusu'ndan kaynağı görünümü üzerine ContentPlaceHolder denetimi yalnızca sürükleyebilirsiniz üzerinde doğru "TODO: menü buraya yazılacak..." metin.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample9.aspx)]

Ardından, LoginContent ContentPlaceHolder LoginView denetimine ekleyin. Ana sayfanın ContentPlaceHolder denetimlere yerleştirilen içerik olarak kabul edilir *varsayılan içerik* ContentPlaceHolder için. Diğer bir deyişle, bu ana sayfa kullanan ASP.NET sayfaları için her ContentPlaceHolder kendi içeriği belirtin veya ana sayfa varsayılan içerik kullanın.

(Link) ve diğer oturum açma ile ilgili denetimler araç kutusu'nın oturum açma sekmesinde bulunur.


![Araç kutusu, LoginView denetimi](an-overview-of-forms-authentication-cs/_static/image30.png)

**Şekil 14**: araç çubuğuna LoginView denetiminde


Ardından, iki eklemek &lt;br /&gt; öğeleri LoginView denetimi hemen sonra ancak hala ContentPlaceHolder içinde. Bu noktada, gezinti &lt;div&gt; öğenin biçimlendirme, aşağıdaki gibi görünmelidir:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample10.aspx)]

(Link)'ın şablonları Tasarımcısı veya bildirim temelli biçimlendirme tanımlanabilir. Visual Studio'nun Tasarımcısı'ndan bir aşağı açılan listesinde yapılandırılmış şablonlarını listeler (link)'ın akıllı etiket genişletin. Metin türü stranger Anonymous; "Hello," Ardından, bir köprü denetimi ekleyin ve "Oturum Aç" için metin ve NavigateUrl özelliklerini ayarlamak ve "~ / Login.aspx" sırasıyla.

Anonymous yapılandırdıktan LoggedInTemplate geçin ve "Yeniden Hoş Geldiniz," metin girin. Ardından LoginName Denetim Araç Kutusu'ndan hemen "Hoş Geldiniz sonrasında geri" metin yerleştirme LoggedInTemplate içine sürükleyin. [LoginName denetim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), adından da anlaşılacağı, şu anda oturum açmış olan kullanıcının adını görüntüler. Dahili olarak, LoginName denetim User.Identity.Name özelliği yalnızca çıkarır

Bu eklemeleri (link)'ın şablonlarına yaptıktan sonra biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample11.aspx)]

Site.master ana sayfaya bu eklenmesiyle bizim Web sitesindeki her bir sayfa olup kullanıcının kimliği doğrulanır bağlı olarak farklı bir ileti görüntüler. Şekil 15 bir tarayıcıdan Jisun kullanıcı tarafından ziyaret edildiğinde Default.aspx sayfasında gösterilir. "Geri Hoş Geldiniz Jisun" iletisini iki kez yinelenir: ana sayfa gezinti bölümünde sol (aracılığıyla, az önce eklediğimiz LoginView denetimi), bir kez de Default.aspx'ın içinde içerik alanının (Panel denetimleri ve üzerinden programlı mantığı).


![LoginView denetimi görüntüler](an-overview-of-forms-authentication-cs/_static/image31.png)

**Şekil 15**: LoginView denetimi görüntüler "Hoş Geldiniz geri Jisun."


Ana sayfaya (link) eklediğimiz çünkü sitemizi her sayfada görünebilir. Ancak, olabilir web sayfaları burada Biz bu iletiyi gösterme istemiyorum. Oturum açma sayfasına bir bağlantı dışında yer yok gibi görünüyor bu yana bir tür oturum açma sayfasına sayfasıdır. Biz LoginView denetimi ContentPlaceHolder ana sayfasında yerleştirilmiş olduğundan, bu varsayılan biçimlendirme içerik sayfamızı kılabilirsiniz. Login.aspx açın ve Tasarımcı'ya gidin. Biz açıkça içerik denetimi tanımlamadığınız beri ana sayfasında LoginContent ContentPlaceHolder için Login.aspx içinde oturum açma sayfasına bu ContentPlaceHolder için ana sayfa varsayılan biçimlendirme gösterir. Bu varsayılan biçimlendirme (LoginView denetimi) LoginContent ContentPlaceHolder gösterir Designer üzerinden – görebilirsiniz.


[![Oturum açma sayfasına varsayılan ana sayfa LoginContent ContentPlaceHolder için içerik gösterir](an-overview-of-forms-authentication-cs/_static/image33.png)](an-overview-of-forms-authentication-cs/_static/image32.png)

**Şekil 16**: oturum açma sayfasına için ana sayfa LoginContent ContentPlaceHolder içerik varsayılan gösterir ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-forms-authentication-cs/_static/image34.png))


Varsayılan biçimlendirme için LoginContent ContentPlaceHolder geçersiz kılmak için Tasarımcısı'nda Bölge sağ tıklayın ve bağlam menüsünden özel içerik oluşturma seçeneği belirleyin. (Visual Studio 2008 ContentPlaceHolder kullanarak içerdiğinde bir akıllı etiket, seçili olduğunda, aynı seçeneği sunar.) Bu yeni bir içerik denetimi sayfanın biçimlendirme ve böylece ekler bize bu sayfa için özel içerik tanımlamanızı sağlar. "Lütfen inç. oturum gibi" Buraya, özel bir ileti eklemek ancak şimdi yalnızca bu alanı boş bırakın.

> [!NOTE]
> Visual Studio 2005'te özel içerik oluşturma oluşturur boş bir ASP.NET sayfası denetiminde içerik. Visual Studio 2008'de, ancak özel içerik oluşturma ana sayfanın varsayılan içeriğinin yeni oluşturulan içerik denetimine kopyalar. Visual Studio 2008 kullanıyorsanız, daha sonra yeni içerik denetimi oluşturduktan sonra üzerinden ana sayfadan kopyalanan içeriği temizlemek emin olun.


Şekil 17 bu değişikliği yaptıktan sonra bir tarayıcıdan sitesini ziyaret ettiğinizde Login.aspx sayfasına gösterir. Hiçbir stranger "Hello," olduğuna dikkat edin veya "tekrar, Hoş Geldiniz *kullanıcıadı*" iletisi sol gezinti bölmesinde &lt;div&gt; Default.aspx ziyaret eden olduğundan.


[![Oturum açma sayfasına varsayılan LoginContent ContentPlaceHolder'ın biçimlendirme gizler](an-overview-of-forms-authentication-cs/_static/image36.png)](an-overview-of-forms-authentication-cs/_static/image35.png)

**Şekil 17**: oturum açma sayfasına varsayılan LoginContent ContentPlaceHolder'ın biçimlendirme gizler ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-forms-authentication-cs/_static/image37.png))


## <a name="step-5-logging-out"></a>Adım 5: Oturum kapatılıyor

Adım 3'te biz sitede bir kullanıcı oturum için bir oturum açma sayfası derlemeye Aranan, ancak bir kullanıcı oturum kapatma hakkında bilgi henüz. Bir kullanıcı oturum yöntemlerine ek olarak, FormsAuthentication sınıfı sağlar bir [SignOut yöntemi](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). SignOut yöntemi, böylece kullanıcı site dışında günlüğü forms kimlik doğrulaması bileti yalnızca bozar.

ASP.NET, ortak bir özelliği günlüktür bağlantı kullanıma sunan bir kullanıcı oturum kapatma için özellikle tasarlanmış bir denetim içerir. [Bu denetim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) "Login" LinkButton veya kullanıcının kimlik doğrulama durumuna bağlı olarak bir "Oturum kapatma" LinkButton görüntüler. "Oturum kapatma" LinkButton kimliği doğrulanmış kullanıcılara gösterilir ancak bir "Login" LinkButton anonim kullanıcılar için işlenir. "Login" ve "Oturum kapatma" LinkButtons için metin LoginStatus kişinin yapılandırılabilir LoginText ve LogoutText özellikleri.

"Login" LinkButton tıklatarak bir yeniden yönlendirme oturum açma sayfasına verildiği geri gönderme neden olur. "Oturum kapatma" LinkButton tıklayarak FormsAuthentication.SignOff yöntemini çağırmak bu denetim neden olur ve ardından kullanıcı bir sayfaya yönlendirir. Sayfa oturum açmış kullanıcının oturumunu bağlıdır üç aşağıdaki değerlerden birine atanan LogoutAction özellikte yönlendirilir:

- – Varsayılan yenileme; Kullanıcı yalnızca ziyaret sayfasına yönlendirir. Yalnızca ziyaret sayfa anonim kullanıcıların izin vermez, ardından FormsAuthenticationModule otomatik olarak kullanıcı oturum açma sayfasına yönlendirir.

Neden bir yeniden yönlendirme burada gerçekleştirilir konusunda merak olabilir. Kullanıcı aynı sayfada kalmak isterse neden açık yeniden yönlendirme gereksinimini? "Kapatma" LinkButton tıklandığında, kullanıcı hala forms kimlik doğrulaması bileti kendi tanımlama bilgileri koleksiyonu olduğundan nedenidir. Sonuç olarak, geri gönderme isteği kimliği doğrulanmış bir istektir. Bu denetim SignOut yöntemini çağırır, ancak FormsAuthenticationModule kullanıcı kimliğini doğrulamasından sonra gerçekleşir. Bu nedenle, açık bir yeniden yönlendirme sayfayı yeniden istemek tarayıcı neden olur. Tarayıcı istekler sayfası yeniden süresi, forms kimlik doğrulaması bileti kaldırıldı ve bu nedenle gelen istek anonim.

- Yeniden yönlendirme – kullanıcı LoginStatus'ın LogoutPageUrl özelliği tarafından belirtilen URL'ye yeniden yönlendirilir.
- RedirectToLoginPage – kullanıcı oturum açma sayfasına yeniden yönlendirilir.

Şimdi bu denetim için ana sayfa ekleyin ve bunlar oturumunuz kapatıldı olduğunu onaylayan bir ileti görüntüler bir sayfaya kullanıcıya göndermek için yeniden yönlendirme seçeneği kullanacak şekilde yapılandırın. Logout.aspx adlı kök dizininde bir sayfa oluşturarak başlayın. Bu sayfa Site.master ana sayfa ile ilişkilendirilecek unutmayın. Ardından, sayfanın biçimlendirme bunlar günlüğe kaydedilmiş çıkışı kullanıcıya açıklayan bir ileti girin.

Ardından, Site.master ana sayfasına dönün ve bu denetim (link) altında LoginContent ContentPlaceHolder'ekleyin. Bu denetim o kullanıcının LogoutAction özelliğini yeniden yönlendirme ve onun LogoutPageUrl özelliğini ayarlama "~ / Logout.aspx".

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample12.aspx)]

LoginStatus LoginView denetimi dışında olduğundan, anonim ve kimliği doğrulanmış kullanıcılar için görünür, ancak LoginStatus "Login" veya "Oturum kapatma" LinkButton düzgün görüntülenmesi için bu normaldir. Bu denetim Ayrıca, Anonymous "Oturum Aç" Köprü gereksiz, bu nedenle kaldırın.

Jisun ziyaret ettiğinde Şekil 18 Default.aspx gösterir. Sol sütunda "geri Jisun oturum kapatma için bir bağlantı birlikte Hoş Geldiniz" iletisi görüntülenir. Oturumu kapatma LinkButton tıklayarak geri gönderimin neden olur, Jisun dışında sistem imzalar ve her Logout.aspx için yeniden yönlendirir. Şekil 19 gösterildiği gibi Jisun Logout.aspx ulaştığında zamana göre aynen zaten imzalanmış ve bu nedenle anonim. Sonuç olarak, metnin sol sütunda görüntülenir ", stranger ve Hoş Geldiniz" oturum açma sayfasına bir bağlantı.


[![Default.aspx Shows](an-overview-of-forms-authentication-cs/_static/image39.png)](an-overview-of-forms-authentication-cs/_static/image38.png)

**Şekil 18**: Default.aspx gösterir "Hoş Geldiniz geri Jisun" "Oturum kapatma" LinkButton birlikte ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-forms-authentication-cs/_static/image40.png))


[![Logout.aspx Shows](an-overview-of-forms-authentication-cs/_static/image42.png)](an-overview-of-forms-authentication-cs/_static/image41.png)

**Şekil 19**: Logout.aspx gösterir "Hoş Geldiniz, stranger" "Login" LinkButton birlikte ([tam boyutlu görüntüyü görüntülemek için tıklatın](an-overview-of-forms-authentication-cs/_static/image43.png))


> [!NOTE]
> (4. adım Login.aspx için yaptığımız gibi) ana sayfanın LoginContent ContentPlaceHolder gizlemek için Logout.aspx sayfa özelleştirmenizi öneririz. "Login" LinkButton bu denetim tarafından işlenen neden olduğundan (altındaki bir "stranger Hello,") ReturnUrl sorgu dizesi parametresi geçerli URL geçirerek oturum açma sayfası kullanıcı gönderir. Kısacası, bunlar oturum açmış çıkışı bir kullanıcı bu LoginStatus'ın "Login" LinkButton ve ardından günlüklerinde tıklarsa, hangi kullanıcı kolayca karıştırır geri Logout.aspx için yönlendirilir.


## <a name="summary"></a>Özet

Bu öğreticide form kimlik doğrulama iş akışı bir incelenmesi başlatıldı ve forms kimlik doğrulaması bir ASP.NET uygulamasındaki uygulama için etkinleştirilmiş. Form kimlik doğrulaması iki sorumlulukları vardır FormsAuthenticationModule tarafından desteklenir: kendi forms kimlik doğrulaması bileti temel alarak kullanıcılara tanımlama ve yetkisiz kullanıcıların oturum açma sayfasına yeniden yönlendirmeye.

.NET Framework'ün FormsAuthentication sınıfı oluşturma, inceleme ve form kimlik doğrulama biletlerini kaldırma yöntemleri içerir. Request.IsAuthenticated özelliğini ve kullanıcı nesnesi istek olup olmadığı doğrulanır ve kullanıcının kimlik bilgilerini belirlemek için ek programlama desteği sağlar. Ayrıca oturum açma ile ilgili birçok ortak görevleri gerçekleştirmek için hızlı ve Kodsuz bir yol geliştiriciler vermek LoginView, LoginStatus ve LoginName Web denetimleri vardır. Sonraki öğreticilerde bu ve diğer oturum açma ile ilgili Web denetimleri daha ayrıntılı inceleyeceğiz.

Bu öğretici, form kimlik doğrulaması basit bir genel bakış sağlanır. Biz değil getirilebilir yapılandırma seçenekleri inceleyin, nasıl cookieless forms kimlik doğrulaması bilet iş arayın veya ASP.NET forms kimlik doğrulaması bileti içeriğini nasıl koruduğunu keşfedin. Bu konular ve daha fazla bilgi için aşağıdakiler ele alınacaktır [sonraki öğretici](forms-authentication-configuration-and-advanced-topics-cs.md).

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [IIS6 ve IIS7 güvenlik arasındaki değişiklikleri](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Oturum açma ASP.NET denetimleri](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Profesyonel ASP.NET 2.0 güvenlik, üyelik ve rol yönetimi](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [`<authentication>` Öğesi](https://msdn.microsoft.com/library/532aee0e.aspx)
- [`<forms>` Öğesi için `<authentication>`](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Bu öğreticide yer alan konularda video eğitim

- [ASP.NET’te Temel Forms Kimlik Doğrulaması Kullanma](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler...

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme serisi pek çok yararlı gözden geçirenler tarafından gözden Bu öğretici oluştu. Bu öğretici için sağlama gözden geçirenler Alicja Maziarz, John Suru ve Teresa Murphy içerir. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](security-basics-and-asp-net-support-cs.md)
> [sonraki](forms-authentication-configuration-and-advanced-topics-cs.md)
