---
title: "Řešení problémů s Azure Data Factory"
description: "Informace o řešení problémů s pomocí Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 38fd14c1-5bb7-4eef-a9f5-b289ff9a6942
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: spelluru
ms.openlocfilehash: 953a2703db7c8991f580a7c963d6cbd94265c213
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-data-factory-issues"></a><span data-ttu-id="417e4-103">Poradce při potížích se službou Data Factory</span><span class="sxs-lookup"><span data-stu-id="417e4-103">Troubleshoot Data Factory issues</span></span>
<span data-ttu-id="417e4-104">Tento článek obsahuje tipy k řešení potíží pro problémy při použití Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="417e4-104">This article provides troubleshooting tips for issues when using Azure Data Factory.</span></span> <span data-ttu-id="417e4-105">V tomto článku nejsou uvedeny všechny možné problémy při použití služby, týká se však některé problémy a Obecné tipy k řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="417e4-105">This article does not list all the possible issues when using the service, but it covers some issues and general troubleshooting tips.</span></span>   

## <a name="troubleshooting-tips"></a><span data-ttu-id="417e4-106">Rady pro řešení potíží</span><span class="sxs-lookup"><span data-stu-id="417e4-106">Troubleshooting tips</span></span>
### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a><span data-ttu-id="417e4-107">Chyba: Pro předplatné není zaregistrované používání oboru názvů Microsoft.DataFactory.</span><span class="sxs-lookup"><span data-stu-id="417e4-107">Error: The subscription is not registered to use namespace 'Microsoft.DataFactory'</span></span>
<span data-ttu-id="417e4-108">Pokud se zobrazí tato chyba, poskytovatel prostředků Azure Data Factory není na vašem počítači zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="417e4-108">If you receive this error, the Azure Data Factory resource provider has not been registered on your machine.</span></span> <span data-ttu-id="417e4-109">Udělejte toto:</span><span class="sxs-lookup"><span data-stu-id="417e4-109">Do the following:</span></span>

1. <span data-ttu-id="417e4-110">Spusťte Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="417e4-110">Launch Azure PowerShell.</span></span>
2. <span data-ttu-id="417e4-111">Přihlaste se k účtu Azure, pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="417e4-111">Log in to your Azure account using the following command.</span></span>

    ```powershell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="417e4-112">Spusťte následující příkaz pro registraci zprostředkovatele služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="417e4-112">Run the following command to register the Azure Data Factory provider.</span></span>

    ```powershell        
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a><span data-ttu-id="417e4-113">Problém: Neoprávněným Chyba při spuštění rutiny služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="417e4-113">Problem: Unauthorized error when running a Data Factory cmdlet</span></span>
<span data-ttu-id="417e4-114">Pravděpodobně pro Azure PowerShell nepoužíváte správné předplatné nebo účet Azure.</span><span class="sxs-lookup"><span data-stu-id="417e4-114">You are probably not using the right Azure account or subscription with the Azure PowerShell.</span></span> <span data-ttu-id="417e4-115">Pomocí následujících rutin vyberte správné předplatné a účet Azure pro použití s Azure PowerShellem.</span><span class="sxs-lookup"><span data-stu-id="417e4-115">Use the following cmdlets to select the right Azure account and subscription to use with the Azure PowerShell.</span></span>

1. <span data-ttu-id="417e4-116">Login-AzureRmAccount - použití správné uživatelské ID a heslo</span><span class="sxs-lookup"><span data-stu-id="417e4-116">Login-AzureRmAccount - Use the right user ID and password</span></span>
2. <span data-ttu-id="417e4-117">Get-AzureRmSubscription - zobrazte všechna předplatná pro účet.</span><span class="sxs-lookup"><span data-stu-id="417e4-117">Get-AzureRmSubscription - View all the subscriptions for the account.</span></span>
3. <span data-ttu-id="417e4-118">Select-AzureRmSubscription &lt;název odběru&gt; -vyberte správné předplatné.</span><span class="sxs-lookup"><span data-stu-id="417e4-118">Select-AzureRmSubscription &lt;subscription name&gt; - Select the right subscription.</span></span> <span data-ttu-id="417e4-119">Použijte stejný jako ten, který používáte pro vytváření dat na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="417e4-119">Use the same one you use to create a data factory on the Azure portal.</span></span>

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a><span data-ttu-id="417e4-120">Problém: Nepodařilo Data Management Gateway Express instalaci spustit z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="417e4-120">Problem: Fail to launch Data Management Gateway Express Setup from Azure portal</span></span>
<span data-ttu-id="417e4-121">Expresní instalace pro Bránu pro správu dat vyžaduje Internet Explorer nebo webový prohlížeč kompatibilní s technologií Microsoft ClickOnce.</span><span class="sxs-lookup"><span data-stu-id="417e4-121">The Express setup for the Data Management Gateway requires Internet Explorer or a Microsoft ClickOnce compatible web browser.</span></span> <span data-ttu-id="417e4-122">Pokud se expresní instalace nezdaří, použijte jeden z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="417e4-122">If the Express Setup fails to start, do one of the following:</span></span>

* <span data-ttu-id="417e4-123">Použijte Internet Explorer nebo Microsoft ClickOnce kompatibilní webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="417e4-123">Use Internet Explorer or a Microsoft ClickOnce compatible web browser.</span></span>

    <span data-ttu-id="417e4-124">Pokud používáte Chrome, přejděte na [internetový obchod Chrome](https://chrome.google.com/webstore/), dejte hledat klíčové slovo ClickOnce, vyberte některé z rozšíření ClickOnce a nainstalujte je.</span><span class="sxs-lookup"><span data-stu-id="417e4-124">If you are using Chrome, go to the [Chrome web store](https://chrome.google.com/webstore/), search with "ClickOnce" keyword, choose one of the ClickOnce extensions, and install it.</span></span>

    <span data-ttu-id="417e4-125">Totéž proveďte pro Firefox (instalace doplňku).</span><span class="sxs-lookup"><span data-stu-id="417e4-125">Do the same for Firefox (install add-in).</span></span> <span data-ttu-id="417e4-126">Na panelu nástrojů klikněte na tlačítko Otevřít nabídku (tři vodorovné čáry v pravém horním rohu), klikněte na Doplňky, dejte hledat klíčové slovo ClickOnce, vyberte některé z rozšíření ClickOnce a nainstalujte je.</span><span class="sxs-lookup"><span data-stu-id="417e4-126">Click Open Menu button on the toolbar (three horizontal lines in the top-right corner), click Add-ons, search with "ClickOnce" keyword, choose one of the ClickOnce extensions, and install it.</span></span>
* <span data-ttu-id="417e4-127">Použití **ruční instalace** odkazu je zobrazen v okně stejné na portálu.</span><span class="sxs-lookup"><span data-stu-id="417e4-127">Use the **Manual Setup** link shown on the same blade in the portal.</span></span> <span data-ttu-id="417e4-128">Tuto metodu použijte k stáhněte instalační soubor a spustit ručně.</span><span class="sxs-lookup"><span data-stu-id="417e4-128">You use this approach to download installation file and run it manually.</span></span> <span data-ttu-id="417e4-129">Po úspěšné instalaci, zobrazí se dialogové okno Konfigurace brány pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="417e4-129">After the installation is successful, you see the Data Management Gateway Configuration dialog box.</span></span> <span data-ttu-id="417e4-130">Zkopírujte **klíč** z okna portálu a použijte ho ve správci konfigurace k ruční registraci brány v příslušné službě.</span><span class="sxs-lookup"><span data-stu-id="417e4-130">Copy the **key** from the portal screen and use it in the configuration manager to manually register the gateway with the service.</span></span>  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a><span data-ttu-id="417e4-131">Problém: Nepodaří připojit k místnímu SQL serveru</span><span class="sxs-lookup"><span data-stu-id="417e4-131">Problem: Fail to connect to on-premises SQL Server</span></span>
<span data-ttu-id="417e4-132">Spusťte **Správce konfigurace brány pro správu dat** na počítači brány a použít **Poradce při potížích s** kartě, abyste otestovali připojení k systému SQL Server z počítače brány.</span><span class="sxs-lookup"><span data-stu-id="417e4-132">Launch **Data Management Gateway Configuration Manager** on the gateway machine and use the **Troubleshooting** tab to test the connection to SQL Server from the gateway machine.</span></span> <span data-ttu-id="417e4-133">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="417e4-133">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>   

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a><span data-ttu-id="417e4-134">Problém: Vstupní řezy jsou ve stavu čekání pro někdy</span><span class="sxs-lookup"><span data-stu-id="417e4-134">Problem: Input slices are in Waiting state for ever</span></span>
<span data-ttu-id="417e4-135">Řezy může být v **čekání** stavu z různých důvodů.</span><span class="sxs-lookup"><span data-stu-id="417e4-135">The slices could be in **Waiting** state due to various reasons.</span></span> <span data-ttu-id="417e4-136">Jeden z běžných příčin je, že **externí** vlastnost není nastavena na **true**.</span><span class="sxs-lookup"><span data-stu-id="417e4-136">One of the common reasons is that the **external** property is not set to **true**.</span></span> <span data-ttu-id="417e4-137">Žádné datové sady, která je vytvořena mimo obor Azure Data Factory by měl být označené jako **externí** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="417e4-137">Any dataset that is produced outside the scope of Azure Data Factory should be marked with **external** property.</span></span> <span data-ttu-id="417e4-138">Tato vlastnost určuje, že data jsou externí a nejsou zálohovány pomocí žádné kanály v rámci objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="417e4-138">This property indicates that the data is external and not backed by any pipelines within the data factory.</span></span> <span data-ttu-id="417e4-139">Jakmile jsou data v příslušných úložištích dostupná, datové řezy se označí jako **připravené**.</span><span class="sxs-lookup"><span data-stu-id="417e4-139">The data slices are marked as **Ready** once the data is available in the respective store.</span></span>

<span data-ttu-id="417e4-140">Použití vlastnosti **external** si můžete prohlédnout v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="417e4-140">See the following example for the usage of the **external** property.</span></span> <span data-ttu-id="417e4-141">Volitelně můžete zadat **externalData*** Pokud nastavíte externí na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="417e4-141">You can optionally specify **externalData*** when you set external to true.</span></span>

<span data-ttu-id="417e4-142">V tématu [datové sady](data-factory-create-datasets.md) článku Další podrobnosti o této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="417e4-142">See [Datasets](data-factory-create-datasets.md) article for more details about this property.</span></span>

```json
{
  "name": "CustomerTable",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "MyLinkedService",
    "typeProperties": {
      "folderPath": "MyContainer/MySubFolder/",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      }
    }
  }
}
```

<span data-ttu-id="417e4-143">Pokud chcete tuto chybu vyřešit, přidejte vlastnost **external** a volitelný oddíl **externalData** do definice JSON vstupní tabulky a potom tuto tabulku vytvořte znovu.</span><span class="sxs-lookup"><span data-stu-id="417e4-143">To resolve the error, add the **external** property and the optional **externalData** section to the JSON definition of the input table and recreate the table.</span></span>

### <a name="problem-hybrid-copy-operation-fails"></a><span data-ttu-id="417e4-144">Problém: Operace kopírování hybridní selže</span><span class="sxs-lookup"><span data-stu-id="417e4-144">Problem: Hybrid copy operation fails</span></span>
<span data-ttu-id="417e4-145">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kroky pro řešení problémů s kopírování do nebo z místních dat úložiště pomocí Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="417e4-145">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for steps to troubleshoot issues with copying to/from an on-premises data store using the Data Management Gateway.</span></span>

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a><span data-ttu-id="417e4-146">Problém: HDInsight na vyžádání zřizování nezdaří</span><span class="sxs-lookup"><span data-stu-id="417e4-146">Problem: On-demand HDInsight provisioning fails</span></span>
<span data-ttu-id="417e4-147">Pokud používáte propojené služby typu HDInsightOnDemand, budete muset zadat linkedServiceName, která odkazuje na Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="417e4-147">When using a linked service of type HDInsightOnDemand, you need to specify a linkedServiceName that points to an Azure Blob Storage.</span></span> <span data-ttu-id="417e4-148">Služba Data Factory využívá toto úložiště k ukládání protokolů a podpůrných souborů pro cluster HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="417e4-148">Data Factory service uses this storage to store logs and supporting files for your on-demand HDInsight cluster.</span></span>  <span data-ttu-id="417e4-149">Někdy se zřizování clusteru HDInsight na vyžádání nezdaří s následující chybou:</span><span class="sxs-lookup"><span data-stu-id="417e4-149">Sometimes provisioning of an on-demand HDInsight cluster fails with the following error:</span></span>

```
Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.
```

<span data-ttu-id="417e4-150">Tato zpráva obvykle indikuje, že umístění účtu úložiště zadané vlastností linkedServiceName se neshoduje s umístěním datového centra, ve kterém dochází ke zřizování HDInsightu.</span><span class="sxs-lookup"><span data-stu-id="417e4-150">This error usually indicates that the location of the storage account specified in the linkedServiceName is not in the same data center location where the HDInsight provisioning is happening.</span></span> <span data-ttu-id="417e4-151">Příklad: Pokud je objekt pro vytváření dat v západní USA a Azure storage je v oblasti Východ USA, na vyžádání zřizování nezdaří západní USA.</span><span class="sxs-lookup"><span data-stu-id="417e4-151">Example: if your data factory is in West US and the Azure storage is in East US, the on-demand provisioning fails in West US.</span></span>

<span data-ttu-id="417e4-152">Navíc existuje ještě druhá vlastnost additionalLinkedServiceNames JSON, která umožňuje zadat další účty úložiště v HDInsightu na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="417e4-152">Additionally, there is a second JSON property additionalLinkedServiceNames where additional storage accounts may be specified in on-demand HDInsight.</span></span> <span data-ttu-id="417e4-153">Tyto další propojené účty úložiště by měl být ve stejném umístění jako HDInsight cluster, nebo se nezdaří s ke stejné chybě.</span><span class="sxs-lookup"><span data-stu-id="417e4-153">Those additional linked storage accounts should be in the same location as the HDInsight cluster, or it fails with the same error.</span></span>

### <a name="problem-custom-net-activity-fails"></a><span data-ttu-id="417e4-154">Problém: Vlastní selže aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="417e4-154">Problem: Custom .NET activity fails</span></span>
<span data-ttu-id="417e4-155">V tématu [ladění kanálu s aktivitou vlastní](data-factory-use-custom-activities.md#troubleshoot-failures) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="417e4-155">See [Debug a pipeline with custom activity](data-factory-use-custom-activities.md#troubleshoot-failures) for detailed steps.</span></span>

## <a name="use-azure-portal-to-troubleshoot"></a><span data-ttu-id="417e4-156">Řešení potíží pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="417e4-156">Use Azure portal to troubleshoot</span></span>
### <a name="using-portal-blades"></a><span data-ttu-id="417e4-157">Pomocí portálu oken</span><span class="sxs-lookup"><span data-stu-id="417e4-157">Using portal blades</span></span>
<span data-ttu-id="417e4-158">V tématu [monitorování kanálu](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) kroky.</span><span class="sxs-lookup"><span data-stu-id="417e4-158">See [Monitor pipeline](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) for steps.</span></span>

### <a name="using-monitor-and-manage-app"></a><span data-ttu-id="417e4-159">Pomocí aplikace Monitorování a správa</span><span class="sxs-lookup"><span data-stu-id="417e4-159">Using Monitor and Manage App</span></span>
<span data-ttu-id="417e4-160">V tématu [monitorování a Správa kanálů služby data factory pomocí monitorování a Správa aplikací](data-factory-monitor-manage-app.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="417e4-160">See [Monitor and manage data factory pipelines using Monitor and Manage App](data-factory-monitor-manage-app.md) for details.</span></span>

## <a name="use-azure-powershell-to-troubleshoot"></a><span data-ttu-id="417e4-161">Pomocí prostředí Azure PowerShell k řešení potíží</span><span class="sxs-lookup"><span data-stu-id="417e4-161">Use Azure PowerShell to troubleshoot</span></span>
### <a name="use-azure-powershell-to-troubleshoot-an-error"></a><span data-ttu-id="417e4-162">Pomocí prostředí Azure PowerShell k řešení potíží s chybu</span><span class="sxs-lookup"><span data-stu-id="417e4-162">Use Azure PowerShell to troubleshoot an error</span></span>
<span data-ttu-id="417e4-163">V tématu [kanálů monitorování pro vytváření dat pomocí Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="417e4-163">See [Monitor Data Factory pipelines using Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) for details.</span></span>

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
