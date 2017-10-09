
Můžete informace o Azure Cosmos DB globální distribuce v této Azure pátek video s Scott Hanselman a Karthik Raman hlavní manažer inženýrství.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

Další informace o tom, jak globální replikace databáze v Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Cosmos DB](../articles/documentdb/documentdb-distribute-data-globally.md).

## <a id="addregion"></a>Přidejte globální databáze oblastí pomocí hello portálu Azure
Je k dispozici ve všech Azure Cosmos DB [oblastí Azure] [ azureregions] celém světě. Po výběru hello výchozí úroveň konzistence pro váš účet databáze, můžete přidružit jeden nebo více oblastí (v závislosti na vaši volbu výchozí konzistence úrovně a globální distribuci potřeby).

1. V hello [portál Azure](https://portal.azure.com/), v levém panelu text hello, klikněte na **Azure Cosmos DB**.
2. V hello **Azure Cosmos DB** okně, vyberte hello databáze účet toomodify.
3. V okně účtu hello, klikněte na tlačítko **replikovat data globálně** nabídce hello.
4. V hello **replikovat data globálně** okně vyberte hello oblasti tooadd nebo odebrat tak, že kliknete na oblasti v mapě hello a klikněte **Uložit**. Je tooadding oblasti náklady, najdete v části hello [stránce s cenami](https://azure.microsoft.com/pricing/details/documentdb/) nebo hello [distribuci dat globálně s DocumentDB](../articles/documentdb/documentdb-distribute-data-globally.md) Další informace najdete v článku.
   
    ![Klikněte na tlačítko oblasti hello v hello mapy tooadd nebo je odebrat][1]
    
Po přidání druhého oblast je hello **ruční převzetí služeb při selhání** na hello je povolena možnost **replikovat data globálně** okna portálu hello. Můžete použít tento proces možnost tootest hello převzetí služeb při selhání nebo změnit hello zápisu primární oblasti. Jakmile přidáte třetí oblast, hello **priorit převzetí služeb při selhání** je zapnuta možnost hello stejné okno, ve kterém můžete změnit hello pořadí převzetí služeb při selhání pro čtení.  

### <a name="selecting-global-database-regions"></a>Výběr oblasti globální databáze
Existují dvě běžné scénáře pro konfiguraci dvou nebo více oblastí:

1. Doručování přístup s nízkou latencí toodata tooend uživatelů bez ohledu na to, kde se nachází kolem zeměkouli hello
2. Přidání místní odolnost pro provozní kontinuitu a zotavení po havárii (BCDR)

Pro uživatele,-podávající nízkou latencí tooend se doporučuje toodeploy hello aplikace i přidat Azure DB Cosmos v oblastech hello, odpovídající uživatelé toowhere hello aplikace jsou umístěné.

BCDR, je doporučeno tooadd oblastí, na základě páru oblast hello popsané v hello [obchodní kontinuitu a zotavení po havárii (BCDR): spárovat oblasti Azure] [ bcdr] článku.

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
