---
title: "aaaAnalyze dat Twitteru pomocí Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Hive tooanalyze Twitter data v Hadoop v HDInsight toofind hello využití frekvenci určité slovo."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 78e4ea33-9714-424d-ac07-3d60ecaebf2e
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 40c0a1afbc1fff10c070d22a99cd9d32d42f230a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a><span data-ttu-id="db672-103">Analýza dat Twitteru pomocí Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="db672-103">Analyze Twitter data using Hive in HDInsight</span></span>
<span data-ttu-id="db672-104">Sociální weby jsou jedním z hlavních řízení vynutí pro přijetí velkých objemů dat hello.</span><span class="sxs-lookup"><span data-stu-id="db672-104">Social websites are one of hello major driving forces for big-data adoption.</span></span> <span data-ttu-id="db672-105">Veřejná rozhraní API poskytované lokality jako Twitter jsou užitečné zdroje dat pro analýzu a pochopení oblíbených trendy.</span><span class="sxs-lookup"><span data-stu-id="db672-105">Public APIs provided by sites like Twitter are a useful source of data for analyzing and understanding popular trends.</span></span>
<span data-ttu-id="db672-106">V tomto kurzu budete získat tweetů pomocí Twitteru, streamování rozhraní API a potom pomocí Apache Hive v Azure HDInsight tooget seznam Twitter uživatelů, kteří odeslané hello většina tweetů, které obsahovaly určité slovo.</span><span class="sxs-lookup"><span data-stu-id="db672-106">In this tutorial, you will get tweets by using a Twitter streaming API, and then use Apache Hive on Azure HDInsight tooget a list of Twitter users who sent hello most tweets that contained a certain word.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="db672-107">Hello kroky v tomto dokumentu vyžadují cluster HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="db672-107">hello steps in this document require a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="db672-108">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="db672-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="db672-109">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="db672-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="db672-110">Kroky konkrétní tooa systémem Linux clusteru, najdete v části [Twitter analýza dat pomocí Hive v HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).</span><span class="sxs-lookup"><span data-stu-id="db672-110">For steps specific tooa Linux-based cluster, see [Analyze Twitter data using Hive in HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db672-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="db672-111">Prerequisites</span></span>
<span data-ttu-id="db672-112">Než začnete tento kurz, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="db672-112">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="db672-113">**Pracovní stanice** pomocí prostředí Azure PowerShell nainstalovaný a nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="db672-113">**A workstation** with Azure PowerShell installed and configured.</span></span>

    <span data-ttu-id="db672-114">tooexecute skriptů prostředí Windows PowerShell, je nutné spustit prostředí Azure PowerShell jako správce a nastavte zásady spouštění hello příliš*RemoteSigned*.</span><span class="sxs-lookup"><span data-stu-id="db672-114">tooexecute Windows PowerShell scripts, you must run Azure PowerShell as administrator and set hello execution policy too*RemoteSigned*.</span></span> <span data-ttu-id="db672-115">V tématu [spustit prostředí Windows PowerShell skripty][powershell-script].</span><span class="sxs-lookup"><span data-stu-id="db672-115">See [Run Windows PowerShell scripts][powershell-script].</span></span>

    <span data-ttu-id="db672-116">Před spuštěním skriptů prostředí Windows PowerShell, zkontrolujte, zda že jsou připojené tooyour předplatného Azure pomocí hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="db672-116">Before running Windows PowerShell scripts, make sure you are connected tooyour Azure subscription by using hello following cmdlet:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="db672-117">Pokud máte víc předplatných Azure, použijte následující rutinu tooset hello aktuální předplatné hello:</span><span class="sxs-lookup"><span data-stu-id="db672-117">If you have multiple Azure subscriptions, use hello following cmdlet tooset hello current subscription:</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="db672-118">Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a k 1. lednu 2017 jsme ji odebrali.</span><span class="sxs-lookup"><span data-stu-id="db672-118">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="db672-119">kroky Hello, v tento dokument použít hello nové rutiny služby HDInsight, které fungují s Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="db672-119">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="db672-120">Postupujte podle kroků hello v [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="db672-120">Please follow hello steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="db672-121">Pokud máte skripty, že toobe potřeba upravit hello toouse nové se rutiny, které pracují s Azure Resource Managerem najdete v tématu [tooAzure migrace založené na správci prostředků vývoj nástroje pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="db672-121">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

* <span data-ttu-id="db672-122">**Cluster Azure HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="db672-122">**An Azure HDInsight cluster**.</span></span> <span data-ttu-id="db672-123">Postup při zřizování clusteru naleznete v tématu [začněte používat HDInsight] [ hdinsight-get-started] nebo [zřizování clusterů HDInsight][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="db672-123">For instructions on cluster provisioning, see [Get started using HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="db672-124">Název clusteru hello budete potřebovat později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="db672-124">You will need hello cluster name later in hello tutorial.</span></span>

<span data-ttu-id="db672-125">Hello následující tabulka uvádí soubory hello použité v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="db672-125">hello following table lists hello files used in this tutorial:</span></span>

| <span data-ttu-id="db672-126">Soubory</span><span class="sxs-lookup"><span data-stu-id="db672-126">Files</span></span> | <span data-ttu-id="db672-127">Popis</span><span class="sxs-lookup"><span data-stu-id="db672-127">Description</span></span> |
| --- | --- |
| <span data-ttu-id="db672-128">/tutorials/Twitter/data/tweets.txt</span><span class="sxs-lookup"><span data-stu-id="db672-128">/tutorials/twitter/data/tweets.txt</span></span> |<span data-ttu-id="db672-129">Hello zdroje dat pro úlohy Hive hello.</span><span class="sxs-lookup"><span data-stu-id="db672-129">hello source data for hello Hive job.</span></span> |
| <span data-ttu-id="db672-130">/tutorials/Twitter/Output</span><span class="sxs-lookup"><span data-stu-id="db672-130">/tutorials/twitter/output</span></span> |<span data-ttu-id="db672-131">Hello výstupní složky pro úlohy Hive hello.</span><span class="sxs-lookup"><span data-stu-id="db672-131">hello output folder for hello Hive job.</span></span> <span data-ttu-id="db672-132">Hello výchozí Hive úlohy výstup název souboru je **000000_0**.</span><span class="sxs-lookup"><span data-stu-id="db672-132">hello default Hive job output file name is **000000_0**.</span></span> |
| <span data-ttu-id="db672-133">tutorials/Twitter/Twitter.hql</span><span class="sxs-lookup"><span data-stu-id="db672-133">tutorials/twitter/twitter.hql</span></span> |<span data-ttu-id="db672-134">soubor skriptu HiveQL Hello.</span><span class="sxs-lookup"><span data-stu-id="db672-134">hello HiveQL script file.</span></span> |
| <span data-ttu-id="db672-135">/tutorials/Twitter/JobStatus</span><span class="sxs-lookup"><span data-stu-id="db672-135">/tutorials/twitter/jobstatus</span></span> |<span data-ttu-id="db672-136">Hello stav úlohy Hadoop.</span><span class="sxs-lookup"><span data-stu-id="db672-136">hello Hadoop job status.</span></span> |

## <a name="get-twitter-feed"></a><span data-ttu-id="db672-137">Get Twitteru</span><span class="sxs-lookup"><span data-stu-id="db672-137">Get Twitter feed</span></span>
<span data-ttu-id="db672-138">V tomto kurzu budete používat hello [Twitter streamování rozhraní API][twitter-streaming-api].</span><span class="sxs-lookup"><span data-stu-id="db672-138">In this tutorial, you will use hello [Twitter streaming APIs][twitter-streaming-api].</span></span> <span data-ttu-id="db672-139">je technologie Hello konkrétní Twitter streamování použijete rozhraní API [stavy nebo filtr][twitter-statuses-filter].</span><span class="sxs-lookup"><span data-stu-id="db672-139">hello specific Twitter streaming API you will use is [statuses/filter][twitter-statuses-filter].</span></span>

> [!NOTE]
> <span data-ttu-id="db672-140">Soubor obsahující 10 000 tweetů a soubor skriptu Hive hello (popsaná v další části hello) byly odeslány ve veřejném kontejneru Blob.</span><span class="sxs-lookup"><span data-stu-id="db672-140">A file containing 10,000 tweets and hello Hive script file (covered in hello next section) have been uploaded in a public Blob container.</span></span> <span data-ttu-id="db672-141">Pokud chcete soubory toouse hello nahrát, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="db672-141">You can skip this section if you want toouse hello uploaded files.</span></span>

<span data-ttu-id="db672-142">[Tweetů data](https://dev.twitter.com/docs/platform-objects/tweets) je uložený ve formátu JavaScript Object Notation (JSON) hello, která obsahuje komplexní vnořené struktury.</span><span class="sxs-lookup"><span data-stu-id="db672-142">[Tweets data](https://dev.twitter.com/docs/platform-objects/tweets) is stored in hello JavaScript Object Notation (JSON) format that contains a complex nested structure.</span></span> <span data-ttu-id="db672-143">Místo psaní velkého počtu řádků kódu s použitím konvenční programovací jazyk, můžete převést tuto strukturu vnořené do tabulky Hive, tak, aby může být dotazován podle jazyka SQL (Structured Query) – jako jazyka nazývaného HiveQL.</span><span class="sxs-lookup"><span data-stu-id="db672-143">Instead of writing many lines of code by using a conventional programming language, you can transform this nested structure into a Hive table, so that it can be queried by a Structured Query Language (SQL)-like language called HiveQL.</span></span>

<span data-ttu-id="db672-144">Twitter používá rozhraní API tooits OAuth tooprovide oprávnění přístupu.</span><span class="sxs-lookup"><span data-stu-id="db672-144">Twitter uses OAuth tooprovide authorized access tooits API.</span></span> <span data-ttu-id="db672-145">OAuth je ověřovací protokol, který umožňuje uživatelům tooapprove aplikace tooact jejich jménem bez sdílení své heslo.</span><span class="sxs-lookup"><span data-stu-id="db672-145">OAuth is an authentication protocol that allows users tooapprove applications tooact on their behalf without sharing their password.</span></span> <span data-ttu-id="db672-146">Další informace najdete na [oauth.net](http://oauth.net/) nebo v hello vynikající [tooOAuth příručka začátečníka](http://hueniverse.com/oauth/) z Hueniverse.</span><span class="sxs-lookup"><span data-stu-id="db672-146">More information can be found at [oauth.net](http://oauth.net/) or in hello excellent [Beginner's Guide tooOAuth](http://hueniverse.com/oauth/) from Hueniverse.</span></span>

<span data-ttu-id="db672-147">první krok toouse Hello OAuth je toocreate novou aplikaci na webu služby Twitter vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="db672-147">hello first step toouse OAuth is toocreate a new application on hello Twitter Developer site.</span></span>

<span data-ttu-id="db672-148">**toocreate aplikace služby Twitter.**</span><span class="sxs-lookup"><span data-stu-id="db672-148">**toocreate a Twitter application**</span></span>

1. <span data-ttu-id="db672-149">Přihlaste se příliš[https://apps.twitter.com/](https://apps.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="db672-149">Sign in too[https://apps.twitter.com/](https://apps.twitter.com/).</span></span> <span data-ttu-id="db672-150">Klikněte na tlačítko hello **nyní** odkaz, pokud nemáte účet služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="db672-150">Click hello **Sign up now** link if you don't have a Twitter account.</span></span>
2. <span data-ttu-id="db672-151">Klikněte na tlačítko **vytvořte novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="db672-151">Click **Create New App**.</span></span>
3. <span data-ttu-id="db672-152">Zadejte **název**, **popis**, **webu**.</span><span class="sxs-lookup"><span data-stu-id="db672-152">Enter **Name**, **Description**, **Website**.</span></span> <span data-ttu-id="db672-153">Můžete použít adresu URL pro hello **webu** pole.</span><span class="sxs-lookup"><span data-stu-id="db672-153">You can make up a URL for hello **Website** field.</span></span> <span data-ttu-id="db672-154">Hello následující tabulka uvádí některé ukázkové hodnoty toouse:</span><span class="sxs-lookup"><span data-stu-id="db672-154">hello following table shows some sample values toouse:</span></span>

   | <span data-ttu-id="db672-155">Pole</span><span class="sxs-lookup"><span data-stu-id="db672-155">Field</span></span> | <span data-ttu-id="db672-156">Hodnota</span><span class="sxs-lookup"><span data-stu-id="db672-156">Value</span></span> |
   | --- | --- |
   |  <span data-ttu-id="db672-157">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="db672-157">Name</span></span> |<span data-ttu-id="db672-158">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="db672-158">MyHDInsightApp</span></span> |
   |  <span data-ttu-id="db672-159">Popis</span><span class="sxs-lookup"><span data-stu-id="db672-159">Description</span></span> |<span data-ttu-id="db672-160">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="db672-160">MyHDInsightApp</span></span> |
   |  <span data-ttu-id="db672-161">Web</span><span class="sxs-lookup"><span data-stu-id="db672-161">Website</span></span> |<span data-ttu-id="db672-162">http://www.myhdinsightapp.com</span><span class="sxs-lookup"><span data-stu-id="db672-162">http://www.myhdinsightapp.com</span></span> |
4. <span data-ttu-id="db672-163">Zkontrolujte **souhlasím**a potom klikněte na **vytvořit aplikaci služby Twitter**.</span><span class="sxs-lookup"><span data-stu-id="db672-163">Check **Yes, I agree**, and then click **Create your Twitter application**.</span></span>
5. <span data-ttu-id="db672-164">Klikněte na tlačítko hello **oprávnění** kartě hello výchozí oprávnění je **jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="db672-164">Click hello **Permissions** tab. hello default permission is **Read only**.</span></span> <span data-ttu-id="db672-165">Toto je dostatečný pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="db672-165">This is sufficient for this tutorial.</span></span>
6. <span data-ttu-id="db672-166">Klikněte na tlačítko hello **klíče a přístupové tokeny** kartě.</span><span class="sxs-lookup"><span data-stu-id="db672-166">Click hello **Keys and Access Tokens** tab.</span></span>
7. <span data-ttu-id="db672-167">Klikněte na tlačítko **vytvořit můj přístupový token**.</span><span class="sxs-lookup"><span data-stu-id="db672-167">Click **Create my access token**.</span></span>
8. <span data-ttu-id="db672-168">Klikněte na tlačítko **testovací OAuth** v pravém horním rohu hello hello stránky.</span><span class="sxs-lookup"><span data-stu-id="db672-168">Click **Test OAuth** in hello upper-right corner of hello page.</span></span>
9. <span data-ttu-id="db672-169">Zapište **uživatelský klíč**, **uživatelský tajný klíč**, **přístupový token**, a **tajný klíč přístupového tokenu**.</span><span class="sxs-lookup"><span data-stu-id="db672-169">Write down **consumer key**, **Consumer secret**, **Access token**, and **Access token secret**.</span></span> <span data-ttu-id="db672-170">Hello hodnoty budete potřebovat později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="db672-170">You will need hello values later in hello tutorial.</span></span>

<span data-ttu-id="db672-171">V tomto kurzu použijete volání webové služby v prostředí Windows PowerShell toomake hello.</span><span class="sxs-lookup"><span data-stu-id="db672-171">In this tutorial, you will use Windows PowerShell toomake hello web service call.</span></span> <span data-ttu-id="db672-172">C# .NET ukázku, najdete v části [analýzu v reálném čase sentimentu Twitter s HBase v HDInsight][hdinsight-hbase-twitter-sentiment].</span><span class="sxs-lookup"><span data-stu-id="db672-172">For a .NET C# sample, see [Analyze real-time Twitter sentiment with HBase in HDInsight][hdinsight-hbase-twitter-sentiment].</span></span> <span data-ttu-id="db672-173">je technologie Hello volání webové služby dalších oblíbených nástroj toomake [ *Curl*][curl].</span><span class="sxs-lookup"><span data-stu-id="db672-173">hello other popular tool toomake web service calls is [*Curl*][curl].</span></span> <span data-ttu-id="db672-174">Curl si můžete stáhnout z [sem][curl-download].</span><span class="sxs-lookup"><span data-stu-id="db672-174">Curl can be downloaded from [here][curl-download].</span></span>

> [!NOTE]
> <span data-ttu-id="db672-175">Pokud použijete příkaz curl hello v systému Windows, použijte dvojité uvozovky místo jednoduchých uvozovek a být pro hodnoty možnosti hello.</span><span class="sxs-lookup"><span data-stu-id="db672-175">When you use hello curl command in Windows, use double quotes instead of single quotes for hello option values.</span></span>

<span data-ttu-id="db672-176">**tooget tweetů**</span><span class="sxs-lookup"><span data-stu-id="db672-176">**tooget tweets**</span></span>

1. <span data-ttu-id="db672-177">Otevřete Windows PowerShell Integrované skriptovací prostředí (ISE) hello.</span><span class="sxs-lookup"><span data-stu-id="db672-177">Open hello Windows PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="db672-178">(Na obrazovce Windows 8 Start hello zadejte **PowerShell_ISE** a pak klikněte na **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="db672-178">(On hello Windows 8 Start screen, type **PowerShell_ISE** and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="db672-179">V tématu [spuštění prostředí Windows PowerShell v systému Windows 8 a Windows][powershell-start].)</span><span class="sxs-lookup"><span data-stu-id="db672-179">See [Start Windows PowerShell on Windows 8 and Windows][powershell-start].)</span></span>
2. <span data-ttu-id="db672-180">Zkopírujte následující skript do podokno skriptu hello hello:</span><span class="sxs-lookup"><span data-stu-id="db672-180">Copy hello following script into hello script pane:</span></span>

    ```powershell
    #region - variables and constants
    $clusterName = "<HDInsightClusterName>" # Enter hello HDInsight cluster name

    # Enter hello OAuth information for your Twitter application
    $oauth_consumer_key = "<TwitterAppConsumerKey>";
    $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
    $oauth_token = "<TwitterAppAccessToken>";
    $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

    $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves hello tweets into this blob.

    $trackString = "Azure, Cloud, HDInsight" # This script gets hello tweets containing these keywords.
    $track = [System.Uri]::EscapeDataString($trackString);
    $lineMax = 10000  # hello script will get this number of tweets. It is about 3 minutes every 100 lines.
    #endregion

    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    Login-AzureRmAccount
    #endregion

    #region - Create a block blob object for writing tweets into Blob storage
    Write-Host "Get hello default storage account name and Blob container name using hello cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $containerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

    Write-Host "Define hello Azure storage connection string ..." -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
    Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

    Write-Host "Create block blob object ..." -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($containerName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    #end region

    # region - Format OAuth strings
    Write-Host "Format oauth strings ..." -ForegroundColor Green
    $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
    $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
    $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

    $signature = "POST&";
    $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
    $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
    $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
    $signature += [System.Uri]::EscapeDataString("track=" + $track);

    $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

    $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
    $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
    $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

    $oauth_authorization = 'OAuth ';
    $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
    $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
    $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
    $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
    $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
    $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
    $oauth_authorization += 'oauth_version="1.0"';

    $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
    #endregion

    #region - Read tweets
    Write-Host "Create HTTP web request ..." -ForegroundColor Green
    [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
    $request.Method = "POST";
    $request.Headers.Add("Authorization", $oauth_authorization);
    $request.ContentType = "application/x-www-form-urlencoded";
    $body = $request.GetRequestStream();

    $body.write($post_body, 0, $post_body.length);
    $body.flush();
    $body.close();
    $response = $request.GetResponse() ;

    Write-Host "Start stream reading ..." -ForegroundColor Green

    Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

    $inrec = $sReader.ReadLine()
    $count = 0
    while (($inrec -ne $null) -and ($count -le $lineMax))
    {
        if ($inrec -ne "")
        {
            Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

            $writeStream.WriteLine($inrec)
            $count ++
        }

        $inrec=$sReader.ReadLine()
    }
    #endregion

    #region - Write tweets tooBlob storage
    Write-Host "Write toohello destination blob ..." -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    $sReader.close()
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. <span data-ttu-id="db672-181">Nastavte hello prvních pět tooeight proměnné ve skriptu hello:</span><span class="sxs-lookup"><span data-stu-id="db672-181">Set hello first five tooeight variables in hello script:</span></span>

    <span data-ttu-id="db672-182">Proměnná</span><span class="sxs-lookup"><span data-stu-id="db672-182">Variable</span></span>|<span data-ttu-id="db672-183">Popis</span><span class="sxs-lookup"><span data-stu-id="db672-183">Description</span></span>
    ---|---
    <span data-ttu-id="db672-184">$clusterName</span><span class="sxs-lookup"><span data-stu-id="db672-184">$clusterName</span></span>|<span data-ttu-id="db672-185">Toto je název clusteru HDInsight hello místo toorun hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="db672-185">This is hello name of hello HDInsight cluster where you want toorun hello application.</span></span>
    <span data-ttu-id="db672-186">$oauth_consumer_key</span><span class="sxs-lookup"><span data-stu-id="db672-186">$oauth_consumer_key</span></span>|<span data-ttu-id="db672-187">Toto je aplikace hello Twitter **uživatelský klíč** jste si poznamenali dříve při vytváření aplikace hello Twitter.</span><span class="sxs-lookup"><span data-stu-id="db672-187">This is hello Twitter application **consumer key** you wrote down earlier when you created hello Twitter application.</span></span>
    <span data-ttu-id="db672-188">$oauth_consumer_secret</span><span class="sxs-lookup"><span data-stu-id="db672-188">$oauth_consumer_secret</span></span>|<span data-ttu-id="db672-189">Toto je aplikace hello Twitter **uživatelský tajný klíč** jste si poznamenali dříve.</span><span class="sxs-lookup"><span data-stu-id="db672-189">This is hello Twitter application **consumer secret** you wrote down earlier.</span></span>
    <span data-ttu-id="db672-190">$oauth_token</span><span class="sxs-lookup"><span data-stu-id="db672-190">$oauth_token</span></span>|<span data-ttu-id="db672-191">Toto je aplikace hello Twitter **přístupový token** jste si poznamenali dříve.</span><span class="sxs-lookup"><span data-stu-id="db672-191">This is hello Twitter application **access token** you wrote down earlier.</span></span>
    <span data-ttu-id="db672-192">$oauth_token_secret</span><span class="sxs-lookup"><span data-stu-id="db672-192">$oauth_token_secret</span></span>|<span data-ttu-id="db672-193">Toto je aplikace hello Twitter **tajný klíč přístupového tokenu** jste si poznamenali dříve.</span><span class="sxs-lookup"><span data-stu-id="db672-193">This is hello Twitter application **access token secret** you wrote down earlier.</span></span>
    <span data-ttu-id="db672-194">$destBlobName</span><span class="sxs-lookup"><span data-stu-id="db672-194">$destBlobName</span></span>|<span data-ttu-id="db672-195">Toto je název objektu blob výstup hello.</span><span class="sxs-lookup"><span data-stu-id="db672-195">This is hello output blob name.</span></span> <span data-ttu-id="db672-196">Hello výchozí hodnota je **tutorials/twitter/data/tweets.txt**.</span><span class="sxs-lookup"><span data-stu-id="db672-196">hello default value is **tutorials/twitter/data/tweets.txt**.</span></span> <span data-ttu-id="db672-197">Pokud změníte výchozí hodnotu hello, budete potřebovat skriptů prostředí Windows PowerShell hello tooupdate odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="db672-197">If you change hello default value, you will need tooupdate hello Windows PowerShell scripts accordingly.</span></span>
    <span data-ttu-id="db672-198">$trackString</span><span class="sxs-lookup"><span data-stu-id="db672-198">$trackString</span></span>|<span data-ttu-id="db672-199">Webová služba Hello vrátí související toothese tweetů klíčová slova.</span><span class="sxs-lookup"><span data-stu-id="db672-199">hello web service will return tweets related toothese keywords.</span></span> <span data-ttu-id="db672-200">Hello výchozí hodnota je **Azure Cloud, HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="db672-200">hello default value is **Azure, Cloud, HDInsight**.</span></span> <span data-ttu-id="db672-201">Pokud změníte výchozí hodnotu hello, aktualizujte skriptů prostředí Windows PowerShell hello odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="db672-201">If you change hello default value, you will update hello Windows PowerShell scripts accordingly.</span></span>
    <span data-ttu-id="db672-202">$lineMax</span><span class="sxs-lookup"><span data-stu-id="db672-202">$lineMax</span></span>|<span data-ttu-id="db672-203">Hello hodnota určuje, kolik skriptu hello tweetů, bude číst.</span><span class="sxs-lookup"><span data-stu-id="db672-203">hello value determines how many tweets hello script will read.</span></span> <span data-ttu-id="db672-204">Trvá přibližně tři minuty tooread 100 tweetů.</span><span class="sxs-lookup"><span data-stu-id="db672-204">It takes about three minutes tooread 100 tweets.</span></span> <span data-ttu-id="db672-205">Můžete nastavit vyšší číslo, ale bude trvat další toodownload čas.</span><span class="sxs-lookup"><span data-stu-id="db672-205">You can set a larger number, but it will take more time toodownload.</span></span>

1. <span data-ttu-id="db672-206">Stiskněte klávesu **F5** toorun hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="db672-206">Press **F5** toorun hello script.</span></span> <span data-ttu-id="db672-207">Pokud narazíte na problémy, jako řešení, vyberte všechny řádky hello a potom stiskněte klávesu **F8**.</span><span class="sxs-lookup"><span data-stu-id="db672-207">If you run into problems, as a workaround, select all hello lines, and then press **F8**.</span></span>
2. <span data-ttu-id="db672-208">Zobrazí se "Dokončit!"</span><span class="sxs-lookup"><span data-stu-id="db672-208">You shall see "Complete!"</span></span> <span data-ttu-id="db672-209">na konci hello hello výstupu.</span><span class="sxs-lookup"><span data-stu-id="db672-209">at hello end of hello output.</span></span> <span data-ttu-id="db672-210">Chybové zprávy, se zobrazí červeně.</span><span class="sxs-lookup"><span data-stu-id="db672-210">Any error messages will be displayed in red.</span></span>

<span data-ttu-id="db672-211">Jako postup ověření, můžete zkontrolovat soubor výstup hello, **/tutorials/twitter/data/tweets.txt**, ve službě Azure Blob storage pomocí Průzkumníka úložiště Azure nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="db672-211">As a validation procedure, you can check hello output file, **/tutorials/twitter/data/tweets.txt**, on your Azure Blob storage by using an Azure storage explorer or Azure PowerShell.</span></span> <span data-ttu-id="db672-212">Ukázkový skript prostředí Windows PowerShell pro výpis soubory, najdete v části [používání Blob storage s HDInsight][hdinsight-storage-powershell].</span><span class="sxs-lookup"><span data-stu-id="db672-212">For a sample Windows PowerShell script for listing files, see [Use Blob storage with HDInsight][hdinsight-storage-powershell].</span></span>

## <a name="create-hiveql-script"></a><span data-ttu-id="db672-213">Vytvořit skript HiveQL</span><span class="sxs-lookup"><span data-stu-id="db672-213">Create HiveQL script</span></span>
<span data-ttu-id="db672-214">Pomocí Azure PowerShell, můžete spustit více příkazy HiveQL jeden na čas, nebo balíček hello HiveQL příkaz do souboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="db672-214">Using Azure PowerShell, you can run multiple HiveQL statements one at a time, or package hello HiveQL statement into a script file.</span></span> <span data-ttu-id="db672-215">V tomto kurzu vytvoříte skript HiveQL.</span><span class="sxs-lookup"><span data-stu-id="db672-215">In this tutorial, you will create a HiveQL script.</span></span> <span data-ttu-id="db672-216">soubor skriptu Hello musí být nahrané tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="db672-216">hello script file must be uploaded tooAzure Blob storage.</span></span> <span data-ttu-id="db672-217">V další části hello bude spuštěn soubor skriptu hello pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="db672-217">In hello next section, you will run hello script file by using Azure PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="db672-218">soubor skriptu Hive Hello a soubor obsahující 10 000 tweetů byly odeslány ve veřejném kontejneru Blob.</span><span class="sxs-lookup"><span data-stu-id="db672-218">hello Hive script file and a file containing 10,000 tweets have been uploaded in a public Blob container.</span></span> <span data-ttu-id="db672-219">Pokud chcete soubory toouse hello nahrát, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="db672-219">You can skip this section if you want toouse hello uploaded files.</span></span>

<span data-ttu-id="db672-220">Hello skript HiveQL provede hello následující:</span><span class="sxs-lookup"><span data-stu-id="db672-220">hello HiveQL script will perform hello following:</span></span>

1. <span data-ttu-id="db672-221">**Odpojit tabulku tweets_raw hello** v případě, že hello tabulka již existuje.</span><span class="sxs-lookup"><span data-stu-id="db672-221">**Drop hello tweets_raw table** in case hello table already exists.</span></span>
2. <span data-ttu-id="db672-222">**Vytvoří tabulku Hive tweets_raw hello**.</span><span class="sxs-lookup"><span data-stu-id="db672-222">**Create hello tweets_raw Hive table**.</span></span> <span data-ttu-id="db672-223">Tato dočasná tabulka strukturovaných Hive obsahuje hello data pro další extrakce, transformace a načítání (ETL) zpracování.</span><span class="sxs-lookup"><span data-stu-id="db672-223">This temporary Hive structured table holds hello data for further extract, transform, and load (ETL) processing.</span></span> <span data-ttu-id="db672-224">Informace o oddílech najdete v tématu [Hive kurzu][apache-hive-tutorial].</span><span class="sxs-lookup"><span data-stu-id="db672-224">For information on partitions, see [Hive tutorial][apache-hive-tutorial].</span></span>
3. <span data-ttu-id="db672-225">**Načtení dat** ze hello zdrojové složky /tutorials/twitter/data.</span><span class="sxs-lookup"><span data-stu-id="db672-225">**Load data** from hello source folder, /tutorials/twitter/data.</span></span> <span data-ttu-id="db672-226">datovou sadu tweetů velké Hello ve vnořených formátu JSON má nyní převedeny na dočasné struktura tabulky Hive.</span><span class="sxs-lookup"><span data-stu-id="db672-226">hello large tweets dataset in nested JSON format has now been transformed into a temporary Hive table structure.</span></span>
4. <span data-ttu-id="db672-227">**Vyřaďte hello tweetů tabulky** v případě, že hello tabulka již existuje.</span><span class="sxs-lookup"><span data-stu-id="db672-227">**Drop hello tweets table** in case hello table already exists.</span></span>
5. <span data-ttu-id="db672-228">**Vytvoření tabulky tweetů hello**.</span><span class="sxs-lookup"><span data-stu-id="db672-228">**Create hello tweets table**.</span></span> <span data-ttu-id="db672-229">Než proti datovou sadu tweetů hello se můžete dotazovat pomocí Hive, musíte toorun jiný proces ETL.</span><span class="sxs-lookup"><span data-stu-id="db672-229">Before you can query against hello tweets dataset by using Hive, you need toorun another ETL process.</span></span> <span data-ttu-id="db672-230">Tento proces ETL definuje podrobnější schématu tabulky pro hello data, která máte uložená v tabulce "twitter_raw" hello.</span><span class="sxs-lookup"><span data-stu-id="db672-230">This ETL process defines a more detailed table schema for hello data that you have stored in hello "twitter_raw" table.</span></span>
6. <span data-ttu-id="db672-231">**Vložit přepsat tabulku**.</span><span class="sxs-lookup"><span data-stu-id="db672-231">**Insert overwrite table**.</span></span> <span data-ttu-id="db672-232">Tento komplexní skript Hive se ji sadu dlouho úloh MapReduce clusterem Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="db672-232">This complex Hive script will kick off a set of long MapReduce jobs by hello Hadoop cluster.</span></span> <span data-ttu-id="db672-233">V závislosti na vaší datovou sadu a hello velikost clusteru to může trvat asi 10 minut.</span><span class="sxs-lookup"><span data-stu-id="db672-233">Depending on your dataset and hello size of your cluster, this could take about 10 minutes.</span></span>
7. <span data-ttu-id="db672-234">**Vložení přepsat directory**.</span><span class="sxs-lookup"><span data-stu-id="db672-234">**Insert overwrite directory**.</span></span> <span data-ttu-id="db672-235">Spusťte soubor tooa datovou sadu dotaz a výstup hello.</span><span class="sxs-lookup"><span data-stu-id="db672-235">Run a query and output hello dataset tooa file.</span></span> <span data-ttu-id="db672-236">Tento dotaz vrátí seznam Twitter uživatelů, kteří odeslané většina tweetů, které obsahovaly hello slovo "Azure".</span><span class="sxs-lookup"><span data-stu-id="db672-236">This query will return a list of Twitter users who sent most tweets that contained hello word "Azure".</span></span>

<span data-ttu-id="db672-237">**toocreate podregistru skript a nahrajte ho tooAzure**</span><span class="sxs-lookup"><span data-stu-id="db672-237">**toocreate a Hive script and upload it tooAzure**</span></span>

1. <span data-ttu-id="db672-238">Otevřete Windows PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="db672-238">Open Windows PowerShell ISE.</span></span>
2. <span data-ttu-id="db672-239">Zkopírujte následující skript do podokno skriptu hello hello:</span><span class="sxs-lookup"><span data-stu-id="db672-239">Copy hello following script into hello script pane:</span></span>

    ```powershell
    #region - variables and constants
    $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
    $subscriptionID = "<Azure Subscription ID>"

    $sourceDataPath = "/tutorials/twitter/data"
    $outputPath = "/tutorials/twitter/output"
    $hqlScriptFile = "tutorials/twitter/twitter.hql"

    $hqlStatements = @"
    set hive.exec.dynamic.partition = true;
    set hive.exec.dynamic.partition.mode = nonstrict;

    DROP TABLE tweets_raw;
    CREATE EXTERNAL TABLE tweets_raw (
        json_response STRING
    )
    STORED AS TEXTFILE LOCATION '$sourceDataPath';

    DROP TABLE tweets;
    CREATE TABLE tweets
    (
        id BIGINT,
        created_at STRING,
        created_at_date STRING,
        created_at_year STRING,
        created_at_month STRING,
        created_at_day STRING,
        created_at_time STRING,
        in_reply_to_user_id_str STRING,
        text STRING,
        contributors STRING,
        retweeted STRING,
        truncated STRING,
        coordinates STRING,
        source STRING,
        retweet_count INT,
        url STRING,
        hashtags array<STRING>,
        user_mentions array<STRING>,
        first_hashtag STRING,
        first_user_mention STRING,
        screen_name STRING,
        name STRING,
        followers_count INT,
        listed_count INT,
        friends_count INT,
        lang STRING,
        user_location STRING,
        time_zone STRING,
        profile_image_url STRING,
        json_response STRING
    );

    FROM tweets_raw
    INSERT OVERWRITE TABLE tweets
    SELECT
        cast(get_json_object(json_response, '$.id_str') as BIGINT),
        get_json_object(json_response, '$.created_at'),
        concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
        substr (get_json_object(json_response, '$.created_at'),27,4)),
        substr (get_json_object(json_response, '$.created_at'),27,4),
        case substr (get_json_object(json_response, '$.created_at'),5,3)
            when "Jan" then "01"
            when "Feb" then "02"
            when "Mar" then "03"
            when "Apr" then "04"
            when "May" then "05"
            when "Jun" then "06"
            when "Jul" then "07"
            when "Aug" then "08"
            when "Sep" then "09"
            when "Oct" then "10"
            when "Nov" then "11"
            when "Dec" then "12" end,
        substr (get_json_object(json_response, '$.created_at'),9,2),
        substr (get_json_object(json_response, '$.created_at'),12,8),
        get_json_object(json_response, '$.in_reply_to_user_id_str'),
        get_json_object(json_response, '$.text'),
        get_json_object(json_response, '$.contributors'),
        get_json_object(json_response, '$.retweeted'),
        get_json_object(json_response, '$.truncated'),
        get_json_object(json_response, '$.coordinates'),
        get_json_object(json_response, '$.source'),
        cast (get_json_object(json_response, '$.retweet_count') as INT),
        get_json_object(json_response, '$.entities.display_url'),
        array(
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
        array(
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
        trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
        trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
        get_json_object(json_response, '$.user.screen_name'),
        get_json_object(json_response, '$.user.name'),
        cast (get_json_object(json_response, '$.user.followers_count') as INT),
        cast (get_json_object(json_response, '$.user.listed_count') as INT),
        cast (get_json_object(json_response, '$.user.friends_count') as INT),
        get_json_object(json_response, '$.user.lang'),
        get_json_object(json_response, '$.user.location'),
        get_json_object(json_response, '$.user.time_zone'),
        get_json_object(json_response, '$.user.profile_image_url'),
        json_response
    WHERE (length(json_response) > 500);

    INSERT OVERWRITE DIRECTORY '$outputPath'
    SELECT name, screen_name, count(1) as cc
        FROM tweets
        WHERE text like "%Azure%"
        GROUP BY name,screen_name
        ORDER BY cc DESC LIMIT 10;
    "@
    #endregion

    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #endregion

    #region - Create a block blob object for writing hello Hive script file
    Write-Host "Get hello default storage account name and container name based on hello cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

    Write-Host "Define hello connection string ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    Write-Host "Create block blob objects referencing hello hql script file" -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)

    Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    $writeStream.Writeline($hqlStatements)
    #endregion

    #region - Write hello Hive script file tooBlob storage
    Write-Host "Write toohello destination blob ... " -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $hqlScriptBlob.UploadFromStream($memStream)
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. <span data-ttu-id="db672-240">Nastavte hello první dvě proměnné ve skriptu hello:</span><span class="sxs-lookup"><span data-stu-id="db672-240">Set hello first two variables in hello script:</span></span>

   | <span data-ttu-id="db672-241">Proměnná</span><span class="sxs-lookup"><span data-stu-id="db672-241">Variable</span></span> | <span data-ttu-id="db672-242">Popis</span><span class="sxs-lookup"><span data-stu-id="db672-242">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="db672-243">$clusterName</span><span class="sxs-lookup"><span data-stu-id="db672-243">$clusterName</span></span> |<span data-ttu-id="db672-244">Zadejte název clusteru HDInsight hello místo toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="db672-244">Enter hello HDInsight cluster name where you want toorun hello application.</span></span> |
   |  <span data-ttu-id="db672-245">$subscriptionID</span><span class="sxs-lookup"><span data-stu-id="db672-245">$subscriptionID</span></span> |<span data-ttu-id="db672-246">Zadejte ID vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="db672-246">Enter your Azure subscription ID.</span></span> |
   |  <span data-ttu-id="db672-247">$sourceDataPath</span><span class="sxs-lookup"><span data-stu-id="db672-247">$sourceDataPath</span></span> |<span data-ttu-id="db672-248">Hello hello dotazů Hive, kde bude číst hello data z umístění úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="db672-248">hello Azure Blob storage location where hello Hive queries will read hello data from.</span></span> <span data-ttu-id="db672-249">Tato proměnná nepotřebujete toochange.</span><span class="sxs-lookup"><span data-stu-id="db672-249">You don't need toochange this variable.</span></span> |
   |  <span data-ttu-id="db672-250">$outputPath</span><span class="sxs-lookup"><span data-stu-id="db672-250">$outputPath</span></span> |<span data-ttu-id="db672-251">Hello umístění úložiště objektů Blob v Azure, kde dotazů Hive hello výstup hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="db672-251">hello Azure Blob storage location where hello Hive queries will output hello results.</span></span> <span data-ttu-id="db672-252">Tato proměnná nepotřebujete toochange.</span><span class="sxs-lookup"><span data-stu-id="db672-252">You don't need toochange this variable.</span></span> |
   |  <span data-ttu-id="db672-253">$hqlScriptFile</span><span class="sxs-lookup"><span data-stu-id="db672-253">$hqlScriptFile</span></span> |<span data-ttu-id="db672-254">Hello umístění a název souboru hello soubor skriptu HiveQL hello.</span><span class="sxs-lookup"><span data-stu-id="db672-254">hello location and hello file name of hello HiveQL script file.</span></span> <span data-ttu-id="db672-255">Tato proměnná nepotřebujete toochange.</span><span class="sxs-lookup"><span data-stu-id="db672-255">You don't need toochange this variable.</span></span> |
4. <span data-ttu-id="db672-256">Stiskněte klávesu **F5** toorun hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="db672-256">Press **F5** toorun hello script.</span></span> <span data-ttu-id="db672-257">Pokud narazíte na problémy, jako řešení, vyberte všechny řádky hello a potom stiskněte klávesu **F8**.</span><span class="sxs-lookup"><span data-stu-id="db672-257">If you run into problems, as a workaround, select all hello lines, and then press **F8**.</span></span>
5. <span data-ttu-id="db672-258">Zobrazí se "Dokončit!"</span><span class="sxs-lookup"><span data-stu-id="db672-258">You shall see "Complete!"</span></span> <span data-ttu-id="db672-259">na konci hello hello výstupu.</span><span class="sxs-lookup"><span data-stu-id="db672-259">at hello end of hello output.</span></span> <span data-ttu-id="db672-260">Chybové zprávy, se zobrazí červeně.</span><span class="sxs-lookup"><span data-stu-id="db672-260">Any error messages will be displayed in red.</span></span>

<span data-ttu-id="db672-261">Jako postup ověření, můžete zkontrolovat soubor výstup hello, **/tutorials/twitter/twitter.hql**, ve službě Azure Blob storage pomocí Průzkumníka úložiště Azure nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="db672-261">As a validation procedure, you can check hello output file, **/tutorials/twitter/twitter.hql**, on your Azure Blob storage by using an Azure storage explorer or Azure PowerShell.</span></span> <span data-ttu-id="db672-262">Ukázkový skript prostředí Windows PowerShell pro výpis soubory, najdete v části [používání Blob storage s HDInsight][hdinsight-storage-powershell].</span><span class="sxs-lookup"><span data-stu-id="db672-262">For a sample Windows PowerShell script for listing files, see [Use Blob storage with HDInsight][hdinsight-storage-powershell].</span></span>

## <a name="process-twitter-data-by-using-hive"></a><span data-ttu-id="db672-263">Zpracování dat Twitteru pomocí Hive</span><span class="sxs-lookup"><span data-stu-id="db672-263">Process Twitter data by using Hive</span></span>
<span data-ttu-id="db672-264">Dokončení veškeré práce přípravy hello.</span><span class="sxs-lookup"><span data-stu-id="db672-264">You have finished all hello preparation work.</span></span> <span data-ttu-id="db672-265">Nyní můžete vyvolat hello skriptu Hive a hello výsledky kontroly.</span><span class="sxs-lookup"><span data-stu-id="db672-265">Now, you can invoke hello Hive script and check hello results.</span></span>

### <a name="submit-a-hive-job"></a><span data-ttu-id="db672-266">Odeslání úlohy Hive</span><span class="sxs-lookup"><span data-stu-id="db672-266">Submit a Hive job</span></span>
<span data-ttu-id="db672-267">Použijte následující skript prostředí Windows PowerShell, toorun skriptu Hive hello hello.</span><span class="sxs-lookup"><span data-stu-id="db672-267">Use hello following Windows PowerShell script toorun hello Hive script.</span></span> <span data-ttu-id="db672-268">Budete potřebovat tooset hello první proměnné.</span><span class="sxs-lookup"><span data-stu-id="db672-268">You will need tooset hello first variable.</span></span>

> [!NOTE]
> <span data-ttu-id="db672-269">toouse hello tweetů a HiveQL skript, který jste nahráli v posledních dvou částech hello sadu $hqlScriptFile too"/tutorials/twitter/twitter.hql hello".</span><span class="sxs-lookup"><span data-stu-id="db672-269">toouse hello tweets and hello HiveQL script you uploaded in hello last two sections, set $hqlScriptFile too"/tutorials/twitter/twitter.hql".</span></span> <span data-ttu-id="db672-270">hello toouse ty, které byly odeslány tooa veřejného objektu blob pro vás, nastavte $hqlScriptFile příliš"wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".</span><span class="sxs-lookup"><span data-stu-id="db672-270">toouse hello ones that have been uploaded tooa public blob for you, set $hqlScriptFile too"wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".</span></span>

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"
$httpUserName = "admin"
$httpUserPassword = "<HDInsight Cluster HTTP User Password>"

#use one of hello following
$hqlScriptFile = "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
$hqlScriptFile = "/tutorials/twitter/twitter.hql"

$statusFolder = "/tutorials/twitter/jobstatus"
#endregion

$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value

$defaultBlobContainerName = $myCluster.DefaultStorageContainer

#region - Invoke Hive
Write-Host "Invoke Hive ... " -ForegroundColor Green

# Create hello HDInsight cluster
$pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential
$response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable

Write-Host "Display hello standard error log ... " -ForegroundColor Green
$jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
#endregion
```

### <a name="check-hello-results"></a><span data-ttu-id="db672-271">Výsledky kontroly hello</span><span class="sxs-lookup"><span data-stu-id="db672-271">Check hello results</span></span>
<span data-ttu-id="db672-272">Použijte následující prostředí Windows PowerShell skriptu toocheck hello výstup úlohy Hive hello.</span><span class="sxs-lookup"><span data-stu-id="db672-272">Use hello following Windows PowerShell script toocheck hello Hive job output.</span></span> <span data-ttu-id="db672-273">Budete potřebovat tooset hello první dvě proměnné.</span><span class="sxs-lookup"><span data-stu-id="db672-273">You will need tooset hello first two variables.</span></span>

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"

$blob = "tutorials/twitter/output/000000_0" # hello name of hello blob toobe downloaded.
#endregion

#region - Create an Azure storage context object
Write-Host "Get hello default storage account name and container name based on hello cluster name ..." -ForegroundColor Green
$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
$defaultBlobContainerName = $myCluster.DefaultStorageContainer

Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

Write-Host "Create a context object ... " -ForegroundColor Green
$storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
#endregion

#region - Download blob and display blob
Write-Host "Download hello blob ..." -ForegroundColor Green
cd $HOME
Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force

Write-Host "Display hello output ..." -ForegroundColor Green
Write-Host "==================================" -ForegroundColor Green
cat "./$blob"
Write-Host "==================================" -ForegroundColor Green
#end region
```

> [!NOTE]
> Tabulka Hive Hello používá \001 jako hello oddělovačem polí. <span data-ttu-id="db672-275">Oddělovač Hello není zobrazená v výstup hello.</span><span class="sxs-lookup"><span data-stu-id="db672-275">hello delimiter is not visible in hello output.</span></span>

<span data-ttu-id="db672-276">Po výsledky analýzy hello byly umístěny do úložiště objektů Blob v Azure, můžete hello data tooan Azure SQL database nebo SQL server pro export, export tooExcel hello dat pomocí Power Query nebo připojení vašich dat toohello aplikace pomocí hello ovladače ODBC Hive.</span><span class="sxs-lookup"><span data-stu-id="db672-276">After hello analysis results have been placed in Azure Blob storage, you can export hello data tooan Azure SQL database/SQL server, export hello data tooExcel by using Power Query, or connect your application toohello data by using hello Hive ODBC Driver.</span></span> <span data-ttu-id="db672-277">Další informace najdete v tématu [Sqoop použití s HDInsight][hdinsight-use-sqoop], [analýza letu zpoždění dat pomocí HDInsight][hdinsight-analyze-flight-delay-data], [ Připojení aplikace Excel tooHDInsight doplněk Power Query][hdinsight-power-query], a [tooHDInsight připojení aplikace Excel s hello ovladače ODBC Microsoft Hive][hdinsight-hive-odbc].</span><span class="sxs-lookup"><span data-stu-id="db672-277">For more information, see [Use Sqoop with HDInsight][hdinsight-use-sqoop], [Analyze flight delay data using HDInsight][hdinsight-analyze-flight-delay-data], [Connect Excel tooHDInsight with Power Query][hdinsight-power-query], and [Connect Excel tooHDInsight with hello Microsoft Hive ODBC Driver][hdinsight-hive-odbc].</span></span>

## <a name="next-steps"></a><span data-ttu-id="db672-278">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db672-278">Next steps</span></span>
<span data-ttu-id="db672-279">V tomto kurzu jsme viděli, jak tootransform nestrukturovaných JSON datové sady do strukturovaných tooquery tabulku Hive zkoumat a analyzovat data z Twitteru pomocí HDInsight v Azure.</span><span class="sxs-lookup"><span data-stu-id="db672-279">In this tutorial we have seen how tootransform an unstructured JSON dataset into a structured Hive table tooquery, explore, and analyze data from Twitter by using HDInsight on Azure.</span></span> <span data-ttu-id="db672-280">toolearn více, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="db672-280">toolearn more, see:</span></span>

* <span data-ttu-id="db672-281">[Začínáme s HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="db672-281">[Get started with HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="db672-282">[Analýza v reálném čase sentimentu Twitter s HBase v HDInsight][hdinsight-hbase-twitter-sentiment]</span><span class="sxs-lookup"><span data-stu-id="db672-282">[Analyze real-time Twitter sentiment with HBase in HDInsight][hdinsight-hbase-twitter-sentiment]</span></span>
* <span data-ttu-id="db672-283">[Analýza dat zpoždění letu pomocí HDInsight][hdinsight-analyze-flight-delay-data]</span><span class="sxs-lookup"><span data-stu-id="db672-283">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-delay-data]</span></span>
* <span data-ttu-id="db672-284">[Připojení aplikace Excel tooHDInsight doplněk Power Query][hdinsight-power-query]</span><span class="sxs-lookup"><span data-stu-id="db672-284">[Connect Excel tooHDInsight with Power Query][hdinsight-power-query]</span></span>
* <span data-ttu-id="db672-285">[Připojení aplikace Excel tooHDInsight s hello ovladače ODBC Microsoft Hive][hdinsight-hive-odbc]</span><span class="sxs-lookup"><span data-stu-id="db672-285">[Connect Excel tooHDInsight with hello Microsoft Hive ODBC Driver][hdinsight-hive-odbc]</span></span>
* <span data-ttu-id="db672-286">[Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="db672-286">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]:hdinsight-hadoop-use-blob-storage.md#access-blobs-using-azure-powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
