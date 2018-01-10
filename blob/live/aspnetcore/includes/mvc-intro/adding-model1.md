# <a name="adding-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="74a4e-101">Bir ASP.NET Core MVC uygulamasına bir modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="74a4e-101">Adding a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="74a4e-102">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="74a4e-102">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="74a4e-103">Bu bölümde, bir veritabanında filmler yönetmek için bazı sınıfları ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="74a4e-103">In this section, you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="74a4e-104">Bu sınıfların olacak "**M**odel" parçası **M**VC uygulama.</span><span class="sxs-lookup"><span data-stu-id="74a4e-104">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="74a4e-105">Bu sınıflarla kullandığınız [Entity Framework Çekirdek](https://docs.microsoft.com/ef/core) (EF bir veritabanıyla çalışmak için temel).</span><span class="sxs-lookup"><span data-stu-id="74a4e-105">You use these classes with [Entity Framework Core](https://docs.microsoft.com/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="74a4e-106">EF çekirdek yazmak zorunda veri erişim kodu basitleştiren bir nesne ilişkisel eşleme (ORM) çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="74a4e-106">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span> <span data-ttu-id="74a4e-107">[EF çekirdek destekleyen birçok veritabanı motoru](https://docs.microsoft.com/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="74a4e-107">[EF Core supports many database engines](https://docs.microsoft.com/ef/core/providers/).</span></span>

<span data-ttu-id="74a4e-108">EF çekirdeği üzerinde herhangi bir bağımlılığı olmadığından oluşturacağınız modeli sınıfları ("düz eski CLR nesnelerden") POCO sınıfları olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="74a4e-108">The model classes you'll create are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="74a4e-109">Bunlar yalnızca veritabanında depolanan verilerin özelliklerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="74a4e-109">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="74a4e-110">Bu öğreticide, modeli sınıfları ilk yazacaksınız ve EF çekirdek veritabanını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="74a4e-110">In this tutorial you'll write the model classes first, and EF Core will create the database.</span></span> <span data-ttu-id="74a4e-111">Burada kapsamında olmayan alternatif bir yaklaşım, zaten varolan bir veritabanından modeli sınıfları oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="74a4e-111">An alternate approach not covered here is to generate model classes from an already-existing database.</span></span> <span data-ttu-id="74a4e-112">Bu yaklaşımı hakkında daha fazla bilgi için bkz: [ASP.NET Core - var olan veritabanı](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="74a4e-112">For information about that approach, see [ASP.NET Core - Existing Database](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span></span>

## <a name="add-a-data-model-class"></a><span data-ttu-id="74a4e-113">Bir veri modeli sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="74a4e-113">Add a data model class</span></span>