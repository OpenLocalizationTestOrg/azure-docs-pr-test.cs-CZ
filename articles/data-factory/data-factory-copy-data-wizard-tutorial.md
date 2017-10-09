---
title: "Kurz: Vytvoření kanálu pomocí průvodce kopírováním | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte kanál služby Azure Data Factory s aktivitou kopírování pomocí Průvodce kopírováním podporovaného službou Data Factory hello"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b87afb8e-53b7-4e1b-905b-0343dd096198
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 567b89e7a54c245c134cd0674690e6f3499b46d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-data-factory-copy-wizard"></a>Kurz: Vytvoření kanálu s aktivitou kopírování pomocí průvodce kopírováním služby Data Factory.
> [!div class="op_single_selector"]
> * [Přehled a požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Šablona Azure Resource Manageru](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

Tento kurz ukazuje, jak toouse hello **Průvodce kopírováním** toocopy dat z Azure SQL database tooan úložiště objektů blob v Azure. 

Hello Azure Data Factory **Průvodce kopírováním** vám umožní vytvořit tooquickly datovém kanálu, který kopíruje data z podporované zdrojové data store tooa podporované cílového úložiště dat. Proto doporučujeme pro váš scénář přesun dat pomocí Průvodce hello jako první krok toocreate ukázkový kanál služby. Seznam úložišť dat podporovaných jako zdroje a cíle najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).  

Tento kurz ukazuje, jak toocreate služby Azure data factory, hello spuštění Průvodce kopírováním přejít pomocí několika kroků tooprovide podrobnosti o váš scénář přijímání nebo přesun dat. Po dokončení kroků v Průvodci hello hello průvodce automaticky vytvoří kanál s aktivitou kopírování toocopy dat z Azure SQL database tooan úložiště objektů blob v Azure. Další informace o aktivitě kopírování najdete v tématu [Aktivity pohybu dat](data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Požadavky
Dokončení požadavky uvedené v hello [přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) článek před provedením tohoto kurzu.

## <a name="create-data-factory"></a>Vytvoření objektu pro vytváření dat
V tomto kroku použijete hello Azure portálu toocreate objekt pro vytváření dat Azure s názvem **ADFTutorialDataFactory**.

1. Přihlaste se příliš[portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **+ nový** z levého horního rohu hello, klikněte na tlačítko **Data + analýzy**a klikněte na tlačítko **Data Factory**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/data-factory-copy-data-wizard-tutorial/new-data-factory-menu.png)
2. V hello **nový objekt pro vytváření dat** okno:
   
   1. Zadejte **ADFTutorialDataFactory** pro hello **název**.
       Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný. Pokud se zobrazí chyba hello: `Data factory name “ADFTutorialDataFactory” is not available`, změňte hello název objektu pro vytváření dat hello (například yournameADFTutorialDataFactoryYYYYMMDD) a zkuste to znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.  
      
       ![Název objektu pro vytváření dat není k dispozici](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-not-available.png)    
   2. Vyberte své **předplatné** Azure.
   3. Pro skupinu prostředků proveďte jednu z následujících kroků hello: 
      
      - Vyberte **použít existující** tooselect existující skupinu prostředků.
      - Vyberte **vytvořit nový** tooenter název pro skupinu prostředků.
          
        Některé hello kroky v tomto kurzu vychází z předpokladu, že použijete název hello: **ADFTutorialResourceGroup** pro skupinu prostředků hello. toolearn o skupinách prostředků najdete v části [pomocí prostředků skupiny prostředků Azure toomanage](../azure-resource-manager/resource-group-overview.md).
   4. Vyberte **umístění** hello data Factory.
   5. Vyberte **Pin toodashboard** políčko v hello dolní části okna hello.  
   6. Klikněte na možnost **Vytvořit**.
      
       ![Okno Nový objekt pro vytváření dat](media/data-factory-copy-data-wizard-tutorial/new-data-factory-blade.png)            
3. Po dokončení vytvoření hello uvidíte hello **Data Factory** jak je uvedeno v hello následující bitové kopie:
   
   ![Domovská stránka objektu pro vytváření dat](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-home-page.png)

## <a name="launch-copy-wizard"></a>Spuštění průvodce kopírováním
1. V okně hello objekt pro vytváření dat, klikněte na tlačítko **kopírování dat [PREVIEW]** toolaunch hello **Průvodce kopírováním**. 
   
   > [!NOTE]
   > Pokud se zobrazí, že hello webový prohlížeč zasekl ve fázi "autorizace …", zakažte/zrušte zaškrtnutí políčka **blokovat soubory cookie třetích stran a data lokality** nastavení v nastavení prohlížeče hello (nebo) se povolit udržování a vytvořte výjimku pro  **Login.microsoftonline.com** a poté se pokuste spustit hello průvodce znovu.
2. V hello **vlastnosti** stránky:
   
   1. Jako **Název úlohy** zadejte **CopyFromBlobToAzureSql**.
   2. Zadejte **popis** (volitelné).
   3. Změna hello **datum a čas zahájení** a hello **datum a čas ukončení** tak, aby hello koncové datum je nastaví tootoday a spustí datum toofive dříve dnů.  
   4. Klikněte na **Další**.  
      
      ![Nástroj pro kopírování – stránka Vlastnosti](./media/data-factory-copy-data-wizard-tutorial/copy-tool-properties-page.png) 
3. Na hello **zdrojového úložiště dat** klikněte na tlačítko **Azure Blob Storage** dlaždici. Tato stránka toospecify hello zdrojového úložiště dat se používá pro úlohy kopie hello. 
   
    ![Nástroj pro kopírování – stránka zdrojového úložiště dat](./media/data-factory-copy-data-wizard-tutorial/copy-tool-source-data-store-page.png)
4. Na hello **zadejte účet úložiště Azure Blob hello** stránky:
   
   1. Do pole **Název propojené služby** zadejte **AzureStorageLinkedService**.
   2. Ujistěte se, že je pro položku **Metoda výběru účtu** vybrána možnost **Z předplatných Azure**.
   3. Vyberte své **předplatné** Azure.  
   4. Vyberte **účtu úložiště Azure** z hello účty k dispozici v rámci předplatného hello vybraný seznam úložiště Azure. Můžete také tooenter nastavení účtu úložiště ručně tak, že vyberete **zadat ručně** možnost pro hello **účet metodu výběru**a potom klikněte na **Další**. 
      
      ![Nástroj pro kopírování – zadání účtu Azure Blob storage hello](./media/data-factory-copy-data-wizard-tutorial/copy-tool-specify-azure-blob-storage-account.png)
5. Na **zvolte hello vstupního souboru nebo složky** stránky:
   
   1. Klikněte dvakrát na **adftutorial** (složka).
   2. Vyberte soubor **emp.txt** a klikněte na **Vybrat**.
      
      ![Nástroj pro kopírování – volba hello vstupního souboru nebo složky](./media/data-factory-copy-data-wizard-tutorial/copy-tool-choose-input-file-or-folder.png)
6. Na hello **zvolte hello vstupního souboru nebo složky** klikněte na tlačítko **Další**. Nezaškrtávejte políčko **Binární kopie**. 
   
    ![Nástroj pro kopírování – volba hello vstupního souboru nebo složky](./media/data-factory-copy-data-wizard-tutorial/chose-input-file-folder.png) 
7. Na hello **nastavení formátu souboru** stránky, zobrazí hello oddělovače a hello schématu, která je automaticky nalezeny průvodcem hello analýzou soubor hello. Můžete také zadat hello oddělovače ručně hello kopie Průvodce toostop auto zjišťování nebo toooverride. Klikněte na tlačítko **Další** po zkontrolujte hello oddělovače a zobrazte náhled dat. 
   
    ![Nástroj pro kopírování – nastavení formátu souboru](./media/data-factory-copy-data-wizard-tutorial/copy-tool-file-format-settings.png)  
8. Na cílovém data hello uložení stránky, vyberte **Azure SQL Database**a klikněte na tlačítko **Další**.
   
    ![Nástroj pro kopírování – volba cílového úložiště](./media/data-factory-copy-data-wizard-tutorial/choose-destination-store.png)
9. Na **zadejte hello Azure SQL database** stránky:
   
   1. Zadejte **AzureSqlLinkedService** pro hello **název připojení** pole.
   2. Ujistěte se, že je pro položku **Metoda výběru serveru/databáze** vybrána možnost **Z předplatných Azure**.
   3. Vyberte své **předplatné** Azure.  
   4. Vyberte možnosti v polích **Název serveru** a **Databáze**.
   5. Zadejte **Uživatelské jméno** a **Heslo**.
   6. Klikněte na **Další**.  
      
      ![Nástroj pro kopírování – určení Azure SQL Database](./media/data-factory-copy-data-wizard-tutorial/specify-azure-sql-database.png)
10. Na hello **mapování tabulky** vyberte **emp** pro hello **cílové** pole hello rozevíracího seznamu, klikněte na tlačítko **šipka dolů** (volitelné) toosee hello schéma a toopreview hello data.
    
     ![Nástroj pro kopírování – mapování tabulek](./media/data-factory-copy-data-wizard-tutorial/copy-tool-table-mapping-page.png) 
11. Na hello **schéma mapování** klikněte na tlačítko **Další**.
    
    ![Nástroj pro kopírování – mapování schématu](./media/data-factory-copy-data-wizard-tutorial/schema-mapping-page.png)
12. Na hello **nastavení výkonu** klikněte na tlačítko **Další**. 
    
    ![Nástroj pro kopírování – nastavení výkonu](./media/data-factory-copy-data-wizard-tutorial/performance-settings.png)
13. Zkontrolujte informace v hello **Souhrn** a klikněte na tlačítko **Dokončit**. Hello Průvodce vytvoří dvě propojené služby, dvě datové sady (vstupní a výstupní) a jeden kanál v objektu pro vytváření dat hello (z kde spouštěna hello Průvodce kopírováním). 
    
    ![Nástroj pro kopírování – nastavení výkonu](./media/data-factory-copy-data-wizard-tutorial/summary-page.png)

## <a name="launch-monitor-and-manage-application"></a>Spuštění aplikace pro monitorování a správu
1. Na hello **nasazení** klikněte na odkaz hello: `Click here toomonitor copy pipeline`.
   
   ![Nástroj pro kopírování – nasazení bylo úspěšné](./media/data-factory-copy-data-wizard-tutorial/copy-tool-deployment-succeeded.png)  
2. monitorování aplikace Hello se spustí na samostatné kartě ve webovém prohlížeči.   
   
   ![Monitorovací aplikace](./media/data-factory-copy-data-wizard-tutorial/monitoring-app.png)   
3. toosee hello poslední stav hodinové řezů, klikněte na tlačítko **aktualizovat** tlačítka na hello **aktivity WINDOWS** seznam v dolní části hello. Uvidíte pět aktivity windows pět dní mezi počáteční a koncový čas pro kanál hello. seznam Hello automaticky neobnoví, tak mohou potřebovat tooclick aktualizovat několik doby, než uvidíte všechny systémy windows hello aktivity ve stavu Připraveno hello. 
4. Okno s aktivity vyberte v seznamu hello. Zobrazit podrobnosti hello o ho v hello **aktivity okno Průzkumníka** na hello správné.

    ![Podrobnosti o okně aktivity](media/data-factory-copy-data-wizard-tutorial/activity-window-details.png)    

    Všimněte si, že jsou data hello 11, 12, 13, 14 nebo 15 v zelenou barvu, která znamená, že hello denní výstup řezy pro těmito daty už vytvořily. Můžete také najdete v části barevné kódování na hello kanálu a hello výstupní datovou sadu v zobrazení diagramu hello. V předchozím kroku hello Všimněte si, že už vytvořily dva řezy, jeden řez je právě zpracovávána, a hello další dvě čekají toobe zpracovat (podle hello barevné kódování). 

    Další informace o používání této aplikace najdete v článku [Monitorování a správa kanálu pomocí monitorovací aplikace](data-factory-monitor-manage-app.md).

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat. Hello následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje a cíle pomocí aktivity kopírování hello: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Podrobnosti o pole nebo vlastnosti, které se zobrazí v Průvodci kopírováním hello pro úložiště dat klikněte na odkaz hello hello úložiště dat v tabulce hello. 
