Teď můžete použít nástroj Průzkumník dat hello v hello Azure portálu toocreate databáze a kolekce. 

1. V hello portál Azure, v nabídce hello navigaci vlevo, klikněte na **Data Explorer (Preview)**. 

2. Na hello **Data Explorer (Preview)** okně klikněte na tlačítko **nové kolekce**a pak zadejte hello následující informace:

    ![Hello okno Průzkumníku dat Azure portálu](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID databáze|Úlohy|Hello název pro novou databázi. Názvy databází musí mít délku 1 až 255 znaků a nesmí obsahovat znaky /, \\, #, ? ani koncové mezery.
    ID kolekce|Items|Hello název nové kolekce. Kolekce názvy mají hello stejné požadavky jako databáze ID znak.
    Kapacita úložiště| Pevná (10 GB)|Použijte výchozí hodnotu hello. Tato hodnota je kapacita úložiště hello hello databáze.
    Propustnost|400 RU|Použijte výchozí hodnotu hello. Pokud chcete tooreduce latence, můžete postupně škálovat propustnost hello později.
    Klíč oddílu|/kategorie|Klíč oddílu, která distribuuje data rovnoměrně tooeach oddílu. Klíč výběru správné oddílu hello je důležité při vytváření kolekce původce. Další, najdete v části toolearn [návrh a vytváření oddílů](../articles/cosmos-db/partition-data.md#designing-for-partitioning).    
3. Po dokončení hello formuláře, klikněte na tlačítko **OK**.

Zobrazuje Průzkumníka data hello nové databáze a kolekce. 
