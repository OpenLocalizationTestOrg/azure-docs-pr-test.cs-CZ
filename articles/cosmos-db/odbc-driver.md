---
title: "aaaConnect tooAzure Cosmos databáze pomocí nástrojů BI analytics | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure Cosmos DB ODBC ovladač toocreate tabulek a zobrazení, tak, aby normalizovaný data lze zobrazit v softwaru analytics BI a data."
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
ms.openlocfilehash: e12a70f7805445f09fac01411e4bfbccc9a29c7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooazure-cosmos-db-using-bi-analytics-tools-with-hello-odbc-driver"></a>Připojit tooAzure Cosmos databáze pomocí nástrojů BI analýzy pomocí ovladače ODBC hello

Umožňuje ovladač Azure Cosmos DB ODBC Hello tooconnect tooAzure Cosmos DB pomocí BI analytics nástroje například SQL Server Integration Services, Power BI Desktop a Tableau, aby mohli analyzovat a vytvořit vizualizacemi vašich dat. Azure Cosmos DB v těch řešení.

ovladač Azure Cosmos DB ODBC Hello je ODBC 3.8 kompatibilní a podporuje syntaxi ANSI SQL-92. Hello ovladač nabízí bohaté funkce toohelp renormalize data v Azure Cosmos DB. Pomocí hello ovladače, může představovat data v Azure Cosmos DB jako tabulky a zobrazení. ovladač Hello umožňuje operace SQL tooperform proti hello tabulek a zobrazení, včetně Seskupit podle dotazy, vložení, aktualizace a odstranění.

## <a name="why-do-i-need-toonormalize-my-data"></a>Proč musím toonormalize svá data?
Azure Cosmos DB je schemaless databáze, tak umožňuje rychlý vývoj aplikací povolením aplikace tooiterate svá data modelu na hello chodu a není je omezit tooa striktní schématu. Izolované databáze Azure Cosmos DB může obsahovat dokumenty JSON různé struktur. Tato služba je skvělá pro rychlý vývoj aplikací, ale když chcete tooanalyze a vytváření sestav dat pomocí analýzy dat a nástroje BI, často potřebuje toobe průmětu hello dat a splňovat určité schéma tooa.

Toto je, kde je ovladač ODBC hello rozdělena na. Pomocí ovladače ODBC hello je můžete nyní renormalized data v Azure Cosmos DB do tabulek a zobrazení hodí tooyour dat analytické a vytváření sestav musí. schémata Hello renormalized nemají žádný vliv na hello základní data a není omezit jen vývojáři tooadhere toothem, jednoduše umožňují vám tooleverage kompatibilní s rozhraním ODBC dat nástrojů pro tooaccess hello. Takže teď vaše databáze Azure Cosmos DB pouze nebude oblíbená položka pro váš vývojový tým, ale bude datoví analytici skvělé příliš.

Teď umožňuje Začínáme pomocí ovladače ODBC hello.

## <a id="install"></a>Krok 1: Instalace ovladače Azure Cosmos DB ODBC hello

1. Stáhněte si hello ovladače pro vaše prostředí:

    * [Microsoft Azure Cosmos DB ODBC 64-bit.msi](https://aka.ms/documentdb-odbc-64x64) pro 64bitový systém Windows
    * [Microsoft Azure Cosmos DB ODBC 32 x 64-bit.msi](https://aka.ms/documentdb-odbc-32x64) pro 32bitovou verzi na 64bitovém systému Windows
    * [Microsoft Azure Cosmos DB ODBC 32-bit.msi](https://aka.ms/documentdb-odbc-32x32) pro 32bitový systém Windows

    Soubor msi spuštění hello místně, které spustí hello **Průvodce instalací ovladačů DB Microsoft Azure Cosmos ODBC**. 
2. Dokončení Průvodce instalací hello pomocí ovladače ODBC hello výchozí vstupní tooinstall hello.
3. Otevřete hello **správce zdrojů dat ODBC** aplikace v počítači, můžete to provedete zadáním **zdroje dat ODBC** v systému Windows hello vyhledávacího pole. 
    Si můžete ověřit kliknutím hello byl nainstalován ovladač hello **ovladače** kartě a zajištění **ovladače ODBC Microsoft Azure Cosmos DB** je uveden.

    ![Azure Cosmos DB správce zdrojů dat ODBC](./media/odbc-driver/odbc-driver.png)

## <a id="connect"></a>Krok 2: Připojení databáze Azure Cosmos DB tooyour

1. Po [ovladač Azure Cosmos DB ODBC hello instalace](#install), v hello **správce zdrojů dat ODBC** okně klikněte na tlačítko **přidat**. Můžete vytvořit uživatele nebo název DSN systému. V tomto příkladu vytváříme uživatelské DSN.
2. V hello **vytvořit nový zdroj dat** vyberte **ovladače ODBC Microsoft Azure Cosmos DB**a potom klikněte na **Dokončit**.
3. V hello **Azure Cosmos DB ODBC ovladač SDN instalace** vyplňte následující hello: 

    ![Azure okno nastavení DSN ovladač ODBC pro Cosmos DB](./media/odbc-driver/odbc-driver-dsn-setup.png)
    - **Název zdroje dat**: vlastní popisný název hello název DSN rozhraní ODBC. Tento název je jedinečný tooyour Azure Cosmos DB účet, takže název správně Pokud máte více účtů.
    - **Popis**: stručný popis zdroje dat hello.
    - **Hostitele**: identifikátor URI pro váš účet Azure Cosmos DB. Můžete načíst to z okna klíče DB Cosmos Azure hello v hello portál Azure, jak ukazuje následující snímek obrazovky hello. 
    - **Přístup ke klíči**: hello primární nebo sekundární, čtení a zápis nebo jen pro čtení klíče z okna klíče DB Cosmos Azure hello v hello portálu Azure, jak ukazuje následující snímek obrazovky hello. Doporučujeme, že abyste použili hello klíč jen pro čtení, pokud hello název zdroje dat se používá pro zpracování dat jen pro čtení a vytváření sestav.
    ![Azure okna klíče DB Cosmos](./media/odbc-driver/odbc-driver-keys.png)
    - **Přístupový klíč pro šifrování**: vybrat nejlepší volbou hello založená na uživatelích hello tohoto počítače. 
4. Klikněte na tlačítko hello **Test** toomake tlačítko, jestli účet Azure Cosmos DB tooyour se můžete připojit. 
5. Klikněte na tlačítko **pokročilé možnosti** a sadu hello následující hodnoty:
    - **Dotaz konzistence**: Vyberte hello [úroveň konzistence](consistency-levels.md) pro operace. Výchozí hodnota Hello je relace.
    - **Počet opakovaných pokusů**: Zadejte počet hello tooretry operace Pokud hello počáteční požadavek z důvodu omezení tooservice nedokončí.
    - **Soubor se schématem**: Zde máte několik možností.
        - Ve výchozím nastavení tato položka, jako je (prázdný), a kontroluje ovladače hello hello první stránka data pro všechny kolekce toodetermine hello schématu jednotlivých kolekcí. To se označuje jako kolekce mapování. Bez soubor schéma definované hello ovladačů má tooperform hello kontroly pro každou relaci ovladačů a může mít za následek vyšší spuštění provoz aplikace pomocí hello DSN. Doporučujeme vždy přiřadit soubor schématu pro zdroje dat DSN.
        - Pokud již máte soubor schématu (může být ten, který jste vytvořili pomocí hello [schématu Editor](#schema-editor)), můžete kliknout na **Procházet**přejděte tooyour souboru, klikněte na tlačítko **Uložit**a potom klikněte na **OK**.
        - Pokud chcete, aby toocreate nové schéma, klikněte na tlačítko **OK**a potom klikněte na **schématu Editor** v hlavním okně hello. Poté pokračujte toohello [schématu Editor](#schema-editor) informace. Při vytváření nového souboru schématu hello, nezapomeňte prosím toogo back toohello **pokročilé možnosti** okno tooinclude hello nově vytvořený soubor se schématem.

6. Po dokončení a zavření hello **Azure Cosmos DB ODBC ovladač DSN instalace** okně hello je nové uživatelské DSN přidat toohello karty Uživatelské DSN.

    ![Nové Azure Cosmos DB název DSN rozhraní ODBC na kartě Uživatelské DSN hello](./media/odbc-driver/odbc-driver-user-dsn.png)

## <a id="#collection-mapping"></a>Krok 3: Vytvoření definice schématu pomocí hello kolekce mapování metody

Existují dva typy metod vzorkování, které můžete použít: **kolekce mapování** nebo **tabulky oddělovače**. Relaci vzorkování můžete využít obě metody vzorkování, ale každá kolekce metodu lze použít pouze konkrétní vzorkování. Následující postup Hello vytvořit schéma pro hello data v jedné nebo více kolekcí pomocí hello kolekce mapování metody. Tato metoda vzorkování načte data hello hello stránce strukturou kolekce toodetermine hello dat hello. Ho transponuje tabulku tooa kolekce na hello straně ODBC. Tato metoda vzorkování je rychlé a efektivní, když bylo zajištěno homogenní hello data v kolekci. Pokud kolekce obsahuje heterogenous typu dat, doporučujeme, abyste použili hello [tabulky oddělovače mapování metoda](#table-mapping) jako poskytuje robustnější metody vzorkování toodetermine hello datové struktury v kolekci hello. 

1. Po dokončení kroků 1 – 4 v [databáze Azure Cosmos DB tooyour připojit](#connect), klikněte na tlačítko **schématu Editor** v hello **Azure Cosmos DB ODBC ovladač DSN instalace** okno.

    ![Schéma editor tlačítka v okně Azure Cosmos DB ODBC ovladač DSN nastavení hello](./media/odbc-driver/odbc-driver-schema-editor.png)
2. V hello **schématu Editor** okně klikněte na tlačítko **vytvořit nový**.
    Hello **Generovat schéma** okně se zobrazí všechny kolekce hello v hello účet Azure Cosmos DB. 
3. Vyberte jeden nebo více kolekcí toosample a pak klikněte na tlačítko **ukázka**. 
4. V hello **zobrazení návrhu** jsou reprezentované kartě, hello databáze, schéma a tabulku. V zobrazení tabulky hello hello kontroly zobrazí hello sadu vlastnosti související s názvy sloupců hello (název serveru SQL, název zdroje atd.).
    Pro každý sloupec můžete upravit název SQL hello sloupce, typ SQL hello, délka SQL (pokud existuje), škálování (pokud existuje), přesnost (pokud existuje) a s možnou hodnotou Null.
    - Můžete nastavit **skrýt sloupce** příliš**true** Pokud chcete tooexclude sloupce z výsledků dotazu. Sloupce označena skrýt sloupce = true nebudou zobrazeny pro výběr a projekce, i když jsou stále součástí hello schématu. Například můžete skrýt všechny vlastnosti systém požadovaný Azure Cosmos DB hello začíná "_".
    - Hello **id** sloupec je hello pouze pole, které nemohou být skrytá. protože je používán jako primární klíč hello ve schématu normalizovaný hello. 
5. Po dokončení definování schématu hello klikněte na tlačítko **soubor** | **Uložit**, přejděte toohello directory toosave hello schématu a pak klikněte na tlačítko **Uložit**.

    Pokud v budoucnu hello chcete toouse toto schéma pomocí názvu DSN, otevřete okno Azure Cosmos DB ODBC ovladač DSN nastavení hello (prostřednictvím hello správce zdrojů dat rozhraní ODBC), klikněte na tlačítko Rozšířené možnosti a potom hello pole souboru schématu, přejděte toohello uložit schématu. Ukládání tooan souboru schématu upravuje existující DSN hello DSN připojení tooscope toohello dat a struktury definované schéma.

## <a id="table-mapping"></a>Krok 4: Vytvoření definice schématu pomocí hello tabulky oddělovače mapování – metoda

Existují dva typy metod vzorkování, které můžete použít: **kolekce mapování** nebo **tabulky oddělovače**. Relaci vzorkování můžete využít obě metody vzorkování, ale každá kolekce metodu lze použít pouze konkrétní vzorkování. 

Hello následujících kroků vytvořte schéma pro hello data v jedné nebo více kolekcí pomocí hello **tabulky oddělovače** mapování metoda. Doporučujeme použít tuto metodu vzorkování, pokud kolekce obsahují heterogenní typ dat. Můžete použít tento hello tooscope metoda vzorkování tooa sadu atributů a jeho odpovídající hodnoty. Například pokud dokument obsahuje vlastnost "Druh", můžete určit obor hello vzorkování toohello hodnoty této vlastnosti. Konečný výsledek Hello hello vzorkování by sadu tabulek pro každou hello hodnot pro typ jste zadali. Zadejte například = Auto vytvoří tabulku Car při typ = roviny byste mohli vytvořit tabulku roviny.

1. Po dokončení kroků 1 – 4 v [databáze Azure Cosmos DB tooyour připojit](#connect), klikněte na tlačítko **schématu Editor** v okně Azure Cosmos DB ODBC ovladač DSN nastavení hello.
2. V hello **schématu Editor** okně klikněte na tlačítko **vytvořit nový**.
    Hello **Generovat schéma** okně se zobrazí všechny kolekce hello v hello účet Azure Cosmos DB. 
3. Vyberte kolekci, která na hello **ukázkové zobrazení** na kartě hello **mapování definice** sloupec pro hello kolekce, klikněte na tlačítko **upravit**. Potom v hello **mapování definice** vyberte **tabulky oddělovače** metoda. Potom hello následující:

    a. V hello **atributy** pole, název typu hello vlastnosti oddělovač. Toto je vlastnost dokumentu, který chcete tooscope hello vzorkování, například města a stiskněte klávesu enter. 

    b. Pokud chcete pouze tooscope hello vzorkování toocertain hodnot pro atribut hello jste právě zadali, vyberte atribut hello v hello výběr a potom zadejte hodnotu v hello **hodnotu** pole, například Praha a stiskněte klávesu enter. Můžete dál tooadd více hodnot atributů. Právě Ujistěte se, že hello správný, že atribut je vybrána, když zadáváte hodnoty.

    Například, pokud zahrnete **atributy** hodnotu města a chcete toolimit vaše tabulky tooonly zahrne řádky s hodnotou města Brno a Dubaj, zadejte města hello atributy pole a New Yorku, a potom Dubaj do hello  **Hodnoty** pole.
4. Klikněte na **OK**. 
5. Po dokončení definice hello mapování pro kolekce hello chcete toosample v hello **schématu Editor** okně klikněte na tlačítko **ukázka**.
     Pro každý sloupec můžete upravit název SQL hello sloupce, typ SQL hello, délka SQL (pokud existuje), škálování (pokud existuje), přesnost (pokud existuje) a s možnou hodnotou Null.
    - Můžete nastavit **skrýt sloupce** příliš**true** Pokud chcete tooexclude sloupce z výsledků dotazu. Sloupce označena skrýt sloupce = true nebudou zobrazeny pro výběr a projekce, i když jsou stále součástí hello schématu. Například můžete skrýt všechny hello Azure Cosmos DB systému požadované vlastnosti počínaje "_".
    - Hello **id** sloupec je hello pouze pole, které nemohou být skrytá. protože je používán jako primární klíč hello ve schématu normalizovaný hello. 
6. Po dokončení definování schématu hello klikněte na tlačítko **soubor** | **Uložit**, přejděte toohello directory toosave hello schématu a pak klikněte na tlačítko **Uložit**.
7. Zpět v hello **Azure Cosmos DB ODBC ovladač DSN instalace** okně klikněte na tlačítko ** Rozšířené možnosti **. Potom v hello **soubor schématu** pole, přejděte toohello uložit soubor schématu a klikněte na tlačítko **OK**. Klikněte na tlačítko **OK** znovu toosave hello DSN. Toto schéma hello uloží jste vytvořili toohello DSN. 

## <a name="optional-creating-views"></a>(Volitelné) Vytváření zobrazení
Můžete definovat a vytvářet zobrazení v rámci procesu hello vzorkování. Tato zobrazení jsou ekvivalentní tooSQL zobrazení. Jsou jen pro čtení a výběry hello oboru a projekce hello definuje Azure Cosmos DB SQL. 

toocreate zobrazení pro vaše data v hello **schématu Editor** okno, v hello **definice zobrazení** sloupce, klikněte na tlačítko **přidat** na řádek hello toosample kolekce hello. Potom v hello **definice zobrazení** okně hello následující:
1. Klikněte na tlačítko **nový**, zadejte název zobrazení hello, například EmployeesfromSeattleView a pak klikněte na tlačítko **OK**.
2. V hello **upravit zobrazení** okno, zadejte dotaz služby Azure Cosmos DB. Dotaz SQL Azure Cosmos databáze musí být například`SELECT c.City, c.EmployeeName, c.Level, c.Age, c.Gender, c.Manager FROM c WHERE c.City = “Seattle”`a potom klikněte na **OK**.

Jak se vám líbí, můžete vytvořit mnoho zobrazení. Po dokončení definující hello zobrazení můžete poté ukázkové hello data. 

## <a name="step-5-view-your-data-in-bi-tools-such-as-power-bi-desktop"></a>Krok 5: Zobrazení dat v nástrojích BI, například Power BI Desktop

Můžete použít nové DSN tooconnect DocumentADB s žádné kompatibilní s rozhraním ODBC nástroje – tento krok jednoduše ukazuje, jak tooconnect tooPower BI Desktop a vytvářet vizualizace Power BI.

1. Otevřít Power BI Desktop.
2. Klikněte na tlačítko **získat Data**.
3. V hello **načíst Data** okně klikněte na tlačítko **jiných** | **ODBC** | **Connect**.
4. V hello **z rozhraní ODBC** období, název zdroje dat vyberte hello jste vytvořili a pak klikněte na tlačítko **OK**. Můžete ponechat hello **pokročilé možnosti** položky, které jsou prázdné.
5. V hello **přístup k datovému zdroji prostřednictvím ovladače ODBC** vyberte **výchozí nebo vlastní** a pak klikněte na **Connect**. Není nutné tooinclude hello **pověření vlastnosti připojovacího řetězce**.
6. V hello **Navigátor** rozbalte okno, v levém podokně hello hello databáze, hello schéma a tabulku vyberte hello. podokno výsledků Hello zahrnuje hello data pomocí hello schématu, které jste vytvořili.
7. toovisualize hello data v Power BI desktop políčko hello před hello název tabulky a pak klikněte na tlačítko **zatížení**.
8. V Power BI Desktop hello zcela vlevo, vyberte na kartě Data hello ![Karta data v Power BI Desktop](./media/odbc-driver/odbc-driver-data-tab.png) tooconfirm data byla importována.
9. Nyní můžete vytvořit vizuální prvky používající Power BI kliknutím na kartu sestavy hello ![kartu sestavy v Power BI Desktop](./media/odbc-driver/odbc-driver-report-tab.png), kliknete na příkaz **nové Visual**a pak přizpůsobení dlaždice. Další informace o vytváření vizualizací v Power BI Desktop najdete v tématu [typů vizualizace v Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-visualization-types-for-reports-and-q-and-a/).

## <a name="troubleshooting"></a>Řešení potíží

Pokud se zobrazí následující chyba hello, ujistěte se, hello **hostitele** a **přístupový klíč** hodnoty, které jste zkopírovali hello portál Azure [kroku 2](#connect) jsou správné a opakujte. Použít hello kopírování tlačítek toohello napravo od hello **hostitele** a **přístupový klíč** hodnoty v hello bez chyb Azure portálu toocopy hello hodnoty.

    [HY000]: [Microsoft][Azure Cosmos DB] (401) HTTP 401 Authentication Error: {"code":"Unauthorized","message":"hello input authorization token can't serve hello request. Please check that hello expected payload is built as per hello protocol, and check hello key being used. Server used hello following payload toosign: 'get\ndbs\n\nfri, 20 jan 2017 03:43:55 gmt\n\n'\r\nActivityId: 9acb3c0d-cb31-4b78-ac0a-413c8d33e373"}`

## <a name="next-steps"></a>Další kroky

toolearn Další informace o databázi Cosmos Azure, najdete v části [co je Azure Cosmos DB?](introduction.md).
