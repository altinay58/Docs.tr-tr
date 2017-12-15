---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: "Ana sayfaları ve kısmi kullanarak kullanıcı Arabirimi yeniden kullanma | Microsoft Docs"
author: microsoft
description: "Adım 7 'KURU ilkesini' uygulayabilmeniz için yöntemler kısmi görünüm şablonları ve ana sayfalar kullanarak kod yinelemesinden ortadan kaldırmak için Görünüm şablonlarımız içinde arar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: c42cd6aca40b08a9f8461532fbfd0589901b64ad
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="re-use-ui-using-master-pages-and-partials"></a><span data-ttu-id="59da8-103">Ana sayfaları ve kısmi kullanarak kullanıcı Arabirimi yeniden kullanma</span><span class="sxs-lookup"><span data-stu-id="59da8-103">Re-use UI Using Master Pages and Partials</span></span>
====================
<span data-ttu-id="59da8-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="59da8-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="59da8-105">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="59da8-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="59da8-106">Bu adım 7 bir ücretsiz olan ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.</span><span class="sxs-lookup"><span data-stu-id="59da8-106">This is step 7 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="59da8-107">Adım 7 biz "KURU ilkesini" uygulayabilirsiniz yollardan kısmi görünüm şablonları ve ana sayfalar kullanarak kod yinelemesinden ortadan kaldırmak için Görünüm şablonlarımız içinde arar.</span><span class="sxs-lookup"><span data-stu-id="59da8-107">Step 7 looks at ways we can apply the "DRY Principle" within our view templates to eliminate code duplication, using partial view templates and master pages.</span></span>
> 
> <span data-ttu-id="59da8-108">ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.</span><span class="sxs-lookup"><span data-stu-id="59da8-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-7-partials-and-master-pages"></a><span data-ttu-id="59da8-109">NerdDinner adım 7: Kısmi ve ana sayfalar</span><span class="sxs-lookup"><span data-stu-id="59da8-109">NerdDinner Step 7: Partials and Master Pages</span></span>

<span data-ttu-id="59da8-110">ASP.NET MVC kapsayan tasarım felsefeleri (genellikle "KURU" adlandırılır) "Yapmak değil yineleyin kendiniz" ilkesini biridir.</span><span class="sxs-lookup"><span data-stu-id="59da8-110">One of the design philosophies ASP.NET MVC embraces is the "Do Not Repeat Yourself" principle (commonly referred to as "DRY").</span></span> <span data-ttu-id="59da8-111">KURU tasarım kod ve sonuçta uygulamaları oluşturmak için hızlı ve sürdürmek daha kolay hale getirir mantığı yinelenmesini ortadan kaldırmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="59da8-111">A DRY design helps eliminate the duplication of code and logic, which ultimately makes applications faster to build and easier to maintain.</span></span>

<span data-ttu-id="59da8-112">Bizim NerdDinner senaryoları çeşitli uygulanan KURU ilkesini zaten gördük.</span><span class="sxs-lookup"><span data-stu-id="59da8-112">We've already seen the DRY principle applied in several of our NerdDinner scenarios.</span></span> <span data-ttu-id="59da8-113">Bazı örnekler: doğrulama mantığımızı arasında her iki düzenleme zorlanacak ve bizim denetleyicisi; senaryoları oluşturmanıza sağlar bizim modeli katman içinde uygulanır "Bulunamadı" görünümü şablon düzenleme, Ayrıntılar ve silme eylem yöntemleri arasında yeniden kullanıyoruz; Biz View() yardımcı yöntemini çağırdığınızda adı açıkça belirtme ihtiyacını ortadan kaldırır, görünümü şablonlarıyla bir kuralı - adlandırma deseni kullanıyoruz; ve DinnerFormViewModel sınıfı hem düzenleme için yeniden kullanıyorsanız ve eylem senaryoları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="59da8-113">A few examples: our validation logic is implemented within our model layer, which enables it to be enforced across both edit and create scenarios in our controller; we are re-using the "NotFound" view template across the Edit, Details and Delete action methods; we are using a convention- naming pattern with our view templates, which eliminates the need to explicitly specify the name when we call the View() helper method; and we are re-using the DinnerFormViewModel class for both Edit and Create action scenarios.</span></span>

<span data-ttu-id="59da8-114">Şimdi biz "KURU ilkesini" uygulayabilirsiniz yollardan kod yinelemesinden da ortadan kaldırmak için Görünüm şablonlarımız içinde bakalım.</span><span class="sxs-lookup"><span data-stu-id="59da8-114">Let's now look at ways we can apply the "DRY Principle" within our view templates to eliminate code duplication there as well.</span></span>

### <a name="re-visiting-our-edit-and-create-view-templates"></a><span data-ttu-id="59da8-115">Bizim düzenleme yeniden ziyaret edip görünüm şablonları oluşturma</span><span class="sxs-lookup"><span data-stu-id="59da8-115">Re-visiting our Edit and Create View Templates</span></span>

<span data-ttu-id="59da8-116">Şu anda Yemeği formumuzun UI görüntülemek için iki farklı görünüm şablonları – "Edit.aspx" ve "Create.aspx" – kullanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="59da8-116">Currently we are using two different view templates – "Edit.aspx" and "Create.aspx" – to display our Dinner form UI.</span></span> <span data-ttu-id="59da8-117">Bunları hızlı visual karşılaştırması oldukları nasıl benzer vurgular.</span><span class="sxs-lookup"><span data-stu-id="59da8-117">A quick visual comparison of them highlights how similar they are.</span></span> <span data-ttu-id="59da8-118">Create formun nasıl göründüğünü aşağıdadır:</span><span class="sxs-lookup"><span data-stu-id="59da8-118">Below is what the create form looks like:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

<span data-ttu-id="59da8-119">Ve burada "Düzenle" formumuzun benzer:</span><span class="sxs-lookup"><span data-stu-id="59da8-119">And here is what our "Edit" form looks like:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

<span data-ttu-id="59da8-120">Büyük bir fark var mı?</span><span class="sxs-lookup"><span data-stu-id="59da8-120">Not much of a difference is there?</span></span> <span data-ttu-id="59da8-121">Başlık ve üstbilgi metni dışında form düzenini ve giriş denetimlerini aynıdır.</span><span class="sxs-lookup"><span data-stu-id="59da8-121">Other than the title and header text, the form layout and input controls are identical.</span></span>

<span data-ttu-id="59da8-122">Biz "Edit.aspx" ve "Create.aspx" Yedekleme biz bulacaksınız görünüm şablonları açarsanız aynı form düzenini ve giriş denetim kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="59da8-122">If we open up the "Edit.aspx" and "Create.aspx" view templates we'll find that they contain identical form layout and input control code.</span></span> <span data-ttu-id="59da8-123">Bu çoğaltma, iki kez biz tanıtmak veya iyi olmayan bir yeni Yemeği özelliği - değiştirmek zaman değişiklik yapmak zorunda bitiş anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="59da8-123">This duplication means we end up having to make changes twice anytime we introduce or change a new Dinner property - which is not good.</span></span>

### <a name="using-partial-view-templates"></a><span data-ttu-id="59da8-124">Kısmi görünüm şablonları kullanma</span><span class="sxs-lookup"><span data-stu-id="59da8-124">Using Partial View Templates</span></span>

<span data-ttu-id="59da8-125">ASP.NET MVC görünümü işleme mantığı için bir sayfasının alt kısmında kapsüllemek için kullanılan "kısmi görünümü" şablonlarını tanımlama yeteneği destekler.</span><span class="sxs-lookup"><span data-stu-id="59da8-125">ASP.NET MVC supports the ability to define "partial view" templates that can be used to encapsulate view rendering logic for a sub-portion of a page.</span></span> <span data-ttu-id="59da8-126">"Kısmi" görünümü işleme mantığı kez tanımlamak için kullanışlı bir yöntem sunar ve sonra birden fazla yerde bir uygulama arasında yeniden kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="59da8-126">"Partials" provide a useful way to define view rendering logic once, and then re-use it in multiple places across an application.</span></span>

<span data-ttu-id="59da8-127">"KURU yukarı" bizim Edit.aspx ve Create.aspx görünüm şablonu çoğaltma yardımcı olmak için "form düzenini ve ortak giriş öğelerini Kapsüller DinnerForm.ascx" adlı bir kısmi görünüm şablonu oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="59da8-127">To help "DRY-up" our Edit.aspx and Create.aspx view template duplication, we can create a partial view template named "DinnerForm.ascx" that encapsulates the form layout and input elements common to both.</span></span> <span data-ttu-id="59da8-128">Bizim/görünümler/azalma dizinde sağ tıklayıp seçerek bunu "Ekle -&gt;görünümü" menü komutu:</span><span class="sxs-lookup"><span data-stu-id="59da8-128">We'll do this by right-clicking on our /Views/Dinners directory and choosing the "Add-&gt;View" menu command:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

<span data-ttu-id="59da8-129">Bu, "Görünüm Ekle" iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="59da8-129">This will display the "Add View" dialog.</span></span> <span data-ttu-id="59da8-130">Biz, biz "DinnerForm" oluşturmak istediğiniz iletişim içinde "kısmi Görünüm Oluştur" onay kutusunu seçin ve biz bunu DinnerFormViewModel sınıfı geçer belirtmek yeni görünüm adı:</span><span class="sxs-lookup"><span data-stu-id="59da8-130">We'll name the new view we want to create "DinnerForm", select the "Create a partial view" checkbox within the dialog, and indicate that we will pass it a DinnerFormViewModel class:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

<span data-ttu-id="59da8-131">Biz "Ekle" düğmesini tıklatın, Visual Studio yeni bir "DinnerForm.ascx" Görünüm şablonu bize "\Views\Dinners" dizini içinde oluşturur.</span><span class="sxs-lookup"><span data-stu-id="59da8-131">When we click the "Add" button, Visual Studio will create a new "DinnerForm.ascx" view template for us within the "\Views\Dinners" directory.</span></span>

<span data-ttu-id="59da8-132">Biz sonra kopyalayıp yinelenen form düzenini yapıştırın / bizim yeni "DinnerForm.ascx" kısmi görünüm şablonuna bizim Edit.aspx/ Create.aspx görünüm şablonlardan denetim kodu girin:</span><span class="sxs-lookup"><span data-stu-id="59da8-132">We can then copy/paste the duplicate form layout / input control code from our Edit.aspx/ Create.aspx view templates into our new "DinnerForm.ascx" partial view template:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

<span data-ttu-id="59da8-133">Biz DinnerForm kısmi şablon çağırın ve form çoğaltma ortadan kaldırmak için düzenleme ve oluşturma görünümü şablonlarımız sonra güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="59da8-133">We can then update our Edit and Create view templates to call the DinnerForm partial template and eliminate the form duplication.</span></span> <span data-ttu-id="59da8-134">Arama Html.RenderPartial("DinnerForm") tarafından görünüm şablonlarımız içinde bunu yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="59da8-134">We can do this by calling Html.RenderPartial("DinnerForm") within our view templates:</span></span>

##### <a name="createaspx"></a><span data-ttu-id="59da8-135">Create.aspx</span><span class="sxs-lookup"><span data-stu-id="59da8-135">Create.aspx</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a><span data-ttu-id="59da8-136">Edit.aspx</span><span class="sxs-lookup"><span data-stu-id="59da8-136">Edit.aspx</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

<span data-ttu-id="59da8-137">Açıkça Html.RenderPartial çağrılırken istediğiniz kısmi şablonun yolunu uygun (örneğin: ~ Views/Dinners/DinnerForm.ascx ").</span><span class="sxs-lookup"><span data-stu-id="59da8-137">You can explicitly qualify the path of the partial template you want when calling Html.RenderPartial (for example: ~Views/Dinners/DinnerForm.ascx").</span></span> <span data-ttu-id="59da8-138">Bizim kodda yine de size ASP.NET MVC içindeki kurala dayalı adlandırma deseni yararlanarak ve yalnızca "DinnerForm" işlenecek kısmi adını belirtme.</span><span class="sxs-lookup"><span data-stu-id="59da8-138">In our code above, though, we are taking advantage of the convention-based naming pattern within ASP.NET MVC, and just specifying "DinnerForm" as the name of the partial to render.</span></span> <span data-ttu-id="59da8-139">Biz bunu yaparken ASP.NET MVC (için DinnersController bu/görünümler/azalma olur) kurala dayalı görünümleri dizinde ilk arar.</span><span class="sxs-lookup"><span data-stu-id="59da8-139">When we do this ASP.NET MVC will look first in the convention-based views directory (for DinnersController this would be /Views/Dinners).</span></span> <span data-ttu-id="59da8-140">Kısmi şablonu bulamazsa vardır, ardından için /Views/Shared dizinini arar.</span><span class="sxs-lookup"><span data-stu-id="59da8-140">If it doesn't find the partial template there it will then look for it in the /Views/Shared directory.</span></span>

<span data-ttu-id="59da8-141">Html.RenderPartial() yalnızca kısmi görünüm adıyla çağrıldığında, ASP.NET MVC kısmi görünüme arama görünümü şablonu tarafından kullanılan aynı modeli ve ViewData sözlük nesneleri geçirin.</span><span class="sxs-lookup"><span data-stu-id="59da8-141">When Html.RenderPartial() is called with just the name of the partial view, ASP.NET MVC will pass to the partial view the same Model and ViewData dictionary objects used by the calling view template.</span></span> <span data-ttu-id="59da8-142">Alternatif olarak, bir alternatif Model nesnesi ve/veya kısmi görünümü kullanmak için ViewData sözlüğü geçirmenize olanak aşırı yüklenmiş Html.RenderPartial() sürümü vardır.</span><span class="sxs-lookup"><span data-stu-id="59da8-142">Alternatively, there are overloaded versions of Html.RenderPartial() that enable you to pass an alternate Model object and/or ViewData dictionary for the partial view to use.</span></span> <span data-ttu-id="59da8-143">Bu yalnızca bir alt kümesini tam modeli/ViewModel geçirmek istediğiniz senaryolar için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="59da8-143">This is useful for scenarios where you only want to pass a subset of the full Model/ViewModel.</span></span>

| <span data-ttu-id="59da8-144">**Yan konu: Neden &lt;%%&gt; yerine &lt;% = %&gt;?**</span><span class="sxs-lookup"><span data-stu-id="59da8-144">**Side Topic: Why &lt;% %&gt; instead of &lt;%= %&gt;?**</span></span> |
| --- |
| <span data-ttu-id="59da8-145">Fark yukarıdaki kodu ile birlikte Zarif şeyler biri biz kullanarak bir &lt;%%&gt; yerine engellemek bir &lt;% = %&gt; Html.RenderPartial() çağrılırken engelleyin.</span><span class="sxs-lookup"><span data-stu-id="59da8-145">One of the subtle things you might have noticed with the code above is that we are using a &lt;% %&gt; block instead of a &lt;%= %&gt; block when calling Html.RenderPartial().</span></span> <span data-ttu-id="59da8-146">&lt;% = %&gt; ASP.NET bloklarında gösteren bir geliştirici belirtilen değere işlemek istediği (örneğin: &lt;% = "Hello" %&gt; "Hello" hale getiren).</span><span class="sxs-lookup"><span data-stu-id="59da8-146">&lt;%= %&gt; blocks in ASP.NET indicate that a developer wants to render a specified value (for example: &lt;%= "Hello" %&gt; would render "Hello").</span></span> <span data-ttu-id="59da8-147">&lt;%%&gt; blokları yerine belirtmek Geliştirici kod yürütmek istiyor ve herhangi bir çıktı bunların içindeki işlenen açıkça yapılmalıdır (örneğin: &lt;Response.Write("Hello") %&gt;.</span><span class="sxs-lookup"><span data-stu-id="59da8-147">&lt;% %&gt; blocks instead indicate that the developer wants to execute code, and that any rendered output within them must be done explicitly (for example: &lt;% Response.Write("Hello") %&gt;.</span></span> <span data-ttu-id="59da8-148">Biz kullandığınız neden bir &lt;%%&gt; yukarıdaki bizim Html.RenderPartial kod bloğu olduğundan Html.RenderPartial() yöntemi bir dize döndürmez ve bunun yerine içeriği doğrudan çağıran şablonu görüntüleme animasyonun çıktı akışı çıkarır.</span><span class="sxs-lookup"><span data-stu-id="59da8-148">The reason we are using a &lt;% %&gt; block with our Html.RenderPartial code above is because the Html.RenderPartial() method doesn't return a string, and instead outputs the content directly to the calling view template's output stream.</span></span> <span data-ttu-id="59da8-149">Bunu performansı verimliliğini artırmak için ve (büyük olasılıkla çok büyük) geçici dize nesnesi oluşturma ihtiyacını ortadan kaldırır Böyle yaparak yapar.</span><span class="sxs-lookup"><span data-stu-id="59da8-149">It does this for performance efficiency reasons, and by doing so it avoids the need to create a (potentially very large) temporary string object.</span></span> <span data-ttu-id="59da8-150">Bu bellek kullanımını azaltır ve genel uygulama verimliliği artırır.</span><span class="sxs-lookup"><span data-stu-id="59da8-150">This reduces memory usage and improves overall application throughput.</span></span> <span data-ttu-id="59da8-151">Html.RenderPartial() kullanarak içinde olduğunda noktalı çağrı sonuna ekle unuttunuz zaman bir ortak hata bir &lt;%%&gt; bloğu.</span><span class="sxs-lookup"><span data-stu-id="59da8-151">One common mistake when using Html.RenderPartial() is to forget to add a semi-colon at the end of the call when it is within a &lt;% %&gt; block.</span></span> <span data-ttu-id="59da8-152">Örneğin, bu kodun bir derleyici hatası neden olur: &lt;Html.RenderPartial("DinnerForm") %&gt; yerine yazmanız gerekir: &lt;% Html.RenderPartial("DinnerForm"); %&gt; çünkü &lt;%%&gt; taşlarıdır müstakil kod deyimleri ve noktalı virgülle sonlandırılması gerekir deyimleri C# kod kullanırken.</span><span class="sxs-lookup"><span data-stu-id="59da8-152">For example, this code will cause a compiler error: &lt;% Html.RenderPartial("DinnerForm") %&gt; You instead need to write: &lt;% Html.RenderPartial("DinnerForm"); %&gt; This is because &lt;% %&gt; blocks are self-contained code statements, and when using C# code statements need to be terminated with a semi-colon.</span></span> |

### <a name="using-partial-view-templates-to-clarify-code"></a><span data-ttu-id="59da8-153">Kod açıklamak için kısmi görünüm şablonları kullanma</span><span class="sxs-lookup"><span data-stu-id="59da8-153">Using Partial View Templates to Clarify Code</span></span>

<span data-ttu-id="59da8-154">Görünüm işleme mantığı birden çok yerde çoğaltılmasını önlemek için "DinnerForm" kısmi görünüm şablonu oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="59da8-154">We created the "DinnerForm" partial view template to avoid duplicating view rendering logic in multiple places.</span></span> <span data-ttu-id="59da8-155">Kısmi görünüm şablonları oluşturmak için en yaygın nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="59da8-155">This is the most common reason to create partial view templates.</span></span>

<span data-ttu-id="59da8-156">Bazen hala bile bunlar yalnızca tek bir yerde çağrıldığında, kısmi görünümleri oluşturmak için mantıklıdır.</span><span class="sxs-lookup"><span data-stu-id="59da8-156">Sometimes it still makes sense to create partial views even when they are only being called in a single place.</span></span> <span data-ttu-id="59da8-157">Çok karmaşık görünüm şablonları genellikle zaman kendi Görünüm işleme mantığı ayıklanan ve birine bölümlenmiş ya da daha iyi kısmi şablonları adlı okumak çok daha kolay hale gelebilir.</span><span class="sxs-lookup"><span data-stu-id="59da8-157">Very complicated view templates can often become much easier to read when their view rendering logic is extracted and partitioned into one or more well named partial templates.</span></span>

<span data-ttu-id="59da8-158">Örneğin, göz önünde bulundurun (hangi biz en kısa süre içinde arayacaktır) Projemizin Site.master dosyasından kod parçacığı aşağıda.</span><span class="sxs-lookup"><span data-stu-id="59da8-158">For example, consider the below code-snippet from the Site.master file in our project (which we will be looking at shortly).</span></span> <span data-ttu-id="59da8-159">Görece düz İleri kodudur kısmen oturum açma/oturum kapatma görüntülemek için mantığı bağlamak için en üstünde – okumak için ekranın sağ "LogOnUserControl" kısmi içinde kapsüllenir:</span><span class="sxs-lookup"><span data-stu-id="59da8-159">The code is relatively straight-forward to read – partly because the logic to display a login/logout link at the top right of the screen is encapsulated within the "LogOnUserControl" partial:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

<span data-ttu-id="59da8-160">Kendiniz kafası bulduğunuz her çalışılırken bir görünüm şablonu içindeki html/kod işaretleme anlamak, oluştuysa, bunlardan bazıları ayıklanan ve iyi adlandırılmış kısmi görünümlere bulunanad daha anlaşılır olması olmayacaktır olup olmadığını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="59da8-160">Whenever you find yourself getting confused trying to understand the html/code markup within a view-template, consider whether it wouldn't be clearer if some of it was extracted and refactored into well-named partial views.</span></span>

### <a name="master-pages"></a><span data-ttu-id="59da8-161">Ana sayfalar</span><span class="sxs-lookup"><span data-stu-id="59da8-161">Master Pages</span></span>

<span data-ttu-id="59da8-162">Kısmi görünümler'ı desteklemenin yanında ASP.NET MVC ortak yerleşim ve bir sitenin üst düzey html tanımlamak için kullanılan "ana sayfa" şablonları oluşturma olanağı da destekler.</span><span class="sxs-lookup"><span data-stu-id="59da8-162">In addition to supporting partial views, ASP.NET MVC also supports the ability to create "master page" templates that can be used to define the common layout and top-level html of a site.</span></span> <span data-ttu-id="59da8-163">İçerik denetimleri sonra geçersiz ya da "görünümler tarafından doldurulmuş" değiştirebilen bölgeleri tanımlamak için ana sayfa eklenebilir yer tutucusu.</span><span class="sxs-lookup"><span data-stu-id="59da8-163">Content placeholder controls can then be added to the master page to identify replaceable regions that can be overridden or "filled in" by views.</span></span> <span data-ttu-id="59da8-164">Bu bir uygulama arasında ortak bir düzen uygulamak için çok etkili (ve KURU) bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="59da8-164">This provides a very effective (and DRY) way to apply a common layout across an application.</span></span>

<span data-ttu-id="59da8-165">Varsayılan olarak, yeni ASP.NET MVC projeleri otomatik olarak kendisine eklenmiş bir ana sayfa şablonu vardır.</span><span class="sxs-lookup"><span data-stu-id="59da8-165">By default, new ASP.NET MVC projects have a master page template automatically added to them.</span></span> <span data-ttu-id="59da8-166">Bu ana sayfa "Site.master" ve \Views\Shared\ klasördeki yaşamlarını adlandırılır:</span><span class="sxs-lookup"><span data-stu-id="59da8-166">This master page is named "Site.master" and lives within the \Views\Shared\ folder:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

<span data-ttu-id="59da8-167">Varsayılan Site.master dosyanın aşağıdaki gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="59da8-167">The default Site.master file looks like below.</span></span> <span data-ttu-id="59da8-168">Üst gezinti menüsü ile birlikte sitenin dış html tanımlar.</span><span class="sxs-lookup"><span data-stu-id="59da8-168">It defines the outer html of the site, along with a menu for navigation at the top.</span></span> <span data-ttu-id="59da8-169">– Bir başlık ve birincil içerik sayfasının burada değiştirilmesi gereken diğer iki değiştirilebilir içerik yer tutucusu denetimleri aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="59da8-169">It contains two replaceable content placeholder controls – one for the title, and the other for where the primary content of a page should be replaced:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

<span data-ttu-id="59da8-170">Tüm NerdDinner uygulamamız için ("Listesinde", "Ayrıntılar", "Düzenle", "Oluşturma", "Bulunamadı", vb.) oluşturduk görünüm şablonları bu Site.master şablona dayalı.</span><span class="sxs-lookup"><span data-stu-id="59da8-170">All of the view templates we've created for our NerdDinner application ("List", "Details", "Edit", "Create", "NotFound", etc) have been based on this Site.master template.</span></span> <span data-ttu-id="59da8-171">Bu varsayılan olarak en çok eklenen "MasterPageFile" özniteliği aracılığıyla belirtilir &lt;% @ sayfa %&gt; biz "Görünümü Ekle" iletişim kutusunu kullanarak bizim görünümler oluşturduğunuzda yönergesi:</span><span class="sxs-lookup"><span data-stu-id="59da8-171">This is indicated via the "MasterPageFile" attribute that was added by default to the top &lt;% @ Page %&gt; directive when we created our views using the "Add View" dialog:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

<span data-ttu-id="59da8-172">Ne bu biz Site.master içeriği değiştirebilirsiniz, ve sahip değişiklikleri otomatik olarak uygulanan ve biz bizim görünüm şablonlardan işleme sırasında kullanılan anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="59da8-172">What this means is that we can change the Site.master content, and have the changes automatically be applied and used when we render any of our view templates.</span></span>

<span data-ttu-id="59da8-173">Şimdi uygulamamız üstbilgisinin "MVC Uygulamam" yerine "NerdDinner" olmayacak şekilde bizim Site.master's üstbilgi bölümü güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="59da8-173">Let's update our Site.master's header section so that the header of our application is "NerdDinner" instead of "My MVC Application".</span></span> <span data-ttu-id="59da8-174">Şimdi; böylece ilk sekme "Bir (HomeController'ın İNDİS() eylem yöntemi tarafından işlenen) Yemeği Bul" olduğundan ve "Ana bilgisayar bir (DinnersController'ın Create() eylem yöntemi tarafından işlenen) Yemeği" adlı yeni bir sekme ekleyelim ayrıca bizim Gezinti Menüsü güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="59da8-174">Let's also update our navigation menu so that the first tab is "Find a Dinner" (handled by the HomeController's Index() action method), and let's add a new tab called "Host a Dinner" (handled by the DinnersController's Create() action method):</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

<span data-ttu-id="59da8-175">Biz Site.master dosyasını ve yenileme kaydettiğinizde bizim üstbilgi göreceğiz bizim tarayıcı Göster uygulamamız içindeki tüm görünümler arasında değişir.</span><span class="sxs-lookup"><span data-stu-id="59da8-175">When we save the Site.master file and refresh our browser we'll see our header changes show up across all views within our application.</span></span> <span data-ttu-id="59da8-176">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="59da8-176">For example:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

<span data-ttu-id="59da8-177">İle */Dinners/düzenleme / [kimlik]* URL'si:</span><span class="sxs-lookup"><span data-stu-id="59da8-177">And with the */Dinners/Edit/[id]* URL:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a><span data-ttu-id="59da8-178">Sonraki adım</span><span class="sxs-lookup"><span data-stu-id="59da8-178">Next Step</span></span>

<span data-ttu-id="59da8-179">Kısmi ve ana sayfa görünümleri düzgün bir şekilde düzenlemenizi sağlayan çok esnek seçenekler sağlar.</span><span class="sxs-lookup"><span data-stu-id="59da8-179">Partials and master pages provide very flexible options that enable you to cleanly organize views.</span></span> <span data-ttu-id="59da8-180">Bunlar görünümü çoğaltma önlemenize yardımcı olması içerik / kod ve görünüm şablonlarınızı okuyun ve sürdürmek daha kolay hale bulacaksınız.</span><span class="sxs-lookup"><span data-stu-id="59da8-180">You'll find that they help you avoid duplicating view content/ code, and make your view templates easier to read and maintain.</span></span>

<span data-ttu-id="59da8-181">Şimdi şimdi daha önce oluşturduğumuz listeleme senaryo yeniden ziyaret ve ölçeklenebilir sayfalama desteğini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="59da8-181">Let's now revisit the listing scenario we built earlier and enable scalable paging support.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="59da8-182">[Önceki](use-viewdata-and-implement-viewmodel-classes.md)
[sonraki](implement-efficient-data-paging.md)</span><span class="sxs-lookup"><span data-stu-id="59da8-182">[Previous](use-viewdata-and-implement-viewmodel-classes.md)
[Next](implement-efficient-data-paging.md)</span></span>