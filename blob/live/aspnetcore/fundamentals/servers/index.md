---
title: "Web server ASP.NET Core uygulamalarında"
author: tdykstra
description: "Web sunucuları Kestrel ve WebListener için ASP.NET Core tanıtır. Birini konusunda rehberlik sağlar ve ne zaman bir ters proxy sunucusu ile kullanılır."
keywords: ASP.NET Core, IServer, web sunucusu, Kestrel, WebListener, ters proxy
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: dba74f39-58cd-4dee-a061-6d15f7346959
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: 04dee100dff91f7868175ff4be01156787e13e81
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="898d0-105">Web server ASP.NET Core uygulamalarında</span><span class="sxs-lookup"><span data-stu-id="898d0-105">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="898d0-106">Tarafından [zel Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), ve [Chris fillerin](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="898d0-106">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="898d0-107">Bir ASP.NET Core uygulama işlem içi HTTP sunucusu uygulamasını ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="898d0-107">An ASP.NET Core application runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="898d0-108">HTTP istekleri ve bunları uygulamasına kümesi olarak ortaya çıkarır sunucusu uygulaması dinler [istek özellikleri](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) içine oluşan bir `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="898d0-108">The server implementation listens for HTTP requests and surfaces them to the application as sets of [request features](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) composed into an `HttpContext`.</span></span>

<span data-ttu-id="898d0-109">ASP.NET Core iki sunucu uygulamaları gelir:</span><span class="sxs-lookup"><span data-stu-id="898d0-109">ASP.NET Core ships two server implementations:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="898d0-110">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="898d0-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="898d0-111">[Kestrel](kestrel.md) platformlar arası HTTP sunucusu dayanır [libuv](https://github.com/libuv/libuv), platformlar arası zaman uyumsuz g/ç kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="898d0-111">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="898d0-112">[HTTP.sys](httpsys.md) yalnızca Windows HTTP sunucu dayanır [Http.Sys çekirdek sürücüsü](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="898d0-112">[HTTP.sys](httpsys.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="898d0-113">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="898d0-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="898d0-114">[Kestrel](kestrel.md) platformlar arası HTTP sunucusu dayanır [libuv](https://github.com/libuv/libuv), platformlar arası zaman uyumsuz g/ç kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="898d0-114">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="898d0-115">[WebListener](weblistener.md) yalnızca Windows HTTP sunucu dayanır [Http.Sys çekirdek sürücüsü](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="898d0-115">[WebListener](weblistener.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

---

## <a name="kestrel"></a><span data-ttu-id="898d0-116">kestrel</span><span class="sxs-lookup"><span data-stu-id="898d0-116">Kestrel</span></span>

<span data-ttu-id="898d0-117">Kestrel varsayılan ASP.NET Core yeni proje şablonları olarak dahil edilen web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="898d0-117">Kestrel is the web server that is included by default in ASP.NET Core new-project templates.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="898d0-118">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="898d0-118">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="898d0-119">Tek başına veya birlikte Kestrel kullanabileceğiniz bir *ters proxy sunucusu*, IIS, Nginx ya da Apache gibi.</span><span class="sxs-lookup"><span data-stu-id="898d0-119">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="898d0-120">Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.</span><span class="sxs-lookup"><span data-stu-id="898d0-120">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel bir ters Ara sunucu olmadan Internet ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

![Kestrel dolaylı olarak IIS, Nginx ya da Apache gibi bir ters proxy sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="898d0-123">Her iki yapılandırma &mdash; ile veya bir ters Ara sunucu olmadan &mdash; Kestrel yalnızca bir iç ağa açılırsa de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="898d0-123">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

<span data-ttu-id="898d0-124">Ne zaman bir ters proxy ile Kestrel kullanılacağı hakkında daha fazla bilgi için bkz: [Kestrel giriş](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="898d0-124">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="898d0-125">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="898d0-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="898d0-126">Uygulamanızı bir iç ağ yalnızca gelen istekleri kabul ederse, tek başına Kestrel kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="898d0-126">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel, iç ağınız ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="898d0-128">Uygulamanızı internet kullanıma sunma, IIS, Nginx ya da Apache olarak kullanmalısınız bir *ters proxy sunucu*.</span><span class="sxs-lookup"><span data-stu-id="898d0-128">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="898d0-129">Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra aşağıdaki çizimde gösterildiği gibi iletir.</span><span class="sxs-lookup"><span data-stu-id="898d0-129">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram.</span></span>

![Kestrel dolaylı olarak IIS, Nginx ya da Apache gibi bir ters proxy sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="898d0-131">Güvenlik (trafiğini Internet'ten gösterilen) kenar dağıtımlar için ters Ara sunucu kullanmak için en önemli nedenidir.</span><span class="sxs-lookup"><span data-stu-id="898d0-131">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="898d0-132">Kestrel 1.x sürümleri tamamlayıcı saldırılarına karşı savunma yoktur.</span><span class="sxs-lookup"><span data-stu-id="898d0-132">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="898d0-133">Bu, içerir, ancak uygun için sınırlı zaman aşımları, boyut sınırları ve eşzamanlı bağlantı sınırlarını değil.</span><span class="sxs-lookup"><span data-stu-id="898d0-133">This includes, but isn't limited to, appropriate timeouts, size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="898d0-134">Ne zaman bir ters proxy ile Kestrel kullanılacağı hakkında daha fazla bilgi için bkz: [Kestrel giriş](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="898d0-134">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

---

<span data-ttu-id="898d0-135">IIS, Nginx ya da Apache Kestrel olmadan kullanamazsınız veya [özel sunucu uygulaması](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="898d0-135">You can't use IIS, Nginx, or Apache without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="898d0-136">ASP.NET Core, böylece platformlar genelinde tutarlı bir şekilde davranabilir kendi işleminde çalışmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="898d0-136">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="898d0-137">IIS, Nginx ve Apache kendi başlatma işlemi ve ortam dikte; bunları kullanmak için doğrudan ASP.NET Core her biri gereksinimlerine uyarlamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="898d0-137">IIS, Nginx, and Apache dictate their own startup process and environment; to use them directly, ASP.NET Core would have to adapt to the needs of each one.</span></span> <span data-ttu-id="898d0-138">Bir web sunucusu uygulaması Kestrel gibi kullanarak ASP.NET Core üzerinde denetim başlatma işlemi ve ortam sağlar.</span><span class="sxs-lookup"><span data-stu-id="898d0-138">Using a web server implementation such as Kestrel gives ASP.NET Core control over the startup process and environment.</span></span> <span data-ttu-id="898d0-139">Bu nedenle ASP.NET Core IIS, Nginx ya da Apache uyarlamak çalışırken yerine yalnızca bu web sunucularının Kestrel proxy istekleri için ayarlarsınız.</span><span class="sxs-lookup"><span data-stu-id="898d0-139">So rather than trying to adapt ASP.NET Core to IIS, Nginx, or Apache, you just set up those web servers to proxy requests to Kestrel.</span></span> <span data-ttu-id="898d0-140">Bu düzenlemenin sağlar, `Program.Main` ve `Startup` temelde dağıttığınız nerede olursa olsun aynı olması için sınıflar.</span><span class="sxs-lookup"><span data-stu-id="898d0-140">This arrangement allows your `Program.Main` and `Startup` classes to be essentially the same no matter where you deploy.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="898d0-141">IIS Kestrel ile</span><span class="sxs-lookup"><span data-stu-id="898d0-141">IIS with Kestrel</span></span>

<span data-ttu-id="898d0-142">ASP.NET Core için ters proxy IIS veya IIS Express kullandığınızda, ASP.NET Core uygulama ayrı bir işlemde için IIS çalışan işlemini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="898d0-142">When you use IIS or IIS Express as a reverse proxy for ASP.NET Core, the ASP.NET Core application runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="898d0-143">Ters proxy ilişki koordine etmek için özel bir IIS modül IIS işleminde çalışır.</span><span class="sxs-lookup"><span data-stu-id="898d0-143">In the IIS process, a special IIS module runs to coordinate the reverse proxy relationship.</span></span>  <span data-ttu-id="898d0-144">Bu *ASP.NET Core Modülü*.</span><span class="sxs-lookup"><span data-stu-id="898d0-144">This is the *ASP.NET Core Module*.</span></span> <span data-ttu-id="898d0-145">Birincil ASP.NET Core modülünün ASP.NET Core uygulamayı başlatmak, onu kilitlendiğinde yeniden başlatın ve HTTP trafiği iletmek için işlevlerdir.</span><span class="sxs-lookup"><span data-stu-id="898d0-145">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core application, restart it when it crashes, and forward HTTP traffic to it.</span></span> <span data-ttu-id="898d0-146">Daha fazla bilgi için bkz: [ASP.NET Core Modülü](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="898d0-146">For more information, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="898d0-147">Nginx Kestrel ile</span><span class="sxs-lookup"><span data-stu-id="898d0-147">Nginx with Kestrel</span></span>

<span data-ttu-id="898d0-148">Nginx Linux'ta bir ters proxy sunucusu olarak Kestrel için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Linux üretim ortamına yayınlama](../../publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="898d0-148">For information about how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Publish to a Linux Production Environment](../../publishing/linuxproduction.md).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="898d0-149">Kestrel Apache</span><span class="sxs-lookup"><span data-stu-id="898d0-149">Apache with Kestrel</span></span>

<span data-ttu-id="898d0-150">Apache Linux'ta bir ters proxy sunucusu olarak Kestrel için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [kullanarak Apache Web sunucusuna ters Ara sunucu](../../publishing/apache-proxy.md).</span><span class="sxs-lookup"><span data-stu-id="898d0-150">For information about how to use Apache on Linux as a reverse proxy server for Kestrel, see [Using Apache Web Server as a reverse proxy](../../publishing/apache-proxy.md).</span></span>

## <a name="httpsys"></a><span data-ttu-id="898d0-151">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="898d0-151">HTTP.sys</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="898d0-152">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="898d0-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="898d0-153">ASP.NET Core uygulamanızı Windows'ta çalıştırma, HTTP.sys alternatif Kestrel olur.</span><span class="sxs-lookup"><span data-stu-id="898d0-153">If you run your ASP.NET Core app on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="898d0-154">HTTP.sys burada uygulamanızı internet kullanıma ve Kestrel desteklemiyor HTTP.sys özelliklerine ihtiyaç senaryoları için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="898d0-154">You can use HTTP.sys for scenarios where you expose your app to the Internet and you need HTTP.sys features that Kestrel doesn't support.</span></span> 

![HTTP.sys doğrudan Internet ile iletişim kurar](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="898d0-156">HTTP.sys, yalnızca bir iç ağ için sunulan uygulamaları için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="898d0-156">HTTP.sys can also be used for applications that are exposed only to an internal network.</span></span> 

![HTTP.sys, iç ağınız ile doğrudan iletişim kurar](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="898d0-158">İç ağ senaryoları için Kestrel genellikle en iyi performans için önerilir; Ancak, bazı senaryolarda, yalnızca HTTP.sys sağlayan bir özellik kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="898d0-158">For internal network scenarios, Kestrel is generally recommended for best performance; but in some scenarios, you might want to use a feature that only HTTP.sys offers.</span></span> <span data-ttu-id="898d0-159">HTTP.sys özellikler hakkında daha fazla bilgi için bkz: [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="898d0-159">For information about HTTP.sys features, see [HTTP.sys](httpsys.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="898d0-160">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="898d0-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="898d0-161">HTTP.sys ASP.NET Core WebListener adlı 1.x.</span><span class="sxs-lookup"><span data-stu-id="898d0-161">HTTP.sys is named WebListener in ASP.NET Core 1.x.</span></span> <span data-ttu-id="898d0-162">Windows, WebListener uygulama için kullanabileceğiniz bir alternatiftir, ASP.NET Core çalıştırırsanız Internet ancak uygulamanıza kullanıma sunmak istediğiniz senaryolar IIS kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="898d0-162">If you run your ASP.NET Core app on Windows, WebListener is an alternative that you can use for scenarios where you want to expose your app to the Internet but you can't use IIS.</span></span>

![Weblistener doğrudan Internet ile iletişim kurar](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="898d0-164">Kestrel desteklemiyor WebListener özelliklerine gereksinim duyarsanız WebListener de Kestrel yerine yalnızca bir iç ağa, sunulan uygulamaları için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="898d0-164">WebListener can also be used in place of Kestrel for applications that are exposed only to an internal network, if you need WebListener features that Kestrel doesn't support.</span></span> 

![Weblistener, iç ağınız ile doğrudan iletişim kurar](weblistener/_static/weblistener-to-internal.png)

<span data-ttu-id="898d0-166">İç ağ senaryoları için Kestrel genellikle en iyi performans için önerilir, ancak bazı senaryolarda yalnızca WebListener sağlayan bir özellik kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="898d0-166">For internal network scenarios, Kestrel is generally recommended for best performance, but in some scenarios you might want to use a feature that only WebListener offers.</span></span> <span data-ttu-id="898d0-167">WebListener özellikleri hakkında daha fazla bilgi için bkz: [WebListener](weblistener.md).</span><span class="sxs-lookup"><span data-stu-id="898d0-167">For information about WebListener features, see [WebListener](weblistener.md).</span></span>

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a><span data-ttu-id="898d0-168">ASP.NET Core sunucu altyapısı ile ilgili notlar</span><span class="sxs-lookup"><span data-stu-id="898d0-168">Notes about ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="898d0-169">[ `IApplicationBuilder` ](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) Bulunan `Startup` sınıfı `Configure` yöntemi çıkarır `ServerFeatures` türündeki özelliği [ `IFeatureCollection` ](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="898d0-169">The [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup` class `Configure` method exposes the `ServerFeatures` property of type [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="898d0-170">Kestrel ve WebListener kullanıma yalnızca tek bir özellik olan [ `IServerAddressesFeature` ](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ancak farklı sunucu uygulamaları ek işlevsellik kullanıma.</span><span class="sxs-lookup"><span data-stu-id="898d0-170">Kestrel and WebListener both expose only a single feature, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="898d0-171">`IServerAddressesFeature`Sunucu uygulaması çalışma zamanında bağlı hangi bağlantı noktasının öğrenmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="898d0-171">`IServerAddressesFeature` can be used to find out which port the server implementation has bound to at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="898d0-172">Özel sunucuları</span><span class="sxs-lookup"><span data-stu-id="898d0-172">Custom servers</span></span>

<span data-ttu-id="898d0-173">Yerleşik sunucuları ihtiyaçlarınızı karşılamıyorsa, özel sunucu uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="898d0-173">If the built-in servers don't meet your needs, you can create a custom server implementation.</span></span> <span data-ttu-id="898d0-174">[.NET (OWIN) kılavuzu için açık Web arabirimi](../owin.md) nasıl yazılacağını göstermektedir bir [Nowin](https://github.com/Bobris/Nowin)-tabanlı [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="898d0-174">The [Open Web Interface for .NET (OWIN) guide](../owin.md) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="898d0-175">En azından desteklemeniz gereken ancak yalnızca uygulamanız gereken, özellik arabirimleri uygulamak ücretsiz [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) ve [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="898d0-175">You're free to implement just the feature interfaces your application needs, though at a minimum you must support [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span></span>

## <a name="next-steps"></a><span data-ttu-id="898d0-176">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="898d0-176">Next steps</span></span>

<span data-ttu-id="898d0-177">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="898d0-177">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="898d0-178">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="898d0-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

- [<span data-ttu-id="898d0-179">Kestrel</span><span class="sxs-lookup"><span data-stu-id="898d0-179">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="898d0-180">IIS ile kestrel</span><span class="sxs-lookup"><span data-stu-id="898d0-180">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="898d0-181">Kestrel Nginx ile</span><span class="sxs-lookup"><span data-stu-id="898d0-181">Kestrel with Nginx</span></span>](../../publishing/linuxproduction.md)
- [<span data-ttu-id="898d0-182">Apache ile kestrel</span><span class="sxs-lookup"><span data-stu-id="898d0-182">Kestrel with Apache</span></span>](../../publishing/apache-proxy.md)
- [<span data-ttu-id="898d0-183">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="898d0-183">HTTP.sys</span></span>](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="898d0-184">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="898d0-184">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

- [<span data-ttu-id="898d0-185">Kestrel</span><span class="sxs-lookup"><span data-stu-id="898d0-185">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="898d0-186">IIS ile kestrel</span><span class="sxs-lookup"><span data-stu-id="898d0-186">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="898d0-187">Kestrel Nginx ile</span><span class="sxs-lookup"><span data-stu-id="898d0-187">Kestrel with Nginx</span></span>](../../publishing/linuxproduction.md)
- [<span data-ttu-id="898d0-188">Apache ile kestrel</span><span class="sxs-lookup"><span data-stu-id="898d0-188">Kestrel with Apache</span></span>](../../publishing/apache-proxy.md)
- [<span data-ttu-id="898d0-189">WebListener</span><span class="sxs-lookup"><span data-stu-id="898d0-189">WebListener</span></span>](weblistener.md)

---