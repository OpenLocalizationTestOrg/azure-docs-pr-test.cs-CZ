---
title: "aaaAzure objekt pro vytváření dat – ukázky"
description: "Poskytuje podrobnosti o vzorků, které jsou součástí hello služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: c0538b90-2695-4c4c-a6c8-82f59111f4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: aa1c15eca21b34b7bcc64358b685d7606baaf691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---samples"></a>Pro vytváření dat Azure – ukázky
## <a name="samples-on-github"></a>Ukázky z webu GitHub
Hello [úložiště GitHub Azure-DataFactory](https://github.com/azure/azure-datafactory) obsahuje několik vzorků, které vám pomohou rychle doba přípravy službou Azure Data Factory (nebo) upravit hello skripty a použít ho v vlastní aplikace. Hello Samples\JSON složka obsahuje fragmenty kódu JSON pro běžné scénáře.

| Ukázka | Popis |
|:--- |:--- |
| [Návod ADF](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) |Tato ukázka poskytuje návod začátku do konce pro zpracování souborů protokolů pomocí Azure Data Factory tooturn dat ze souborů protokolů v tooinsights. <br/><br/>V tomto návodu kanál služby Data Factory hello shromažďuje protokoly ukázkové, procesy a vylepšuje hello dat z protokolů u referenčních dat a transformuje hello data tooevaluate hello účinnosti marketingovou kampaň, která byla nedávno spuštěná. |
| [JSON – ukázky](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) |Tato ukázka obsahuje příklady JSON pro běžné scénáře. |
| [Stažení programu vzorek dat protokolu HTTP](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) |Tento ukázkový server Showcase stahování dat z tooAzure koncový bod HTTP Blob Storage pomocí vlastní aktivity .NET. |
| [Mezi ukázka Net aktivity AppDomain tečku](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |Tato ukázka vám umožní tooauthor vlastní aktivity rozhraní .NET, která není omezené tooassembly verze používané hello ADF Spouštěče (například WindowsAzure.Storage v4.3.0 Newtonsoft.Json v6.0.x, atd.). |
| [Spustit skript jazyka R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |Tato ukázka obsahuje objekt pro vytváření dat hello vlastní se aktivity, které můžou být použité tooinvoke RScript.exe. Tato ukázka funguje pouze s vlastní cluster HDInsight (ne na vyžádání), který již má nainstalované R na. |
| [Vyvolání úlohy Spark v HDInsight Hadoop clusteru](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) |Tento příklad ukazuje, jak toouse MapReduce aktivity tooinvoke Spark program. Hello spark program právě zkopíruje data z jednoho tooanother kontejner objektů Blob v Azure. |
| [Analýza Twitter pomocí Azure Machine Learning dávkové vyhodnocování aktivity](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) |Tento příklad ukazuje, jak toouse AzureMLBatchScoringActivity tooinvoke Azure Machine Learning modelu, který provede analýzu postojích twitter, vyhodnocování předpovědi atd. |
| [Analýza Twitter pomocí vlastní aktivity](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |Tento příklad ukazuje, jak toouse vlastní aktivity tooinvoke .NET model Azure Machine Learning, který provádí twitter postojích analýzy, vyhodnocování předpovědi atd. |
| [Parametrizované kanály pro Azure Machine Learning](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) |Ukázka Hello poskytuje začátku do konce jazyka C# kódu toodeploy N kanály pro vyhodnocování a každý parametr jiné oblasti, kde hello seznam oblastí pochází z parameters.txt souboru, který je součástí této ukázce přeučování. |
| [Referenční dokumentace aktualizace dat pro úlohy Azure Stream Analytics](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |Tento příklad ukazuje, jak toouse Azure Data Factory a Azure Stream Analytics společně toorun hello dotazy s hello referenční data a nastavení aktualizace pro referenční data podle plánu. |
| [Hybridní kanál s místními Hortonworks Hadoop](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) |Ukázka Hello používá místního clusteru Hadoop jako cíl výpočetní ke spuštění úloh v objektu pro vytváření dat, stejně jako HDInsight na základě clusteru Hadoop v cloudu, měli byste přidat další výpočetní cíle. |
| [Převodní nástroj JSON](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) |Tento nástroj umožňuje tooconvert JSONs z předchozí toolatest too2015-07-01-preview verze nebo 2015-07-01-preview (výchozí). |
| [Vstupní soubor ukázka U-SQL](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |Tento soubor je ukázkový soubor používaná aktivitou U-SQL. |
| [Odstranění objektů blob souboru](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity) | Tato ukázka umožňující prezentovat soubor C#, který můžete použít jako součást ADF vlastních .net aktivity toodelete souborů ze zdroje hello umístění objektu Blob Azure, jakmile hello soubory byly zkopírovány.|

## <a name="azure-resource-manager-templates"></a>Šablony Azure Resource Manageru
Můžete najít hello následující šablon Azure Resource Manageru pro vytváření dat na Githubu.

| Šablona | Popis |
| --- | --- |
| [Zkopírujte z Azure Blob Storage tooAzure databáze SQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) |Nasazení Tato šablona vytvoří objekt pro vytváření dat Azure s kanál, kopíruje data z hello zadat databázi Azure SQL toohello úložiště objektů blob v Azure |
| [Kopírování ze služby Salesforce tooAzure úložiště objektů Blob](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) |Nasazení této šablony vytvoří objekt pro vytváření dat Azure s kanál, kopíruje data z hello zadaný úložiště objektů blob Azure toohello účtu Salesforce. |
| [Transformace dat pomocí skriptu Hive v clusteru Azure HDInsight](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) |Nasazení Tato šablona vytvoří objekt pro vytváření dat Azure s kanál, který transformuje data pomocí skriptu hello ukázka Hive v clusteru Azure HDInsight Hadoop. |

## <a name="samples-in-azure-portal"></a>Ukázky na portálu Azure
Můžete použít hello **ukázkové kanály** dlaždici na domovskou stránku hello kanály ukázka toodeploy objekt pro vytváření dat a jejich přidružených entit (datové sady a propojených služeb) v tooyour data factory.

1. Objekt pro vytváření dat vytvořit nebo otevřít existující datovou továrnu. V tématu [kopírování dat z úložiště objektů Blob tooSQL databázi pomocí služby Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) pro kroky toocreate služby data factory.
2. V hello **DATA FACTORY** hello data Factory klikněte na hello **ukázkové kanály** dlaždici.

    ![Ukázka kanály dlaždice](./media/data-factory-samples/SamplePipelinesTile.png)
3. V hello **ukázkové kanály** okně klikněte na tlačítko hello **ukázka** , které chcete toodeploy.

    ![Okno ukázku kanálů](./media/data-factory-samples/SampleTile.png)
4. Zadejte nastavení konfigurace pro ukázku hello. Například váš klíč pro účet a název účtu úložiště Azure, název serveru Azure SQL, databáze, ID uživatele a heslo atd.

    ![Ukázka okna](./media/data-factory-samples/SampleBlade.png)
5. Jakmile jste hotovi s určujícím hello konfigurační nastavení, klikněte na tlačítko **vytvořit** toocreate nebo nasazení hello ukázka kanálů a propojené služby nebo tabulek používané hello kanály.
6. Hello stav nasazení se zobrazí na dlaždici ukázka hello dříve jste klikli na hello **ukázkové kanály** okno.

    ![Stav nasazení](./media/data-factory-samples/DeploymentStatus.png)
7. Až se zobrazí hello **nasazení bylo úspěšné** zprávu na dlaždici hello hello ukázce zavřít hello **ukázkové kanály** okno.  
8. Na **DATA FACTORY** okně uvidíte propojených služeb, datových sad a kanálů přidají tooyour data factory.  

    ![Okno Objekt pro vytváření dat](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="samples-in-visual-studio"></a>V sadě Visual Studio – ukázky
### <a name="prerequisites"></a>Požadavky
Musíte mít nainstalované ve vašem počítači hello tyto položky:

* Visual Studio 2013 nebo Visual Studio 2015.
* Stáhněte si sadu Azure SDK pro Visual Studio 2013 nebo Visual Studio 2015. Přejděte příliš[stránku položek ke stažení Azure](https://azure.microsoft.com/downloads/) a klikněte na tlačítko **VS 2013** nebo **VS 2015** v hello **.NET** části.
* Stáhnout hello nejnovější modul plug-in Azure Data Factory pro Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) nebo [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Pokud používáte Visual Studio 2013, můžete také aktualizovat modul plug-in hello provedením následujících kroků hello: hello nabídce klikněte na **nástroje** -> **rozšíření a aktualizace**  ->  **Online** -> **Galerie sady Visual Studio** -> **Microsoft Azure Data Factory Tools for Visual Studio**  ->  **Aktualizace**.

### <a name="use-data-factory-templates"></a>Použití šablon objekt pro vytváření dat
1. Klikněte na tlačítko **soubor** v nabídce hello bodu příliš**nový**a klikněte na tlačítko **projektu**.
2. V hello **nový projekt** dialogové okno pole, hello následující kroky:

   1. Vyberte **DataFactory** pod **šablony**.
   2. Vyberte **šablony objekt pro vytváření dat** v pravém podokně hello.
   3. Zadejte **název** pro projekt hello.
   4. Vyberte **umístění** pro projekt hello.
   5. Klikněte na **OK**.

      ![Dialogové okno Nový projekt](./media/data-factory-samples/vs-new-project-adf-templates.png)
3. V hello **šablony objekt pro vytváření dat** dialogové okno, vyberte hello vzorové šablony z hello **případ použití šablony** a klikněte na **Další**. Hello následující kroky vás provedou pomocí hello **odběratele profilace** šablony. Kroky jsou podobné pro hello Další ukázky.

    ![Dialogové okno šablony objekt pro vytváření dat](./media/data-factory-samples/vs-data-factory-templates-dialog.png)
4. V hello **konfigurace objektu pro vytváření dat** dialogové okno, klikněte na tlačítko **Další** na hello **základy objektu pro vytváření dat** stránky.
5. Na hello **objekt pro vytváření dat konfigurace** stránky, hello následující kroky:
   1. Vyberte **vytvořit nový objekt pro vytváření dat**. Můžete také vybrat **použít existující datovou továrnu**.
   2. Zadejte **název** hello data Factory.
   3. Vyberte hello **předplatného Azure** do kterých chcete hello data factory toobe vytvořili.
   4. Vyberte hello **skupiny prostředků** hello data Factory.
   5. Vyberte hello **západní USA**, **východní USA**, nebo **Severní Evropa** pro hello **oblast**.
   6. Klikněte na **Další**.
6. V hello **konfigurace úložiště dat** zadejte existující **Azure SQL database** a **účtu úložiště Azure** (nebo) vytvořte databáze nebo úložiště a klikněte na tlačítko Další.
7. V hello **konfigurace výpočetních** vyberte výchozí hodnoty a klikněte na tlačítko **Další**.
8. V hello **Souhrn** zkontrolujte všechna nastavení a klikněte na tlačítko **Další**.
9. V hello **stav nasazení** stránky, počkejte na dokončení hello nasazení a klikněte na tlačítko **Dokončit**.
10. Klikněte pravým tlačítkem na projekt v Průzkumníku řešení hello a klikněte na tlačítko **publikovat**.
11. Pokud se zobrazí **přihlášení tooyour účtu Microsoft** dialogové okno, zadejte svoje přihlašovací údaje pro hello účet, který má předplatné Azure a klikněte na tlačítko **přihlášení**.
12. Měli byste vidět hello následující dialogové okno:

    ![Dialogové okno publikování](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
13. V hello **objekt pro vytváření dat konfigurace** stránky, hello následující kroky:

    1. Potvrďte, že **použít existující datovou továrnu** možnost.
    2. Vyberte hello **objekt pro vytváření dat** měl vyberte při použití šablony hello.
    3. Klikněte na tlačítko **Další** tooswitch toohello **publikování položky** stránky. (Stisknutím klávesy **KARTĚ** toomove mimo hello název pole tooif hello **Další** tlačítko je zakázané.)
14. V hello **publikování položky** stránky, ujistěte se, že všechny hello datové továrny entity jsou vybrané a klikněte na tlačítko **Další** tooswitch toohello **Souhrn** stránky.     
15. Zkontrolujte hello shrnutí a klikněte na **Další** toostart hello nasazení proces a zobrazení hello **stav nasazení**.
16. V hello **stav nasazení** stránky, měli byste vidět hello stav procesu nasazení hello. Po dokončení hello nasazení, klikněte na tlačítko Dokončit.

V tématu [vytvoření vaší první objekt pro vytváření dat (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) podrobnosti o použití entit služby Data Factory tooauthor Visual Studio a publikováním tooAzure.          
