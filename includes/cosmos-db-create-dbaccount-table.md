1. V novém okně přihlásit toohello [portál Azure](https://portal.azure.com/).
2. V levé nabídce hello, klikněte na **nový**, klikněte na tlačítko **databáze**a potom v části **Azure Cosmos DB**, klikněte na tlačítko **vytvořit**.
   
   ![Snímek obrazovky portálu Azure, zvýraznění více služeb a Azure Cosmos DB hello](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-1.png)

3. V hello **nový účet** okno, zadejte požadovanou konfiguraci hello hello účet Azure Cosmos DB. 

    V databázi Azure Cosmos můžete vybrat jeden ze čtyř programovacích modelů: Gremlin (graf), MongoDB, SQL (DocumentDB) a Tabulka (klíč-hodnota). 
    
    V této úvodní jsme budete mít programové ošetření hello tabulky rozhraní API, zvolíte **tabulky (klíč hodnota)** při vyplňování formuláře hello. Pokud ale máte data grafu pro aplikaci sociálních médií, data dokumentu z aplikace katalogu nebo data migrovaná z aplikace MongoDB, je dobré si uvědomit, že databáze Azure Cosmos může poskytnout vysoce dostupnou a globálně distribuovanou platformu databázové služby pro všechny důležité podnikové aplikace.

    Vyplňte nové okno účtu hello pomocí hello informace na snímku obrazovky hello jako vodítko. Zvolíte jedinečné hodnoty jako při nastavování účtu, takže hodnoty nebudou přesně odpovídat hello – snímek obrazovky. 
 
    ![Snímek obrazovky okna Nový Azure DB Cosmos hello](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-2.png)

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID|*Jedinečná hodnota*|Jedinečný název zvolíte účet Azure Cosmos DB tooidentify hello. *Documents.Azure.com* je ID připojením toohello zadejte toocreate váš identifikátor URI, takže použijte jedinečné, ale osobní ID. Hello ID může obsahovat jenom malá písmena, čísla a hello '-' znak a musí být v rozmezí 3 až 50 znaků.
    Rozhraní API|Table (key-value) (Tabulka (klíč-hodnota))|Jsme budete programování proti hello [tabulky API](../articles/cosmos-db/table-introduction.md) dále v tomto článku.|
    Předplatné|*Vaše předplatné*|Hello předplatné Azure, že chcete toouse pro účet Azure Cosmos DB hello. 
    Skupina prostředků|*Hello stejnou hodnotu jako ID*|Hello nový název skupiny prostředků pro váš účet. Pro jednoduchost můžete použít hello stejný název jako vaše ID. 
    Umístění|*Hello oblast nejbližší tooyour uživatelů*|Hello zeměpisného umístění, ve které toohost účtu Azure Cosmos DB. Vyberte umístění hello nejbližší uživatelé tooyour toogive je hello nejrychlejší přístup k datům toohello.   

4. Klikněte na tlačítko **vytvořit** toocreate hello účtu.
5. Na panelu nástrojů hello, klikněte na tlačítko **oznámení** procesu nasazení toomonitor hello.

    ![Oznámení o zahájení nasazení](./media/cosmos-db-create-dbaccount-table/notification.png)

6.  Po dokončení nasazení hello otevřete hello nový účet z hello všechny prostředky dlaždici. 

    ![Účet DocumentDB na hello dlaždici všechny prostředky](./media/cosmos-db-create-dbaccount-table/all-resources.png)
