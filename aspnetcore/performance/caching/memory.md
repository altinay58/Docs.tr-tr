---
title: Bellek içi ASP.NET Core, önbelleğe alma
author: rick-anderson
description: ASP.NET Core bellekte önbelleğe öğrenin.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/memory
ms.openlocfilehash: 4835e2331afca7a648abac6bc35d255ec6356067
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/24/2018
---
# <a name="cache-in-memory-in-aspnet-core"></a>Bellek içi ASP.NET Core, önbelleğe alma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), ve [Steve Smith](https://ardalis.com/)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="caching-basics"></a>Önbelleğe alma temelleri

Önbelleğe alma önemli ölçüde performans ve ölçeklenebilirlik, bir uygulamanın içeriği oluşturmak için gereken iş azaltarak artırabilirsiniz. Seyrek değişen verileri ile en iyi önbelleğe alma çalışır. Önbelleğe alma yapar çok döndürülen verilerin bir kopyasını özgün kaynağından hızlıdır. Yazma ve hiçbir zaman önbelleğe alınmış verileri bağımlı uygulamanızı test etme gerekir.

ASP.NET Core birkaç farklı önbellek destekler. En basit önbellek dayanır [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), web sunucusu bellekte bir önbellek temsil eder. Bir sunucu grubunda birden çok sunucu çalışan uygulamaları oturumları bellek içi önbellek kullanırken Yapışkan emin olun. Yapışkan oturumları tüm istemciden gelen sonraki istekleri aynı sunucuya gidin emin olun. Örneğin, Azure Web apps kullanımı [uygulama isteği yönlendirme](https://www.iis.net/learn/extensions/planning-for-arr) tüm istekler aynı sunucuya yönlendirmek için (ARR).

Bir web grubunda olmayan Yapışkan oturumları gerektiren bir [dağıtılmış önbellek](distributed.md) önbellek tutarlılık sorunları önlemek için. Bazı uygulamalar için bir bellek içi önbellek daha yüksek ölçek genişletme dağıtılmış önbellek destekleyebilir. Dağıtılmış önbellek kullanarak bir dış işlem için önbelleği boşaltır. 

`IMemoryCache` Sürece, önbelleği önbellek girişlerinin bellek baskısı altında Tahliye [önbelleğe öncelik](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) ayarlanır `CacheItemPriority.NeverRemove`. Ayarlayabileceğiniz `CacheItemPriority` önbellek çıkarır öğeleri bellek baskısı altında önceliğini ayarlamak için.

Bellek içi önbellek herhangi bir nesne depolayabilir; Dağıtılmış önbellek arabirimi sınırlıdır `byte[]`.

## <a name="using-imemorycache"></a>IMemoryCache kullanma

Bellek içi önbelleğe alma bir *hizmet* , uygulamayı kullanarak başvurulan [bağımlılık ekleme](../../fundamentals/dependency-injection.md). Çağrı `AddMemoryCache` içinde `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=8)] 

İstek `IMemoryCache` oluşturucuda örneği:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor&highlight=3,5-999)] 

`IMemoryCache` NuGet paketi "Microsoft.Extensions.Caching.Memory" gerektirir.

Aşağıdaki kod [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) birer önbellekte olup olmadığını denetlemek için. Bir süre önbelleğe değil, yeni bir girdi oluşturulur ve önbellek ile eklenen [ayarlamak](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Geçerli saati ve önbelleğe alınan saat görüntülenir:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Önbelleğe alınan `DateTime` değeri, zaman aşımı süresi (ve bellek baskısı nedeniyle hiçbir çıkarma) içinde istekler varken önbellekte kalır. Aşağıdaki resimde, geçerli saati ve önbellekten daha eski bir zaman gösterilmektedir:

![Dizin görünümünün görüntülenen iki farklı saatleri](memory/_static/time.png)

Aşağıdaki kod [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) ve [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) verileri önbelleğe. 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Aşağıdaki kod çağrıları [almak](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) önbelleğe alınmış zaman getirmek için:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

Bkz: [IMemoryCache yöntemleri](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) ve [CacheExtensions yöntemleri](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) önbellek yöntemleri açıklaması.

## <a name="using-memorycacheentryoptions"></a>MemoryCacheEntryOptions kullanma

Aşağıdaki örnek:

- Mutlak sona erme zamanı ayarlar. Bu giriş önbelleğe alınacak en fazla süreyi ve öğe kayan zaman aşımı sürekli olarak yenilendiğinde çok eski hale gelmesini engeller.
- Kayan süre sonu zamanı ayarlar. Bu önbelleğe alınan öğe erişim istekleri kayan sona erme saati sıfırlanır.
- Önbellek önceliği ayarlar `CacheItemPriority.NeverRemove`. 
- Ayarlar bir [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , çağrılır giriş önbellekten çıkarılmasına sonra. Öğeyi önbellekten kaldırır kodundan farklı bir iş parçacığı üzerinde geri arama çalıştırın.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-20)]

## <a name="cache-dependencies"></a>Önbellek bağımlılıkları

Aşağıdaki örnek, bağımlı giriş süresi dolarsa önbellek girişinin süresi dolacak şekilde gösterilmiştir. A `CancellationChangeToken` önbelleğe alınmış öğesine eklenir. Zaman `Cancel` üzerinde adlı `CancellationTokenSource`, her iki önbellek girişlerinin çıkarılacak. 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Kullanarak bir `CancellationTokenSource` grup olarak çıkarılacak birden fazla önbellek girişlerinin sağlar. İle `using` önbellek girişlerinin Yukarıdaki kod modelinde oluşturulan içinde `using` blok tetikleyiciler ve sona erme ayarları devralır.

## <a name="additional-notes"></a>Ek Notlar

- Bir önbellek öğesi yeniden doldurmak için bir geri çağırma kullanırken:

  - Geri çağırma tamamlanmadığından kurmadı birden çok istek önbelleğe alınan anahtar değeri boş bulabilirsiniz. 
  - Bu, önbelleğe alınan öğe yeniden birkaç iş parçacığı neden olabilir.

- Başka bir oluşturmak için bir önbellek girişi kullanıldığında, alt üst girişin sona erme belirteçleri ve zaman tabanlı sona erme ayarları kopyalar. Alt tarafından el ile temizleme süresi dolmuş ya da üst girişinin güncelleştirme değil.

## <a name="additional-resources"></a>Ek kaynaklar

* [Dağıtılmış önbellekle çalışma](xref:performance/caching/distributed)
* [Değişiklik belirteçleri değişikliklerle Algıla](xref:fundamentals/primitives/change-tokens)
* [Yanıtları önbelleğe alma](xref:performance/caching/response)
* [Yanıtları Önbelleğe Alma Ara Yazılımı](xref:performance/caching/middleware)
* [Önbellek Etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Dağıtılmış Önbellek Etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
