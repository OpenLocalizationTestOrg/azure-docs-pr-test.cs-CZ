---
title: "problémy s Azure Data Factory aaaTroubleshoot"
description: "Zjistěte, jak tootroubleshoot problémy s používáním Azure Data Factory."
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
ms.openlocfilehash: cf65bcf3e1c3f061d3ac1dbf32e99cc7b014f9dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-data-factory-issues"></a><span data-ttu-id="38803-103">Poradce při potížích se službou Data Factory</span><span class="sxs-lookup"><span data-stu-id="38803-103">Troubleshoot Data Factory issues</span></span>
<span data-ttu-id="38803-104">Tento článek obsahuje tipy k řešení potíží pro problémy při použití Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="38803-104">This article provides troubleshooting tips for issues when using Azure Data Factory.</span></span> <span data-ttu-id="38803-105">V tomto článku nejsou uvedeny všechny hello možných problémů při používání služby hello, týká se však některé problémy a Obecné tipy k řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="38803-105">This article does not list all hello possible issues when using hello service, but it covers some issues and general troubleshooting tips.</span></span>   

## <a name="troubleshooting-tips"></a><span data-ttu-id="38803-106">Rady pro řešení potíží</span><span class="sxs-lookup"><span data-stu-id="38803-106">Troubleshooting tips</span></span>
### <a name="error-hello-subscription-is-not-registered-toouse-namespace-microsoftdatafactory"></a><span data-ttu-id="38803-107">Chyba: hello předplatné není registrované toouse oboru názvů 'Microsoft.DataFactory.</span><span class="sxs-lookup"><span data-stu-id="38803-107">Error: hello subscription is not registered toouse namespace 'Microsoft.DataFactory'</span></span>
<span data-ttu-id="38803-108">Pokud se zobrazí tato chyba, poskytovatel prostředků Azure Data Factory hello nebyl registrován na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="38803-108">If you receive this error, hello Azure Data Factory resource provider has not been registered on your machine.</span></span> <span data-ttu-id="38803-109">Hello následující:</span><span class="sxs-lookup"><span data-stu-id="38803-109">Do hello following:</span></span>

1. <span data-ttu-id="38803-110">Spusťte Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="38803-110">Launch Azure PowerShell.</span></span>
2. <span data-ttu-id="38803-111">Přihlaste se tooyour účet Azure pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="38803-111">Log in tooyour Azure account using hello following command.</span></span>

    ```powershell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="38803-112">Spusťte následující příkaz tooregister hello Azure Data Factory zprostředkovatele hello.</span><span class="sxs-lookup"><span data-stu-id="38803-112">Run hello following command tooregister hello Azure Data Factory provider.</span></span>

    ```powershell        
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a><span data-ttu-id="38803-113">Problém: Neoprávněným Chyba při spuštění rutiny služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="38803-113">Problem: Unauthorized error when running a Data Factory cmdlet</span></span>
<span data-ttu-id="38803-114">Používáte pravděpodobně není pravé hello účet Azure nebo předplatné s hello prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="38803-114">You are probably not using hello right Azure account or subscription with hello Azure PowerShell.</span></span> <span data-ttu-id="38803-115">Pomocí následující rutiny tooselect hello vpravo Azure účet a předplatné toouse s hello prostředí Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="38803-115">Use hello following cmdlets tooselect hello right Azure account and subscription toouse with hello Azure PowerShell.</span></span>

1. <span data-ttu-id="38803-116">Login-AzureRmAccount - použití hello správné uživatelské ID a heslo</span><span class="sxs-lookup"><span data-stu-id="38803-116">Login-AzureRmAccount - Use hello right user ID and password</span></span>
2. <span data-ttu-id="38803-117">Get-AzureRmSubscription - zobrazení všech hello předplatná pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="38803-117">Get-AzureRmSubscription - View all hello subscriptions for hello account.</span></span>
3. <span data-ttu-id="38803-118">Select-AzureRmSubscription &lt;název odběru&gt; -vyberte správné předplatné hello.</span><span class="sxs-lookup"><span data-stu-id="38803-118">Select-AzureRmSubscription &lt;subscription name&gt; - Select hello right subscription.</span></span> <span data-ttu-id="38803-119">Použití hello stejný jako ten, použijte toocreate objekt pro vytváření dat na hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="38803-119">Use hello same one you use toocreate a data factory on hello Azure portal.</span></span>

### <a name="problem-fail-toolaunch-data-management-gateway-express-setup-from-azure-portal"></a><span data-ttu-id="38803-120">Problém: Selhání toolaunch Data Management Gateway Express instalace z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="38803-120">Problem: Fail toolaunch Data Management Gateway Express Setup from Azure portal</span></span>
<span data-ttu-id="38803-121">Hello Expresní instalace pro hello Brána pro správu dat vyžaduje Internet Explorer nebo Microsoft ClickOnce kompatibilní webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="38803-121">hello Express setup for hello Data Management Gateway requires Internet Explorer or a Microsoft ClickOnce compatible web browser.</span></span> <span data-ttu-id="38803-122">Pokud hello Expresní instalace selže toostart, proveďte jednu z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="38803-122">If hello Express Setup fails toostart, do one of hello following:</span></span>

* <span data-ttu-id="38803-123">Použijte Internet Explorer nebo Microsoft ClickOnce kompatibilní webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="38803-123">Use Internet Explorer or a Microsoft ClickOnce compatible web browser.</span></span>

    <span data-ttu-id="38803-124">Pokud používáte Chrome, přejděte toohello [internetového obchodu Chrome](https://chrome.google.com/webstore/), hledání with – klíčové slovo "ClickOnce", vyberte jedno z rozšíření hello ClickOnce a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="38803-124">If you are using Chrome, go toohello [Chrome web store](https://chrome.google.com/webstore/), search with "ClickOnce" keyword, choose one of hello ClickOnce extensions, and install it.</span></span>

    <span data-ttu-id="38803-125">Hello stejné pro Firefox (instalace doplňku).</span><span class="sxs-lookup"><span data-stu-id="38803-125">Do hello same for Firefox (install add-in).</span></span> <span data-ttu-id="38803-126">Klikněte na tlačítko Otevřít nabídku na panelu nástrojů hello (tři vodorovné čáry v pravém horním rohu hello), klikněte na tlačítko Doplňky, hledání with – klíčové slovo "ClickOnce", vyberte jedno z rozšíření ClickOnce hello a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="38803-126">Click Open Menu button on hello toolbar (three horizontal lines in hello top-right corner), click Add-ons, search with "ClickOnce" keyword, choose one of hello ClickOnce extensions, and install it.</span></span>
* <span data-ttu-id="38803-127">Použití hello **ruční instalace** odkazu je zobrazen na hello stejné okno hello portálu.</span><span class="sxs-lookup"><span data-stu-id="38803-127">Use hello **Manual Setup** link shown on hello same blade in hello portal.</span></span> <span data-ttu-id="38803-128">Použijte tento postup toodownload instalační soubor a spustit ručně.</span><span class="sxs-lookup"><span data-stu-id="38803-128">You use this approach toodownload installation file and run it manually.</span></span> <span data-ttu-id="38803-129">Po úspěšné instalaci hello se zobrazí dialogové okno Konfigurace brány pro správu dat hello.</span><span class="sxs-lookup"><span data-stu-id="38803-129">After hello installation is successful, you see hello Data Management Gateway Configuration dialog box.</span></span> <span data-ttu-id="38803-130">Kopírování hello **klíč** z úvodní obrazovka portálu a použít ho v hello configuration manager toomanually registraci brány hello službou hello.</span><span class="sxs-lookup"><span data-stu-id="38803-130">Copy hello **key** from hello portal screen and use it in hello configuration manager toomanually register hello gateway with hello service.</span></span>  

### <a name="problem-fail-tooconnect-tooon-premises-sql-server"></a><span data-ttu-id="38803-131">Problém: Selhání tooconnect tooon místní systém SQL Server</span><span class="sxs-lookup"><span data-stu-id="38803-131">Problem: Fail tooconnect tooon-premises SQL Server</span></span>
<span data-ttu-id="38803-132">Spusťte **Správce konfigurace brány pro správu dat** hello počítač brány a použít hello **Poradce při potížích s** kartě tootest hello připojení tooSQL serveru z počítače brány hello.</span><span class="sxs-lookup"><span data-stu-id="38803-132">Launch **Data Management Gateway Configuration Manager** on hello gateway machine and use hello **Troubleshooting** tab tootest hello connection tooSQL Server from hello gateway machine.</span></span> <span data-ttu-id="38803-133">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.</span><span class="sxs-lookup"><span data-stu-id="38803-133">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>   

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a><span data-ttu-id="38803-134">Problém: Vstupní řezy jsou ve stavu čekání pro někdy</span><span class="sxs-lookup"><span data-stu-id="38803-134">Problem: Input slices are in Waiting state for ever</span></span>
<span data-ttu-id="38803-135">Hello řezy může být v **čekání** stavu z důvodu toovarious důvodů.</span><span class="sxs-lookup"><span data-stu-id="38803-135">hello slices could be in **Waiting** state due toovarious reasons.</span></span> <span data-ttu-id="38803-136">Jednou z běžných příčin hello je tento hello **externí** příliš není nastavena vlastnost**true**.</span><span class="sxs-lookup"><span data-stu-id="38803-136">One of hello common reasons is that hello **external** property is not set too**true**.</span></span> <span data-ttu-id="38803-137">Všechny datovou sadu, která je vytvořené mimo hello oboru objektu pro vytváření dat Azure by měl být označené jako **externí** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="38803-137">Any dataset that is produced outside hello scope of Azure Data Factory should be marked with **external** property.</span></span> <span data-ttu-id="38803-138">Tato vlastnost označuje, že hello data je externí a nejsou zálohovány pomocí žádné kanály v rámci objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="38803-138">This property indicates that hello data is external and not backed by any pipelines within hello data factory.</span></span> <span data-ttu-id="38803-139">Hello datové řezy jsou označeny jako **připraven** po hello dat je k dispozici v úložišti příslušných hello.</span><span class="sxs-lookup"><span data-stu-id="38803-139">hello data slices are marked as **Ready** once hello data is available in hello respective store.</span></span>

<span data-ttu-id="38803-140">Viz následující ukázka pro použití hello hello hello **externí** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="38803-140">See hello following example for hello usage of hello **external** property.</span></span> <span data-ttu-id="38803-141">Volitelně můžete zadat **externalData*** Pokud nastavíte externí tootrue.</span><span class="sxs-lookup"><span data-stu-id="38803-141">You can optionally specify **externalData*** when you set external tootrue.</span></span>

<span data-ttu-id="38803-142">V tématu [datové sady](data-factory-create-datasets.md) článku Další podrobnosti o této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="38803-142">See [Datasets](data-factory-create-datasets.md) article for more details about this property.</span></span>

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

<span data-ttu-id="38803-143">tooresolve hello chyba, přidejte hello **externí** vlastnost a hello volitelné **externalData** části definici JSON toohello hello vstupní tabulky a znovu vytvořte tabulku hello.</span><span class="sxs-lookup"><span data-stu-id="38803-143">tooresolve hello error, add hello **external** property and hello optional **externalData** section toohello JSON definition of hello input table and recreate hello table.</span></span>

### <a name="problem-hybrid-copy-operation-fails"></a><span data-ttu-id="38803-144">Problém: Operace kopírování hybridní selže</span><span class="sxs-lookup"><span data-stu-id="38803-144">Problem: Hybrid copy operation fails</span></span>
<span data-ttu-id="38803-145">V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) pro kroky tootroubleshoot problémy s kopírování do nebo z místních dat úložiště pomocí hello Brána pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="38803-145">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for steps tootroubleshoot issues with copying to/from an on-premises data store using hello Data Management Gateway.</span></span>

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a><span data-ttu-id="38803-146">Problém: HDInsight na vyžádání zřizování nezdaří</span><span class="sxs-lookup"><span data-stu-id="38803-146">Problem: On-demand HDInsight provisioning fails</span></span>
<span data-ttu-id="38803-147">Pokud používáte propojené služby typu HDInsightOnDemand, je nutné toospecify linkedServiceName, který odkazuje tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="38803-147">When using a linked service of type HDInsightOnDemand, you need toospecify a linkedServiceName that points tooan Azure Blob Storage.</span></span> <span data-ttu-id="38803-148">Služba data Factory používá tento protokol toostore úložiště a podpůrné soubory pro váš cluster HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="38803-148">Data Factory service uses this storage toostore logs and supporting files for your on-demand HDInsight cluster.</span></span>  <span data-ttu-id="38803-149">Někdy zřizování clusteru HDInsight na vyžádání selže s touto hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="38803-149">Sometimes provisioning of an on-demand HDInsight cluster fails with hello following error:</span></span>

```
Failed toocreate cluster. Exception: Unable toocomplete hello cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.
```

<span data-ttu-id="38803-150">Tato chyba obvykle označuje, že není v hello hello umístění účtu úložiště hello zadaný v hello linkedServiceName stejném datovém centru umístění, kde se děje hello HDInsight zřizování.</span><span class="sxs-lookup"><span data-stu-id="38803-150">This error usually indicates that hello location of hello storage account specified in hello linkedServiceName is not in hello same data center location where hello HDInsight provisioning is happening.</span></span> <span data-ttu-id="38803-151">Příklad: Pokud je objekt pro vytváření dat v západní USA a hello úložiště Azure je v oblasti Východ USA, hello zřizování selže na vyžádání v západní USA.</span><span class="sxs-lookup"><span data-stu-id="38803-151">Example: if your data factory is in West US and hello Azure storage is in East US, hello on-demand provisioning fails in West US.</span></span>

<span data-ttu-id="38803-152">Navíc existuje ještě druhá vlastnost additionalLinkedServiceNames JSON, která umožňuje zadat další účty úložiště v HDInsightu na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="38803-152">Additionally, there is a second JSON property additionalLinkedServiceNames where additional storage accounts may be specified in on-demand HDInsight.</span></span> <span data-ttu-id="38803-153">Tyto další propojené účty úložiště by měla být v hello stejné umístění jako hello HDInsight cluster, nebo selže s hello stejné chybě.</span><span class="sxs-lookup"><span data-stu-id="38803-153">Those additional linked storage accounts should be in hello same location as hello HDInsight cluster, or it fails with hello same error.</span></span>

### <a name="problem-custom-net-activity-fails"></a><span data-ttu-id="38803-154">Problém: Vlastní selže aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="38803-154">Problem: Custom .NET activity fails</span></span>
<span data-ttu-id="38803-155">V tématu [ladění kanálu s aktivitou vlastní](data-factory-use-custom-activities.md#troubleshoot-failures) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="38803-155">See [Debug a pipeline with custom activity](data-factory-use-custom-activities.md#troubleshoot-failures) for detailed steps.</span></span>

## <a name="use-azure-portal-tootroubleshoot"></a><span data-ttu-id="38803-156">Použití portálu Azure tootroubleshoot</span><span class="sxs-lookup"><span data-stu-id="38803-156">Use Azure portal tootroubleshoot</span></span>
### <a name="using-portal-blades"></a><span data-ttu-id="38803-157">Pomocí portálu oken</span><span class="sxs-lookup"><span data-stu-id="38803-157">Using portal blades</span></span>
<span data-ttu-id="38803-158">V tématu [monitorování kanálu](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) kroky.</span><span class="sxs-lookup"><span data-stu-id="38803-158">See [Monitor pipeline](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) for steps.</span></span>

### <a name="using-monitor-and-manage-app"></a><span data-ttu-id="38803-159">Pomocí aplikace Monitorování a správa</span><span class="sxs-lookup"><span data-stu-id="38803-159">Using Monitor and Manage App</span></span>
<span data-ttu-id="38803-160">V tématu [monitorování a Správa kanálů služby data factory pomocí monitorování a Správa aplikací](data-factory-monitor-manage-app.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="38803-160">See [Monitor and manage data factory pipelines using Monitor and Manage App](data-factory-monitor-manage-app.md) for details.</span></span>

## <a name="use-azure-powershell-tootroubleshoot"></a><span data-ttu-id="38803-161">Použití Azure PowerShell tootroubleshoot</span><span class="sxs-lookup"><span data-stu-id="38803-161">Use Azure PowerShell tootroubleshoot</span></span>
### <a name="use-azure-powershell-tootroubleshoot-an-error"></a><span data-ttu-id="38803-162">Použití Azure PowerShell tootroubleshoot chybu</span><span class="sxs-lookup"><span data-stu-id="38803-162">Use Azure PowerShell tootroubleshoot an error</span></span>
<span data-ttu-id="38803-163">V tématu [kanálů monitorování pro vytváření dat pomocí Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="38803-163">See [Monitor Data Factory pipelines using Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) for details.</span></span>

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
