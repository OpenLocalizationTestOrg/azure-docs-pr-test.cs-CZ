
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-hello-connection-string-from-hello-azure-portal"></a><span data-ttu-id="fc2f8-101">Získat připojovací řetězec hello z hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fc2f8-101">Obtain hello connection string from hello Azure portal</span></span>
<span data-ttu-id="fc2f8-102">Použití hello [portál Azure](https://portal.azure.com/) tooobtain hello připojovací řetězec potřebné pro váš klientský program toointeract s Azure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="fc2f8-102">Use hello [Azure portal](https://portal.azure.com/) tooobtain hello connection string necessary for your client program toointeract with Azure SQL Database:</span></span> 

1. <span data-ttu-id="fc2f8-103">Klikněte na tlačítko **Procházet** > **databází SQL**.</span><span class="sxs-lookup"><span data-stu-id="fc2f8-103">Click **BROWSE** > **SQL databases**.</span></span>
2. <span data-ttu-id="fc2f8-104">Zadejte název hello databáze do hello filtru textovém poli téměř hello levém hello **databází SQL** okno.</span><span class="sxs-lookup"><span data-stu-id="fc2f8-104">Enter hello name of your database into hello filter text box near hello upper-left of hello **SQL databases** blade.</span></span>
3. <span data-ttu-id="fc2f8-105">Klikněte na tlačítko hello řádek pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="fc2f8-105">Click hello row for your database.</span></span>
4. <span data-ttu-id="fc2f8-106">Po hello okno se zobrazí pro vaši databázi, minimalizujte hello standardní pro visual užitečný můžete kliknout na ovládací prvky toocollapse hello okna, které jste použili pro procházení a filtrování databáze.</span><span class="sxs-lookup"><span data-stu-id="fc2f8-106">After hello blade appears for your database, for visual convenience you can click hello standard minimize controls toocollapse hello blades  you used for browsing and database filtering.</span></span> 
   
    ![Filtrovat tooisolate databáze][10-FilterDatabase]
5. <span data-ttu-id="fc2f8-108">V okně hello pro vaši databázi, klikněte na tlačítko **zobrazit databázové připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="fc2f8-108">On hello blade for your database, click **Show database connection strings**.</span></span>
6. <span data-ttu-id="fc2f8-109">Pokud máte v úmyslu toouse hello ADO.NET připojení knihovny, zkopírujte hello řetězec s názvem bez přípony **ADO**.</span><span class="sxs-lookup"><span data-stu-id="fc2f8-109">If you intend toouse hello ADO.NET connection library, copy hello string labeled **ADO**.</span></span> 
   
    ![Zkopírujte hello ADO připojovací řetězec pro vaši databázi][20-CopyAdoConnectionString]
7. <span data-ttu-id="fc2f8-111">Do jednoho formátu nebo jiné vložte informace o připojovacím řetězci hello do váš klientský program kód.</span><span class="sxs-lookup"><span data-stu-id="fc2f8-111">In one format or another, paste hello connection string information into your client program code.</span></span>

<span data-ttu-id="fc2f8-112">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="fc2f8-112">For more information, see:</span></span><br/><span data-ttu-id="fc2f8-113">[Připojovací řetězce a konfigurační soubory](http://msdn.microsoft.com/library/ms254494.aspx).</span><span class="sxs-lookup"><span data-stu-id="fc2f8-113">[Connection Strings and Configuration Files](http://msdn.microsoft.com/library/ms254494.aspx).</span></span>

<!-- Image references. -->

[10-FilterDatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png

[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
