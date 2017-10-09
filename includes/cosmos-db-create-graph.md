Teď můžete použít nástroj Průzkumník dat hello v hello Azure portálu toocreate databázi grafu. 

1. V hello portál Azure, v nabídce hello navigaci vlevo, klikněte na **Data Explorer (Preview)**. 
2. V hello **Data Explorer (Preview)** okně klikněte na tlačítko **nový graf**, potom vyplňte stránku hello pomocí hello následující informace.

    ![Průzkumník dat v hello portálu Azure](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer.png)

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID databáze|sample-database|Hello ID pro novou databázi. Názvy databází musí mít délku 1 až 255 znaků a nesmí obsahovat znaky `/ \ # ?` ani koncové mezery.
    ID grafu|sample-graph|Hello ID pro nový graf. Názvy grafu mají hello znak stejné požadavky jako ID databáze.
    Kapacita úložiště| 10 GB|Ponechte výchozí hodnotu hello. Toto je kapacita úložiště hello hello databáze.
    Propustnost|400 RU/s|Ponechte výchozí hodnotu hello. Je možné škálovat nahoru propustnost hello později Pokud chcete, aby tooreduce latence.
    Klíč oddílu|/userid|Klíč oddílu, který provede distribuci dat rovnoměrně tooeach oddílu. Výběr hello správný klíč oddílu je důležité při vytváření původce graf, další informace o jeho [návrh a vytváření oddílů](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

3. Jakmile vyplňování formuláře hello, klikněte na možnost **OK**.
