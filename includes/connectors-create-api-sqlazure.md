### <a name="prerequisites"></a>Požadavky
* Účet Azure; můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
* [Azure SQL Database](../articles/sql-database/sql-database-get-started.md) se svými informacemi o připojení, včetně hello název serveru, názvu databáze a uživatelského jména a hesla. Tato informace je obsažena v hello připojovací řetězec databáze SQL:
  
    Server = tcp:*yoursqlservername*. database.windows.net,1433;Initial katalogu =*yourqldbname*; Zachovat bezpečnostní údaje = False; ID uživatele = {your_username}; Heslo = {your_password}; MultipleActiveResultSets = False; Šifrování = True; TrustServerCertificate = False; Časový limit připojení = 30;
  
    Další informace o [databází SQL Azure](https://azure.microsoft.com/services/sql-database).

> [!NOTE]
> Při vytváření databáze SQL Azure, můžete také vytvořit hello ukázkové databáze součástí SQL. 
> 
> 

Před použitím vaší databázi SQL Azure v aplikaci logiky, připojte tooyour databáze SQL. Můžete k tomu snadno v rámci aplikace logiky na hello portálu Azure.  

Připojte tooyour, které Azure SQL Database pomocí hello následující kroky:  

1. Vytvoření aplikace logiky. V Návrháři hello Logic Apps přidejte aktivační událost a potom přidat akci. Vyberte **zobrazit Microsoft spravované rozhraní API** v hello rozevíracím seznamu a pak zadejte "sql" hello vyhledávacího pole. Vyberte jednu z akcí hello:  
   
    ![Krok vytvoření připojení SQL Azure](./media/connectors-create-api-sqlazure/sql-actions.png)
2. Pokud jste dosud nevytvořili žádné tooSQL připojení databáze, budete vyzváni k podrobnosti připojení hello:  
   
    ![Krok vytvoření připojení SQL Azure](./media/connectors-create-api-sqlazure/connection-details.png) 
3. Zadejte podrobnosti databáze SQL hello. Vyžadují se vlastnosti s hvězdičkou.
   
   | Vlastnost | Podrobnosti |
   | --- | --- |
   | Připojit prostřednictvím brány |Nechte zaškrtnuté políčko. Používá se při připojování tooan místní systém SQL Server. |
   | Název připojení * |Zadejte libovolný název pro připojení. |
   | Název systému SQL Server * |Zadejte název serveru hello; což je něco podobného jako *servername.database.windows.net*. název serveru Hello se zobrazí v hello vlastnosti SQL Database v hello portál Azure a také zobrazuje na hello připojovací řetězec. |
   | Název databáze SQL * |Zadejte název hello dáte vaší databázi SQL. Položka je uvedena ve vlastnostech SQL Database hello v připojovacím řetězci hello: Initial Catalog =*yoursqldbname*. |
   | Uživatelské jméno * |Zadejte uživatelské jméno hello, kterou jste vytvořili při vytvoření hello databáze SQL. Položka je uvedena ve vlastnostech SQL Database hello v hello portálu Azure. |
   | Heslo * |Zadejte hello heslo, které jste vytvořili při vytvoření hello databáze SQL. |
   
    Tyto přihlašovací údaje jsou použité tooauthorize vaše tooconnect logiku aplikace a přistupovat k datům SQL. Po dokončení podrobné informace o připojení vypadat podobně jako toohello následující:  
   
    ![Krok vytvoření připojení SQL Azure](./media/connectors-create-api-sqlazure/sample-connection.png) 
4. Vyberte **Vytvořit**. 
5. Všimněte si hello připojení bylo vytvořeno. Teď pokračujte hello další kroky v aplikaci logiky: 
   
    ![Krok vytvoření připojení SQL Azure](./media/connectors-create-api-sqlazure/table.png)

