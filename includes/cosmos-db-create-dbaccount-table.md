1. V nové okno prohlížeče, přihlaste se k [portál Azure](https://portal.azure.com/).
2. V nabídce vlevo klikněte na **Nový**, potom na **Databáze** a nakonec v části **Databáze Azure Cosmos** klikněte na **Vytvořit**. 
   
   ![Snímek obrazovky webu Azure Portal se zvýrazněním položek Další služby a Databáze Azure Cosmos](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-1.png)

3. V **nový účet** zadejte nastavení pro nový účet Azure Cosmos DB. 
 
    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID|*Zadejte jedinečný název*|Zadejte jedinečný název pro identifikaci tohoto účtu Azure Cosmos DB. Jelikož je řetězec *documents.azure.com* připojený k ID, které poskytnete k vytvoření identifikátoru URI, použijte jedinečné, ale snadno rozpoznatelné ID.<br><br>Toto ID může obsahovat pouze malá písmena, číslice a znak spojovníku (-) a musí se skládat ze 3 až 50 znaků.
    Rozhraní API|Tabulky Azure|Rozhraní API Určuje typ účtu chcete-li vytvořit. Poskytuje Azure Cosmos DB pět rozhraní API pro vyhovuje potřebám vaší aplikace: SQL (databáze dokumentu), Gremlin (grafu databáze), MongoDB (databáze dokumentu), Azure Table a Cassandra, každý, které aktuálně vyžadují samostatný účet.<br><br>Vyberte **Azure Table** vzhledem k tomu, že v tento rychlý start vytváříte tabulku, která funguje s rozhraním API pro tabulku.<br><br>[Další informace o rozhraní API tabulky](../articles/cosmos-db/table-introduction.md) |
    Předplatné|*Zadejte stejný jedinečný název, který výše uvedeného v ID*|Vyberte předplatné Azure, který chcete použít pro tento účet Azure Cosmos DB. 
    Skupina prostředků|*Stejná hodnota jako ID*|Zadejte nový název skupiny prostředků pro váš účet. V zájmu jednoduchosti můžete použít název, který se shoduje s vaším ID. 
    Umístění|*Vyberte oblast nejbližší uživatelům.*|Vyberte zeměpisné umístění, ve kterém k hostování účtu Azure Cosmos DB. Použijte umístění, které je nejblíže k uživatelům poskytnout nejrychlejší přístup k datům.
    Povolit geografická redundance| Ponechte prázdné | Tím se vytvoří replikované verzi databáze v druhé (spárované) oblasti. Nechte prázdné.  
    Připnutí na řídicí panel | Vyberte | Zaškrtněte toto políčko, aby nový databázový účet se přidá do řídicího panelu portálu pro snadný přístup.

    Poté klikněte na **Vytvořit**.  

    ![Snímek okna Nový v databázi Azure Cosmos](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-2.png)

4. Vytvoření účtu trvá několik minut. Během účet vytvoření portálu zobrazí **nasazení Azure DB Cosmos** dlaždici.

    ![Podokno Oznámení portálu Azure Portal](./media/cosmos-db-create-dbaccount-table/deploying-cosmos-db.png)

    Po vytvoření účtu **Blahopřejeme! Byl vytvořen účet Azure Cosmos DB** zobrazí se stránka.
