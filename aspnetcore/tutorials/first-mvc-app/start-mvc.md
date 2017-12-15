---
title: "ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama"
author: rick-anderson
description: "ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama"
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 10/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 4b86eb22e367d2305b7995421aec6f37008c5637
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="7e77e-104">ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="7e77e-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="7e77e-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7e77e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="7e77e-106">Bu öğretici 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="7e77e-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="7e77e-107">macOS: [Mac için Visual Studio ile ASP.NET Core MVC uygulama oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="7e77e-107">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="7e77e-108">Windows: [Visual Studio ile ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="7e77e-108">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="7e77e-109">macOS, Linux ve Windows: [Visual Studio Code ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="7e77e-109">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="7e77e-110">Visual Studio ve .NET Core yükleyin</span><span class="sxs-lookup"><span data-stu-id="7e77e-110">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7e77e-111">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="7e77e-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7e77e-112">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="7e77e-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7e77e-113">Visual Studio Community 2017 yükleyin.</span><span class="sxs-lookup"><span data-stu-id="7e77e-113">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="7e77e-114">Topluluk indirme seçin.</span><span class="sxs-lookup"><span data-stu-id="7e77e-114">Select the Community download.</span></span> <span data-ttu-id="7e77e-115">Visual Studio yüklü 2017 varsa bu adımı atlayın.</span><span class="sxs-lookup"><span data-stu-id="7e77e-115">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="7e77e-116">Visual Studio 2017 giriş sayfası yükleyicisi</span><span class="sxs-lookup"><span data-stu-id="7e77e-116">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="7e77e-117">Yükleyiciyi çalıştırın ve aşağıdaki iş yüklerini seçin:</span><span class="sxs-lookup"><span data-stu-id="7e77e-117">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="7e77e-118">**ASP.NET ve web geliştirme** (altında **Web ve bulut**)</span><span class="sxs-lookup"><span data-stu-id="7e77e-118">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="7e77e-119">**.NET core platformlar arası geliştirme** (altında **diğer Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="7e77e-119">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET ve web geliştirme ** (altında ** Web ve bulut **)](start-mvc/_static/web_workload.png)

![* *.NET çekirdek arası arası platfrom geliştirme ** (altında ** diğer Toolsets **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="7e77e-122">Bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="7e77e-122">Create a web app</span></span>

<span data-ttu-id="7e77e-123">Visual Studio'dan seçin **Dosya > Yeni > Proje**.</span><span class="sxs-lookup"><span data-stu-id="7e77e-123">From Visual Studio, select  **File > New > Project**.</span></span>

![Dosya > Yeni > Proje](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="7e77e-125">Tamamlamak **yeni proje** iletişim:</span><span class="sxs-lookup"><span data-stu-id="7e77e-125">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="7e77e-126">Sol bölmede, dokunun **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="7e77e-126">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="7e77e-127">Orta bölmede, dokunun **ASP.NET çekirdek Web uygulaması (.NET çekirdek)**</span><span class="sxs-lookup"><span data-stu-id="7e77e-127">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="7e77e-128">(Kod kopyaladığınızda, ad alanı eşleşecek şekilde "MvcMovie" proje adı önemlidir.) "MvcMovie" proje adı</span><span class="sxs-lookup"><span data-stu-id="7e77e-128">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="7e77e-129">Dokunun **Tamam**</span><span class="sxs-lookup"><span data-stu-id="7e77e-129">Tap **OK**</span></span>

![<span data-ttu-id="7e77e-130">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web .net core</span><span class="sxs-lookup"><span data-stu-id="7e77e-130">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7e77e-131">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="7e77e-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7e77e-132">Tamamlamak **yeni ASP.NET çekirdek Web uygulaması (.NET Core) - MvcMovie** iletişim:</span><span class="sxs-lookup"><span data-stu-id="7e77e-132">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="7e77e-133">Sürüm Seçici açılan kutusunda **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="7e77e-133">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="7e77e-134">Seçin **Application(Model-View-Controller) Web**</span><span class="sxs-lookup"><span data-stu-id="7e77e-134">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="7e77e-135">Dokunun **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="7e77e-135">Tap **OK**.</span></span>

![<span data-ttu-id="7e77e-136">Yeni Proje iletişim kutusunda, sol bölmede, ASP.NET Core web .net core</span><span class="sxs-lookup"><span data-stu-id="7e77e-136">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7e77e-137">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="7e77e-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7e77e-138">Tamamlamak **yeni ASP.NET çekirdek Web uygulaması (.NET Core) - MvcMovie** iletişim:</span><span class="sxs-lookup"><span data-stu-id="7e77e-138">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="7e77e-139">Sürüm Seçici açılan kutu dokunun içinde **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="7e77e-139">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="7e77e-140">Dokunun **Web uygulaması**</span><span class="sxs-lookup"><span data-stu-id="7e77e-140">Tap **Web Application**</span></span>
* <span data-ttu-id="7e77e-141">Varsayılan tutmak **doğrulaması yok**</span><span class="sxs-lookup"><span data-stu-id="7e77e-141">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="7e77e-142">Dokunun **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="7e77e-142">Tap **OK**.</span></span>

![Yeni ASP.NET Core web uygulaması](start-mvc/_static/p3.png)

---

<span data-ttu-id="7e77e-144">Visual Studio, yeni oluşturduğunuz MVC proje için varsayılan bir şablon kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7e77e-144">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="7e77e-145">Bir çalışma şu anda bir proje adı girerek ve birkaç seçenek seçerek uygulamanız.</span><span class="sxs-lookup"><span data-stu-id="7e77e-145">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="7e77e-146">Bu bir basit başlangıç projesi ve başlatmak için uygun bir yerdir,</span><span class="sxs-lookup"><span data-stu-id="7e77e-146">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="7e77e-147">Dokunun **F5** uygulamayı hata ayıklama modunda çalıştırmak için veya **Ctrl-F5** olmayan hata ayıklama modunda.</span><span class="sxs-lookup"><span data-stu-id="7e77e-147">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Uygulamayı çalıştırma](start-mvc/_static/1.png)

* <span data-ttu-id="7e77e-149">Visual Studio başlatır [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) ve uygulamanızı çalışır.</span><span class="sxs-lookup"><span data-stu-id="7e77e-149">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="7e77e-150">Adres çubuğunun bildirim `localhost:port#` bir şey yok gibi ve `example.com`.</span><span class="sxs-lookup"><span data-stu-id="7e77e-150">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7e77e-151">Çünkü `localhost` , yerel bilgisayarınızın standart barındırıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="7e77e-151">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="7e77e-152">Visual Studio web projesini oluşturduğunda, rastgele bir bağlantı noktası web sunucusu için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7e77e-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="7e77e-153">Yukarıdaki resimde 5000 bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="7e77e-153">In the image above, the port number is 5000.</span></span> <span data-ttu-id="7e77e-154">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="7e77e-154">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="7e77e-155">Uygulama başlatma **Ctrl + F5** (olmayan hata ayıklama modu), kod değişiklikleri yapabilir, dosyayı kaydedin, tarayıcıyı yenilemek ve kod değişiklikleri görmek olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="7e77e-155">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="7e77e-156">Çoğu geliştirici, hızlı bir şekilde uygulamayı başlatın ve değişiklikleri görmek için olmayan hata ayıklama modu kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="7e77e-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="7e77e-157">Hata ayıklama veya hata ayıklama olmayan modundan uygulamada başlatabilirsiniz **hata ayıklama** menü öğesi:</span><span class="sxs-lookup"><span data-stu-id="7e77e-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menü hata ayıklama](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="7e77e-159">Uygulama dokunarak ayıklayabilirsiniz **IIS Express** düğmesi</span><span class="sxs-lookup"><span data-stu-id="7e77e-159">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="7e77e-161">Varsayılan şablonu, çalışma sunar **hakkında giriş** ve **kişi** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="7e77e-161">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="7e77e-162">Yukarıdaki tarayıcı resimde bu bağlantıları göstermez.</span><span class="sxs-lookup"><span data-stu-id="7e77e-162">The browser image above doesn't show these links.</span></span> <span data-ttu-id="7e77e-163">Tarayıcınız boyutuna bağlı olarak, bunları göstermek için Gezinti simgesini gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="7e77e-163">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![sağ üstteki gezinti simgesi](start-mvc/_static/2.png)

<span data-ttu-id="7e77e-165">Hata ayıklama modunda çalışıyormuş dokunun **Shift + F5** hata ayıklamasını durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="7e77e-165">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="7e77e-166">Bu öğreticinin sonraki bölümünde, biz MVC hakkında bilgi edinin ve biraz kod yazmaya başlamadan.</span><span class="sxs-lookup"><span data-stu-id="7e77e-166">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="7e77e-167">Sonraki</span><span class="sxs-lookup"><span data-stu-id="7e77e-167">Next</span></span>](adding-controller.md)  