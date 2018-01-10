---
title: "Tepki proje şablonu kullanın"
author: SteveSandersonMS
description: "Tepki ve oluşturma tepki-uygulama için ASP.NET Core tek sayfalı uygulama (SPA) Sürüm Adayı proje şablonu ile başlayacağınızı öğrenin."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: 6a0de008979823baae17eb66df2d991300c197f2
ms.sourcegitcommit: fc98e93464ccf37d9904e89a71cdddbd4bbdb86a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="use-the-react-project-template-release-candidate"></a><span data-ttu-id="9b86b-103">(Sürüm Adayı) tepki proje şablonu kullanın</span><span class="sxs-lookup"><span data-stu-id="9b86b-103">Use the React project template (release candidate)</span></span>

> [!NOTE]
> <span data-ttu-id="9b86b-104">Bu belge hakkında yayımlanan tepki proje şablonu değil.</span><span class="sxs-lookup"><span data-stu-id="9b86b-104">This documentation is not about the released React project template.</span></span> <span data-ttu-id="9b86b-105">**Bu belge hakkında tepki şablonunun Sürüm Adayı ' dir.**</span><span class="sxs-lookup"><span data-stu-id="9b86b-105">**This documentation is about the release candidate of the React template.**</span></span> <span data-ttu-id="9b86b-106">Yayımlanmış bir sürüm erken 2018 dağıtmayı umuyoruz.</span><span class="sxs-lookup"><span data-stu-id="9b86b-106">We hope to ship the released version in early 2018.</span></span>

<span data-ttu-id="9b86b-107">Güncelleştirilmiş tepki proje şablonu uygun bir başlama noktası için ASP.NET Core tepki kullanarak uygulamaları sağlar ve [oluşturma tepki-uygulama](https://github.com/facebookincubator/create-react-app) (CRA) kuralları zengin, istemci tarafı kullanıcı arabirimini (UI).</span><span class="sxs-lookup"><span data-stu-id="9b86b-107">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="9b86b-108">Şablon, hem bir API arka ucu olarak görev yapması için bir ASP.NET Core projesi hem de bir kullanıcı Arabirimi, ancak her ikisinde de yerleşik ve tek bir birim olarak yayımlanan bir tek uygulama projesi barındırma kullanım görev yapması için standart bir CRA tepki proje oluşturmak için eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="9b86b-108">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="9b86b-109">Yeni uygulama oluştur</span><span class="sxs-lookup"><span data-stu-id="9b86b-109">Create a new app</span></span>

<span data-ttu-id="9b86b-110">Başlamak için seçtiğiniz olun [güncelleştirilmiş tepki proje şablonu yüklü](xref:spa/index#installation).</span><span class="sxs-lookup"><span data-stu-id="9b86b-110">To get started, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span> <span data-ttu-id="9b86b-111">Bu yönergeler, .NET Core dahil önceki tepki proje şablonu için geçerli olmayan 2.0.x SDK.</span><span class="sxs-lookup"><span data-stu-id="9b86b-111">These instructions don't apply to the previous React project template included in the .NET Core 2.0.x SDK.</span></span>

<span data-ttu-id="9b86b-112">Komutunu kullanarak bir komut isteminden yeni bir proje oluşturun `dotnet new react` boş bir dizinde.</span><span class="sxs-lookup"><span data-stu-id="9b86b-112">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="9b86b-113">Örneğin, aşağıdaki komutları uygulaması oluşturmak bir *-yeni-Uygulamam* dizini ve bu dizine geçin:</span><span class="sxs-lookup"><span data-stu-id="9b86b-113">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new -o my-new-app
cd my-new-app
```

<span data-ttu-id="9b86b-114">Uygulama, Visual Studio veya .NET Core CLI çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9b86b-114">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9b86b-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b86b-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9b86b-116">Oluşturulan açmak *.csproj* dosya ve uygulama buradan normal olarak çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9b86b-116">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="9b86b-117">Oluşturma işlemi birkaç dakika sürebilir ilk çalıştırmada npm bağımlılıkları yükler.</span><span class="sxs-lookup"><span data-stu-id="9b86b-117">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="9b86b-118">Sonraki derlemeleri çok daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="9b86b-118">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9b86b-119">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9b86b-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9b86b-120">Adlı bir ortam değişkeni olduğundan emin olun `ASPNETCORE_Environment` değeriyle `Development`.</span><span class="sxs-lookup"><span data-stu-id="9b86b-120">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="9b86b-121">(İçinde olmayan PowerShell komut istemleri) Windows üzerinde çalışan `SET ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="9b86b-121">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="9b86b-122">Linux veya macOS, çalıştırılan `export ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="9b86b-122">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="9b86b-123">Çalıştırma `dotnet build` uygulamanızı doğrulamak için derlemeler doğru.</span><span class="sxs-lookup"><span data-stu-id="9b86b-123">Run `dotnet build` to verify your app builds correctly.</span></span> <span data-ttu-id="9b86b-124">İlk çalıştırılmasında oluşturma işlemi birkaç dakika sürebilir npm bağımlılıkları yükler.</span><span class="sxs-lookup"><span data-stu-id="9b86b-124">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="9b86b-125">Sonraki derlemeleri çok daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="9b86b-125">Subsequent builds are much faster.</span></span>

<span data-ttu-id="9b86b-126">Çalıştırma `dotnet run` uygulamayı başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="9b86b-126">Run `dotnet run` to start the app.</span></span>

---

<span data-ttu-id="9b86b-127">Proje şablonu, bir ASP.NET Core uygulama ve tepki bir uygulama oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9b86b-127">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="9b86b-128">ASP.NET Core uygulama veri erişimi, yetkilendirme ve diğer sunucu tarafı sorunları için kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="9b86b-128">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="9b86b-129">Bulunan tepki uygulama *ClientApp* alt, tüm kullanıcı Arabirimi sorunları için kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="9b86b-129">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="9b86b-130">Sayfaları, görüntüler, stil, modüller, vb. ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9b86b-130">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="9b86b-131">*ClientApp* standart CRA tepki uygulama dizindir.</span><span class="sxs-lookup"><span data-stu-id="9b86b-131">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="9b86b-132">Resmi görmek [CRA belgelerine](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="9b86b-132">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="9b86b-133">Bu şablon tarafından oluşturulan tepki uygulaması ile CRA tarafından oluşturulan arasında küçük farklar vardır; Ancak, uygulama özelliklerini farklı değildir.</span><span class="sxs-lookup"><span data-stu-id="9b86b-133">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="9b86b-134">Şablon tarafından oluşturulan uygulama içeren bir [önyükleme](https://getbootstrap.com/)-tabanlı düzeni ve temel bir yönlendirme örneği.</span><span class="sxs-lookup"><span data-stu-id="9b86b-134">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="9b86b-135">Npm paket yüklemek için</span><span class="sxs-lookup"><span data-stu-id="9b86b-135">Install npm packages</span></span>

<span data-ttu-id="9b86b-136">Üçüncü taraf npm paketleri yüklemek için bir komut isteminde kullanın *ClientApp* alt dizin.</span><span class="sxs-lookup"><span data-stu-id="9b86b-136">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="9b86b-137">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9b86b-137">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="9b86b-138">Yayımlama ve dağıtma</span><span class="sxs-lookup"><span data-stu-id="9b86b-138">Publish and deploy</span></span>

<span data-ttu-id="9b86b-139">Geliştirme, uygulama geliştiriciye kolaylık sağlamak için en iyi hale getirilmiş bir modda çalışır.</span><span class="sxs-lookup"><span data-stu-id="9b86b-139">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="9b86b-140">Örneğin, (hata ayıklama sırasında özgün kaynak kodunuzu görebilmeniz için) kaynak eşlemeleri JavaScript paketleri içerir.</span><span class="sxs-lookup"><span data-stu-id="9b86b-140">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="9b86b-141">Uygulama JavaScript, HTML ve CSS değişiklikleri diskte dosya izleyen ve otomatik olarak yeniden derler ve bu dosyaları değiştirmek gördüğünde yeniden yükler.</span><span class="sxs-lookup"><span data-stu-id="9b86b-141">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="9b86b-142">Üretimde, uygulamanızın performansını en iyi duruma getirilmiş sürüm hizmet.</span><span class="sxs-lookup"><span data-stu-id="9b86b-142">In production, serve a version of your app that is optimized for performance.</span></span> <span data-ttu-id="9b86b-143">Bu, otomatik olarak gerçekleşecek şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="9b86b-143">This is configured to happen automatically.</span></span> <span data-ttu-id="9b86b-144">Derleme yapılandırması yayımladığınızda, istemci-tarafı kodunuzu küçültülmüş, transpiled derlemesinde yayar.</span><span class="sxs-lookup"><span data-stu-id="9b86b-144">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="9b86b-145">Geliştirme yapı farklı olarak, üretim yapı Node.js sunucu üzerinde yüklü olmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="9b86b-145">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="9b86b-146">Standart kullanabilirsiniz [ASP.NET Core yayımlama ve dağıtım yöntemleri](xref:publishing/index).</span><span class="sxs-lookup"><span data-stu-id="9b86b-146">You can use standard [ASP.NET Core publish and deployment methods](xref:publishing/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="9b86b-147">CRA sunucunun bağımsız olarak çalıştırın</span><span class="sxs-lookup"><span data-stu-id="9b86b-147">Run the CRA server independently</span></span>

<span data-ttu-id="9b86b-148">Proje ASP.NET Core uygulama geliştirme modunda başlatıldığında kendi örneğini CRA geliştirme sunucusu, arka planda başlatmak için yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="9b86b-148">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="9b86b-149">Ayrı bir sunucu el ile çalıştırmayı gerekmediği anlamına gelir kolay olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="9b86b-149">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="9b86b-150">Bu varsayılan kurulum bir dezavantajı yoktur.</span><span class="sxs-lookup"><span data-stu-id="9b86b-150">There is a drawback to this default setup.</span></span> <span data-ttu-id="9b86b-151">C# kodunuzu ve ASP.NET uygulama yeniden başlatılmalıdır çekirdek her değiştirdiğinizde CRA sunucuyu yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="9b86b-151">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="9b86b-152">Birkaç saniye yeniden başlatılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9b86b-152">A few seconds are required to start back up.</span></span> <span data-ttu-id="9b86b-153">Sık C# kod düzenlemeleri yapıyorsanız ve CRA sunucunun yeniden başlatılmasını beklemek istemiyorsanız, harici olarak CRA sunucunun bağımsız olarak ASP.NET Core işlemini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9b86b-153">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="9b86b-154">Bunu yapmak için:</span><span class="sxs-lookup"><span data-stu-id="9b86b-154">To do so:</span></span>

1. <span data-ttu-id="9b86b-155">Bir komut isteminde, geçiş *ClientApp* alt ve başlatma CRA geliştirme sunucusu:</span><span class="sxs-lookup"><span data-stu-id="9b86b-155">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

2. <span data-ttu-id="9b86b-156">Aşağıdakilerden birini kendi başlatmayı yerine dış CRA sunucu örneği kullanmak için ASP.NET Core uygulamanızı değiştirin.</span><span class="sxs-lookup"><span data-stu-id="9b86b-156">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="9b86b-157">İçinde *başlangıç* sınıfı, yerine `spa.UseReactDevelopmentServer` aşağıdaki çağırmayla:</span><span class="sxs-lookup"><span data-stu-id="9b86b-157">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="9b86b-158">ASP.NET Core uygulamanızı başlattığınızda CRA sunucu başlatma olmaz.</span><span class="sxs-lookup"><span data-stu-id="9b86b-158">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="9b86b-159">El ile başlatılan örnek yerine kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9b86b-159">The instance you started manually is used instead.</span></span> <span data-ttu-id="9b86b-160">Bu, başlatma ve hızlı yeniden sağlar.</span><span class="sxs-lookup"><span data-stu-id="9b86b-160">This enables it to start and restart faster.</span></span> <span data-ttu-id="9b86b-161">Artık her zaman yeniden oluşturmak tepki uygulamanız için bekliyor.</span><span class="sxs-lookup"><span data-stu-id="9b86b-161">It's no longer waiting for your React app to rebuild each time.</span></span>