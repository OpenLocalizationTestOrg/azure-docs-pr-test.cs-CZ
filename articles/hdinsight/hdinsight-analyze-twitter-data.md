---
title: "Analýza dat Twitteru pomocí Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Další informace o použití Hive k analýze dat Twitteru systému Hadoop v HDInsight k vyhledání využití četnost určité slovo."
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
ms.openlocfilehash: 711d364c36c3aba699326f4a76d42891ba3219fb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a><span data-ttu-id="87efb-103">Analýza dat Twitteru pomocí Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="87efb-103">Analyze Twitter data using Hive in HDInsight</span></span>
<span data-ttu-id="87efb-104">Sociální weby jsou jedním z hlavních řízení vynutí pro velké objemy dat přijetí.</span><span class="sxs-lookup"><span data-stu-id="87efb-104">Social websites are one of the major driving forces for big-data adoption.</span></span> <span data-ttu-id="87efb-105">Veřejná rozhraní API poskytované lokality jako Twitter jsou užitečné zdroje dat pro analýzu a pochopení oblíbených trendy.</span><span class="sxs-lookup"><span data-stu-id="87efb-105">Public APIs provided by sites like Twitter are a useful source of data for analyzing and understanding popular trends.</span></span>
<span data-ttu-id="87efb-106">V tomto kurzu získáte tweetů pomocí Twitteru, streamování rozhraní API a pak použít Apache Hive v Azure HDInsight k získání seznamu Twitter uživatelů, kteří odeslané většina tweetů, které obsahovaly určité slovo.</span><span class="sxs-lookup"><span data-stu-id="87efb-106">In this tutorial, you will get tweets by using a Twitter streaming API, and then use Apache Hive on Azure HDInsight to get a list of Twitter users who sent the most tweets that contained a certain word.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="87efb-107">Kroky v tomto dokumentu vyžadují cluster HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="87efb-107">The steps in this document require a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="87efb-108">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="87efb-108">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="87efb-109">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="87efb-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="87efb-110">Konkrétní kroky do clusteru se systémem Linux najdete v tématu [Twitter analýza dat pomocí Hive v HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).</span><span class="sxs-lookup"><span data-stu-id="87efb-110">For steps specific to a Linux-based cluster, see [Analyze Twitter data using Hive in HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87efb-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="87efb-111">Prerequisites</span></span>
<span data-ttu-id="87efb-112">Je nutné, abyste před zahájením tohoto kurzu měli tyto položky:</span><span class="sxs-lookup"><span data-stu-id="87efb-112">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="87efb-113">**Pracovní stanice** pomocí prostředí Azure PowerShell nainstalovaný a nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="87efb-113">**A workstation** with Azure PowerShell installed and configured.</span></span>

    <span data-ttu-id="87efb-114">Spuštění skriptů prostředí Windows PowerShell, musíte spustit prostředí Azure PowerShell jako správce a nastavte zásady spouštění *RemoteSigned*.</span><span class="sxs-lookup"><span data-stu-id="87efb-114">To execute Windows PowerShell scripts, you must run Azure PowerShell as administrator and set the execution policy to *RemoteSigned*.</span></span> <span data-ttu-id="87efb-115">V tématu [spustit prostředí Windows PowerShell skripty][powershell-script].</span><span class="sxs-lookup"><span data-stu-id="87efb-115">See [Run Windows PowerShell scripts][powershell-script].</span></span>

    <span data-ttu-id="87efb-116">Před spuštěním skriptů prostředí Windows PowerShell, zkontrolujte, zda že jste připojeni k předplatnému Azure pomocí následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="87efb-116">Before running Windows PowerShell scripts, make sure you are connected to your Azure subscription by using the following cmdlet:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="87efb-117">Pokud máte víc předplatných Azure, použijte následující rutinu nastavte aktuální předplatné:</span><span class="sxs-lookup"><span data-stu-id="87efb-117">If you have multiple Azure subscriptions, use the following cmdlet to set the current subscription:</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="87efb-118">Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a k 1. lednu 2017 jsme ji odebrali.</span><span class="sxs-lookup"><span data-stu-id="87efb-118">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="87efb-119">Kroky v tomto dokumentu používají nové rutiny služby HDInsight, které pracují s Azure Resource Managerem.</span><span class="sxs-lookup"><span data-stu-id="87efb-119">The steps in this document use the new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="87efb-120">Podle postupu v tématu [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) si nainstalujte nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87efb-120">Please follow the steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) to install the latest version of Azure PowerShell.</span></span> <span data-ttu-id="87efb-121">Pokud máte skripty, které je potřeba upravit tak, aby používaly nové rutiny, které pracují s nástrojem Azure Resource Manager, najdete další informace v tématu [Migrace na vývojové nástroje založené na Azure Resource Manageru pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="87efb-121">If you have scripts that need to be modified to use the new cmdlets that work with Azure Resource Manager, see [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

* <span data-ttu-id="87efb-122">**Cluster Azure HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="87efb-122">**An Azure HDInsight cluster**.</span></span> <span data-ttu-id="87efb-123">Postup při zřizování clusteru naleznete v tématu [začněte používat HDInsight] [ hdinsight-get-started] nebo [zřizování clusterů HDInsight][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="87efb-123">For instructions on cluster provisioning, see [Get started using HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="87efb-124">Později v tomto kurzu budete potřebovat název clusteru.</span><span class="sxs-lookup"><span data-stu-id="87efb-124">You will need the cluster name later in the tutorial.</span></span>

<span data-ttu-id="87efb-125">Následující tabulka uvádí soubory používané v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="87efb-125">The following table lists the files used in this tutorial:</span></span>

| <span data-ttu-id="87efb-126">Soubory</span><span class="sxs-lookup"><span data-stu-id="87efb-126">Files</span></span> | <span data-ttu-id="87efb-127">Popis</span><span class="sxs-lookup"><span data-stu-id="87efb-127">Description</span></span> |
| --- | --- |
| <span data-ttu-id="87efb-128">/tutorials/Twitter/data/tweets.txt</span><span class="sxs-lookup"><span data-stu-id="87efb-128">/tutorials/twitter/data/tweets.txt</span></span> |<span data-ttu-id="87efb-129">Zdroj dat pro úlohy Hive.</span><span class="sxs-lookup"><span data-stu-id="87efb-129">The source data for the Hive job.</span></span> |
| <span data-ttu-id="87efb-130">/tutorials/Twitter/Output</span><span class="sxs-lookup"><span data-stu-id="87efb-130">/tutorials/twitter/output</span></span> |<span data-ttu-id="87efb-131">Výstupní složky pro úlohy Hive.</span><span class="sxs-lookup"><span data-stu-id="87efb-131">The output folder for the Hive job.</span></span> <span data-ttu-id="87efb-132">Je název souboru výstup úlohy Hive výchozí **000000_0**.</span><span class="sxs-lookup"><span data-stu-id="87efb-132">The default Hive job output file name is **000000_0**.</span></span> |
| <span data-ttu-id="87efb-133">tutorials/Twitter/Twitter.hql</span><span class="sxs-lookup"><span data-stu-id="87efb-133">tutorials/twitter/twitter.hql</span></span> |<span data-ttu-id="87efb-134">Soubor skriptu HiveQL.</span><span class="sxs-lookup"><span data-stu-id="87efb-134">The HiveQL script file.</span></span> |
| <span data-ttu-id="87efb-135">/tutorials/Twitter/JobStatus</span><span class="sxs-lookup"><span data-stu-id="87efb-135">/tutorials/twitter/jobstatus</span></span> |<span data-ttu-id="87efb-136">Stav úlohy Hadoop.</span><span class="sxs-lookup"><span data-stu-id="87efb-136">The Hadoop job status.</span></span> |

## <a name="get-twitter-feed"></a><span data-ttu-id="87efb-137">Get Twitteru</span><span class="sxs-lookup"><span data-stu-id="87efb-137">Get Twitter feed</span></span>
<span data-ttu-id="87efb-138">V tomto kurzu budete používat [Twitter streamování rozhraní API][twitter-streaming-api].</span><span class="sxs-lookup"><span data-stu-id="87efb-138">In this tutorial, you will use the [Twitter streaming APIs][twitter-streaming-api].</span></span> <span data-ttu-id="87efb-139">Konkrétní Twitter streamování použijete rozhraní API je [stavy nebo filtr][twitter-statuses-filter].</span><span class="sxs-lookup"><span data-stu-id="87efb-139">The specific Twitter streaming API you will use is [statuses/filter][twitter-statuses-filter].</span></span>

> [!NOTE]
> <span data-ttu-id="87efb-140">Soubor obsahující 10 000 tweetů a soubor skriptu Hive (popsaná v další části) byly odeslány ve veřejném kontejneru Blob.</span><span class="sxs-lookup"><span data-stu-id="87efb-140">A file containing 10,000 tweets and the Hive script file (covered in the next section) have been uploaded in a public Blob container.</span></span> <span data-ttu-id="87efb-141">Pokud chcete použít odeslané soubory, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="87efb-141">You can skip this section if you want to use the uploaded files.</span></span>

<span data-ttu-id="87efb-142">[Tweetů data](https://dev.twitter.com/docs/platform-objects/tweets) je uložený ve formátu JavaScript Object Notation (JSON), která obsahuje komplexní vnořené struktury.</span><span class="sxs-lookup"><span data-stu-id="87efb-142">[Tweets data](https://dev.twitter.com/docs/platform-objects/tweets) is stored in the JavaScript Object Notation (JSON) format that contains a complex nested structure.</span></span> <span data-ttu-id="87efb-143">Místo psaní velkého počtu řádků kódu s použitím konvenční programovací jazyk, můžete převést tuto strukturu vnořené do tabulky Hive, tak, aby může být dotazován podle jazyka SQL (Structured Query) – jako jazyka nazývaného HiveQL.</span><span class="sxs-lookup"><span data-stu-id="87efb-143">Instead of writing many lines of code by using a conventional programming language, you can transform this nested structure into a Hive table, so that it can be queried by a Structured Query Language (SQL)-like language called HiveQL.</span></span>

<span data-ttu-id="87efb-144">K poskytování autorizovaný přístup k jeho API používá Twitter OAuth.</span><span class="sxs-lookup"><span data-stu-id="87efb-144">Twitter uses OAuth to provide authorized access to its API.</span></span> <span data-ttu-id="87efb-145">OAuth je ověřovací protokol, který umožňuje uživatelům schválit aplikace tak, aby fungoval na ně bez sdílení své heslo.</span><span class="sxs-lookup"><span data-stu-id="87efb-145">OAuth is an authentication protocol that allows users to approve applications to act on their behalf without sharing their password.</span></span> <span data-ttu-id="87efb-146">Další informace najdete na [oauth.net](http://oauth.net/) nebo vynikající [Průvodce začátečníka OAuth](http://hueniverse.com/oauth/) z Hueniverse.</span><span class="sxs-lookup"><span data-stu-id="87efb-146">More information can be found at [oauth.net](http://oauth.net/) or in the excellent [Beginner's Guide to OAuth](http://hueniverse.com/oauth/) from Hueniverse.</span></span>

<span data-ttu-id="87efb-147">Prvním krokem k používají OAuth je vytvořit novou aplikaci na webu služby Twitter Developer.</span><span class="sxs-lookup"><span data-stu-id="87efb-147">The first step to use OAuth is to create a new application on the Twitter Developer site.</span></span>

<span data-ttu-id="87efb-148">**Vytvořit aplikaci služby Twitter.**</span><span class="sxs-lookup"><span data-stu-id="87efb-148">**To create a Twitter application**</span></span>

1. <span data-ttu-id="87efb-149">Přihlaste se k [https://apps.twitter.com/](https://apps.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="87efb-149">Sign in to [https://apps.twitter.com/](https://apps.twitter.com/).</span></span> <span data-ttu-id="87efb-150">Klikněte na tlačítko **nyní** odkaz, pokud nemáte účet služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="87efb-150">Click the **Sign up now** link if you don't have a Twitter account.</span></span>
2. <span data-ttu-id="87efb-151">Klikněte na tlačítko **vytvořte novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="87efb-151">Click **Create New App**.</span></span>
3. <span data-ttu-id="87efb-152">Zadejte **název**, **popis**, **webu**.</span><span class="sxs-lookup"><span data-stu-id="87efb-152">Enter **Name**, **Description**, **Website**.</span></span> <span data-ttu-id="87efb-153">Můžete použít pro adresu URL **webu** pole.</span><span class="sxs-lookup"><span data-stu-id="87efb-153">You can make up a URL for the **Website** field.</span></span> <span data-ttu-id="87efb-154">Následující tabulka uvádí některé ukázkové hodnoty použít:</span><span class="sxs-lookup"><span data-stu-id="87efb-154">The following table shows some sample values to use:</span></span>

   | <span data-ttu-id="87efb-155">Pole</span><span class="sxs-lookup"><span data-stu-id="87efb-155">Field</span></span> | <span data-ttu-id="87efb-156">Hodnota</span><span class="sxs-lookup"><span data-stu-id="87efb-156">Value</span></span> |
   | --- | --- |
   |  <span data-ttu-id="87efb-157">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87efb-157">Name</span></span> |<span data-ttu-id="87efb-158">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="87efb-158">MyHDInsightApp</span></span> |
   |  <span data-ttu-id="87efb-159">Popis</span><span class="sxs-lookup"><span data-stu-id="87efb-159">Description</span></span> |<span data-ttu-id="87efb-160">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="87efb-160">MyHDInsightApp</span></span> |
   |  <span data-ttu-id="87efb-161">Web</span><span class="sxs-lookup"><span data-stu-id="87efb-161">Website</span></span> |<span data-ttu-id="87efb-162">http://www.myhdinsightapp.com</span><span class="sxs-lookup"><span data-stu-id="87efb-162">http://www.myhdinsightapp.com</span></span> |
4. <span data-ttu-id="87efb-163">Zkontrolujte **souhlasím**a potom klikněte na **vytvořit aplikaci služby Twitter**.</span><span class="sxs-lookup"><span data-stu-id="87efb-163">Check **Yes, I agree**, and then click **Create your Twitter application**.</span></span>
5. <span data-ttu-id="87efb-164">Klikněte **oprávnění** kartě.</span><span class="sxs-lookup"><span data-stu-id="87efb-164">Click the **Permissions** tab.</span></span> <span data-ttu-id="87efb-165">Výchozí oprávnění je **jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="87efb-165">The default permission is **Read only**.</span></span> <span data-ttu-id="87efb-166">Toto je dostatečný pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="87efb-166">This is sufficient for this tutorial.</span></span>
6. <span data-ttu-id="87efb-167">Klikněte **klíče a přístupové tokeny** kartě.</span><span class="sxs-lookup"><span data-stu-id="87efb-167">Click the **Keys and Access Tokens** tab.</span></span>
7. <span data-ttu-id="87efb-168">Klikněte na tlačítko **vytvořit můj přístupový token**.</span><span class="sxs-lookup"><span data-stu-id="87efb-168">Click **Create my access token**.</span></span>
8. <span data-ttu-id="87efb-169">Klikněte na tlačítko **testovací OAuth** v pravém horním rohu stránky.</span><span class="sxs-lookup"><span data-stu-id="87efb-169">Click **Test OAuth** in the upper-right corner of the page.</span></span>
9. <span data-ttu-id="87efb-170">Zapište **uživatelský klíč**, **uživatelský tajný klíč**, **přístupový token**, a **tajný klíč přístupového tokenu**.</span><span class="sxs-lookup"><span data-stu-id="87efb-170">Write down **consumer key**, **Consumer secret**, **Access token**, and **Access token secret**.</span></span> <span data-ttu-id="87efb-171">Později v tomto kurzu budete potřebovat hodnoty.</span><span class="sxs-lookup"><span data-stu-id="87efb-171">You will need the values later in the tutorial.</span></span>

<span data-ttu-id="87efb-172">V tomto kurzu budete používat prostředí Windows PowerShell aby volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="87efb-172">In this tutorial, you will use Windows PowerShell to make the web service call.</span></span> <span data-ttu-id="87efb-173">C# .NET ukázku, najdete v části [analýzu v reálném čase sentimentu Twitter s HBase v HDInsight][hdinsight-hbase-twitter-sentiment].</span><span class="sxs-lookup"><span data-stu-id="87efb-173">For a .NET C# sample, see [Analyze real-time Twitter sentiment with HBase in HDInsight][hdinsight-hbase-twitter-sentiment].</span></span> <span data-ttu-id="87efb-174">Dalších oblíbených nástroj volání webové služby je [ *Curl*][curl].</span><span class="sxs-lookup"><span data-stu-id="87efb-174">The other popular tool to make web service calls is [*Curl*][curl].</span></span> <span data-ttu-id="87efb-175">Curl si můžete stáhnout z [sem][curl-download].</span><span class="sxs-lookup"><span data-stu-id="87efb-175">Curl can be downloaded from [here][curl-download].</span></span>

> [!NOTE]
> <span data-ttu-id="87efb-176">Pokud použijete příkaz curl v systému Windows, použijte dvojité uvozovky místo jednoduchých uvozovek a být pro hodnoty možnosti.</span><span class="sxs-lookup"><span data-stu-id="87efb-176">When you use the curl command in Windows, use double quotes instead of single quotes for the option values.</span></span>

<span data-ttu-id="87efb-177">**Chcete-li získat tweetů**</span><span class="sxs-lookup"><span data-stu-id="87efb-177">**To get tweets**</span></span>

1. <span data-ttu-id="87efb-178">Otevřete prostředí Windows PowerShell Integrované skriptovací prostředí (ISE).</span><span class="sxs-lookup"><span data-stu-id="87efb-178">Open the Windows PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="87efb-179">(Na obrazovce Start pro Windows 8, zadejte **PowerShell_ISE** a pak klikněte na **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="87efb-179">(On the Windows 8 Start screen, type **PowerShell_ISE** and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="87efb-180">V tématu [spuštění prostředí Windows PowerShell v systému Windows 8 a Windows][powershell-start].)</span><span class="sxs-lookup"><span data-stu-id="87efb-180">See [Start Windows PowerShell on Windows 8 and Windows][powershell-start].)</span></span>
2. <span data-ttu-id="87efb-181">Zkopírujte následující skript do podokna skriptu:</span><span class="sxs-lookup"><span data-stu-id="87efb-181">Copy the following script into the script pane:</span></span>

    ```powershell
    #region - variables and constants
    $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

    # Enter the OAuth information for your Twitter application
    $oauth_consumer_key = "<TwitterAppConsumerKey>";
    $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
    $oauth_token = "<TwitterAppAccessToken>";
    $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

    $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

    $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
    $track = [System.Uri]::EscapeDataString($trackString);
    $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
    #endregion

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    Login-AzureRmAccount
    #endregion

    #region - Create a block blob object for writing tweets into Blob storage
    Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $containerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

    Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
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

    #region - Write tweets to Blob storage
    Write-Host "Write to the destination blob ..." -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    $sReader.close()
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. <span data-ttu-id="87efb-182">Nastavte první pět až osm proměnné ve skriptu:</span><span class="sxs-lookup"><span data-stu-id="87efb-182">Set the first five to eight variables in the script:</span></span>

    <span data-ttu-id="87efb-183">Proměnná</span><span class="sxs-lookup"><span data-stu-id="87efb-183">Variable</span></span>|<span data-ttu-id="87efb-184">Popis</span><span class="sxs-lookup"><span data-stu-id="87efb-184">Description</span></span>
    ---|---
    <span data-ttu-id="87efb-185">$clusterName</span><span class="sxs-lookup"><span data-stu-id="87efb-185">$clusterName</span></span>|<span data-ttu-id="87efb-186">Toto je název clusteru HDInsight, ve které chcete aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="87efb-186">This is the name of the HDInsight cluster where you want to run the application.</span></span>
    <span data-ttu-id="87efb-187">$oauth_consumer_key</span><span class="sxs-lookup"><span data-stu-id="87efb-187">$oauth_consumer_key</span></span>|<span data-ttu-id="87efb-188">Toto je aplikace Twitter **uživatelský klíč** jste si poznamenali dříve při vytváření aplikace služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="87efb-188">This is the Twitter application **consumer key** you wrote down earlier when you created the Twitter application.</span></span>
    <span data-ttu-id="87efb-189">$oauth_consumer_secret</span><span class="sxs-lookup"><span data-stu-id="87efb-189">$oauth_consumer_secret</span></span>|<span data-ttu-id="87efb-190">Toto je aplikace Twitter **uživatelský tajný klíč** jste si poznamenali dříve.</span><span class="sxs-lookup"><span data-stu-id="87efb-190">This is the Twitter application **consumer secret** you wrote down earlier.</span></span>
    <span data-ttu-id="87efb-191">$oauth_token</span><span class="sxs-lookup"><span data-stu-id="87efb-191">$oauth_token</span></span>|<span data-ttu-id="87efb-192">Toto je aplikace Twitter **přístupový token** jste si poznamenali dříve.</span><span class="sxs-lookup"><span data-stu-id="87efb-192">This is the Twitter application **access token** you wrote down earlier.</span></span>
    <span data-ttu-id="87efb-193">$oauth_token_secret</span><span class="sxs-lookup"><span data-stu-id="87efb-193">$oauth_token_secret</span></span>|<span data-ttu-id="87efb-194">Toto je aplikace Twitter **tajný klíč přístupového tokenu** jste si poznamenali dříve.</span><span class="sxs-lookup"><span data-stu-id="87efb-194">This is the Twitter application **access token secret** you wrote down earlier.</span></span>
    <span data-ttu-id="87efb-195">$destBlobName</span><span class="sxs-lookup"><span data-stu-id="87efb-195">$destBlobName</span></span>|<span data-ttu-id="87efb-196">Toto je název výstupního objektu blob.</span><span class="sxs-lookup"><span data-stu-id="87efb-196">This is the output blob name.</span></span> <span data-ttu-id="87efb-197">Výchozí hodnota je **tutorials/twitter/data/tweets.txt**.</span><span class="sxs-lookup"><span data-stu-id="87efb-197">The default value is **tutorials/twitter/data/tweets.txt**.</span></span> <span data-ttu-id="87efb-198">Pokud změníte výchozí hodnotu, budete muset aktualizovat skriptů prostředí Windows PowerShell odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="87efb-198">If you change the default value, you will need to update the Windows PowerShell scripts accordingly.</span></span>
    <span data-ttu-id="87efb-199">$trackString</span><span class="sxs-lookup"><span data-stu-id="87efb-199">$trackString</span></span>|<span data-ttu-id="87efb-200">Webová služba vrátí tweetů související s Tato klíčová slova.</span><span class="sxs-lookup"><span data-stu-id="87efb-200">The web service will return tweets related to these keywords.</span></span> <span data-ttu-id="87efb-201">Výchozí hodnota je **Azure Cloud, HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="87efb-201">The default value is **Azure, Cloud, HDInsight**.</span></span> <span data-ttu-id="87efb-202">Pokud změníte výchozí hodnotu, bude odpovídajícím způsobem aktualizovat skriptů prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87efb-202">If you change the default value, you will update the Windows PowerShell scripts accordingly.</span></span>
    <span data-ttu-id="87efb-203">$lineMax</span><span class="sxs-lookup"><span data-stu-id="87efb-203">$lineMax</span></span>|<span data-ttu-id="87efb-204">Hodnota určuje, kolik tweetů, skript bude číst.</span><span class="sxs-lookup"><span data-stu-id="87efb-204">The value determines how many tweets the script will read.</span></span> <span data-ttu-id="87efb-205">Číst 100 tweetů trvá přibližně tři minuty.</span><span class="sxs-lookup"><span data-stu-id="87efb-205">It takes about three minutes to read 100 tweets.</span></span> <span data-ttu-id="87efb-206">Můžete nastavit vyšší číslo, ale bude trvat delší dobu ke stažení.</span><span class="sxs-lookup"><span data-stu-id="87efb-206">You can set a larger number, but it will take more time to download.</span></span>

1. <span data-ttu-id="87efb-207">Stisknutím klávesy **F5** spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="87efb-207">Press **F5** to run the script.</span></span> <span data-ttu-id="87efb-208">Pokud narazíte na problémy, jako řešení, vyberte všechny řádky a stiskněte klávesu **F8**.</span><span class="sxs-lookup"><span data-stu-id="87efb-208">If you run into problems, as a workaround, select all the lines, and then press **F8**.</span></span>
2. <span data-ttu-id="87efb-209">Zobrazí se "Dokončit!"</span><span class="sxs-lookup"><span data-stu-id="87efb-209">You shall see "Complete!"</span></span> <span data-ttu-id="87efb-210">na konci výstupu.</span><span class="sxs-lookup"><span data-stu-id="87efb-210">at the end of the output.</span></span> <span data-ttu-id="87efb-211">Chybové zprávy, se zobrazí červeně.</span><span class="sxs-lookup"><span data-stu-id="87efb-211">Any error messages will be displayed in red.</span></span>

<span data-ttu-id="87efb-212">Jako postup ověření, můžete zkontrolovat výstupní soubor **/tutorials/twitter/data/tweets.txt**, ve službě Azure Blob storage pomocí Průzkumníka úložiště Azure nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87efb-212">As a validation procedure, you can check the output file, **/tutorials/twitter/data/tweets.txt**, on your Azure Blob storage by using an Azure storage explorer or Azure PowerShell.</span></span> <span data-ttu-id="87efb-213">Ukázkový skript prostředí Windows PowerShell pro výpis soubory, najdete v části [používání Blob storage s HDInsight][hdinsight-storage-powershell].</span><span class="sxs-lookup"><span data-stu-id="87efb-213">For a sample Windows PowerShell script for listing files, see [Use Blob storage with HDInsight][hdinsight-storage-powershell].</span></span>

## <a name="create-hiveql-script"></a><span data-ttu-id="87efb-214">Vytvořit skript HiveQL</span><span class="sxs-lookup"><span data-stu-id="87efb-214">Create HiveQL script</span></span>
<span data-ttu-id="87efb-215">Pomocí Azure PowerShell, můžete současně spustit více příkazy HiveQL jeden nebo balíček příkaz HiveQL do souboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="87efb-215">Using Azure PowerShell, you can run multiple HiveQL statements one at a time, or package the HiveQL statement into a script file.</span></span> <span data-ttu-id="87efb-216">V tomto kurzu vytvoříte skript HiveQL.</span><span class="sxs-lookup"><span data-stu-id="87efb-216">In this tutorial, you will create a HiveQL script.</span></span> <span data-ttu-id="87efb-217">Soubor skriptu musí být nahrán do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="87efb-217">The script file must be uploaded to Azure Blob storage.</span></span> <span data-ttu-id="87efb-218">V další části bude spuštěn soubor skriptu pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87efb-218">In the next section, you will run the script file by using Azure PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="87efb-219">Soubor skriptu Hive a soubor obsahující 10 000 tweetů byly odeslány ve veřejném kontejneru Blob.</span><span class="sxs-lookup"><span data-stu-id="87efb-219">The Hive script file and a file containing 10,000 tweets have been uploaded in a public Blob container.</span></span> <span data-ttu-id="87efb-220">Pokud chcete použít odeslané soubory, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="87efb-220">You can skip this section if you want to use the uploaded files.</span></span>

<span data-ttu-id="87efb-221">Skript HiveQL provede následující:</span><span class="sxs-lookup"><span data-stu-id="87efb-221">The HiveQL script will perform the following:</span></span>

1. <span data-ttu-id="87efb-222">**Odpojit tabulku tweets_raw** v případě, že v tabulce již existuje.</span><span class="sxs-lookup"><span data-stu-id="87efb-222">**Drop the tweets_raw table** in case the table already exists.</span></span>
2. <span data-ttu-id="87efb-223">**Vytvoří tabulku Hive tweets_raw**.</span><span class="sxs-lookup"><span data-stu-id="87efb-223">**Create the tweets_raw Hive table**.</span></span> <span data-ttu-id="87efb-224">Tato dočasná tabulka strukturovaných Hive uchovává data pro další extrakce, transformace a načítání (ETL) zpracování.</span><span class="sxs-lookup"><span data-stu-id="87efb-224">This temporary Hive structured table holds the data for further extract, transform, and load (ETL) processing.</span></span> <span data-ttu-id="87efb-225">Informace o oddílech najdete v tématu [Hive kurzu][apache-hive-tutorial].</span><span class="sxs-lookup"><span data-stu-id="87efb-225">For information on partitions, see [Hive tutorial][apache-hive-tutorial].</span></span>
3. <span data-ttu-id="87efb-226">**Načtení dat** ze zdrojové složky, /tutorials/twitter/data.</span><span class="sxs-lookup"><span data-stu-id="87efb-226">**Load data** from the source folder, /tutorials/twitter/data.</span></span> <span data-ttu-id="87efb-227">Datovou sadu tweetů velké ve vnořených formátu JSON má nyní převedeny na dočasné struktura tabulky Hive.</span><span class="sxs-lookup"><span data-stu-id="87efb-227">The large tweets dataset in nested JSON format has now been transformed into a temporary Hive table structure.</span></span>
4. <span data-ttu-id="87efb-228">**Odpojit tabulku tweetů** v případě, že v tabulce již existuje.</span><span class="sxs-lookup"><span data-stu-id="87efb-228">**Drop the tweets table** in case the table already exists.</span></span>
5. <span data-ttu-id="87efb-229">**Vytvoření tabulky tweetů**.</span><span class="sxs-lookup"><span data-stu-id="87efb-229">**Create the tweets table**.</span></span> <span data-ttu-id="87efb-230">Předtím, než proti datovou sadu tweetů se můžete dotazovat pomocí Hive, budete muset spustit jiným procesem ETL.</span><span class="sxs-lookup"><span data-stu-id="87efb-230">Before you can query against the tweets dataset by using Hive, you need to run another ETL process.</span></span> <span data-ttu-id="87efb-231">Tento proces ETL definuje podrobnější schématu tabulky pro data, která máte uložená v tabulce "twitter_raw".</span><span class="sxs-lookup"><span data-stu-id="87efb-231">This ETL process defines a more detailed table schema for the data that you have stored in the "twitter_raw" table.</span></span>
6. <span data-ttu-id="87efb-232">**Vložit přepsat tabulku**.</span><span class="sxs-lookup"><span data-stu-id="87efb-232">**Insert overwrite table**.</span></span> <span data-ttu-id="87efb-233">Tento komplexní skript Hive se ji sadu dlouho úloh MapReduce clusterem Hadoop.</span><span class="sxs-lookup"><span data-stu-id="87efb-233">This complex Hive script will kick off a set of long MapReduce jobs by the Hadoop cluster.</span></span> <span data-ttu-id="87efb-234">V závislosti na datovou sadu a velikost clusteru to může trvat asi 10 minut.</span><span class="sxs-lookup"><span data-stu-id="87efb-234">Depending on your dataset and the size of your cluster, this could take about 10 minutes.</span></span>
7. <span data-ttu-id="87efb-235">**Vložení přepsat directory**.</span><span class="sxs-lookup"><span data-stu-id="87efb-235">**Insert overwrite directory**.</span></span> <span data-ttu-id="87efb-236">Spuštění dotazu a výstupní datové sady do souboru.</span><span class="sxs-lookup"><span data-stu-id="87efb-236">Run a query and output the dataset to a file.</span></span> <span data-ttu-id="87efb-237">Tento dotaz vrátí seznam Twitter uživatelů, kteří odeslané většina tweetů, obsahující slovo "Azure".</span><span class="sxs-lookup"><span data-stu-id="87efb-237">This query will return a list of Twitter users who sent most tweets that contained the word "Azure".</span></span>

<span data-ttu-id="87efb-238">**K vytvoření skriptu Hive a nahrajte ho do Azure**</span><span class="sxs-lookup"><span data-stu-id="87efb-238">**To create a Hive script and upload it to Azure**</span></span>

1. <span data-ttu-id="87efb-239">Otevřete Windows PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="87efb-239">Open Windows PowerShell ISE.</span></span>
2. <span data-ttu-id="87efb-240">Zkopírujte následující skript do podokna skriptu:</span><span class="sxs-lookup"><span data-stu-id="87efb-240">Copy the following script into the script pane:</span></span>

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

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #endregion

    #region - Create a block blob object for writing the Hive script file
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

    Write-Host "Define the connection string ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)

    Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    $writeStream.Writeline($hqlStatements)
    #endregion

    #region - Write the Hive script file to Blob storage
    Write-Host "Write to the destination blob ... " -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $hqlScriptBlob.UploadFromStream($memStream)
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. <span data-ttu-id="87efb-241">Nastavte první dvě proměnné ve skriptu:</span><span class="sxs-lookup"><span data-stu-id="87efb-241">Set the first two variables in the script:</span></span>

   | <span data-ttu-id="87efb-242">Proměnná</span><span class="sxs-lookup"><span data-stu-id="87efb-242">Variable</span></span> | <span data-ttu-id="87efb-243">Popis</span><span class="sxs-lookup"><span data-stu-id="87efb-243">Description</span></span> |
   | --- | --- |
   |  <span data-ttu-id="87efb-244">$clusterName</span><span class="sxs-lookup"><span data-stu-id="87efb-244">$clusterName</span></span> |<span data-ttu-id="87efb-245">Zadejte název clusteru HDInsight, ve které chcete aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="87efb-245">Enter the HDInsight cluster name where you want to run the application.</span></span> |
   |  <span data-ttu-id="87efb-246">$subscriptionID</span><span class="sxs-lookup"><span data-stu-id="87efb-246">$subscriptionID</span></span> |<span data-ttu-id="87efb-247">Zadejte ID vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="87efb-247">Enter your Azure subscription ID.</span></span> |
   |  <span data-ttu-id="87efb-248">$sourceDataPath</span><span class="sxs-lookup"><span data-stu-id="87efb-248">$sourceDataPath</span></span> |<span data-ttu-id="87efb-249">Umístění úložiště objektů Blob v Azure dotazy Hive, kde bude číst data z.</span><span class="sxs-lookup"><span data-stu-id="87efb-249">The Azure Blob storage location where the Hive queries will read the data from.</span></span> <span data-ttu-id="87efb-250">Nemusíte měnit tuto proměnnou.</span><span class="sxs-lookup"><span data-stu-id="87efb-250">You don't need to change this variable.</span></span> |
   |  <span data-ttu-id="87efb-251">$outputPath</span><span class="sxs-lookup"><span data-stu-id="87efb-251">$outputPath</span></span> |<span data-ttu-id="87efb-252">Umístění úložiště objektů Blob v Azure, kde bude dotazů Hive vypsání výsledků.</span><span class="sxs-lookup"><span data-stu-id="87efb-252">The Azure Blob storage location where the Hive queries will output the results.</span></span> <span data-ttu-id="87efb-253">Nemusíte měnit tuto proměnnou.</span><span class="sxs-lookup"><span data-stu-id="87efb-253">You don't need to change this variable.</span></span> |
   |  <span data-ttu-id="87efb-254">$hqlScriptFile</span><span class="sxs-lookup"><span data-stu-id="87efb-254">$hqlScriptFile</span></span> |<span data-ttu-id="87efb-255">Umístění a název souboru souboru skriptu HiveQL.</span><span class="sxs-lookup"><span data-stu-id="87efb-255">The location and the file name of the HiveQL script file.</span></span> <span data-ttu-id="87efb-256">Nemusíte měnit tuto proměnnou.</span><span class="sxs-lookup"><span data-stu-id="87efb-256">You don't need to change this variable.</span></span> |
4. <span data-ttu-id="87efb-257">Stisknutím klávesy **F5** spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="87efb-257">Press **F5** to run the script.</span></span> <span data-ttu-id="87efb-258">Pokud narazíte na problémy, jako řešení, vyberte všechny řádky a stiskněte klávesu **F8**.</span><span class="sxs-lookup"><span data-stu-id="87efb-258">If you run into problems, as a workaround, select all the lines, and then press **F8**.</span></span>
5. <span data-ttu-id="87efb-259">Zobrazí se "Dokončit!"</span><span class="sxs-lookup"><span data-stu-id="87efb-259">You shall see "Complete!"</span></span> <span data-ttu-id="87efb-260">na konci výstupu.</span><span class="sxs-lookup"><span data-stu-id="87efb-260">at the end of the output.</span></span> <span data-ttu-id="87efb-261">Chybové zprávy, se zobrazí červeně.</span><span class="sxs-lookup"><span data-stu-id="87efb-261">Any error messages will be displayed in red.</span></span>

<span data-ttu-id="87efb-262">Jako postup ověření, můžete zkontrolovat výstupní soubor **/tutorials/twitter/twitter.hql**, ve službě Azure Blob storage pomocí Průzkumníka úložiště Azure nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87efb-262">As a validation procedure, you can check the output file, **/tutorials/twitter/twitter.hql**, on your Azure Blob storage by using an Azure storage explorer or Azure PowerShell.</span></span> <span data-ttu-id="87efb-263">Ukázkový skript prostředí Windows PowerShell pro výpis soubory, najdete v části [používání Blob storage s HDInsight][hdinsight-storage-powershell].</span><span class="sxs-lookup"><span data-stu-id="87efb-263">For a sample Windows PowerShell script for listing files, see [Use Blob storage with HDInsight][hdinsight-storage-powershell].</span></span>

## <a name="process-twitter-data-by-using-hive"></a><span data-ttu-id="87efb-264">Zpracování dat Twitteru pomocí Hive</span><span class="sxs-lookup"><span data-stu-id="87efb-264">Process Twitter data by using Hive</span></span>
<span data-ttu-id="87efb-265">Dokončení veškeré práce přípravy.</span><span class="sxs-lookup"><span data-stu-id="87efb-265">You have finished all the preparation work.</span></span> <span data-ttu-id="87efb-266">Nyní můžete vyvolání skriptu Hive a zkontrolovat výsledky.</span><span class="sxs-lookup"><span data-stu-id="87efb-266">Now, you can invoke the Hive script and check the results.</span></span>

### <a name="submit-a-hive-job"></a><span data-ttu-id="87efb-267">Odeslání úlohy Hive</span><span class="sxs-lookup"><span data-stu-id="87efb-267">Submit a Hive job</span></span>
<span data-ttu-id="87efb-268">Pomocí následujícího skriptu prostředí Windows PowerShell pro spuštění skriptu Hive.</span><span class="sxs-lookup"><span data-stu-id="87efb-268">Use the following Windows PowerShell script to run the Hive script.</span></span> <span data-ttu-id="87efb-269">Musíte nastavit proměnnou první.</span><span class="sxs-lookup"><span data-stu-id="87efb-269">You will need to set the first variable.</span></span>

> [!NOTE]
> <span data-ttu-id="87efb-270">Chcete-li použít tweetů a HiveQL skript, který jste nahráli v posledních dvou částech, nastavte $hqlScriptFile na "/ tutorials/twitter/twitter.hql".</span><span class="sxs-lookup"><span data-stu-id="87efb-270">To use the tweets and the HiveQL script you uploaded in the last two sections, set $hqlScriptFile to "/tutorials/twitter/twitter.hql".</span></span> <span data-ttu-id="87efb-271">Chcete-li použít ty, které byly nahrány do veřejného objektu blob pro vás, nastavte $hqlScriptFile na "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".</span><span class="sxs-lookup"><span data-stu-id="87efb-271">To use the ones that have been uploaded to a public blob for you, set $hqlScriptFile to "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".</span></span>

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"
$httpUserName = "admin"
$httpUserPassword = "<HDInsight Cluster HTTP User Password>"

#use one of the following
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

# Create the HDInsight cluster
$pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential
$response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable

Write-Host "Display the standard error log ... " -ForegroundColor Green
$jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
#endregion
```

### <a name="check-the-results"></a><span data-ttu-id="87efb-272">Kontrola výsledků</span><span class="sxs-lookup"><span data-stu-id="87efb-272">Check the results</span></span>
<span data-ttu-id="87efb-273">Zkontrolujte výstup úlohy Hive pomocí následujícího skriptu prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="87efb-273">Use the following Windows PowerShell script to check the Hive job output.</span></span> <span data-ttu-id="87efb-274">Musíte nastavit první dvě proměnné.</span><span class="sxs-lookup"><span data-stu-id="87efb-274">You will need to set the first two variables.</span></span>

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"

$blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
#endregion

#region - Create an Azure storage context object
Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
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
Write-Host "Download the blob ..." -ForegroundColor Green
cd $HOME
Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force

Write-Host "Display the output ..." -ForegroundColor Green
Write-Host "==================================" -ForegroundColor Green
cat "./$blob"
Write-Host "==================================" -ForegroundColor Green
#end region
```

> [!NOTE]
> V tabulce Hive \001 používá jako oddělovač polí. <span data-ttu-id="87efb-276">Oddělovač, který není viditelný ve výstupu.</span><span class="sxs-lookup"><span data-stu-id="87efb-276">The delimiter is not visible in the output.</span></span>

<span data-ttu-id="87efb-277">Po výsledky analýzy byly umístěny do úložiště objektů Blob v Azure, můžete exportovat data na Azure SQL database nebo SQL server, export dat do aplikace Excel pomocí doplňku Power Query nebo připojení aplikace k datům pomocí ovladače ODBC Hive.</span><span class="sxs-lookup"><span data-stu-id="87efb-277">After the analysis results have been placed in Azure Blob storage, you can export the data to an Azure SQL database/SQL server, export the data to Excel by using Power Query, or connect your application to the data by using the Hive ODBC Driver.</span></span> <span data-ttu-id="87efb-278">Další informace najdete v tématu [Sqoop použití s HDInsight][hdinsight-use-sqoop], [analýza letu zpoždění dat pomocí HDInsight][hdinsight-analyze-flight-delay-data], [připojení aplikace Excel do prostředí HDInsight pomocí doplňku Power Query][hdinsight-power-query], a [připojení aplikace Excel do HDInsight pomocí ovladače ODBC Microsoft Hive][hdinsight-hive-odbc].</span><span class="sxs-lookup"><span data-stu-id="87efb-278">For more information, see [Use Sqoop with HDInsight][hdinsight-use-sqoop], [Analyze flight delay data using HDInsight][hdinsight-analyze-flight-delay-data], [Connect Excel to HDInsight with Power Query][hdinsight-power-query], and [Connect Excel to HDInsight with the Microsoft Hive ODBC Driver][hdinsight-hive-odbc].</span></span>

## <a name="next-steps"></a><span data-ttu-id="87efb-279">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87efb-279">Next steps</span></span>
<span data-ttu-id="87efb-280">V tomto kurzu jsme viděli, jak k transformaci datové sadě služby nestrukturovaných JSON do strukturovaných tabulku Hive k dotazování, prozkoumejte a analýze dat z Twitteru pomocí HDInsight v Azure.</span><span class="sxs-lookup"><span data-stu-id="87efb-280">In this tutorial we have seen how to transform an unstructured JSON dataset into a structured Hive table to query, explore, and analyze data from Twitter by using HDInsight on Azure.</span></span> <span data-ttu-id="87efb-281">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="87efb-281">To learn more, see:</span></span>

* <span data-ttu-id="87efb-282">[Začínáme s HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="87efb-282">[Get started with HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="87efb-283">[Analýza v reálném čase sentimentu Twitter s HBase v HDInsight][hdinsight-hbase-twitter-sentiment]</span><span class="sxs-lookup"><span data-stu-id="87efb-283">[Analyze real-time Twitter sentiment with HBase in HDInsight][hdinsight-hbase-twitter-sentiment]</span></span>
* <span data-ttu-id="87efb-284">[Analýza dat zpoždění letu pomocí HDInsight][hdinsight-analyze-flight-delay-data]</span><span class="sxs-lookup"><span data-stu-id="87efb-284">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-delay-data]</span></span>
* <span data-ttu-id="87efb-285">[Připojení Excelu k prostředí HDInsight pomocí doplňku Power Query][hdinsight-power-query]</span><span class="sxs-lookup"><span data-stu-id="87efb-285">[Connect Excel to HDInsight with Power Query][hdinsight-power-query]</span></span>
* <span data-ttu-id="87efb-286">[Připojení aplikace Excel do HDInsight pomocí ovladače ODBC Microsoft Hive][hdinsight-hive-odbc]</span><span class="sxs-lookup"><span data-stu-id="87efb-286">[Connect Excel to HDInsight with the Microsoft Hive ODBC Driver][hdinsight-hive-odbc]</span></span>
* <span data-ttu-id="87efb-287">[Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="87efb-287">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>

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
