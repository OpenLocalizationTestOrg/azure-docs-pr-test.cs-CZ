
<span data-ttu-id="09182-101">Můžete informace o Azure Cosmos DB globální distribuce v této Azure pátek video s Scott Hanselman a Karthik Raman hlavní manažer inženýrství.</span><span class="sxs-lookup"><span data-stu-id="09182-101">You can learn about Azure Cosmos DB global distribution in this Azure Friday video with Scott Hanselman and Principal Engineering Manager Karthik Raman.</span></span>

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

<span data-ttu-id="09182-102">Další informace o tom, jak globální replikace databáze v Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Cosmos DB](../articles/documentdb/documentdb-distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="09182-102">For more information about how global database replication works in Cosmos DB, see [Distribute data globally with Cosmos DB](../articles/documentdb/documentdb-distribute-data-globally.md).</span></span>

## <span data-ttu-id="09182-103"><a id="addregion"></a>Přidejte globální databáze oblastí pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="09182-103"><a id="addregion"></a>Add global database regions using hello Azure Portal</span></span>
<span data-ttu-id="09182-104">Je k dispozici ve všech Azure Cosmos DB [oblastí Azure] [ azureregions] celém světě.</span><span class="sxs-lookup"><span data-stu-id="09182-104">Azure Cosmos DB is available in all [Azure regions][azureregions] world-wide.</span></span> <span data-ttu-id="09182-105">Po výběru hello výchozí úroveň konzistence pro váš účet databáze, můžete přidružit jeden nebo více oblastí (v závislosti na vaši volbu výchozí konzistence úrovně a globální distribuci potřeby).</span><span class="sxs-lookup"><span data-stu-id="09182-105">After selecting hello default consistency level for your database account, you can associate one or more regions (depending on your choice of default consistency level and global distribution needs).</span></span>

1. <span data-ttu-id="09182-106">V hello [portál Azure](https://portal.azure.com/), v levém panelu text hello, klikněte na **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="09182-106">In hello [Azure portal](https://portal.azure.com/), in hello left bar, click **Azure Cosmos DB**.</span></span>
2. <span data-ttu-id="09182-107">V hello **Azure Cosmos DB** okně, vyberte hello databáze účet toomodify.</span><span class="sxs-lookup"><span data-stu-id="09182-107">In hello **Azure Cosmos DB** blade, select hello database account toomodify.</span></span>
3. <span data-ttu-id="09182-108">V okně účtu hello, klikněte na tlačítko **replikovat data globálně** nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="09182-108">In hello account blade, click **Replicate data globally** from hello menu.</span></span>
4. <span data-ttu-id="09182-109">V hello **replikovat data globálně** okně vyberte hello oblasti tooadd nebo odebrat tak, že kliknete na oblasti v mapě hello a klikněte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="09182-109">In hello **Replicate data globally** blade, select hello regions tooadd or remove by clicking regions in hello map, and then click **Save**.</span></span> <span data-ttu-id="09182-110">Je tooadding oblasti náklady, najdete v části hello [stránce s cenami](https://azure.microsoft.com/pricing/details/documentdb/) nebo hello [distribuci dat globálně s DocumentDB](../articles/documentdb/documentdb-distribute-data-globally.md) Další informace najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="09182-110">There is a cost tooadding regions, see hello [pricing page](https://azure.microsoft.com/pricing/details/documentdb/) or hello [Distribute data globally with DocumentDB](../articles/documentdb/documentdb-distribute-data-globally.md) article for more information.</span></span>
   
    ![Klikněte na tlačítko oblasti hello v hello mapy tooadd nebo je odebrat][1]
    
<span data-ttu-id="09182-112">Po přidání druhého oblast je hello **ruční převzetí služeb při selhání** na hello je povolena možnost **replikovat data globálně** okna portálu hello.</span><span class="sxs-lookup"><span data-stu-id="09182-112">Once you add a second region, hello **Manual Failover** option is enabled on hello **Replicate data globally** blade in hello portal.</span></span> <span data-ttu-id="09182-113">Můžete použít tento proces možnost tootest hello převzetí služeb při selhání nebo změnit hello zápisu primární oblasti.</span><span class="sxs-lookup"><span data-stu-id="09182-113">You can use this option tootest hello failover process or change hello primary write region.</span></span> <span data-ttu-id="09182-114">Jakmile přidáte třetí oblast, hello **priorit převzetí služeb při selhání** je zapnuta možnost hello stejné okno, ve kterém můžete změnit hello pořadí převzetí služeb při selhání pro čtení.</span><span class="sxs-lookup"><span data-stu-id="09182-114">Once you add a third region, hello **Failover Priorities** option is enabled on hello same blade so that you can change hello failover order for reads.</span></span>  

### <a name="selecting-global-database-regions"></a><span data-ttu-id="09182-115">Výběr oblasti globální databáze</span><span class="sxs-lookup"><span data-stu-id="09182-115">Selecting global database regions</span></span>
<span data-ttu-id="09182-116">Existují dvě běžné scénáře pro konfiguraci dvou nebo více oblastí:</span><span class="sxs-lookup"><span data-stu-id="09182-116">There are two common scenarios for configuring two or more regions:</span></span>

1. <span data-ttu-id="09182-117">Doručování přístup s nízkou latencí toodata tooend uživatelů bez ohledu na to, kde se nachází kolem zeměkouli hello</span><span class="sxs-lookup"><span data-stu-id="09182-117">Delivering low-latency access toodata tooend users no matter where they are located around hello globe</span></span>
2. <span data-ttu-id="09182-118">Přidání místní odolnost pro provozní kontinuitu a zotavení po havárii (BCDR)</span><span class="sxs-lookup"><span data-stu-id="09182-118">Adding regional resiliency for business continuity and disaster recovery (BCDR)</span></span>

<span data-ttu-id="09182-119">Pro uživatele,-podávající nízkou latencí tooend se doporučuje toodeploy hello aplikace i přidat Azure DB Cosmos v oblastech hello, odpovídající uživatelé toowhere hello aplikace jsou umístěné.</span><span class="sxs-lookup"><span data-stu-id="09182-119">For delivering low-latency tooend-users, it is recommended toodeploy both hello application and add Azure Cosmos DB in hello regions thats correspond toowhere hello application's users are located.</span></span>

<span data-ttu-id="09182-120">BCDR, je doporučeno tooadd oblastí, na základě páru oblast hello popsané v hello [obchodní kontinuitu a zotavení po havárii (BCDR): spárovat oblasti Azure] [ bcdr] článku.</span><span class="sxs-lookup"><span data-stu-id="09182-120">For BCDR, it is recommended tooadd regions based on hello region pairs described in hello [Business continuity and disaster recovery (BCDR): Azure Paired Regions][bcdr] article.</span></span>

<!--

## <a id="selectwriteregion"></a>Select hello write region

While all regions associated with your Cosmos DB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive hello write (insert, upsert, replace, delete) requests. tooset hello active write region, do hello following  


1. In hello **Azure Cosmos DB** blade, select hello database account toomodify.
2. In hello account blade, click **Replicate data globally** from hello menu.
3. In hello **Replicate data globally** blade, click **Manual Failover** from hello top bar.
    ![Change hello write region under Azure Cosmos DB Account > Replicate data globally > Manual Failover][2]
4. Select a read region toobecome hello new write region, click hello checkbox tooconfirm triggering a failover, and click OK
    ![Change hello write region by selecting a new region in list under Azure Cosmos DB Account > Replicate data globally > Manual Failover][3]

--->

<!--Image references-->
[1]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-add-region.png
[2]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-1.png
[3]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-2.png

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: ../articles/cosmos-db/consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
