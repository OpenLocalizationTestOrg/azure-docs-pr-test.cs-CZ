
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-hello-connection-string-from-hello-azure-portal"></a>Získat připojovací řetězec hello z hello portálu Azure
Použití hello [portál Azure](https://portal.azure.com/) tooobtain hello připojovací řetězec potřebné pro váš klientský program toointeract s Azure SQL Database: 

1. Klikněte na tlačítko **Procházet** > **databází SQL**.
2. Zadejte název hello databáze do hello filtru textovém poli téměř hello levém hello **databází SQL** okno.
3. Klikněte na tlačítko hello řádek pro vaši databázi.
4. Po hello okno se zobrazí pro vaši databázi, minimalizujte hello standardní pro visual užitečný můžete kliknout na ovládací prvky toocollapse hello okna, které jste použili pro procházení a filtrování databáze. 
   
    ![Filtrovat tooisolate databáze][10-FilterDatabase]
5. V okně hello pro vaši databázi, klikněte na tlačítko **zobrazit databázové připojovací řetězce**.
6. Pokud máte v úmyslu toouse hello ADO.NET připojení knihovny, zkopírujte hello řetězec s názvem bez přípony **ADO**. 
   
    ![Zkopírujte hello ADO připojovací řetězec pro vaši databázi][20-CopyAdoConnectionString]
7. Do jednoho formátu nebo jiné vložte informace o připojovacím řetězci hello do váš klientský program kód.

Další informace naleznete v tématu:<br/>[Připojovací řetězce a konfigurační soubory](http://msdn.microsoft.com/library/ms254494.aspx).

<!-- Image references. -->

[10-FilterDatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png

[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
