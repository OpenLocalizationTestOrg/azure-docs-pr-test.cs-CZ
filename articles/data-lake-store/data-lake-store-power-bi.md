---
title: "aaaAnalyze dat v Data Lake Store pomocí Power BI | Microsoft Docs"
description: "Použijte Power BI tooanalyze data uložená v Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57d19d27-e135-49d9-a7ea-46c48ef4e3bd
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 6a1bfa80fd1b0dda59b7eaaae9ca1585ba42783e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>Analýza dat v Data Lake Store pomocí Power BI
V tomto článku se dozvíte, jak Power BI Desktop tooanalyze toouse a vizualizovat data uložená v Azure Data Lake Store.

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Účet Azure Data Lake Store**. Postupujte podle pokynů hello [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](data-lake-store-get-started-portal.md). Tento článek předpokládá, že jste již vytvořili účet Data Lake Store, názvem **mybidatalakestore**a odesláno ukázkový datový soubor (**Drivers.txt**) tooit. Tento ukázkový soubor je k dispozici ke stažení [úložiště Git Azure Data Lake](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).
* **Power BI Desktop**. Si můžete stáhnout z [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331). 

## <a name="create-a-report-in-power-bi-desktop"></a>Vytvoření sestavy v Power BI Desktop
1. Power BI Desktop spustíte ve vašem počítači.
2. Z hello **Domů** pásu karet, klikněte na tlačítko **načíst Data**a pak klikněte na tlačítko Další. V hello **načíst Data** dialogové okno, klikněte na tlačítko **Azure**, klikněte na tlačítko **Azure Data Lake Store**a potom klikněte na **Connect**.
   
    ![Připojit tooData Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account.png "Connect tooData Lake Store")
3. Pokud se zobrazí dialogové okno o konektoru hello používán fáze vývoje, opt toocontinue.
4. V hello **Microsoft Azure Data Lake Store** dialogové okno, zadejte účet Data Lake Store tooyour hello adresy URL a pak klikněte na tlačítko **OK**.
   
    ![Adresa URL pro Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "adresu URL pro Data Lake Store")
5. V hello další dialogové okno, klikněte na **přihlášení** toosign do účtu Data Lake Store. Budete přesměrování tooyour organizace přihlašovací stránku. Postupujte podle pokynů toosign hello v úvahu hello.
   
    ![Přihlaste se k Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "přihlásit ke službě Data Lake Store")
6. Po úspěšném přihlášení, klikněte na tlačítko **Connect**.
   
    ![Připojit tooData Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Connect tooData Lake Store")
7. Další dialogové okno Hello ukazuje hello soubor, který jste nahráli tooyour účtu Data Lake Store. Ověřte informace o hello a pak klikněte na tlačítko **zatížení**.
   
    ![Načtení dat z Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "načtení dat z Data Lake Store")
8. Po hello úspěšně načtení dat do Power BI, zobrazí se následující pole v hello hello **pole** kartě.
   
    ![Importovat pole](./media/data-lake-store-power-bi/imported-fields.png "importovat pole")
   
    Ale toovisualize a analyzovat hello data, jsme raději toobe data hello k dispozici pro následující pole hello
   
    ![Požadovaného pole](./media/data-lake-store-power-bi/desired-fields.png "požadovaných polí")
   
    V dalších krocích hello budeme aktualizovat hello dotazu tooconvert hello importovat data ve formátu požadované hello.
9. Z hello **Domů** pásu karet, klikněte na tlačítko **upravit dotazy**.
   
    ![Upravit dotazy](./media/data-lake-store-power-bi/edit-queries.png "úpravy dotazů")
10. V editoru dotazů hello pod hello **obsahu** sloupce, klikněte na tlačítko **binární**.
    
    ![Upravit dotazy](./media/data-lake-store-power-bi/convert-query1.png "úpravy dotazů")
11. Zobrazí se ikona souboru, který představuje hello **Drivers.txt** soubor, který jste nahráli. Klikněte pravým tlačítkem na soubor hello a klikněte na tlačítko **CSV**.    
    
    ![Upravit dotazy](./media/data-lake-store-power-bi/convert-query2.png "úpravy dotazů")
12. Měli byste vidět výstup, jak je uvedeno níže. Vaše data jsou nyní k dispozici ve formátu, které můžete použít toocreate vizualizace.
    
    ![Upravit dotazy](./media/data-lake-store-power-bi/convert-query3.png "úpravy dotazů")
13. Z hello **Domů** pásu karet, klikněte na tlačítko **zavřete a použít**a potom klikněte na **zavřete a použít**.
    
    ![Upravit dotazy](./media/data-lake-store-power-bi/load-edited-query.png "úpravy dotazů")
14. Po aktualizaci dotazu hello hello **pole** karta zobrazí hello nová pole, které jsou k dispozici pro vizualizaci.
    
    ![Aktualizovat pole](./media/data-lake-store-power-bi/updated-query-fields.png "aktualizovat pole")
15. Dejte nám vytvořte toorepresent výsečového grafu hello ovladače v každé město pro dané země. toodo tedy zkontrolujte hello následující možnosti.
    
    1. Z karty vizualizace hello klikněte na symbol hello pro výsečového grafu.
       
        ![Vytvoření výsečového grafu](./media/data-lake-store-power-bi/create-pie-chart.png "vytvořit výsečového grafu")
    2. Hello sloupce, které přidáme jsou toouse **sloupec 4** (název města hello) a **7 sloupci** (název země hello). Přetáhněte tyto sloupce z **pole** kartě příliš**vizualizace** kartě, jak je uvedeno níže.
       
        ![Vytváření vizualizací](./media/data-lake-store-power-bi/create-visualizations.png "vytváření vizualizací")
    3. Výsečový graf Hello by měl nyní vypadat podobně jako hello níže.
       
        ![Výsečový graf](./media/data-lake-store-power-bi/pie-chart.png "vytváření vizualizací")
16. Výběrem konkrétní země z úrovně filtry stránku hello nyní můžete vidět hello počet ovladačů v každé město hello vybrané země. Například v položce hello **vizualizace** v části **stránky úrovně filtry**, vyberte **Brazílie**.
    
    ![Vyberte zemi](./media/data-lake-store-power-bi/select-country.png "vyberte zemi")
17. Výsečový graf Hello je automaticky aktualizovány toodisplay hello ovladače v hello města Brazílie.
    
    ![Ovladače v určité zemi](./media/data-lake-store-power-bi/driver-per-country.png "ovladače podle země")
18. Z hello **soubor** nabídky, klikněte na tlačítko **Uložit** toosave hello vizualizace jako soubor Power BI Desktop.

## <a name="publish-report-toopower-bi-service"></a>Publikování sestav tooPower BI služby
Po vytvoření hello vizualizace v Power BI Desktop, můžete ji sdílet s ostatními ji publikujete toohello služby Power BI. Návod, jak toodo, který najdete v části [publikování z Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Viz také
* [Analýza dat v Data Lake Store pomocí Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

