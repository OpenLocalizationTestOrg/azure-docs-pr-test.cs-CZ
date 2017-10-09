<span data-ttu-id="13bd6-101">Aplikace .NET mohou použít hello **StackExchange.Redis** mezipaměti klienta, které lze konfigurovat v sadě Visual Studio pomocí balíčku NuGet, který zjednodušuje konfiguraci aplikací klientů mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="13bd6-101">.NET applications can use hello **StackExchange.Redis** cache client, which can be configured in Visual Studio using a NuGet package that simplifies hello configuration of cache client applications.</span></span> 

> [!NOTE]
> <span data-ttu-id="13bd6-102">Další informace najdete v tématu hello [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) githubu stránku a hello [dokumentaci ke klientu mezipaměti StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis#documentation).</span><span class="sxs-lookup"><span data-stu-id="13bd6-102">For more information, see hello [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) github page and  hello [StackExchange.Redis cache client documentation](http://github.com/StackExchange/StackExchange.Redis#documentation).</span></span>
> 
> 

<span data-ttu-id="13bd6-103">tooconfigure klientskou aplikaci v sadě Visual Studio pomocí balíčku StackExchange.Redis NuGet, hello klikněte pravým tlačítkem na projekt hello v **Průzkumníku řešení** a zvolte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="13bd6-103">tooconfigure a client application in Visual Studio using hello StackExchange.Redis NuGet package, right-click hello project in **Solution Explorer** and choose **Manage NuGet Packages**.</span></span> 

![Správa balíčků NuGet](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

<span data-ttu-id="13bd6-105">Typ **StackExchange.Redis** nebo **StackExchange.Redis.StrongName** do textového pole hledání text hello, vyberte požadovanou verzi hello hello výsledky a klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="13bd6-105">Type **StackExchange.Redis** or **StackExchange.Redis.StrongName** into hello search text box, select hello desired version from hello results, and click **Install**.</span></span>

> [!NOTE]
> <span data-ttu-id="13bd6-106">Pokud dáváte přednost toouse silným názvem verzi hello **StackExchange.Redis** klientské knihovny, zvolte **StackExchange.Redis.StrongName**; jinak vyberte **StackExchange.Redis**.</span><span class="sxs-lookup"><span data-stu-id="13bd6-106">If you prefer toouse a strong-named version of hello **StackExchange.Redis** client library, choose **StackExchange.Redis.StrongName**; otherwise choose **StackExchange.Redis**.</span></span>
> 
> 

![Balíček StackExchange.Redis NuGet](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

<span data-ttu-id="13bd6-108">Hello NuGet balíček stáhne a přidá hello požadované odkazy na sestavení pro vašeho klienta aplikace tooaccess Azure Redis Cache pomocí klienta mezipaměti StackExchange.Redis hello.</span><span class="sxs-lookup"><span data-stu-id="13bd6-108">hello NuGet package downloads and adds hello required assembly references for your client application tooaccess Azure Redis Cache with hello StackExchange.Redis cache client.</span></span>

> [!NOTE]
> <span data-ttu-id="13bd6-109">Pokud jste dříve nakonfigurovali vašeho projektu toouse StackExchange.Redis, můžete zkontrolovat pro balíček aktualizace toohello z hello **Správce balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="13bd6-109">If you have previously configured your project toouse StackExchange.Redis, you can check for updates toohello package from hello **NuGet Package Manager**.</span></span> <span data-ttu-id="13bd6-110">toocheck pro a nainstalujte aktualizované verze hello balíčku StackExchange.Redis NuGet, klikněte na tlačítko **aktualizace** v hello hello **Správce balíčků NuGet** okno.</span><span class="sxs-lookup"><span data-stu-id="13bd6-110">toocheck for and install updated versions of hello StackExchange.Redis NuGet package, click **Updates** in hello hello **NuGet Package Manager** window.</span></span> <span data-ttu-id="13bd6-111">Pokud aktualizaci toohello balíčku StackExchange.Redis NuGet je k dispozici, můžete je aktualizovat vaše projektu toouse hello aktualizované verze.</span><span class="sxs-lookup"><span data-stu-id="13bd6-111">If an update toohello StackExchange.Redis NuGet package is available, you can update your project toouse hello updated version.</span></span>
> 
> 

<span data-ttu-id="13bd6-112">Můžete taky nainstalovat hello balíčku StackExchange.Redis NuGet kliknutím **Správce balíčků NuGet**, **Konzola správce balíčků** z hello **nástroje** nabídce a spuštěné hello Následující příkaz z hello **Konzola správce balíčků** okno.</span><span class="sxs-lookup"><span data-stu-id="13bd6-112">You can also install hello StackExchange.Redis NuGet package by clicking **NuGet Package Manager**, **Package Manager Console** from hello **Tools** menu, and running hello following command from hello **Package Manager Console** window.</span></span>
    
```
Install-Package StackExchange.Redis
```
