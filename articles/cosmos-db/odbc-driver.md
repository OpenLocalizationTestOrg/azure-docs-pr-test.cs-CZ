---
title: "Připojte se k databázi Cosmos Azure pomocí nástrojů BI analytics | Microsoft Docs"
description: "Naučte se používat ovladač Azure Cosmos DB ODBC k vytvoření tabulky a zobrazení tak, aby normalizovaný data lze zobrazit v softwaru analytics BI a data."
keywords: "rozhraní ODBC, ovladače odbc"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 9967f4e5-4b71-4cd7-8324-221a8c789e6b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: rest-api
ms.topic: article
ms.date: 05/24/2017
ms.author: mimig
ms.openlocfilehash: 2df792c00b7a789dbefa64bfe0245f1ad73c3faa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-azure-cosmos-db-using-bi-analytics-tools-with-the-odbc-driver"></a>Připojte se k databázi Cosmos Azure pomocí nástrojů BI analýzy pomocí ovladače ODBC

Ovladač Azure Cosmos DB ODBC umožňuje připojení k databázi Cosmos Azure pomocí nástroje Analýza BI například Tableau, SQL Server Integration Services a Power BI Desktop, aby mohli analyzovat a vytvořit vizualizacemi vašich dat. Azure Cosmos DB v těchto řešeních.

Ovladač Azure Cosmos DB ODBC je kompatibilní s ODBC 3.8 a podporuje syntaxi ANSI SQL-92. Ovladač nabízí bohaté funkce můžete renormalize data v Azure Cosmos DB. Použití ovladače, může představovat data v Azure Cosmos DB jako tabulky a zobrazení. Ovladač umožňuje provádět operace SQL u tabulky a zobrazení, včetně skupiny dotazy, vložení, aktualizace a odstraní.

## <a name="why-do-i-need-to-normalize-my-data"></a>Proč musím normalizaci svá data?
Azure Cosmos DB je schemaless databáze, takže umožňuje rychlý vývoj aplikací povolením aplikace k iteraci jejich datového modelu za chodu a není je omezit striktní schématu. Izolované databáze Azure Cosmos DB může obsahovat dokumenty JSON různé struktur. Tato služba je skvělá pro rychlý vývoj aplikací, ale když chcete k analýze a vytváření sestav dat pomocí analýzy dat a nástroje BI, často potřebuje data vyrovnány a splňovat určité schéma.

Toto je, kde odeslán ovladač ODBC. Pomocí ovladače ODBC, můžete nyní renormalized data v Azure Cosmos DB do tabulek a zobrazení hodí k vašim datovým potřebám analýz a generování sestav. Renormalized schémata nemají žádný vliv na základní data a není omezit jen vývojáři řídit k nim, jednoduše umožňují vám využít kompatibilní s rozhraním ODBC nástroje pro přístup k datům. Takže teď vaše databáze Azure Cosmos DB pouze nebude oblíbená položka pro váš vývojový tým, ale bude datoví analytici skvělé příliš.

Teď umožňuje Začínáme pomocí ovladače ODBC.

## <a id="install"></a>Krok 1: Instalace ovladač Azure Cosmos DB ODBC

1. Stáhněte si ovladače pro vaše prostředí:

    * [Microsoft Azure Cosmos DB ODBC 64-bit.msi](https://aka.ms/documentdb-odbc-64x64) pro 64bitový systém Windows
    * [Microsoft Azure Cosmos DB ODBC 32 x 64-bit.msi](https://aka.ms/documentdb-odbc-32x64) pro 32bitovou verzi na 64bitovém systému Windows
    * [Microsoft Azure Cosmos DB ODBC 32-bit.msi](https://aka.ms/documentdb-odbc-32x32) pro 32bitový systém Windows

    Spusťte soubor msi místně, která se spouští **Průvodce instalací ovladačů DB Microsoft Azure Cosmos ODBC**. 
2. Dokončení Průvodce instalací pomocí výchozího vstup pro instalaci ovladače ODBC.
3. Otevřete **správce zdrojů dat ODBC** aplikace v počítači, můžete to provedete zadáním **zdroje dat ODBC** v systému Windows vyhledávacího pole. 
    Můžete potvrdit, ovladač byl nainstalován kliknutím **ovladače** kartě a zajištění **ovladače ODBC Microsoft Azure Cosmos DB** je uveden.

    ![Azure Cosmos DB správce zdrojů dat ODBC](./media/odbc-driver/odbc-driver.png)

## <a id="connect"></a>Krok 2: Připojení k databázi Azure Cosmos DB

1. Po [instalaci ovladače Azure Cosmos DB ODBC](#install)v **správce zdrojů dat ODBC** okně klikněte na tlačítko **přidat**. Můžete vytvořit uživatele nebo název DSN systému. V tomto příkladu vytváříme uživatelské DSN.
2. V **vytvořit nový zdroj dat** vyberte **ovladače ODBC Microsoft Azure Cosmos DB**a potom klikněte na **Dokončit**.
3. V **Azure Cosmos DB ODBC ovladač SDN instalace** vyplňte následující: 

    ![Azure okno nastavení DSN ovladač ODBC pro Cosmos DB](./media/odbc-driver/odbc-driver-dsn-setup.png)
    - **Název zdroje dat**: vlastní popisný název DSN rozhraní ODBC. Tento název je jedinečný pro váš účet Azure Cosmos DB, takže název správně Pokud máte více účtů.
    - **Popis**: stručný popis zdroje dat.
    - **Hostitele**: identifikátor URI pro váš účet Azure Cosmos DB. Můžete načíst to z okna Azure Cosmos DB klíče na portálu Azure, jak je znázorněno na následujícím snímku obrazovky. 
    - **Přístup ke klíči**: primární nebo sekundární, čtení a zápis nebo jen pro čtení klíče z okna Azure Cosmos DB klíče na portálu Azure, jak je znázorněno na následujícím snímku obrazovky. Doporučujeme, že abyste použili klíč jen pro čtení, pokud název zdroje dat se používá pro zpracování dat jen pro čtení a vytváření sestav.
    ![Azure okna klíče DB Cosmos](./media/odbc-driver/odbc-driver-keys.png)
    - **Přístupový klíč pro šifrování**: vybrat nejlepší volbou založená na uživatelích tohoto počítače. 
4. Klikněte **Test** tlačítko a ujistěte se, zda se můžete připojit ke svému účtu Azure Cosmos DB. 
5. Klikněte na tlačítko **pokročilé možnosti** a nastavte následující hodnoty:
    - **Dotaz konzistence**: vyberte [úroveň konzistence](consistency-levels.md) pro operace. Výchozí hodnota je relace.
    - **Počet opakovaných pokusů**: Zadejte počet pokusů o opakování operace, pokud úvodního požadavku není dokončena z důvodu omezení služby.
    - **Soubor se schématem**: Zde máte několik možností.
        - Ve výchozím nastavení tato položka, jako je (prázdný), a kontroluje ovladače data na první stránce pro všechny kolekce k určení schéma každou kolekci. To se označuje jako kolekce mapování. Bez soubor schéma definované ovladače se má provést kontrola pro každou relaci ovladačů a může mít za následek vyšší spuštění provoz aplikace pomocí DSN. Doporučujeme vždy přiřadit soubor schématu pro zdroje dat DSN.
        - Pokud již máte soubor schématu (může být ten, který jste vytvořili pomocí [schématu Editor](#schema-editor)), můžete kliknout na **Procházet**, přejděte k souboru, klikněte na **Uložit**a potom klikněte na **OK**.
        - Pokud chcete vytvořit nové schéma, klikněte na tlačítko **OK**a potom klikněte na **schématu Editor** v hlavním okně. Pak pokračujte [schématu Editor](#schema-editor) informace. Při vytváření nového souboru schématu, nezapomeňte prosím vraťte se **pokročilé možnosti** okno zahrnout schématu nově vytvořený soubor.

6. Po dokončení a zavření **Azure Cosmos DB ODBC ovladač DSN instalace** okně Nový uživatelský název DSN se přidá do karty Uživatelské DSN.

    ![Nové Azure Cosmos DB název DSN rozhraní ODBC na kartě Uživatelské DSN](./media/odbc-driver/odbc-driver-user-dsn.png)

## <a id="#collection-mapping"></a>Krok 3: Vytvoření definice schématu pomocí metody kolekce mapování

Existují dva typy metod vzorkování, které můžete použít: **kolekce mapování** nebo **tabulky oddělovače**. Relaci vzorkování můžete využít obě metody vzorkování, ale každá kolekce metodu lze použít pouze konkrétní vzorkování. Následující postup vytvořit schéma pro data v jedné nebo více kolekcí pomocí metody kolekce mapování. Tato metoda vzorkování načte data na stránce určit strukturu dat kolekce. Ho transponuje kolekci do tabulky na straně ODBC. Tato metoda vzorkování je rychlé a efektivní, když jsou data v kolekci homogenního. Pokud kolekce obsahuje heterogenous typu dat, doporučujeme použít [tabulky oddělovače mapování metoda](#table-mapping) jako poskytuje robustnější metody vzorkování určit datové struktury v kolekci. 

1. Po dokončení kroků 1 – 4 v [připojit k databázi Azure Cosmos DB](#connect), klikněte na tlačítko **schématu Editor** v **Azure Cosmos DB ODBC ovladač DSN instalace** okno.

    ![Schéma editor tlačítka v okně Azure Cosmos DB ODBC ovladač DSN nastavení](./media/odbc-driver/odbc-driver-schema-editor.png)
2. V **schématu Editor** okně klikněte na tlačítko **vytvořit nový**.
    **Generovat schéma** okno zobrazí všechny kolekce v účtu Azure Cosmos DB. 
3. Vyberte jeden nebo více kolekcí pro ukázkové a pak klikněte na tlačítko **ukázka**. 
4. V **zobrazení návrhu** jsou reprezentované kartě, databáze, schéma a tabulku. V zobrazení tabulky zobrazí kontroly sadu vlastnosti související s názvy sloupců (název serveru SQL, název zdroje atd.).
    Pro každý sloupec můžete upravit název sloupce SQL, typ SQL, délka SQL (pokud existuje), škálování (pokud existuje), přesnost (pokud existuje) a s možnou hodnotou Null.
    - Můžete nastavit **skrýt sloupce** k **true** Pokud chcete vyloučit sloupce z výsledků dotazu. Sloupce označena skrýt sloupce = true nebudou zobrazeny pro výběr a projekce, i když jsou stále součástí schématu. Například můžete skrýt všechny vlastnosti systém požadovaný Azure Cosmos DB počínaje "_".
    - **Id** sloupec je pouze pole, které nemohou být skrytá. protože je používán jako primární klíč v normalizovaný schématu. 
5. Po dokončení definování schématu, klikněte na tlačítko **soubor** | **Uložit**, přejděte do adresáře uložte schématu a pak klikněte na tlačítko **Uložit**.

    Pokud chcete v budoucnu toto schéma používají, pomocí názvu DSN, otevřete okno Azure Cosmos DB ODBC ovladač DSN nastavení (pomocí Správce zdrojů dat ODBC), klikněte na tlačítko Upřesnit možnosti a pak v dialogovém okně soubor schématu přejděte na schéma uložené. Ukládání souboru schématu do existující DSN upravuje DSN připojení k oboru k datům a struktury definované schéma.

## <a id="table-mapping"></a>Krok 4: Vytvoření definice schématu pomocí tabulky oddělovače mapování – metoda

Existují dva typy metod vzorkování, které můžete použít: **kolekce mapování** nebo **tabulky oddělovače**. Relaci vzorkování můžete využít obě metody vzorkování, ale každá kolekce metodu lze použít pouze konkrétní vzorkování. 

Následujících kroků vytvořte schéma pro data v jedné nebo více kolekcí pomocí **tabulky oddělovače** mapování metoda. Doporučujeme použít tuto metodu vzorkování, pokud kolekce obsahují heterogenní typ dat. Tuto metodu můžete použít k určení rozsahu vzorkování na sadu atributů a jeho odpovídající hodnoty. Například pokud dokument obsahuje vlastnost "Druh", můžete určit obor vzorkuje hodnoty této vlastnosti. Konečný výsledek vzorkuje by sadu tabulek pro každou z hodnot pro typ jste zadali. Zadejte například = Auto vytvoří tabulku Car při typ = roviny byste mohli vytvořit tabulku roviny.

1. Po dokončení kroků 1 – 4 v [připojit k databázi Azure Cosmos DB](#connect), klikněte na tlačítko **schématu Editor** v okně Azure Cosmos DB ODBC ovladač DSN nastavení.
2. V **schématu Editor** okně klikněte na tlačítko **vytvořit nový**.
    **Generovat schéma** okno zobrazí všechny kolekce v účtu Azure Cosmos DB. 
3. Vyberte kolekci, která na **ukázkové zobrazení** ve **mapování definice** sloupec pro kolekce, klikněte na tlačítko **upravit**. Potom v **mapování definice** vyberte **tabulky oddělovače** metoda. Potom udělejte následující:

    a. V **atributy** zadejte název vlastnosti oddělovač. Toto je vlastnost dokumentu, který chcete určit obor vzorkování na, například města a stiskněte klávesu enter. 

    b. Pokud chcete určit obor vzorkování na určité hodnoty pro atribut, který jste právě zadali, vyberte atribut v výběr a potom zadejte hodnotu do **hodnotu** pole, například Praha a stiskněte klávesu enter. Můžete přidat více hodnot atributů. Právě zkontrolujte, že je vybraný správný atribut, když zadáváte hodnoty.

    Například, pokud zahrnete **atributy** hodnotu města a budete chtít omezit tabulka zahrnovat pouze řádky s hodnotou města Brno a Dubaj, zadejte město v atributy pole a New Yorku a pak Dubaj do **Hodnoty** pole.
4. Klikněte na **OK**. 
5. Po dokončení definice mapování pro kolekce, které chcete v ukázkové **schématu Editor** okně klikněte na tlačítko **ukázka**.
     Pro každý sloupec můžete upravit název sloupce SQL, typ SQL, délka SQL (pokud existuje), škálování (pokud existuje), přesnost (pokud existuje) a s možnou hodnotou Null.
    - Můžete nastavit **skrýt sloupce** k **true** Pokud chcete vyloučit sloupce z výsledků dotazu. Sloupce označena skrýt sloupce = true nebudou zobrazeny pro výběr a projekce, i když jsou stále součástí schématu. Například můžete skrýt všechny vlastnosti požadované systému Azure Cosmos DB počínaje "_".
    - **Id** sloupec je pouze pole, které nemohou být skrytá. protože je používán jako primární klíč v normalizovaný schématu. 
6. Po dokončení definování schématu, klikněte na tlačítko **soubor** | **Uložit**, přejděte do adresáře uložte schématu a pak klikněte na tlačítko **Uložit**.
7. Zpět v **Azure Cosmos DB ODBC ovladač DSN instalace** okně klikněte na tlačítko ** Rozšířené možnosti **. Pak na **soubor schématu** pole, přejděte na soubor uložený schéma a klikněte na tlačítko **OK**. Klikněte na tlačítko **OK** znovu a uložit DSN. To umožňuje ušetřit schéma, které jste vytvořili s DSN. 

## <a name="optional-creating-views"></a>(Volitelné) Vytváření zobrazení
Můžete definovat a vytvářet zobrazení v rámci procesu vzorkování. Tato zobrazení jsou ekvivalentní zobrazení SQL. Jsou jen pro čtení a oboru se výběr a definované projekce Azure Cosmos DB SQL. 

Vytvoření zobrazení pro vaše data v **schématu Editor** okno v **definice zobrazení** sloupce, klikněte na tlačítko **přidat** na řádek kolekce ukázce. Potom v **definice zobrazení** okno, postupujte takto:
1. Klikněte na tlačítko **nový**, zadejte název zobrazení, například EmployeesfromSeattleView a pak klikněte na tlačítko **OK**.
2. V **upravit zobrazení** okno, zadejte dotaz služby Azure Cosmos DB. Dotaz SQL Azure Cosmos databáze musí být například`SELECT c.City, c.EmployeeName, c.Level, c.Age, c.Gender, c.Manager FROM c WHERE c.City = “Seattle”`a potom klikněte na **OK**.

Jak se vám líbí, můžete vytvořit mnoho zobrazení. Po dokončení definování zobrazení, můžete pak Vzorkovat data. 

## <a name="step-5-view-your-data-in-bi-tools-such-as-power-bi-desktop"></a>Krok 5: Zobrazení dat v nástrojích BI, například Power BI Desktop

Nové DSN můžete použít pro připojení k žádné kompatibilní s rozhraním ODBC nástroje DocumentADB – tento krok jednoduše ukazuje, jak se připojit k Power BI Desktop a vytvořit vizualizaci Power BI.

1. Otevřít Power BI Desktop.
2. Klikněte na tlačítko **získat Data**.
3. V **načíst Data** okně klikněte na tlačítko **jiných** | **ODBC** | **Connect**.
4. V **z rozhraní ODBC** okno, vyberte zdroj dat jste vytvořili a potom klikněte na **OK**. Můžete nechat **pokročilé možnosti** položky, které jsou prázdné.
5. V **přístup k datovému zdroji prostřednictvím ovladače ODBC** vyberte **výchozí nebo vlastní** a pak klikněte na **Connect**. Nemusíte zahrnovat **pověření vlastnosti připojovacího řetězce**.
6. V **Navigátor** okno, v levém podokně rozbalte databázi schématu a pak vyberte v tabulce. V podokně Výsledky zahrnuje data pomocí schématu, kterou jste vytvořili.
7. K vizualizaci dat v Power BI desktop, zaškrtněte políčko před název tabulky a pak klikněte na tlačítko **zatížení**.
8. V Power BI Desktop vkládá zcela vlevo, vyberte kartu dat ![Karta data v Power BI Desktop](./media/odbc-driver/odbc-driver-data-tab.png) Potvrďte vaše data byla importována.
9. Nyní můžete vytvořit vizuální prvky používající Power BI kliknutím na kartu sestavy ![kartu sestavy v Power BI Desktop](./media/odbc-driver/odbc-driver-report-tab.png), kliknete na příkaz **nové Visual**a pak přizpůsobení dlaždice. Další informace o vytváření vizualizací v Power BI Desktop najdete v tématu [typů vizualizace v Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-visualization-types-for-reports-and-q-and-a/).

## <a name="troubleshooting"></a>Řešení potíží

Pokud se zobrazí následující chyba, zkontrolujte **hostitele** a **přístupový klíč** hodnoty, které jste zkopírovali na portálu Azure v [kroku 2](#connect) jsou správné a opakujte. Pomocí tlačítek kopírovat na pravé straně **hostitele** a **přístupový klíč** hodnoty v portálu Azure a Kopírovat bez hodnoty chyb.

    [HY000]: [Microsoft][Azure Cosmos DB] (401) HTTP 401 Authentication Error: {"code":"Unauthorized","message":"The input authorization token can't serve the request. Please check that the expected payload is built as per the protocol, and check the key being used. Server used the following payload to sign: 'get\ndbs\n\nfri, 20 jan 2017 03:43:55 gmt\n\n'\r\nActivityId: 9acb3c0d-cb31-4b78-ac0a-413c8d33e373"}`

## <a name="next-steps"></a>Další kroky

Další informace o databázi Cosmos Azure najdete v tématu [co je Azure Cosmos DB?](introduction.md).
