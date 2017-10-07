---
title: aaaPower BI kurz pro Azure Cosmos DB konektor | Microsoft Docs
description: "Použijte tento kurz tooimport Power BI JSON, vytvořit pronikavého sestavy a vizualizovat data pomocí Azure Cosmos DB a Power BI connector hello."
keywords: Power bi kurzu, vizualizaci dat, power bi connector
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: cd1b7f70-ef99-40b7-ab1c-f5f3e97641f7
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: mimig
ms.openlocfilehash: ca0bb8b76db8ef2ec936722b682af6a9488a3501
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="power-bi-tutorial-for-azure-cosmos-db-visualize-data-using-hello-power-bi-connector"></a>Power BI kurz pro Azure Cosmos DB: vizualizovat data pomocí Power BI connector hello
[PowerBI.com](https://powerbi.microsoft.com/) je online služba, kde můžete vytvářet a sdílet řídicí panely a sestavy s daty, která je důležité tooyou a vaší organizace.  Power BI Desktop je vyhrazená sestavy vývojového nástroje, který vám umožní tooretrieve data z různých zdrojů dat, sloučení a transformovat hello data, vytvářet výkonné sestavy a vizualizací a publikování hello sestavy tooPower BI.  Hello nejnovější verzi nástroje Power BI Desktop můžete teď připojit tooyour Cosmos DB účet prostřednictvím konektoru hello Cosmos DB pro Power BI.   

V tomto kurzu Power BI jsme provede hello kroky tooconnect tooa Cosmos DB účtu na Power BI Desktop, přejděte tooa kolekce, kde chceme tooextract hello dat pomocí hello Navigátor transformace dat pomocí Power BI Desktop Query tabulkovém formátu ve formátu JSON Editor a vytvářet a publikovat tooPowerBI.com sestavy.

Po dokončení tohoto kurzu Power BI, budete moct tooanswer hello následující otázky:  

* Jak lze vytvářet sestavy s daty z databáze Cosmos pomocí Power BI Desktop?
* Připojení tooa Cosmos DB účtu na Power BI Desktop
* Jak můžete načítat data z kolekce v Power BI Desktop?
* Jak můžete převést vnořené data JSON v Power BI Desktop?
* Jak může sdílet Moje sestavy na PowerBI.com a publikovat?

> [!NOTE]
> Hello Power BI connector pro Azure Cosmos DB připojí tooPower BI Desktop pro extrakci a transformaci dat.. Sestavy vytvořené v Power BI Desktop pak lze publikované tooPowerBI.com. Přímé extrakce a transformaci dat Azure Cosmos databáze nelze provést na PowerBI.com. 

## <a name="prerequisites"></a>Požadavky
Než budete postupovat hello pokyny v tomto kurzu Power BI, zajistěte, abyste měli přístup toohello následující prostředky:

* [nejnovější verze Power BI Desktop Hello](https://powerbi.microsoft.com/desktop).
* Účet pro přístup k tooour ukázku nebo data ve vašem účtu Cosmos DB.
  * Hello ukázkový účet bude zahrnovat hello sopka data zobrazená v tomto kurzu. Tento ukázkový účet není vázán žádné SLA a je určena pouze pro demonstrační účely.  Jsme rezervovat hello správné toomake úpravy toothis ukázkový účet včetně, ale mimo jiné, ukončení hello účet, změna hello klíč, omezení přístupu, změna a odstranit hello data, kdykoli bez předstihu upozornění nebo důvod.
    * Adresa URL: https://analytics.documents.azure.com
    * Klíč jen pro čtení: MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR + YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw ==
  * Nebo toocreate svůj vlastní účet, najdete v části [vytvoření účtu Azure Cosmos DB databáze pomocí portálu Azure hello](https://azure.microsoft.com/documentation/articles/create-account/). Potom tooget ukázkových sopka dat, který je podobný toowhat použili v tomto kurzu (ale neobsahuje bloky GeoJSON hello), najdete v tématu hello [NOAA lokality](https://www.ngdc.noaa.gov/nndc/struts/form?t=102557&s=5&d=5) a k následnému importování dat hello pomocí hello [Azure Cosmos DB migrace dat Nástroj](import-data.md).

tooshare sestavy na PowerBI.com, musíte mít účet na PowerBI.com.  toolearn Další informace o Power BI pro volné a Power BI Pro, navštivte [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing).

## <a name="lets-get-started"></a>Pusťme se do toho
V tomto kurzu budeme Představte si, že geologist, studujete vulkány kolem hello, world.  Hello sopka data jsou uložena v účtu Cosmos DB a dokumenty JSON hello vypadat hello následující ukázka dokumentu.

    {
        "Volcano Name": "Rainier",
           "Country": "United States",
          "Region": "US-Washington",
          "Location": {
            "type": "Point",
            "coordinates": [
              -121.758,
              46.87
            ]
          },
          "Elevation": 4392,
          "Type": "Stratovolcano",
          "Status": "Dendrochronology",
          "Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
    }

Chcete tooretrieve hello sopka data z hello Cosmos DB účtu a vizualizovat data v interaktivní sestavy Power BI jako hello následující sestavy.

![Po dokončení tohoto kurzu Power BI s konektorem hello Power BI, budete moct toovisualize dat pomocí hello sopka sestavy Power BI Desktop](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)

Připraveno toogive it a zkuste to? Můžeme začít.

1. Na pracovní stanici spusťte Power BI Desktop.
2. Po spuštění Power BI Desktop *úvodní* obrazovka se zobrazí.
   
    ![Power BI Desktop úvodní obrazovce - Power BI connector](./media/powerbi-visualize/power_bi_connector_welcome.png)
3. Můžete **načíst Data**, najdete v části **poslední zdroje**, nebo **otevřít další sestavy** přímo z hello *úvodní* obrazovky.  Klikněte na tlačítko hello X na hello pravém horním rohu tooclose úvodní obrazovka. Hello **sestavy** Power BI Desktop zobrazí.
   
    ![Power BI Desktop zobrazení sestavy - Power BI connector](./media/powerbi-visualize/power_bi_connector_pbireportview.png)
4. Vyberte hello **Domů** pásu karet, a potom klikněte na **načíst Data**.  Hello **načíst Data** okno by se měla objevit.
5. Klikněte na **Azure**, vyberte **Microsoft Azure DocumentDB (Beta)**a potom klikněte na **Connect**. 

    ![Power BI Desktop získat Data - Power BI connector](./media/powerbi-visualize/power_bi_connector_pbigetdata.png)   
6. Na hello **Preview konektor** klikněte na tlačítko **pokračovat**. Hello **připojit ke službě Microsoft Azure DocumentDB** se zobrazí v okně.
7. Zadejte hello Cosmos DB účet adresu URL koncového bodu chcete vytvořit tooretrieve hello data z, jak je uvedeno níže a pak klikněte na tlačítko **OK**. toouse svůj vlastní účet, můžete načíst pole Adresa URL z hello URI v hello hello ** [klíče](manage-account.md#keys) ** okno hello portálu Azure. toouse hello ukázku účet, zadejte `https://analytics.documents.azure.com` pro adresu URL hello. 
   
    Nezadávejte název hello databáze, název kolekce a příkaz jazyka SQL jako tato pole jsou volitelná.  Místo toho použijeme hello Navigátor tooselect hello databázi a kolekci tooidentify kde hello data pocházejí z.
   
    ![Power BI kurz pro Azure Cosmos DB Power BI connector – okno připojení plochy](./media/powerbi-visualize/power_bi_connector_pbiconnectwindow.png)
8. Pokud se připojujete toothis koncový bod pro hello poprvé, budete vyzváni k hello klíč účtu. Pro svůj vlastní účet načíst klíč hello z hello **primární klíč** pole v hello ** [klíče jen pro čtení](manage-account.md#keys) ** okno hello portálu Azure. Pro účet ukázkový text hello, hello klíč je `MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR+YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw==`. Zadejte příslušný klíč hello a pak klikněte na tlačítko **Connect**.
   
    Doporučujeme používat klíč hello jen pro čtení, při vytváření sestav.  To zabrání ohrožením z hello hlavní klíč toopotential bezpečnostní rizika. klíč jen pro čtení Hello je k dispozici z hello [klíče](manage-account.md#keys) okno hello portálu Azure, nebo můžete použít ukázkové informace o účtu hello výše uvedeného.
   
    ![Power BI kurz pro Azure Cosmos DB Power BI connector – klíč účtu](./media/powerbi-visualize/power_bi_connector_pbidocumentdbkey.png)
    
    > [!NOTE] 
    > Pokud dojde k chybě, která uvádí, že "hello zadaná databáze nebyl nalezen." najdete v části řešení hello kroky v tomto [Power BI problém](https://community.powerbi.com/t5/Issues/Document-DB-Power-BI/idi-p/208200).
    
9. Když se účet hello úspěšně připojí, hello **Navigátor** se zobrazí.  Hello **Navigátor** se zobrazí seznam databází v rámci účtu hello.
10. Klikněte na tlačítko a rozbalte v databázi hello kde hello dat pro sestavu hello bude pocházet z, pokud používáte účet ukázkový text hello, vyberte **volcanodb**.   
11. Nyní vyberte kolekci, která se budou načítat data hello z. Pokud používáte účet ukázkový text hello, vyberte **volcano1**.
    
    Podokno náhledu Hello zobrazuje seznam **záznam** položky.  Dokument je reprezentován jako **záznam** typu v Power BI. Podobně vnořený blok v dokumentu JSON je také **záznam**.
    
    ![Power BI kurz pro Azure Cosmos DB Power BI connector – okno Navigátor](./media/powerbi-visualize/power_bi_connector_pbinavigator.png)
12. Klikněte na tlačítko **upravit** toolaunch hello Editor dotazů na nové datové hello tootransform okno.

## <a name="flattening-and-transforming-json-documents"></a>Vyrovnání a transformace dokumentů JSON
1. Přepínač toohello okno editoru dotazů Power BI, kde hello **dokumentu** sloupec v prostředním podokně hello.
   ![Editor dotazů aplikace Power BI Desktop](./media/powerbi-visualize/power_bi_connector_pbiqueryeditor.png)
2. Klikněte na rozbalovací hello na pravé straně hello hello **dokumentu** záhlaví sloupce.  Zobrazí se Hello kontextovou nabídku s seznam polí.  Vyberte hello polí je nutné pro sestavu, pro instanci, sopka název, zemi, oblast, umístění, zvýšení oprávnění, typ, stav a poslední erupcí vědět a pak klikněte na tlačítko **OK**.
   
    ![Power BI kurz pro Azure Cosmos DB Power BI connector – rozbalte dokumenty](./media/powerbi-visualize/power_bi_connector_pbiqueryeditorexpander.png)
3. Hello prostředním podokně se zobrazí náhled výsledků hello s poli hello vybrané.
   
    ![Power BI kurz pro Azure Cosmos DB Power BI connector – vyrovnání výsledky](./media/powerbi-visualize/power_bi_connector_pbiresultflatten.png)
4. V našem příkladu hello vlastnost umístění je blok GeoJSON v dokumentu.  Jak vidíte, jako je reprezentována umístění **záznam** typu v Power BI Desktop.  
5. Klikněte na rozbalovací hello na pravé straně hello záhlaví sloupce hello umístění.  Zobrazí se Hello kontextovou nabídku s souřadnice a typ pole.  Umožňuje vybrat hello souřadnice pole a klikněte na tlačítko **OK**.
   
    ![Power BI kurz pro Azure Cosmos DB Power BI connector – záznam o umístění](./media/powerbi-visualize/power_bi_connector_pbilocationrecord.png)
6. Hello prostředním podokně teď zobrazuje sloupec souřadnice **seznamu** typu.  Jak je znázorněno na začátku hello hello kurzu, hello GeoJSON data v tomto kurzu je bod typu s hodnoty zeměpisné šířky a délky zaznamenané v hello souřadnice pole.
   
    element souřadnice [0] Hello reprezentuje zeměpisnou délku během šířky představuje souřadnice [1].
    ![Power BI kurz pro Azure Cosmos DB Power BI connector - souřadnice seznamu](./media/powerbi-visualize/power_bi_connector_pbiresultflattenlist.png)
7. tooflatten hello koordinuje pole, vytvoříme **vlastní sloupec** názvem LatLong.  Vyberte hello **přidat sloupec** pásu karet a klikněte na **přidat sloupec vlastní**.  Hello **přidat sloupec vlastní** okno by se měla objevit.
8. Zadejte název pro nový sloupec hello, například LatLong.
9. Potom zadejte hello vlastního vzorce pro nový sloupec hello.  Pro náš příklad jsme se řetězení hello zeměpisnou šířku a délku hodnot oddělených čárkou, jak je uvedeno níže pomocí hello následující vzorec: `Text.From([coordinates]{1})&","&Text.From([coordinates]{0})`. Klikněte na **OK**.
   
    Další informace na Data Analysis výrazy (DAX) včetně funkce jazyka DAX, navštivte [DAX základní v Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/554619-dax-basics-in-power-bi-desktop).
   
    ![Power BI kurz pro Azure Cosmos DB Power BI connector – přidat sloupec vlastní](./media/powerbi-visualize/power_bi_connector_pbicustomlatlong.png)
10. Nyní hello prostředním podokně se zobrazí nový sloupec LatLong hello naplněný hello zeměpisnou šířku a hodnoty zeměpisné délky oddělených čárkou.
    
    ![Power BI kurz pro Azure Cosmos DB Power BI connector – vlastní LatLong sloupce](./media/powerbi-visualize/power_bi_connector_pbicolumnlatlong.png)
    
    Pokud obdržíte chybu v hello nový sloupec, ujistěte se, že hello použít kroky v části Nastavení dotazu odpovídat hello následující obrázek:
    
    ![Použité kroky by měla být zdroje, navigace, rozšířit dokumentu, rozšířit Document.Location, přidat vlastní](./media/powerbi-visualize/power-bi-applied-steps.png)
    
    Pokud vaše postup se liší, odstraňte text hello dodatečné kroky a znovu zkuste přidat vlastní sloupec hello. 
11. Nyní jsme dokončili sloučení hello dat do tabulkovém formátu.  Můžete využít všechny funkce hello k dispozici v hello Editor dotazů tooshape a transformovat data do databáze. Cosmos.  Pokud používáte ukázkový text hello, změnit hello datový typ pro zvýšení oprávnění příliš**celé číslo** změnou hello **datový typ** na hello **Domů** pásu karet.
    
    ![Power BI kurz pro Azure Cosmos DB Power BI connector - Změna typu sloupce](./media/powerbi-visualize/power_bi_connector_pbichangetype.png)
12. Klikněte na tlačítko **zavřete a použít** toosave hello datového modelu.
    
    ![Power BI kurz pro Azure Cosmos DB Power BI connector - zavřete & použít](./media/powerbi-visualize/power_bi_connector_pbicloseapply.png)

<a id="build-the-reports"></a>
## <a name="build-hello-reports"></a>Vytváření sestav hello
Zobrazení Power BI Desktop sestavy je, kde můžete začít s vytvářením sestav toovisualize data.  Můžete vytvořit sestavy přetažením polí do hello **sestavy** plátno.

![Power BI Desktop zobrazení sestavy - Power BI connector](./media/powerbi-visualize/power_bi_connector_pbireportview2.png)

V zobrazení sestavy hello byste měli najít:

1. Hello **pole** podokně, to je, kde uvidíte seznam modelů dat s poli, můžete použít pro sestavy.
2. Hello **vizualizace** podokně. Sestava může obsahovat jeden nebo více vizualizací.  Vyberte typy visual hello hodí potřeb z hello **vizualizace** podokně.
3. Hello **sestavy** plátno, to je, kde vytvoříte hello vizuální prvky sestavy.
4. Hello **sestavy** stránky. Můžete přidat více stránek sestavy v Power BI Desktop.

Následující Hello ukazuje hello základní kroky pro vytvoření jednoduché sestavy interaktivní zobrazení mapy.

1. Pro náš příklad vytvoříme zobrazení mapa zobrazuje umístění každého sopka hello.  V hello **vizualizace** podokně, klikněte na typ visual hello mapy jako zvýrazněných v výše uvedený snímek obrazovky hello.  Měli byste vidět typ visual hello mapy vykresluje na hello **sestavy** plátno.  Hello **vizualizace** podokně by měl zobrazit také sadu vlastnosti související toohello typu visual mapování.
2. Nyní, přetáhnout myší pole LatLong hello z hello **pole** podokně toohello **umístění** vlastnost **vizualizace** podokně.
3. V dalším kroku přetažení hello sopka název pole toohello **legendy** vlastnost.  
4. Potom přetažení hello zvýšení pole toohello **velikost** vlastnost.  
5. Teď byste měli vidět hello mapy visual zobrazující sadu bublinách označující hello umístění každého sopka hello velikosti bublin hello korelace toohello zvýšení sopka hello.
6. Nyní jste vytvořili základní sestavy.  Hello sestavy můžete dále přizpůsobit přidáním více vizualizací.  V našem případě jsme přidali sopka typ průřez toomake hello sestavy interaktivní.  
   
    ![Snímek obrazovky hello závěrečnou zprávu Power BI Desktop po dokončení kurzu hello Power BI pro Azure Cosmos DB](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)

## <a name="publish-and-share-your-report"></a>Publikování a sdílení sestavy
tooshare sestavy, musíte mít účet na PowerBI.com.

1. V Power BI Desktop hello, klikněte na hello **Domů** pásu karet.
2. Klikněte na **Publikovat**.  Výzvami tooenter hello uživatelské jméno a heslo bude pro váš účet na PowerBI.com.
3. Po ověření přihlašovacích údajů hello hello sestava je vybraného cílového umístění publikované tooyour.
4. Klikněte na tlačítko **otevřete 'PowerBITutorial.pbix' v Power BI** toosee a sdílení sestavy na PowerBI.com.
   
    ![Publikování tooPower BI úspěchu! Otevřete kurz v Power BI](./media/powerbi-visualize/power_bi_connector_open_in_powerbi.png)

## <a name="create-a-dashboard-in-powerbicom"></a>Vytvořit řídicí panel na PowerBI.com
Teď, když máte sestavy, umožňuje sdílet na webu PowerBI.com

Když publikujete sestavy z Power BI Desktop tooPowerBI.com, generuje **sestavy** a **datovou sadu** ve vašem klientovi PowerBI.com. Například po publikované sestavy názvem **PowerBITutorial** tooPowerBI.com, uvidíte v obou hello PowerBITutorial **sestavy** a **datové sady** částech na PowerBI.com.

   ![Snímek obrazovky hello nové sestavy a datové sady na PowerBI.com](./media/powerbi-visualize/powerbi-reports-datasets.png)

toocreate lze sdílet řídicí panel, klikněte na tlačítko hello **Pin Live stránky** tlačítko na PowerBI.com sestavy.

   ![Snímek obrazovky hello nové sestavy a datové sady na PowerBI.com](./media/powerbi-visualize/power-bi-pin-live-tile.png)

Potom postupujte podle pokynů hello v [připnout dlaždice ze sestavy](https://powerbi.microsoft.com/documentation/powerbi-service-pin-a-tile-to-a-dashboard-from-a-report/#pin-a-tile-from-a-report) toocreate nový řídicí panel. 

Ad hoc změny tooreport můžete také provést před vytvořením řídicího panelu. Doporučujeme však používat Power BI Desktop tooperform hello úpravy a znovu publikovat sestavy tooPowerBI.com hello.

## <a name="refresh-data-in-powerbicom"></a>Aktualizovat data na PowerBI.com
Existují dva způsoby toorefresh data, ad hoc a naplánované.

Pro ad hoc aktualizace, klikněte jednoduše na eclipses hello (...) podle hello **datovou sadu**, například PowerBITutorial. Zobrazí seznam akcí, včetně **aktualizovat**. Klikněte na tlačítko **aktualizovat** toorefresh hello data.

![Snímek obrazovky aktualizace teď na PowerBI.com](./media/powerbi-visualize/power-bi-refresh-now.png)

Pro plánované aktualizace hello následující.

1. Klikněte na tlačítko **naplánovat aktualizaci** v hello akce. 

    ![Snímek obrazovky hello naplánovat aktualizaci na PowerBI.com](./media/powerbi-visualize/power-bi-schedule-refresh.png)
2. V hello **nastavení** rozbalte **přihlašovací údaje ke zdroji dat**. 
3. Klikněte na **upravit přihlašovací údaje**. 
   
    se zobrazí místní okno Konfigurace Hello. 
4. Zadejte pro tuto datovou sadu hello klíče tooconnect toohello Cosmos DB účet a potom klikněte na tlačítko **přihlášení**. 
5. Rozbalte položku **naplánovat aktualizaci** a nastavit plán hello chcete datovou sadu toorefresh hello. 
6. Klikněte na tlačítko **použít** a dokončení nastavení plánované aktualizace hello.

## <a name="next-steps"></a>Další kroky
* toolearn Další informace o Power BI, najdete v části [Začínáme s Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
* toolearn Další informace o Cosmos databáze, najdete v části hello [cílovou stránku dokumentace Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).

