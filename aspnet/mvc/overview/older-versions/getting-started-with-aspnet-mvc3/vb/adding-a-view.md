---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: "Bir görünüm (VB) ekleme | Microsoft Docs"
author: Rick-Anderson
description: "Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7e8564c743510780b93d56bc1215f4c5b1faeb43
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view-vb"></a><span data-ttu-id="2f686-103">Bir görünüm (VB) ekleme</span><span class="sxs-lookup"><span data-stu-id="2f686-103">Adding a View (VB)</span></span>
====================
<span data-ttu-id="2f686-104">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="2f686-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="2f686-105">Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek.</span><span class="sxs-lookup"><span data-stu-id="2f686-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="2f686-106">Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="2f686-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="2f686-107">Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="2f686-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="2f686-108">Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="2f686-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="2f686-109">Visual Studio Web Developer Express SP1 önkoşulları</span><span class="sxs-lookup"><span data-stu-id="2f686-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="2f686-110">ASP.NET MVC 3 araçları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="2f686-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="2f686-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)</span><span class="sxs-lookup"><span data-stu-id="2f686-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="2f686-112">Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="2f686-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="2f686-113">VB.NET kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2f686-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="2f686-114">[VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="2f686-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="2f686-115">C# tercih ederseniz, geçiş [C# sürümü](../cs/adding-a-view.md) Bu öğreticinin.</span><span class="sxs-lookup"><span data-stu-id="2f686-115">If you prefer C#, switch to the [C# version](../cs/adding-a-view.md) of this tutorial.</span></span>


<span data-ttu-id="2f686-116">Bu bölümde değiştirmek için yapacağız `HelloWorldController` sınıfı bir görünüm şablonu dosyasına düzgün bir şekilde kullanmak için bir istemci HTML yanıtlarını oluşturma işlemine yalıtma.</span><span class="sxs-lookup"><span data-stu-id="2f686-116">In this section we're going to modify the `HelloWorldController` class to use a view template file to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="2f686-117">Şablonu görüntüleme kullanarak başlayalım `Index` yönteminde `HelloWorldController` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2f686-117">Let's start by using a view template with the `Index` method in the `HelloWorldController` class.</span></span> <span data-ttu-id="2f686-118">Şu anda `Index` yöntemi bir iletiyle içinde denetleyici sınıfı sabit kodlanmış bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="2f686-118">Currently the `Index` method returns a string with a message that is hard-coded within the controller class.</span></span> <span data-ttu-id="2f686-119">Değişiklik `Index` döndürülecek yöntemi bir `View` nesnesi, aşağıda gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="2f686-119">Change the `Index` method to return a `View` object, as shown in the following:</span></span>

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

<span data-ttu-id="2f686-120">Şimdi bir görünüm şablonu ile çağırabileceği bizim projesine ekleyelim `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2f686-120">Let's now add a view template to our project that we can invoke with the `Index` method.</span></span> <span data-ttu-id="2f686-121">Bunu yapmak için sağ tıklatın içinde `Index` yöntemi ve tıklatın **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="2f686-121">To do this, right-click inside the `Index` method and click **Add View**.</span></span>

<span data-ttu-id="2f686-122">[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2f686-122">[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)</span></span>

<span data-ttu-id="2f686-123">**Görünüm Ekle** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2f686-123">The **Add View** dialog box appears.</span></span> <span data-ttu-id="2f686-124">Varsayılan girişleri bırakabilir ve tıklatın **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="2f686-124">Leave the default entries and click the **Add** button.</span></span>

<span data-ttu-id="2f686-125">[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="2f686-125">[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)</span></span>

<span data-ttu-id="2f686-126">*MvcMovie\Views\HelloWorld* klasör ve *MvcMovie\Views\HelloWorld\Index.vbhtml* dosyası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2f686-126">The *MvcMovie\Views\HelloWorld* folder and the *MvcMovie\Views\HelloWorld\Index.vbhtml* file are created.</span></span> <span data-ttu-id="2f686-127">Bunları görebilirsiniz **Çözüm Gezgini**:</span><span class="sxs-lookup"><span data-stu-id="2f686-127">You can see them in **Solution Explorer**:</span></span>

<span data-ttu-id="2f686-128">[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="2f686-128">[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)</span></span>

<span data-ttu-id="2f686-129">Bazı HTML altında eklemek `<h2>` etiketi.</span><span class="sxs-lookup"><span data-stu-id="2f686-129">Add some HTML under the `<h2>` tag.</span></span> <span data-ttu-id="2f686-130">Değiştirilen *MvcMovie\Views\HelloWorld\Index.vbhtml* dosya aşağıda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2f686-130">The modified *MvcMovie\Views\HelloWorld\Index.vbhtml* file is shown below.</span></span>

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

<span data-ttu-id="2f686-131">Uygulamayı çalıştırın ve Gözat &quot;Merhaba Dünya&quot; denetleyici (`http://localhost:xxxx/HelloWorld`).</span><span class="sxs-lookup"><span data-stu-id="2f686-131">Run the application and browse to the &quot;hello world&quot; controller (`http://localhost:xxxx/HelloWorld`).</span></span> <span data-ttu-id="2f686-132">`Index` Denetleyicinizi yönteminde kadar iş yapmak olmadı; yalnızca bir deyim çalışan `return View()`, biz istemci yanıta işlemek için bir görünüm şablon dosyası kullanmak istediğinizi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="2f686-132">The `Index` method in your controller didn't do much work; it simply ran the statement `return View()`, which indicated that we wanted to use a view template file to render a response to the client.</span></span> <span data-ttu-id="2f686-133">Kullanılacak görünüm şablonu dosyasının adı biz açıkça belirtmediğinden, ASP.NET MVC kullanarak varsayılan *Index.vbhtml* görünüm dosyası içinde *\Views\HelloWorld* klasör.</span><span class="sxs-lookup"><span data-stu-id="2f686-133">Because we did not explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the *Index.vbhtml* view file within the *\Views\HelloWorld* folder.</span></span> <span data-ttu-id="2f686-134">Aşağıdaki görüntü görünümünde sabit kodlanmış dize gösterir.</span><span class="sxs-lookup"><span data-stu-id="2f686-134">The image below shows the string hard-coded in the view.</span></span>

<span data-ttu-id="2f686-135">[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="2f686-135">[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)</span></span>

<span data-ttu-id="2f686-136">Oldukça iyi görünür.</span><span class="sxs-lookup"><span data-stu-id="2f686-136">Looks pretty good.</span></span> <span data-ttu-id="2f686-137">Ancak, tarayıcının başlık çubuğunda yazmadığını fark &quot;dizin&quot; ve büyük başlık sayfasında diyor &quot;MVC Uygulamam.&quot; Bu değiştirelim.</span><span class="sxs-lookup"><span data-stu-id="2f686-137">However, notice that the browser's title bar says &quot;Index&quot; and the big title on the page says &quot;My MVC Application.&quot; Let's change those.</span></span>

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="2f686-138">Görünümleri ve Düzen sayfaları değiştirme</span><span class="sxs-lookup"><span data-stu-id="2f686-138">Changing views and layout pages</span></span>

<span data-ttu-id="2f686-139">İlk olarak, metin değiştirelim &quot;MVC Uygulamam.&quot; Bu metin paylaşılır ve her sayfada görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2f686-139">First, let's change the text &quot;My MVC Application.&quot; That text is shared and appears on every page.</span></span> <span data-ttu-id="2f686-140">Uygulamamızı her sayfasında olsa bile gerçekte bizim projesinde, yalnızca tek bir yerde görünür.</span><span class="sxs-lookup"><span data-stu-id="2f686-140">It actually appears in only one place in our project, even though it's on every page in our application.</span></span> <span data-ttu-id="2f686-141">Git */görünümler/paylaşılan* klasöründe **Çözüm Gezgini** açarak  *\_Layout.vbhtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="2f686-141">Go to the */Views/Shared* folder in **Solution Explorer** and open the *\_Layout.vbhtml* file.</span></span> <span data-ttu-id="2f686-142">Bu dosyayı bir düzen sayfası olarak adlandırılır ve paylaşılan ise &quot;Kabuk&quot; , diğer tüm sayfalar kullanın.</span><span class="sxs-lookup"><span data-stu-id="2f686-142">This file is called a layout page and it's the shared &quot;shell&quot; that all other pages use.</span></span>

<span data-ttu-id="2f686-143">Not `@RenderBody()` dosyasının altına kod satırı.</span><span class="sxs-lookup"><span data-stu-id="2f686-143">Note the `@RenderBody()` line of code near the bottom of the file.</span></span> <span data-ttu-id="2f686-144">`RenderBody`Burada oluşturduğunuz tüm sayfalar gösterir, bir yer tutucudur &quot;Sarmalanan&quot; düzeni sayfasında.</span><span class="sxs-lookup"><span data-stu-id="2f686-144">`RenderBody` is a placeholder where all the pages you create show up, &quot;wrapped&quot; in the layout page.</span></span> <span data-ttu-id="2f686-145">Değişiklik `<h1>` gelen başlık  **&quot;**  MVC Uygulamam&quot; için &quot;MVC film uygulaması&quot;.</span><span class="sxs-lookup"><span data-stu-id="2f686-145">Change the `<h1>` heading from **&quot;** My MVC Application&quot; to &quot;MVC Movie App&quot;.</span></span>

[!code-html[Main](adding-a-view/samples/sample3.html)]

<span data-ttu-id="2f686-146">Uygulamayı çalıştırın ve şimdi yazacaktır Not &quot;MVC film uygulaması&quot;.</span><span class="sxs-lookup"><span data-stu-id="2f686-146">Run the application and note it now says &quot;MVC Movie App&quot;.</span></span> <span data-ttu-id="2f686-147">Tıklatın **hakkında** bağlantısı ve sayfa gösterir &quot;MVC film uygulaması&quot;, çok.</span><span class="sxs-lookup"><span data-stu-id="2f686-147">Click the **About** link, and that page shows &quot;MVC Movie App&quot;, too.</span></span>

<span data-ttu-id="2f686-148">Tam  *\_Layout.vbhtml* dosya aşağıda gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="2f686-148">The complete *\_Layout.vbhtml* file is shown below:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

<span data-ttu-id="2f686-149">Şimdi, dizin sayfası (Görünüm) başlığı değiştirelim.</span><span class="sxs-lookup"><span data-stu-id="2f686-149">Now, let's change the title of the Index page (view).</span></span>

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

<span data-ttu-id="2f686-150">Açık *MvcMovie\Views\HelloWorld\Index.vbhtml*.</span><span class="sxs-lookup"><span data-stu-id="2f686-150">Open *MvcMovie\Views\HelloWorld\Index.vbhtml*.</span></span> <span data-ttu-id="2f686-151">Bir değişiklik yapmak için iki yerde vardır: ilk olarak, metin görünür tarayıcının başlık ve ardından ikincil üstbilgisinde ( `<h2>` öğesi).</span><span class="sxs-lookup"><span data-stu-id="2f686-151">There are two places to make a change: first, the text that appears in the title of the browser, and then in the secondary header (the `<h2>` element).</span></span> <span data-ttu-id="2f686-152">Hangi bölümünün uygulamanın hangi bit kod değişiklikleri görebilmeleri biz biraz farklı yapmanız.</span><span class="sxs-lookup"><span data-stu-id="2f686-152">We'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>

<span data-ttu-id="2f686-153">Uygulamayı çalıştırın ve Gözat`http://localhost:xx/HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="2f686-153">Run the application and browse to`http://localhost:xx/HelloWorld`.</span></span> <span data-ttu-id="2f686-154">Tarayıcı başlığı, birincil başlık ve ikincil başlıklar değişmiş dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2f686-154">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="2f686-155">Büyük küçük değişikliklerle, uygulamanızdaki bir görünüme değişiklik kolaydır.</span><span class="sxs-lookup"><span data-stu-id="2f686-155">It's easy to make big changes in your application with small changes to a view.</span></span> <span data-ttu-id="2f686-156">(Değişiklikleri tarayıcıda görmüyorsanız, önbelleğe alınmış içeriği görüntülüyor olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f686-156">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="2f686-157">Yüklenecek sunucudan yanıt zorlamak için tarayıcınızda CTRL + F5'e basın.)</span><span class="sxs-lookup"><span data-stu-id="2f686-157">Press Ctrl+F5 in your browser to force the response from the server to be loaded.)</span></span>

<span data-ttu-id="2f686-158">[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="2f686-158">[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)</span></span>

<span data-ttu-id="2f686-159">Bizim az biti &quot;veri&quot; (Bu durumda &quot;Hello World!&quot; ileti) sabit kodlanmış, ancak değil.</span><span class="sxs-lookup"><span data-stu-id="2f686-159">Our little bit of &quot;data&quot; (in this case the &quot;Hello World!&quot; message) is hard-coded, though.</span></span> <span data-ttu-id="2f686-160">MVC uygulamamız V (görünümler) vardır ve biz C (denetleyicileri), ancak henüz hiçbir M (modeli) olduğuna.</span><span class="sxs-lookup"><span data-stu-id="2f686-160">Our MVC application has V (views) and we've got C (controllers), but no M (model) yet.</span></span> <span data-ttu-id="2f686-161">Kısa bir süre içinde biz nasıl adım geçireceğiz bir veritabanı oluşturun ve model verileri alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f686-161">Shortly, we'll walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="2f686-162">Denetleyici geçirme verileri görüntülemek için</span><span class="sxs-lookup"><span data-stu-id="2f686-162">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="2f686-163">Bir veritabanına gidin ve modelleri hakkında konuşun önce ancak şimdi ilk bilgi denetleyicisinden bir görünüme geçirme hakkında konuşun.</span><span class="sxs-lookup"><span data-stu-id="2f686-163">Before we go to a database and talk about models, though, let's first talk about passing information from the Controller to a View.</span></span> <span data-ttu-id="2f686-164">Şablonu görüntüleme bir istemci için bir HTML yanıt işlemek için gerektirdiği istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="2f686-164">We want to pass what a view template requires in order to render an HTML response to a client.</span></span> <span data-ttu-id="2f686-165">Bu nesneler genelde oluşturulur ve bir görünüm şablonu için bir denetleyici sınıfı tarafından geçirilen ve şablonu görüntüleme gerektiren veri içermelidir — ve daha fazla.</span><span class="sxs-lookup"><span data-stu-id="2f686-165">These objects are typically created and passed by a controller class to a view template, and they should contain only the data that the view template requires — and no more.</span></span>

<span data-ttu-id="2f686-166">Daha önce ile `HelloWorldController` sınıfı, `Welcome` eylem yöntemine geçen bir `name` ve `numTimes` parametresi ve parametre değerleri tarayıcıya sonra çıktı.</span><span class="sxs-lookup"><span data-stu-id="2f686-166">Previously with the `HelloWorldController` class, the `Welcome` action method took a `name` and a `numTimes` parameter and then output the parameter values to the browser.</span></span> <span data-ttu-id="2f686-167">Bunun yerine bu yanıt doğrudan işlemeye devam denetleyiciniz daha şimdi bunun yerine Biz bu verileri bir görünüm için Çantası.</span><span class="sxs-lookup"><span data-stu-id="2f686-167">Rather than have the controller continue to render this response directly, let's instead we'll put that data in a bag for the View.</span></span> <span data-ttu-id="2f686-168">Denetleyicileri ve görünümleri kullanabilir bir `ViewBag` verileri tutacak nesne.</span><span class="sxs-lookup"><span data-stu-id="2f686-168">Controllers and Views can use a `ViewBag` object to hold that data.</span></span> <span data-ttu-id="2f686-169">Bir görünüm şablonu otomatik olarak geçirilen ve olması veri paketi içeriğini kullanarak HTML yanıtı işlemek için kullanılan.</span><span class="sxs-lookup"><span data-stu-id="2f686-169">That will be passed over to a view template automatically, and used to render the HTML response using the contents of the bag as data.</span></span> <span data-ttu-id="2f686-170">Böylece denetleyicisi tek şey ve başka bir görünüm şablonu ile ilgilenir — bize temiz korumak etkinleştirme &quot;sorunları ayrılması&quot; uygulama içinde.</span><span class="sxs-lookup"><span data-stu-id="2f686-170">That way the controller is concerned with one thing and the view template with another — enabling us to maintain clean &quot;separation of concerns&quot; within the application.</span></span>

<span data-ttu-id="2f686-171">Alternatif olarak, biz özel bir sınıf tanımlama, ardından söz konusu nesne örneği bizim kendi, verilerle doldurmak oluşturup görünüme iletmek.</span><span class="sxs-lookup"><span data-stu-id="2f686-171">Alternatively, we could define a custom class, then create an instance of that object on our own, fill it with data and pass it to the View.</span></span> <span data-ttu-id="2f686-172">Özel bir Model görünüm için olduğundan, genellikle bir ViewModel denir.</span><span class="sxs-lookup"><span data-stu-id="2f686-172">That is often called a ViewModel, because it's a custom Model for the View.</span></span> <span data-ttu-id="2f686-173">Küçük miktardaki veriler için ancak ViewBag iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="2f686-173">For small amounts of data, however, the ViewBag works great.</span></span>

<span data-ttu-id="2f686-174">Geri dönüp *HelloWorldController.vb* dosya değişikliği `Welcome` NumTimes ve ileti ViewBag yerleştirilecek denetleyicisi içinde yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2f686-174">Return to the *HelloWorldController.vb* file change the `Welcome` method inside the controller to put the Message and NumTimes into the ViewBag.</span></span> <span data-ttu-id="2f686-175">Görünüm Paketi dinamik bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="2f686-175">The ViewBag is a dynamic object.</span></span> <span data-ttu-id="2f686-176">Ne olursa olsun, istediğiniz koyabilirsiniz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2f686-176">That means you can put whatever you want in to it.</span></span> <span data-ttu-id="2f686-177">İçindeki bir şey put kadar görünüm paketi tanımlı özelliği yok.</span><span class="sxs-lookup"><span data-stu-id="2f686-177">The ViewBag has no defined properties until you put something inside it.</span></span>

<span data-ttu-id="2f686-178">Tam `HelloWorldController.vb` aynı dosyada yeni sınıf ile.</span><span class="sxs-lookup"><span data-stu-id="2f686-178">The complete `HelloWorldController.vb` with the new class in the same file.</span></span>

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

<span data-ttu-id="2f686-179">Şimdi bizim ViewBag görünümüne otomatik olarak geçirilir veri içeriyor.</span><span class="sxs-lookup"><span data-stu-id="2f686-179">Now our ViewBag contains data that will be passed over to the View automatically.</span></span> <span data-ttu-id="2f686-180">Biz beğendiğinizi, yeniden alternatif olarak Biz bu gibi kendi nesnesindeki geçmiş:</span><span class="sxs-lookup"><span data-stu-id="2f686-180">Again, alternatively we could have passed in our own object like this if we liked:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

<span data-ttu-id="2f686-181">İhtiyacımız artık bir `WelcomeView` şablonu!</span><span class="sxs-lookup"><span data-stu-id="2f686-181">Now we need a `WelcomeView` template!</span></span> <span data-ttu-id="2f686-182">Yeni kod derlenmiş şekilde uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2f686-182">Run the application so the new code is compiled.</span></span> <span data-ttu-id="2f686-183">Tarayıcıyı kapatın, içinde sağ `Welcome` yöntemi ve ardından **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="2f686-183">Close the browser, right-click inside the `Welcome` method, and then click **Add View**.</span></span>

<span data-ttu-id="2f686-184">İşte, **Görünüm Ekle** gibi iletişim kutusu görünür.</span><span class="sxs-lookup"><span data-stu-id="2f686-184">Here's what your **Add View** dialog box looks like.</span></span>

<span data-ttu-id="2f686-185">[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="2f686-185">[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)</span></span>

<span data-ttu-id="2f686-186">Altında aşağıdaki kodu ekleyin `<h2>` öğesinde yeni *Hoş Geldiniz.* vbhtml dosyası.</span><span class="sxs-lookup"><span data-stu-id="2f686-186">Add the following code under the `<h2>` element in the new *Welcome.*vbhtml file.</span></span> <span data-ttu-id="2f686-187">Biz döngü olmak ve söyleyin &quot;Hello&quot; sayıda kullanıcı biz gerektiğini bildiren!</span><span class="sxs-lookup"><span data-stu-id="2f686-187">We'll make a loop and say &quot;Hello&quot; as many times as the user says we should!</span></span>

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

<span data-ttu-id="2f686-188">Uygulamayı çalıştırın ve göz atın`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`</span><span class="sxs-lookup"><span data-stu-id="2f686-188">Run the application and browse to `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`</span></span>

<span data-ttu-id="2f686-189">Artık veriler URL'den gerçekleştirilecek ve denetleyiciye otomatik olarak geçirildi.</span><span class="sxs-lookup"><span data-stu-id="2f686-189">Now data is taken from the URL and passed to the controller automatically.</span></span> <span data-ttu-id="2f686-190">Denetleyici paketler veri bir `Model` nesnesini ve nesne görünümüne geçirir.</span><span class="sxs-lookup"><span data-stu-id="2f686-190">The controller packages up the data into a `Model` object and passes that object to the view.</span></span> <span data-ttu-id="2f686-191">Verileri HTML olarak kullanıcıya görüntüler daha görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="2f686-191">The view than displays the data as HTML to the user.</span></span>

<span data-ttu-id="2f686-192">[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="2f686-192">[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)</span></span>

<span data-ttu-id="2f686-193">Bir tür iyi, bir &quot;M&quot; modeli, ancak veritabanı türü değil.</span><span class="sxs-lookup"><span data-stu-id="2f686-193">Well, that was a kind of an &quot;M&quot; for model, but not the database kind.</span></span> <span data-ttu-id="2f686-194">Ne biz öğrendiğinize ve film bir veritabanı oluşturmak atalım.</span><span class="sxs-lookup"><span data-stu-id="2f686-194">Let's take what we've learned and create a database of movies.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2f686-195">[Önceki](adding-a-controller.md)
[sonraki](adding-a-model.md)</span><span class="sxs-lookup"><span data-stu-id="2f686-195">[Previous](adding-a-controller.md)
[Next](adding-a-model.md)</span></span>