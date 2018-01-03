<span data-ttu-id="cfd26-101">Aşağıdaki özellikleri ekleyin `Movie` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="cfd26-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="cfd26-102">`ID` Alan veritabanı için birincil anahtarı gerekli.</span><span class="sxs-lookup"><span data-stu-id="cfd26-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="cfd26-103">Veritabanı bağlamı sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="cfd26-103">Add a database context class</span></span>

<span data-ttu-id="cfd26-104">Aşağıdakileri ekleyin `DbContext` türetilmiş sınıf adlı *MovieContext.cs* için *modelleri* klasörü:[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="cfd26-104">Add the following `DbContext` derived class named *MovieContext.cs* to the *Models* folder: [!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span></span>

<span data-ttu-id="cfd26-105">Önceki kod oluşturur bir `DbSet` özelliği için varlık kümesi.</span><span class="sxs-lookup"><span data-stu-id="cfd26-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="cfd26-106">Entity Framework terminoloji, bir varlık kümesine genellikle bir veritabanı tablosuna karşılık gelir ve bir varlık tablosunda bir satırı karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="cfd26-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>