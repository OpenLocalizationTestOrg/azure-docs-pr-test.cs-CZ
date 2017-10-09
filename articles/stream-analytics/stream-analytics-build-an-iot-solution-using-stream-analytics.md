---
title: "aaaBuild řešení IoT pomocí služby Stream Analytics | Microsoft Docs"
description: "Úvodní kurz pro hello řešení Stream Analytics IoT mýtná celnice scénáře"
keywords: "řešení IOT, okno funkce"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: e37fc5b56c4ffc4a2d7b820afe0c17631e577ea0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Vytvoření řešení IoT pomocí služby Stream Analytics
## <a name="introduction"></a>Úvod
V tomto kurzu se dozvíte, jak toouse Azure Stream Analytics tooget v reálném čase přehled o vašich datech. Datové proudy dat, například klikněte na tlačítko-datové proudy, protokoly a události generované zařízení mohou vývojáři kombinovat snadno s staré záznamy nebo referenční data tooderive podnikových statistik. Jako výpočetní služba plně spravovaná, v reálném čase datový proud, který je hostován v Microsoft Azure Azure Stream Analytics poskytuje integrované odolnost proti chybám, s nízkou latencí a škálovatelnost tooget jste nahoru a spouštění v minutách.

Po dokončení tohoto kurzu, budete moci:

* Seznamte se s portál Azure Stream Analytics hello.
* Nakonfigurujte a nasaďte úlohu streamování.
* Vyjádřete reálného problémy a řešení je pomocí dotazovacího jazyka pro hello Stream Analytics.
* Vývoj streamování řešení pro vaše zákazníky pomocí služby Stream Analytics s jistotou.
* Použijte hello sledování a protokolování prostředí tootroubleshoot problémy.

## <a name="prerequisites"></a>Požadavky
Můžete se třeba hello následující toocomplete požadavky v tomto kurzu:

* nejnovější verze Hello [prostředí Azure PowerShell](/powershell/azure/overview)
* 2017 Visual Studio 2015, nebo hello volné [Visual Studio Community](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)
* [Předplatného Azure](https://azure.microsoft.com/pricing/free-trial/)
* Oprávnění správce na počítači hello
* Stažení [TollApp.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TollApp/TollApp.zip) z hello Microsoft Download Center
* Volitelné: Zdrojový kód pro hello TollApp událostí generátor v [Githubu](https://aka.ms/azure-stream-analytics-toll-source)

## <a name="scenario-introduction-hello-toll"></a>Scénář Úvod: "Hello, Projedou!"
Stanice projedou je běžné jevu. Narazíte je na mnoha rychlostních, mosty a tunely mezi hello, world. Každé stanici projedou má více kabin projedou. Při ruční kabin zastavíte toopay hello projedou tooan průvodce. Senzor nad každý stánek na automatizované kabin kontroluje RFID karta, která je čelního skla připojené toohello vaše vozidel jako předáte stánek projedou hello. Je snadno toovisualize hello průchod vozidel přes tyto stanice projedou jako datového proudu událostí za které zajímavé operace lze provádět.

![Obrázek automobilů na projedou kabin](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Příchozí data
V tomto kurzu pracuje se dvěma datových proudů. Snímače nainstalována v hello vstupu a ukončení hello projedou stanic vytvořit hello první datového proudu. druhý datový proud Hello je statický vyhledávání datovou sadu, která má vehicle registrační data.

### <a name="entry-data-stream"></a>Vstupní datový proud
Hello vstupní datový proud obsahuje informace o aut při vstupu projedou stanice.

| TollID | EntryTime | LicensePlate | Stav | Ujistěte se | Model | VehicleType | VehicleWeight | Projedou | Značka |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 |2014-09-10 12:01:00.000 |JNB 7001 |NY |Honda |CRV |1 |0 |7 | |
| 1 |2014-09-10 12:02:00.000 |YXZ 1001 |NY |Toyota |Camry |1 |0 |4 |123456789 |
| 3 |2014-09-10 12:02:00.000 |ABC 1004 |BERTE |Ford |Taurus |1 |0 |5 |456789123 |
| 2 |2014-09-10 12:03:00.000 |XYZ 1003 |BERTE |Toyota |Koruny |1 |0 |4 | |
| 1 |2014-09-10 12:03:00.000 |BNJ 1007 |NY |Honda |CRV |1 |0 |5 |789123456 |
| 2 |2014-09-10 12:05:00.000 |CDE 1007 |NI |Toyota |4 x 4 |1 |0 |6 |321987654 |

Zde je stručný popis hello sloupce:

| Sloupec | Popis |
| --- | --- |
| TollID |Hello projedou stánek ID, která jednoznačně identifikuje stánek projedou |
| EntryTime |Hello datum a čas položky hello vehicle toohello projedou stánek v UTC |
| LicensePlate |Hello registrační počet hello vehicle |
| Stav |Stav v USA |
| Ujistěte se |výrobce Hello automobilu hello |
| Model |číslo modelu Hello automobilu hello |
| VehicleType |Buď 1 pro osobní vozidel či 2 pro komerční vozidel |
| WeightType |Vehicle váhy v tunách; 0 pro osobní vozidel |
| Projedou |Hodnota projedou Hello v USD |
| Značka |Hello e-Tag na hello automobilu, který zautomatizuje platebních; prázdný, kde byla hello platebních provést ručně |

### <a name="exit-data-stream"></a>Výstupní datový proud
Hello výstupní datový proud obsahuje informace o aut ponechat hello projedou stanice.

| **TollId** | **ExitTime** | **LicensePlate** |
| --- | --- | --- |
| 1 |2014-09-10T12:03:00.0000000Z |JNB 7001 |
| 1 |2014-09-10T12:03:00.0000000Z |YXZ 1001 |
| 3 |2014-09-10T12:04:00.0000000Z |ABC 1004 |
| 2 |2014-09-10T12:07:00.0000000Z |XYZ 1003 |
| 1 |2014-09-10T12:08:00.0000000Z |BNJ 1007 |
| 2 |2014-09-10T12:07:00.0000000Z |CDE 1007 |

Zde je stručný popis hello sloupce:

| Sloupec | Popis |
| --- | --- |
| TollID |Hello projedou stánek ID, která jednoznačně identifikuje stánek projedou |
| ExitTime |Hello datum a čas ukončení hello vehicle z projedou stánek v UTC |
| LicensePlate |Hello registrační počet hello vehicle |

### <a name="commercial-vehicle-registration-data"></a>Vehicle komerční registrační data
kurz Hello používá statický snímek databáze komerční vehicle registrace.

| LicensePlate | RegistrationId | Platnost |
| --- | --- | --- |
| SVT 6023 |285429838 |1 |
| XLZ 3463 |362715656 |0 |
| ZÁ 1005 |876133137 |1 |
| RIV 8632 |992711956 |0 |
| SNY 7188 |592133890 |0 |
| ELH 9896 |678427724 |1 |

Zde je stručný popis hello sloupce:

| Sloupec | Popis |
| --- | --- |
| LicensePlate |Hello registrační počet hello vehicle |
| RegistrationId |ID registrace Hello vehicle |
| Platnost |Stav registrace hello vehicle Hello: 0, pokud je aktivní, vehicle registrace 1, pokud platnost registrace |

## <a name="set-up-hello-environment-for-azure-stream-analytics"></a>Nastavení hello prostředí pro Azure Stream Analytics
toocomplete tento kurz, musíte mít předplatné Microsoft Azure. Společnost Microsoft nabízí bezplatné zkušební verze pro služby Microsoft Azure.

Pokud účet Azure nemáte, můžete [požadavku bezplatná zkušební verze](http://azure.microsoft.com/pricing/free-trial/).

> [!NOTE]
> toosign pro bezplatnou zkušební verzi, musíte mobilní zařízení, které může přijímat textové zprávy a platná platební karta.
> 
> 

Ujistěte se, že toofollow hello kroky v části "Vyčištění účtu Azure" hello na konci hello tohoto článku tak, aby bylo možné vytvořit hello nejlepší využívání Azure kreditu.

## <a name="provision-azure-resources-required-for-hello-tutorial"></a>Zřídit prostředky Azure potřebné pro kurz hello
Tento kurz vyžaduje dva tooreceive centra událostí *položka* a *ukončete* datových proudů. Azure SQL Database výstupy hello výsledky úlohy Stream Analytics hello. Azure Storage ukládá referenční data o vehicle registrace.

Můžete použít hello Setup.ps1 skript ve složce TollApp hello na Githubu toocreate všechny požadované prostředky. V zájmu hello času doporučujeme ho spustíte. Pokud vás zajímají další informace o tom, jak tooconfigure těchto prostředků ve hello portál Azure, najdete v toolearn příloha toohello "Konfigurace kurz prostředků na portálu Azure".

Stáhněte a uložte hello podpora [TollApp](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TollApp/TollApp.zip) složky a soubory.

Otevřete **Microsoft Azure PowerShell** okno *jako správce*. Pokud ještě nemáte prostředí Azure PowerShell, postupujte podle pokynů hello v [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview) tooinstall ho.

Protože Windows automaticky blokuje soubory .exe, .dll a .ps1, je třeba zásady spouštění hello tooset před spuštěním skriptu hello. Zkontrolujte, že je spuštěný okno Azure PowerShell hello *jako správce*. Spustit **Set-ExecutionPolicy unrestricted**. Po zobrazení výzvy zadejte **Y**.

![Snímek obrazovky "Set-ExecutionPolicy unrestricted" spuštěné v okno Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

Spustit **Get-ExecutionPolicy** toomake jistotu, že příkaz hello fungovala.

![Snímek obrazovky "Get-ExecutionPolicy" spuštěné v okno Azure PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

Přejděte toohello adresář, který má hello skripty a generátor aplikace.

![Snímek obrazovky "cd .\TollApp\TollApp" spuštěné v okno Azure PowerShell hello](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

Typ **.\\ Setup.ps1** tooset si účet Azure, vytvořit a nakonfigurovat všechny požadované prostředky a spustit toogenerate události. až toocreate oblast Hello skriptu náhodně vybere vašich prostředků. tooexplicitly určit oblasti, můžete předat hello **-umístění** parametr jako hello následující ukázka:

**. \\Setup.ps1-umístění "Střed USA"**

![Snímek obrazovky stránky hello Azure přihlášení](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

Hello skript se otevře hello **přihlásit** stránky pro Microsoft Azure. Zadejte přihlašovací údaje účtu.

> [!NOTE]
> Pokud má váš účet přístup toomultiple odběry, bude název odběru hello kladené tooenter chcete toouse hello kurzu.
> 
> 

skript Hello může trvat několik minut toorun. Po dokončení hello výstup by měl vypadat jako následující snímek obrazovky hello.

![Snímek obrazovky výstupu skriptu hello v okně prostředí Azure PowerShell hello](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

Zobrazí se také další okno, které je podobné toohello následující snímek obrazovky. Tato aplikace odesílá události tooAzure Event Hubs, což je požadovaná toorun hello kurzu. Ano nezadávejte zastavení aplikace hello nebo toto okno zavřít, dokud nedokončíte hello kurzu.

![Snímek obrazovky "Odesílající event hub data"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

Můžete musí být schopný toosee vašich prostředků na portálu Azure nyní. Přejděte příliš<https://portal.azure.com>a přihlaste se pomocí přihlašovacích údajů účtu. Všimněte si, že aktuálně některé funkce využívá portálu classic hello. Tyto kroky bude jasně označen.

### <a name="azure-event-hubs"></a>Azure Event Hubs
V hello portálu Azure, klikněte na **další služby** na hello dolní části hello správy v levém podokně. Typ **služby Event hubs** v hello zadaná pole a klikněte na tlačítko **služby Event hubs**. Spustí se nové hello toodisplay okno prohlížeče **SERVICE BUS** oblasti v hello **portálu classic**. Zde se zobrazí hello Centrum událostí vytvořené hello Setup.ps1 skriptu.

![Service Bus](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image8.png)

Klikněte na tlačítko hello ten, který začíná *tolldata*. Klikněte na tlačítko hello **EVENT HUBS** kartě. Zobrazí se dva centra událostí s názvem *položka* a *ukončete* vytvořit v tomto oboru názvů.

![Karta centra událostí portálu classic hello](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image9.png)

### <a name="azure-storage-container"></a>Kontejner úložiště Azure
1. Vraťte se zpátky toohello karta prohlížeče otevřete tooAzure portál. Klikněte na tlačítko **úložiště** na levé straně hello Azure portálu toosee hello Azure Storage kontejneru, který se používá v kurzu hello hello.
   
    ![Položky nabídky úložiště](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image11.png)
2. Klikněte na tlačítko hello jeden, který bude začínat *tolldata*. Klikněte na tlačítko hello **kontejnery** kartě toosee hello vytvořit kontejner.
   
    ![Karta kontejnery v hello portálu Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image10.png)
3. Klikněte na tlačítko hello **tolldata** kontejneru toosee hello nahrát soubor JSON, který má vehicle registrační data.
   
    ![Snímek obrazovky aplikace hello registration.json souboru v kontejneru hello](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image12.png)

### <a name="azure-sql-database"></a>Azure SQL Database
1. Zpět na první kartě hello, kterou jste otevřeli v prohlížeči hello toohello portálu Azure. Klikněte na tlačítko **databází SQL** na levé straně hello Azure portálu toosee hello SQL database, která bude použita v kurzu hello a klikněte na tlačítko hello **tolldatadb**.
   
    ![Snímek obrazovky hello vytvoření databáze SQL](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)
2. Název serveru hello kopie bez hello číslo portu (*servername*. database.windows.net, např.).
    ![Snímek obrazovky hello vytvořit databázi databáze SQL](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15a.png)

## <a name="connect-toohello-database-from-visual-studio"></a>Připojit databáze toohello ze sady Visual Studio
Pomocí výsledků dotazu v sadě Visual Studio tooaccess v databázi výstup hello.

Připojte databáze SQL toohello (hello cíl) ze sady Visual Studio:

1. Otevřete Visual Studio a pak klikněte na tlačítko **nástroje** > **připojit tooDatabase**.
2. Pokud se zobrazí dotaz, klikněte na tlačítko **Microsoft SQL Server** jako zdroj dat.
   
    ![Dialogové okno Změnit zdroj dat](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)
3. V hello **název serveru** pole, vložte hello název, který jste zkopírovali v předchozím oddílu hello ze hello portálu Azure (to znamená, *servername*. database.windows.net).
4. Klikněte na tlačítko **použít ověřování systému SQL Server**.
5. Zadejte **tolladmin** v hello **uživatelské jméno** pole a **123toll!** v hello **heslo** pole.
6. Klikněte na tlačítko **vyberte nebo zadejte název databáze**a vyberte **TollDataDB** jako hello databáze.
   
    ![Přidat dialogové okno připojení](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)
7. Klikněte na **OK**.
8. Otevřete Průzkumníka serveru.
   
    ![Průzkumník serveru](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)
9. Viz čtyři tabulky v databázi TollDataDB hello.
   
    ![Tabulky v databázi TollDataDB hello](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator-tollapp-sample-project"></a>Generátor událostí: TollApp ukázkový projekt
Hello skript prostředí PowerShell automaticky spustí toosend události pomocí programu hello TollApp ukázkové aplikace. Tooperform nepotřebujete žádné další kroky.

Ale pokud vás zajímají podrobnosti implementace najdete hello zdrojový kód hello TollApp aplikace v Githubu [ukázky/TollApp](https://aka.ms/azure-stream-analytics-toll-source).

![Snímek obrazovky ukázkový kód zobrazený v sadě Visual Studio](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy Stream Analytics
1. V hello portálu Azure klikněte na tlačítko hello zeleného znaménka v levém horním rohu hello toocreate stránku hello nové úlohy Stream Analytics. Vyberte **Intelligence + analýzy** a pak klikněte na **úlohy služby Stream Analytics**.
   
    ![tlačítko Nový](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)
2. Zadejte název úlohy, ověřování odběru hello opravte a potom vytvořit novou skupinu prostředků v hello stejné oblasti jako hello úložiště centra událostí (výchozí hodnota je jihu USA pro skript hello).
3. Klikněte na tlačítko **Pin toodashboard** a potom **vytvořit** v hello dolní části stránky hello.
   
    ![Vytvoření úlohy Stream Analytics možnost](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>Definování vstupních zdrojů
1. Úloha Hello vytvoříte a otevřete stránku hello úlohy. Nebo můžete kliknout na hello vytvořil úlohu analýzy na řídicí panel portálu hello.

2. Klikněte na tlačítko hello **VSTUPY** kartě toodefine hello zdrojová data.
   
    ![Karta vstupy Hello](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.png)
3. Klikněte na tlačítko **přidat vstup**.
   
    ![Hello přidat vstup možnost](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)
4. Zadejte **EntryStream** jako **vstupní ALIAS**.
5. Typ zdroje je **datový proud**
6. Zdroj je **centra událostí**.
7. **Obor názvů sběrnice** by měla být hello TollData, jeden v hello rozevírací nabídku.
8. **Název centra událostí** by mělo být nastavené příliš**položka**.
9. **Název zásady centra událostí*je **RootManageSharedAccessKey** (hello výchozí hodnota).
10. Vyberte **JSON** pro **formát SERIALIZACE událostí** a **UTF8** pro **kódování**.
   
    Nastavení bude vypadat jako:
   
    ![Nastavení centra událostí](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)

10. Klikněte na tlačítko **vytvořit** dole hello v Průvodci hello toofinish stránku hello.
    
    Teď, když jste vytvořili hello vstupního datového proudu, postupujte podle hello stejné kroky toocreate hello konec datového proudu. Být jisti tooenter hodnoty jako na následující snímek obrazovky hello.
    
    ![Nastavení pro hello výstupní proud](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)
    
    Jste definovali dva vstupní datové proudy:
    
    ![Vstupní datové proudy definované v hello portálu Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.png)
    
    V dalším kroku přidáte referenčního datového vstupu pro soubor hello objektů blob, který obsahuje car registrační data.
11. Klikněte na tlačítko **přidat**a pak postupujte podle hello stejný proces pro vstupy datového proudu hello ale vybrat **referenční DATA** místo **datový proud** a hello **vstupní Alias**  je **registrace**.

12. účet úložiště, který začíná **tolldata**. název kontejneru Hello by měl být **tolldata**a hello **vzorek cesty** by měla být **registration.json**. Tento název souboru je velká a malá písmena a musí být **malá**.
    
    ![Nastavení úložiště blog](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)
13. Klikněte na tlačítko **vytvořit** toofinish hello průvodce.

Nyní jsou definovány všechny vstupy.

## <a name="define-output"></a>Definujte výstupní
1. V podokně přehled úlohy Stream Analytics hello vyberte **výstupy**.
   
    ![Karta Výstup Hello a možnost "Přidat výstup"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.png)
2. Klikněte na tlačítko **Přidat**.
3. Sada hello **alias pro výstup** too'output a potom **jímky** příliš**databáze SQL**.
3. Vyberte název serveru hello, která byla použita v hello hello článku v části "Connect tooDatabase ze sady Visual Studio". Název databáze Hello je **TollDataDB**.
4. Zadejte **tolladmin** v hello **uživatelské jméno** pole, **123toll!** v hello **heslo** pole, a **TollDataRefJoin** v hello **tabulky** pole.
   
    ![Nastavení databáze SQL](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.png)
5. Klikněte na možnost **Vytvořit**.

## <a name="azure-stream-analytics-query"></a>Azure Stream analytics dotazu
Hello **dotazu** karta obsahuje dotaz SQL, transformací hello příchozí data.

![Dotaz přidat karta toohello dotazů](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.png)

V tomto kurzu pokusí tooanswer několik obchodní otázky, které se týkají tootoll dat a konstrukce Stream Analytics dotazy, které lze použít v Azure Stream Analytics tooprovide příslušných odpovědí.

Než začnete vaše první práce Stream Analytics, podíváme se na několik scénáře a syntaxe dotazů hello.

## <a name="introduction-tooazure-stream-analytics-query-language"></a>Úvod tooAzure Stream Analytics query language
- - -
Řekněme, že je potřeba toocount hello počet vozidel, které projedou stánek zadejte. Protože to je nepřetržitý proud událostí, máte toodefine "období času." Pojďme upravit toobe otázku hello "kolik vozidel zadejte projedou stánek každé tři minuty?". To je obvykle označují tooas hello přeskakujícího počet.

Podívejme se na hello Azure Stream Analytics dotaz, který odpoví na tuto otázku:

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

Jak vidíte, Azure Stream Analytics používá dotazovací jazyk, který je například SQL a přidává několik rozšíření toospecify souvisejících s časem aspektů hello dotazu.

Další podrobnosti najdete v tématu o [Správa času](https://msdn.microsoft.com/library/azure/mt582045.aspx) a [Oddílová](https://msdn.microsoft.com/library/azure/dn835019.aspx) konstrukce používaných v dotazu hello z webu MSDN.

## <a name="testing-azure-stream-analytics-queries"></a>Testování dotazů Azure Stream Analytics
Teď, když jste napsali svůj první dotaz Azure Stream Analytics, je čas tootest pomocí ukázkových datových souborů umístěná ve složce TollApp aplikace hello následující cesty:

**.. \\TollApp\\TollApp\\dat**

Tato složka obsahuje hello následující soubory:

* Entry.JSON
* Exit.JSON
* Registration.JSON

## <a name="question-1-number-of-vehicles-entering-a-toll-booth"></a>Otázka č. 1: Počet vozidel zadávání stánek projedou
1. Otevřete hello portál Azure a přejděte tooyour vytvořit úlohy Azure Stream Analytics. Klikněte na tlačítko hello **dotazu** kartě a vložte dotaz z předchozí části hello.

2. toovalidate tento dotaz ukázková data, nahrávání dat hello do hello EntryStream vstup kliknutím hello... symbolů a výběr **nahrát ukázková data ze souboru**.

    ![Snímek obrazovky hello Entry.json souboru](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)
3. V podokně hello, který se zobrazí vyberte hello souboru (Entry.json) na místní počítač a klikněte na **OK**. Hello **Test** ikonu teď osvětlení a možné klepnout.
   
    ![Snímek obrazovky hello Entry.json souboru](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.png)
3. Ověřte, že výstup hello hello dotazu podle očekávání:
   
    ![Výsledky testu hello](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.png)

## <a name="question-2-report-total-time-for-each-car-toopass-through-hello-toll-booth"></a>Otázka č. 2: Sestava celkovou dobu pro každý car toopass prostřednictvím stánek projedou hello
Průměrná doba Hello, které je nutné pro car toopass prostřednictvím projedou hello pomáhá tooassess hello efektivitu hello proces a zkušeností zákazníků se hello.

toofind hello celkový čas, je nutné toojoin hello EntryTime datový proud s hello ExitTime datového proudu. Datové proudy hello na sloupce TollId a LicencePlate se připojí. Hello **připojení** operátor vyžaduje, abyste toospecify dočasné volnost, který popisuje hello přijatelný časový rozdíl mezi hello připojené k události. Budete používat **DATEDIFF** funkce toospecify, že události musí být delší než 15 minut od sebe navzájem. Použijí se taky hello **DATEDIFF** tooexit funkce a záznam časy toocompute hello skutečný čas automobilu stráví v hello výběr poplatků stanice. Všimněte si hello rozdíl hello použití **DATEDIFF** při použití v **vyberte** příkaz ne **připojení** podmínku.

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. tootest tento dotaz aktualizace hello dotaz na hello **dotazu** pro úlohu hello. Přidat hello testovací soubor pro **ExitStream** stejně jako **EntryStream** byla zadaná výše.
   
2. Klikněte na tlačítko **Test**.

3. Vyberte hello políčko tootest hello dotazů a zobrazení hello výstup:
   
    ![Výstup hello testu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3-report-all-commercial-vehicles-with-expired-registration"></a>Otázka č. 3: Sestavy všech komerční vozidel s vypršenou platností registrace
Azure Stream Analytics můžete použít statické snímky dat toojoin s dočasné datové proudy. toodemonstrate tato funkce, které hello použijte následující ukázkový dotaz.

Pokud komerční vehicle je registrován s hello projedou společnosti, můžete předat prostřednictvím hello projedou stánek bez zastavení pro kontroly. Registrace komerční Vehicle vyhledávací tabulka tooidentify použijete všechny vozidel komerční, jejichž platnost vypršela registrace.

```
SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
FROM EntryStream TIMESTAMP BY EntryTime
JOIN Registration
ON EntryStream.LicensePlate = Registration.LicensePlate
WHERE Registration.Expired = '1'
```

tootest dotazu pomocí referenční data, musíte toodefine vstupní zdroj pro hello referenční data, která jste již dokončili.

tootest tento dotaz dotaz hello vložení do hello **dotazu** , klikněte na **Test**a zadejte hello dva vstupní zdroje a hello registrace vzorová data a klikněte na tlačítko **Test**.  
   
![Výstup hello testu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)

## <a name="start-hello-stream-analytics-job"></a>Spuštění úlohy Stream Analytics hello
Nyní je čas toofinish hello konfiguraci a spuštění hello úlohy. Uložit hello dotazů od 3 otázku, která bude vytvoření výstupu, odpovídá hello schéma hello **TollDataRefJoin** výstupní tabulka.

Úloha přejděte toohello **řídicí panel**a klikněte na tlačítko **spustit**.

![Snímek obrazovky hello tlačítko Start na řídicím panelu úlohy hello](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.png)

V poli hello dialogové okno, které se otevře, změnit hello **spustit výstup** čas příliš**vlastní čas**. Změňte hodinu tooone hello hodinu před hello aktuální čas. Tato změna zajistí, že jsou všechny události z centra událostí hello zpracovaných od spuštění toogenerate hello události od začátku hello hello kurzu. Klikněte na tlačítko hello **spustit** tlačítko toostart hello úlohy.

![Výběr vlastních času](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

Počáteční hello úlohy může trvat několik minut. Stav hello můžete zobrazit na stránku hello nejvyšší úrovně pro Stream Analytics.

![Snímek obrazovky hello stav úlohy hello](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.png)

## <a name="check-results-in-visual-studio"></a>Výsledky kontroly v sadě Visual Studio
1. Otevřete Průzkumníka serveru Visual Studia a klikněte pravým tlačítkem na hello **TollDataRefJoin** tabulky.
2. Klikněte na tlačítko **zobrazit Data tabulky** toosee hello výstupu úlohy.
   
    ![Výběr "Zobrazit tabulku Data" v Průzkumníku serveru](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>Horizontální navýšení kapacity Azure Stream Analytics úlohy
Azure Stream Analytics je navržený tak, tooelastically škálovat tak, aby ho může zpracovávat velké množství dat. Hello Azure Stream Analytics dotazu lze použít **PARTITION BY** klauzule tootell hello systém, který bude tento krok škálovat. **PartitionId** je speciální sloupec, který hello systému přidá ID oddílu hello toomatch vstupu hello (Centrum událostí).

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. Aktuální úlohu zastavení hello, aktualizace hello dotaz hello **dotazu** kartě a otevřete hello **nastavení** zařízení na řídicím panelu hello úlohy. Klikněte na tlačítko **škálování**.
   
    **JEDNOTEK STREAMING** definovat může přijímat hello množství výpočetního výkonu, který hello úlohy.
2. Změna hello rozevíracího seznamu od 1 z 6.
   
    ![Snímek obrazovky výběru 6 jednotky streamování](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.png)
3. Přejděte toohello **výstupy** kartě a změňte název hello tabulky SQL hello příliš**TollDataTumblingCountPartitioned**.

Pokud spustíte úlohu hello nyní může Azure Stream Analytics rozložení práce mezi víc zdrojích výpočtů a dosáhnout lepší propustnosti. Upozorňujeme, že tento hello TollApp aplikace také odesílá události podle TollId na oddíly.

## <a name="monitor"></a>Monitorování
Hello **monitorování** oblast obsahuje statistiku hello spuštění úlohy. Prvním konfigurace je potřeba účet úložiště hello toouse hello stejné oblasti (název poplatků jako hello zbytek tohoto dokumentu).   

![Snímek obrazovky monitorování](media/stream-analytics-build-an-iot-solution-using-stream-analytics/monitoring.png)

Dostanete **protokoly aktivity** z řídicího panelu úloha hello **nastavení** také oblasti.


## <a name="conclusion"></a>Závěr
V tomto kurzu se zavedl toohello služby Azure Stream Analytics. Je prokázat, jak tooconfigure vstupy a výstupy pro hello úlohy služby Stream Analytics. Pomocí hello Projedou Data scénář, hello kurzu popsaných nejběžnějších problémů, které vznikají v prostoru hello dat v provozu a o tom, jak lze vyřešit pomocí jednoduchého dotazy podobné jazyku SQL v Azure Stream Analytics. kurz Hello popsané konstrukce rozšíření SQL pro práci s dočasná data. Vám ukázal, jak toojoin datových proudů, jak tooenrich hello datový proud statická referenční data a jak tooscale na vyšší propustnost tooachieve dotazu.

I když tento kurz představuje dobrou úvod, není kompletní, a to jakýmkoli způsobem. Můžete najít další typy dotazů pomocí jazyka SAQL hello v [dotaz příkladů běžných vzorů využití Stream Analytics](stream-analytics-stream-analytics-query-patterns.md).
Odkazovat toohello [online dokumentaci](https://azure.microsoft.com/documentation/services/stream-analytics/) toolearn Další informace o Azure Stream Analytics.

## <a name="clean-up-your-azure-account"></a>Vyčištění účtu Azure
1. Zastavení úlohy Stream Analytics hello v hello portálu Azure.
   
    Hello Setup.ps1 skript vytvoří dvě centrům událostí a databázi SQL. Následující pokyny vám pomohou vyčištění prostředků na konci hello tohoto kurzu hello Hello.
2. V okně prostředí PowerShell, zadejte **.\\ CleanUp.ps1** toostart hello skript, který odstraní prostředky využívané v kurzu hello.
   
   > [!NOTE]
   > Prostředky jsou identifikovány názvem hello. Ujistěte se, že jednotlivé položky pečlivě zkontrolovat před potvrzením odebrání.
   > 
   > 


