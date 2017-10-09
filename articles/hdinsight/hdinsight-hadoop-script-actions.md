---
title: "aaaScript vývoj akcí s HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toocustomize Hadoop clusterů s akce skriptu. Akce skriptu lze použít tooinstall systémem Hadoop clusteru nebo toochange hello konfigurací aplikace nainstalované v clusteru s podporou další software."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 836d68a8-8b21-4d69-8b61-281a7fe67f21
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 4fc3a389df8a003f7129ab00b4cd9bc7ad81a419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a><span data-ttu-id="d5120-104">Vývoj skriptů akce skriptu pro clustery se systémem HDInsight Windows</span><span class="sxs-lookup"><span data-stu-id="d5120-104">Develop Script Action scripts for HDInsight Windows-based clusters</span></span>
<span data-ttu-id="d5120-105">Zjistěte, jak toowrite akce skriptu skriptů pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d5120-105">Learn how toowrite Script Action scripts for HDInsight.</span></span> <span data-ttu-id="d5120-106">Informace o použití akce skriptu skriptů najdete v tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="d5120-106">For information on using Script Action scripts, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md).</span></span> <span data-ttu-id="d5120-107">Hello stejný článek napsán pro clustery HDInsight se systémem Linux naleznete v části [vyvíjet akce skriptu skripty pro HDInsight](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="d5120-107">For hello same article written for Linux-based HDInsight clusters, see [Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions-linux.md).</span></span>



> [!IMPORTANT]
> <span data-ttu-id="d5120-108">Hello kroky v této dokumentu funguje jenom pro clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="d5120-108">hello steps in this document only work for Windows-based HDInsight clusters.</span></span> <span data-ttu-id="d5120-109">HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="d5120-109">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="d5120-110">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d5120-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d5120-111">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d5120-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="d5120-112">Informace o použití akce skriptu s clustery se systémem Linux najdete v tématu [vývoj akcí skriptů v prostředí HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="d5120-112">For information on using script actions with Linux-based clusters, see [Script action development with HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).</span></span>
>
>



<span data-ttu-id="d5120-113">Akce skriptu lze použít tooinstall systémem Hadoop clusteru nebo toochange hello konfigurací aplikace nainstalované v clusteru s podporou další software.</span><span class="sxs-lookup"><span data-stu-id="d5120-113">Script Action can be used tooinstall additional software running on a Hadoop cluster or toochange hello configuration of applications installed on a cluster.</span></span> <span data-ttu-id="d5120-114">Akce skriptů jsou skripty, které jsou spuštěny na uzlech clusteru hello při nasazených clusterů HDInsight, a jsou prováděna po uzly v clusteru hello dokončení konfigurace HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d5120-114">Script actions are scripts that run on hello cluster nodes when HDInsight clusters are deployed, and they are executed once nodes in hello cluster complete HDInsight configuration.</span></span> <span data-ttu-id="d5120-115">Akce skriptu se spustí v části oprávnění k účtu správce systému a poskytuje úplná přístupová práva toohello uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5120-115">A script action is executed under system admin account privileges and provides full access rights toohello cluster nodes.</span></span> <span data-ttu-id="d5120-116">Každý cluster lze zadat seznam toobe skript akce provést v hello pořadí, ve kterém jsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="d5120-116">Each cluster can be provided with a list of script actions toobe executed in hello order in which they are specified.</span></span>

> [!NOTE]
> <span data-ttu-id="d5120-117">Pokud se setkáte hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="d5120-117">If you experience hello following error message:</span></span>
>
> <span data-ttu-id="d5120-118">System.Management.Automation.CommandNotFoundException; ExceptionMessage: hello termín 'Uložit-HDIFile' nebyl rozpoznán jako hello název rutiny, funkce, soubor skriptu nebo spustitelného programu.</span><span class="sxs-lookup"><span data-stu-id="d5120-118">System.Management.Automation.CommandNotFoundException; ExceptionMessage : hello term 'Save-HDIFile' is not recognized as hello name of a cmdlet, function, script file, or operable program.</span></span> <span data-ttu-id="d5120-119">Kontrola pravopisu hello hello názvu, nebo pokud byl zahrnut cestu, ověřte, zda text hello cesta správné a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="d5120-119">Check hello spelling of hello name, or if a path was included, verify that hello path is correct and try again.</span></span>
> <span data-ttu-id="d5120-120">Je to proto, že jste nezahrnuli hello pomocné metody.</span><span class="sxs-lookup"><span data-stu-id="d5120-120">It is because you didn't include hello helper methods.</span></span>  <span data-ttu-id="d5120-121">V tématu [pomocné metody pro vlastní skripty](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).</span><span class="sxs-lookup"><span data-stu-id="d5120-121">See [Helper methods for custom scripts](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).</span></span>
>
>

## <a name="sample-scripts"></a><span data-ttu-id="d5120-122">Ukázkové skripty</span><span class="sxs-lookup"><span data-stu-id="d5120-122">Sample scripts</span></span>
<span data-ttu-id="d5120-123">Hello akce skriptu pro vytváření clusterů HDInsight v operačním systému Windows, je skript prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d5120-123">For creating HDInsight clusters on Windows operating system, hello Script Action is Azure PowerShell script.</span></span> <span data-ttu-id="d5120-124">Hello následující skript je ukázkou pro konfiguraci hello lokality konfigurační soubory:</span><span class="sxs-lookup"><span data-stu-id="d5120-124">hello following script is a sample for configuring hello site configuration files:</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable tooconfigure $ConfigFileName because it is not part of hello HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

<span data-ttu-id="d5120-125">skript Hello trvá čtyři parametry, hello název konfiguračního souboru, vlastnosti hello chcete toomodify, hodnota hello chcete tooset a popis.</span><span class="sxs-lookup"><span data-stu-id="d5120-125">hello script takes four parameters, hello configuration file name, hello property you want toomodify, hello value you want tooset, and a description.</span></span> <span data-ttu-id="d5120-126">Například:</span><span class="sxs-lookup"><span data-stu-id="d5120-126">For example:</span></span>

    hive-site.xml hive.metastore.client.socket.timeout 90

<span data-ttu-id="d5120-127">Tyto parametry nastaví hello hive.metastore.client.socket.timeout hodnotu too90 v souboru hive-site.xml hello.</span><span class="sxs-lookup"><span data-stu-id="d5120-127">These parameters sets hello hive.metastore.client.socket.timeout value too90 in hello hive-site.xml file.</span></span>  <span data-ttu-id="d5120-128">Hello výchozí hodnota je 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="d5120-128">hello default value is 60 seconds.</span></span>

<span data-ttu-id="d5120-129">Tento vzorový skript naleznete také v [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).</span><span class="sxs-lookup"><span data-stu-id="d5120-129">This sample script can also be found at [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).</span></span>

<span data-ttu-id="d5120-130">HDInsight nabízí několik skriptů tooinstall další součásti v clusterech HDInsight:</span><span class="sxs-lookup"><span data-stu-id="d5120-130">HDInsight provides several scripts tooinstall additional components on HDInsight clusters:</span></span>

| <span data-ttu-id="d5120-131">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="d5120-131">Name</span></span> | <span data-ttu-id="d5120-132">Skript</span><span class="sxs-lookup"><span data-stu-id="d5120-132">Script</span></span> |
| --- | --- |
| <span data-ttu-id="d5120-133">**Nainstalujte Spark**</span><span class="sxs-lookup"><span data-stu-id="d5120-133">**Install Spark**</span></span> |<span data-ttu-id="d5120-134">https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1.</span><span class="sxs-lookup"><span data-stu-id="d5120-134">https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1.</span></span> <span data-ttu-id="d5120-135">V tématu [instalací a použitím clustery Spark v HDInsight][hdinsight-install-spark].</span><span class="sxs-lookup"><span data-stu-id="d5120-135">See [Install and use Spark on HDInsight clusters][hdinsight-install-spark].</span></span> |
| <span data-ttu-id="d5120-136">**Nainstalujte jazyk R**</span><span class="sxs-lookup"><span data-stu-id="d5120-136">**Install R**</span></span> |<span data-ttu-id="d5120-137">https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1.</span><span class="sxs-lookup"><span data-stu-id="d5120-137">https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1.</span></span> <span data-ttu-id="d5120-138">V tématu [instalací a použitím R v clusterech HDInsight][hdinsight-r-scripts].</span><span class="sxs-lookup"><span data-stu-id="d5120-138">See [Install and use R on HDInsight clusters][hdinsight-r-scripts].</span></span> |
| <span data-ttu-id="d5120-139">**Nainstalujte Solr**</span><span class="sxs-lookup"><span data-stu-id="d5120-139">**Install Solr**</span></span> |<span data-ttu-id="d5120-140">https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="d5120-140">https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1.</span></span> <span data-ttu-id="d5120-141">V tématu [instalace a použití clusterů v HDInsight Solr](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="d5120-141">See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span> |
| <span data-ttu-id="d5120-142">- **Nainstalujte Giraph**</span><span class="sxs-lookup"><span data-stu-id="d5120-142">- **Install Giraph**</span></span> |<span data-ttu-id="d5120-143">https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="d5120-143">https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1.</span></span> <span data-ttu-id="d5120-144">V tématu [instalace a použití clusterů v HDInsight Giraph](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="d5120-144">See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span> |

<span data-ttu-id="d5120-145">Akce skriptu můžete nasadit z hello portál Azure, Azure PowerShell nebo pomocí hello HDInsight .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="d5120-145">Script Action can be deployed from hello Azure portal, Azure PowerShell or by using hello HDInsight .NET SDK.</span></span>  <span data-ttu-id="d5120-146">Další informace najdete v tématu [HDInsight přizpůsobit clustery pomocí akce skriptu][hdinsight-cluster-customize].</span><span class="sxs-lookup"><span data-stu-id="d5120-146">For more information, see [Customize HDInsight clusters using Script Action][hdinsight-cluster-customize].</span></span>

> [!NOTE]
> <span data-ttu-id="d5120-147">Ukázkové skripty Hello pracovat pouze s verze clusteru HDInsight verze 3.1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="d5120-147">hello sample scripts work only with HDInsight cluster version 3.1 or above.</span></span> <span data-ttu-id="d5120-148">Další informace o verzích clusterů HDInsight, naleznete v části [verze clusteru HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="d5120-148">For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>
>
>

## <a name="helper-methods-for-custom-scripts"></a><span data-ttu-id="d5120-149">Pomocné metody pro vlastní skripty</span><span class="sxs-lookup"><span data-stu-id="d5120-149">Helper methods for custom scripts</span></span>
<span data-ttu-id="d5120-150">Pomocné metody akcí skriptů jsou nástroje, které můžete použít při zápisu vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="d5120-150">Script Action helper methods are utilities that you can use while writing custom scripts.</span></span> <span data-ttu-id="d5120-151">Tyto metody jsou definovány v [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1)a můžou být součástí skripty pomocí hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="d5120-151">These methods are defined in [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1), and can be included in your scripts using hello following sample:</span></span>

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module toomake writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed tooload HDInsightUtilities module, exiting ...";
        exit;
    }

<span data-ttu-id="d5120-152">Zde jsou hello pomocné metody, které jsou poskytovány tento skript:</span><span class="sxs-lookup"><span data-stu-id="d5120-152">Here are hello helper methods that are provided by this script:</span></span>

| <span data-ttu-id="d5120-153">Pomocná metoda</span><span class="sxs-lookup"><span data-stu-id="d5120-153">Helper method</span></span> | <span data-ttu-id="d5120-154">Popis</span><span class="sxs-lookup"><span data-stu-id="d5120-154">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d5120-155">**Uložit HDIFile**</span><span class="sxs-lookup"><span data-stu-id="d5120-155">**Save-HDIFile**</span></span> |<span data-ttu-id="d5120-156">Stažení souboru z hello zadaný identifikátor URI (Uniform Resource) tooa umístění na hello místní disk, který je přidružen cluster pro uzly přiřazené toohello hello virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="d5120-156">Download a file from hello specified Uniform Resource Identifier (URI) tooa location on hello local disk that is associated with hello Azure VM node assigned toohello cluster.</span></span> |
| <span data-ttu-id="d5120-157">**Rozbalte položku HDIZippedFile**</span><span class="sxs-lookup"><span data-stu-id="d5120-157">**Expand-HDIZippedFile**</span></span> |<span data-ttu-id="d5120-158">Rozbalte soubor ZIP.</span><span class="sxs-lookup"><span data-stu-id="d5120-158">Unzip a zipped file.</span></span> |
| <span data-ttu-id="d5120-159">**Vyvolání HDICmdScript**</span><span class="sxs-lookup"><span data-stu-id="d5120-159">**Invoke-HDICmdScript**</span></span> |<span data-ttu-id="d5120-160">Spusťte skript z cmd.exe.</span><span class="sxs-lookup"><span data-stu-id="d5120-160">Run a script from cmd.exe.</span></span> |
| <span data-ttu-id="d5120-161">**Zápis HDILog**</span><span class="sxs-lookup"><span data-stu-id="d5120-161">**Write-HDILog**</span></span> |<span data-ttu-id="d5120-162">Zapište výstup z vlastní skript hello používané pro akci skriptu.</span><span class="sxs-lookup"><span data-stu-id="d5120-162">Write output from hello custom script used for a script action.</span></span> |
| <span data-ttu-id="d5120-163">**Get-Services**</span><span class="sxs-lookup"><span data-stu-id="d5120-163">**Get-Services**</span></span> |<span data-ttu-id="d5120-164">Získejte seznam služby spuštěné na počítači hello kde hello skript spustí.</span><span class="sxs-lookup"><span data-stu-id="d5120-164">Get a list of services running on hello machine where hello script executes.</span></span> |
| <span data-ttu-id="d5120-165">**Get-Service**</span><span class="sxs-lookup"><span data-stu-id="d5120-165">**Get-Service**</span></span> |<span data-ttu-id="d5120-166">S názvem hello konkrétní služby jako vstup, získat podrobné informace pro konkrétní službu (název služby, zpracovat ID, stavu, atd.) v počítači hello kde hello skript spustí.</span><span class="sxs-lookup"><span data-stu-id="d5120-166">With hello specific service name as input, get detailed information for a specific service (service name, process ID, state, etc.) on hello machine where hello script executes.</span></span> |
| <span data-ttu-id="d5120-167">**Get-HDIServices**</span><span class="sxs-lookup"><span data-stu-id="d5120-167">**Get-HDIServices**</span></span> |<span data-ttu-id="d5120-168">Získejte seznam služby HDInsight na hello počítači spuštěny, kde hello skript spustí.</span><span class="sxs-lookup"><span data-stu-id="d5120-168">Get a list of HDInsight services running on hello computer where hello script executes.</span></span> |
| <span data-ttu-id="d5120-169">**Get-HDIService**</span><span class="sxs-lookup"><span data-stu-id="d5120-169">**Get-HDIService**</span></span> |<span data-ttu-id="d5120-170">S hello konkrétní HDInsight název služby jako vstup, získat podrobné informace pro konkrétní službu (název služby, zpracovat ID, stavu, atd.) v počítači hello kde hello skript spustí.</span><span class="sxs-lookup"><span data-stu-id="d5120-170">With hello specific HDInsight service name as input, get detailed information for a specific service (service name, process ID, state, etc.) on hello machine where hello script executes.</span></span> |
| <span data-ttu-id="d5120-171">**Get-ServicesRunning**</span><span class="sxs-lookup"><span data-stu-id="d5120-171">**Get-ServicesRunning**</span></span> |<span data-ttu-id="d5120-172">Získáte seznam služeb, které jsou spuštěny v počítači hello kde hello skript spustí.</span><span class="sxs-lookup"><span data-stu-id="d5120-172">Get a list of services that are running on hello computer where hello script executes.</span></span> |
| <span data-ttu-id="d5120-173">**Get-ServiceRunning**</span><span class="sxs-lookup"><span data-stu-id="d5120-173">**Get-ServiceRunning**</span></span> |<span data-ttu-id="d5120-174">Zkontrolujte, zda specifické služby (podle názvu) je spuštěn v počítači hello, kde hello skript spustí.</span><span class="sxs-lookup"><span data-stu-id="d5120-174">Check if a specific service (by name) is running on hello computer where hello script executes.</span></span> |
| <span data-ttu-id="d5120-175">**Get-HDIServicesRunning**</span><span class="sxs-lookup"><span data-stu-id="d5120-175">**Get-HDIServicesRunning**</span></span> |<span data-ttu-id="d5120-176">Získejte seznam služby HDInsight na hello počítači spuštěny, kde hello skript spustí.</span><span class="sxs-lookup"><span data-stu-id="d5120-176">Get a list of HDInsight services running on hello computer where hello script executes.</span></span> |
| <span data-ttu-id="d5120-177">**Get-HDIServiceRunning**</span><span class="sxs-lookup"><span data-stu-id="d5120-177">**Get-HDIServiceRunning**</span></span> |<span data-ttu-id="d5120-178">Zkontrolujte, zda specifické služby HDInsight (podle názvu) je spuštěn na hello počítači, kde hello skript spustí.</span><span class="sxs-lookup"><span data-stu-id="d5120-178">Check if a specific HDInsight service (by name) is running on hello computer where hello script executes.</span></span> |
| <span data-ttu-id="d5120-179">**Get-HDIHadoopVersion**</span><span class="sxs-lookup"><span data-stu-id="d5120-179">**Get-HDIHadoopVersion**</span></span> |<span data-ttu-id="d5120-180">Vrátí hello Hadoop nainstalovaná na počítači hello kde hello skript spustí.</span><span class="sxs-lookup"><span data-stu-id="d5120-180">Get hello version of Hadoop installed on hello computer where hello script executes.</span></span> |
| <span data-ttu-id="d5120-181">**Test IsHDIHeadNode**</span><span class="sxs-lookup"><span data-stu-id="d5120-181">**Test-IsHDIHeadNode**</span></span> |<span data-ttu-id="d5120-182">Zkontrolujte, zda text hello kde hello skript spustí hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="d5120-182">Check if hello computer where hello script executes is a head node.</span></span> |
| <span data-ttu-id="d5120-183">**Test IsActiveHDIHeadNode**</span><span class="sxs-lookup"><span data-stu-id="d5120-183">**Test-IsActiveHDIHeadNode**</span></span> |<span data-ttu-id="d5120-184">Zkontrolujte, zda text hello kde hello skript spustí active hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="d5120-184">Check if hello computer where hello script executes is an active head node.</span></span> |
| <span data-ttu-id="d5120-185">**Test IsHDIDataNode**</span><span class="sxs-lookup"><span data-stu-id="d5120-185">**Test-IsHDIDataNode**</span></span> |<span data-ttu-id="d5120-186">Zkontrolujte, zda text hello kde hello skript spustí datový uzel.</span><span class="sxs-lookup"><span data-stu-id="d5120-186">Check if hello computer where hello script executes is a data node.</span></span> |
| <span data-ttu-id="d5120-187">**Upravit HDIConfigFile**</span><span class="sxs-lookup"><span data-stu-id="d5120-187">**Edit-HDIConfigFile**</span></span> |<span data-ttu-id="d5120-188">Upravte hello konfigurační soubory hive-site.xml, core-site.xml, hdfs-site.xml, mapred-site.xml nebo yarn-site.xml.</span><span class="sxs-lookup"><span data-stu-id="d5120-188">Edit hello config files hive-site.xml, core-site.xml, hdfs-site.xml, mapred-site.xml, or yarn-site.xml.</span></span> |

## <a name="best-practices-for-script-development"></a><span data-ttu-id="d5120-189">Osvědčené postupy pro vývoj skriptů</span><span class="sxs-lookup"><span data-stu-id="d5120-189">Best practices for script development</span></span>
<span data-ttu-id="d5120-190">Při vývoji vlastních skriptů pro cluster služby HDInsight, existuje několik osvědčených postupů tookeep nezapomeňte:</span><span class="sxs-lookup"><span data-stu-id="d5120-190">When you develop a custom script for an HDInsight cluster, there are several best practices tookeep in mind:</span></span>

* <span data-ttu-id="d5120-191">Kontrola verze Hadoop hello</span><span class="sxs-lookup"><span data-stu-id="d5120-191">Check for hello Hadoop version</span></span>

    <span data-ttu-id="d5120-192">Pouze HDInsight verze 3.1 (Hadoop 2.4) a vyšší podpora použití akce skriptu tooinstall vlastní součásti v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5120-192">Only HDInsight version 3.1 (Hadoop 2.4) and above support using Script Action tooinstall custom components on a cluster.</span></span> <span data-ttu-id="d5120-193">Ve vašem vlastního skriptu, je nutné použít hello **Get-HDIHadoopVersion** Pomocná metoda toocheck hello Hadoop verze než budete pokračovat v provádění další úloh ve skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="d5120-193">In your custom script, you must use hello **Get-HDIHadoopVersion** helper method toocheck hello Hadoop version before proceeding with performing other tasks in hello script.</span></span>
* <span data-ttu-id="d5120-194">Zadejte, že stabilní odkazy tooscript prostředky</span><span class="sxs-lookup"><span data-stu-id="d5120-194">Provide stable links tooscript resources</span></span>

    <span data-ttu-id="d5120-195">Uživatelé měli ujistit, všechny hello skripty a další artefaktů použít v hello přizpůsobení clusteru zůstaly dostupné v celém hello životnost hello clusteru a že hello verze těchto souborů se nezmění pro dobu trvání hello.</span><span class="sxs-lookup"><span data-stu-id="d5120-195">Users should make sure that all hello scripts and other artifacts used in hello customization of a cluster remain available throughout hello lifetime of hello cluster and that hello versions of these files do not change for hello duration.</span></span> <span data-ttu-id="d5120-196">Pokud hello obnovování uzlů v clusteru hello je vyžadováno, je nutné tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="d5120-196">These resources are required if hello reimaging of nodes in hello cluster is required.</span></span> <span data-ttu-id="d5120-197">Hello osvědčeným postupem je toodownload a archivaci, které se nacházejí v účtu úložiště, který hello uživatelské ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="d5120-197">hello best practice is toodownload and archive everything in a Storage account that hello user controls.</span></span> <span data-ttu-id="d5120-198">To může být hello výchozí účet úložiště nebo některé z dalších účtů úložiště hello zadaný v době hello nasazení pro vlastní cluster.</span><span class="sxs-lookup"><span data-stu-id="d5120-198">This can be hello default Storage account or any of hello additional Storage accounts specified at hello time of deployment for a customized cluster.</span></span>
    <span data-ttu-id="d5120-199">V hello Spark a R přizpůsobit clusteru ukázky zadaný hello dokumentace, například jsme provedli místní kopie hello prostředků v rámci tohoto účtu úložiště: https://hdiconfigactions.blob.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="d5120-199">In hello Spark and R customized cluster samples provided in hello documentation, for example, we have made a local copy of hello resources in this Storage account: https://hdiconfigactions.blob.core.windows.net/.</span></span>
* <span data-ttu-id="d5120-200">Zajistěte, aby hello clusteru přizpůsobení skriptu idempotent</span><span class="sxs-lookup"><span data-stu-id="d5120-200">Ensure that hello cluster customization script is idempotent</span></span>

    <span data-ttu-id="d5120-201">Měli očekávat, že je hello uzly clusteru HDInsight obnovit z Image během doby života hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5120-201">You must expect that hello nodes of an HDInsight cluster is reimaged during hello cluster lifetime.</span></span> <span data-ttu-id="d5120-202">spuštění skriptu přizpůsobení clusteru Hello vždy, když je obnovit z Image clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5120-202">hello cluster customization script is run whenever a cluster is reimaged.</span></span> <span data-ttu-id="d5120-203">Tento skript musí být navrženou toobe idempotent v hello smyslu, že při obnovování, hello skriptu by měl zajistěte, aby že tento cluster hello se vrátí stejné přizpůsobené stavu, který byl právě po hello skript spustili pro hello první čas, kdy hello clusteru bylo původně toohello vytvořit.</span><span class="sxs-lookup"><span data-stu-id="d5120-203">This script must be designed toobe idempotent in hello sense that upon reimaging, hello script should ensure that hello cluster is returned toohello same customized state that it was in just after hello script ran for hello first time when hello cluster was initially created.</span></span> <span data-ttu-id="d5120-204">Například pokud vlastní skript nainstalovat aplikaci na D:\AppLocation na prvním spuštění, pak na každé následné spuštění při obnovování, by měl hello skriptu zkontrolujte, zda text hello aplikace existuje v hello D:\AppLocation umístění než budete pokračovat s jinými kroky ve skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="d5120-204">For example, if a custom script installed an application at D:\AppLocation on its first run, then on each subsequent run, upon reimaging, hello script should check whether hello application exists at hello D:\AppLocation location before proceeding with other steps in hello script.</span></span>
* <span data-ttu-id="d5120-205">Vlastní součásti nainstalovat na optimální umístění hello</span><span class="sxs-lookup"><span data-stu-id="d5120-205">Install custom components in hello optimal location</span></span>

    <span data-ttu-id="d5120-206">Když se obnoví z Image uzly clusteru, hello C:\ prostředků disku a jednotku D:\ systému můžete naformátována, tím hello dojít ke ztrátě dat a aplikací nainstalovaných na těchto jednotkách.</span><span class="sxs-lookup"><span data-stu-id="d5120-206">When cluster nodes are reimaged, hello C:\ resource drive and D:\ system drive can be reformatted, resulting in hello loss of data and applications that had been installed on those drives.</span></span> <span data-ttu-id="d5120-207">To může také dojít, pokud do virtuálních počítačů (VM) Azure uzlu, který je součástí clusteru hello přestane fungovat a bude nahrazen nový uzel.</span><span class="sxs-lookup"><span data-stu-id="d5120-207">This could also happen if an Azure virtual machine (VM) node that is part of hello cluster goes down and is replaced by a new node.</span></span> <span data-ttu-id="d5120-208">Součásti můžete nainstalovat na jednotku D:\ hello nebo v umístění C:\apps hello v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="d5120-208">You can install components on hello D:\ drive or in hello C:\apps location on hello cluster.</span></span> <span data-ttu-id="d5120-209">Všech jiných umístění na jednotce C:\ hello jsou vyhrazené.</span><span class="sxs-lookup"><span data-stu-id="d5120-209">All other locations on hello C:\ drive are reserved.</span></span> <span data-ttu-id="d5120-210">Zadejte hello umístění, kde jsou aplikace nebo knihovny toobe nainstalován ve skriptu přizpůsobení hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5120-210">Specify hello location where applications or libraries are toobe installed in hello cluster customization script.</span></span>
* <span data-ttu-id="d5120-211">Ujistěte se, vysokou dostupnost clusteru architektury hello</span><span class="sxs-lookup"><span data-stu-id="d5120-211">Ensure high availability of hello cluster architecture</span></span>

    <span data-ttu-id="d5120-212">HDInsight má aktivní – pasivní architekturu pro zajištění vysoké dostupnosti, v head jeden uzel, který je v aktivním režimu (kde hello HDInsight jsou spuštěny služby) a hello jiných hlavního uzlu v pohotovostním režimu (v HDInsight, které nejsou spuštěny služby).</span><span class="sxs-lookup"><span data-stu-id="d5120-212">HDInsight has an active-passive architecture for high availability, in which one head node is in active mode (where hello HDInsight services are running) and hello other head node is in standby mode (in which HDInsight services are not running).</span></span> <span data-ttu-id="d5120-213">uzly Hello přepínače aktivní a pasivní režim, pokud jsou přerušení služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d5120-213">hello nodes switch active and passive modes if HDInsight services are interrupted.</span></span> <span data-ttu-id="d5120-214">Pokud akce skriptu použité tooinstall služeb v obou head uzlů pro vysokou dostupnost, Poznámka že této hello HDInsight mechanismus převzetí služeb při selhání není možné tooautomatically selhání přes tyto služby uživatel nainstaloval.</span><span class="sxs-lookup"><span data-stu-id="d5120-214">If a script action is used tooinstall services on both head nodes for high availability, note that hello HDInsight failover mechanism is not able tooautomatically fail over these user-installed services.</span></span> <span data-ttu-id="d5120-215">Proto uživatel nainstaloval služeb v HDInsight head uzlů, které jsou očekávané toobe vysokou dostupností musí mít vlastní mechanismus převzetí služeb při selhání, pokud v režimu aktivní pasivní nebo v režimu aktivní aktivní.</span><span class="sxs-lookup"><span data-stu-id="d5120-215">So user-installed services on HDInsight head nodes that are expected toobe highly available must either have their own failover mechanism if in active-passive mode or be in active-active mode.</span></span>

    <span data-ttu-id="d5120-216">Příkaz akce skriptu HDInsight spustí na obou hlavních uzlech při role hello hlavní uzel je zadán jako hodnota v hello *ClusterRoleCollection* parametr.</span><span class="sxs-lookup"><span data-stu-id="d5120-216">An HDInsight Script Action command runs on both head nodes when hello head-node role is specified as a value in hello *ClusterRoleCollection* parameter.</span></span> <span data-ttu-id="d5120-217">Proto při návrhu vlastní skript, ujistěte se, že váš skript známa tento instalační program.</span><span class="sxs-lookup"><span data-stu-id="d5120-217">So when you design a custom script, make sure that your script is aware of this setup.</span></span> <span data-ttu-id="d5120-218">Nespouštějte k potížím, kde hello stejné služby jsou instalaci a spuštění na oba uzly head hello a jejich skončili neslučitelných mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="d5120-218">You should not run into problems where hello same services are installed and started on both of hello head nodes and they end up competing with each other.</span></span> <span data-ttu-id="d5120-219">Navíc mějte na paměti, že dojde ke ztrátě dat během obnovování, takže softwaru nainstalované prostřednictvím akce skriptu má toobe odolné toosuch události.</span><span class="sxs-lookup"><span data-stu-id="d5120-219">Also, be aware that data is lost during reimaging, so software installed via Script Action has toobe resilient toosuch events.</span></span> <span data-ttu-id="d5120-220">Aplikace by měla být navrženou toowork s vysokou dostupností data, která se distribuuje do mnoha uzly.</span><span class="sxs-lookup"><span data-stu-id="d5120-220">Applications should be designed toowork with highly available data that is distributed across many nodes.</span></span> <span data-ttu-id="d5120-221">Všimněte si, že až 1/5 hello uzlů v clusteru lze obnovit z Image na hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="d5120-221">Note that as many as 1/5 of hello nodes in a cluster can be reimaged at hello same time.</span></span>
* <span data-ttu-id="d5120-222">Konfigurace úložiště objektů Azure Blob toouse hello vlastní komponenty</span><span class="sxs-lookup"><span data-stu-id="d5120-222">Configure hello custom components toouse Azure Blob storage</span></span>

    <span data-ttu-id="d5120-223">Hello vlastní součásti, které instalujete na uzlech clusteru hello pravděpodobně výchozí konfigurace toouse Hadoop Distributed File System (HDFS) úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5120-223">hello custom components that you install on hello cluster nodes might have a default configuration toouse Hadoop Distributed File System (HDFS) storage.</span></span> <span data-ttu-id="d5120-224">Místo toho byste měli změnit hello konfigurace toouse úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="d5120-224">You should change hello configuration toouse Azure Blob storage instead.</span></span> <span data-ttu-id="d5120-225">Na obnovení z Image clusteru systému souborů HDFS hello získá formátu a by ztratíte všechna data, která je uložena existuje.</span><span class="sxs-lookup"><span data-stu-id="d5120-225">On a cluster reimage, hello HDFS file system gets formatted and you would lose any data that is stored there.</span></span> <span data-ttu-id="d5120-226">Použití úložiště objektů Blob v Azure místo toho zajišťuje, aby vaše data se uchovávají.</span><span class="sxs-lookup"><span data-stu-id="d5120-226">Using Azure Blob storage instead ensures that your data is retained.</span></span>

## <a name="common-usage-patterns"></a><span data-ttu-id="d5120-227">Obecné vzory využití</span><span class="sxs-lookup"><span data-stu-id="d5120-227">Common usage patterns</span></span>
<span data-ttu-id="d5120-228">Tato část obsahuje pokyny k implementaci některé hello běžné využití vzory, které se mohou vyskytnout při zápisu vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="d5120-228">This section provides guidance on implementing some of hello common usage patterns that you might run into while writing your own custom script.</span></span>

### <a name="configure-environment-variables"></a><span data-ttu-id="d5120-229">Nakonfigurujte proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="d5120-229">Configure environment variables</span></span>
<span data-ttu-id="d5120-230">Často v vývoj akcí skriptů, si myslíte, že hello potřebovat tooset proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="d5120-230">Often in script action development, you feel hello need tooset environment variables.</span></span> <span data-ttu-id="d5120-231">Například je nejpravděpodobnější scénář při stahování binární z externího webu, nainstalujte na hello clusteru a přidejte hello umístění tam, kde je proměnná prostředí nainstalovaná tooyour 'PATH'.</span><span class="sxs-lookup"><span data-stu-id="d5120-231">For instance, a most likely scenario is when you download a binary from an external site, install it on hello cluster, and add hello location of where it is installed tooyour ‘PATH’ environment variable.</span></span> <span data-ttu-id="d5120-232">Hello následující fragment kódu ukazuje, jak tooset proměnné prostředí v hello vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="d5120-232">hello following snippet shows you how tooset environment variables in hello custom script.</span></span>

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

<span data-ttu-id="d5120-233">Tento příkaz nastaví proměnnou prostředí hello **MDS_RUNNER_CUSTOM_CLUSTER** toohello hodnotu "true" a také nastaví hello oboru této proměnné toobe celého systému.</span><span class="sxs-lookup"><span data-stu-id="d5120-233">This statement sets hello environment variable **MDS_RUNNER_CUSTOM_CLUSTER** toohello value 'true' and also sets hello scope of this variable toobe machine-wide.</span></span> <span data-ttu-id="d5120-234">V některých případech je důležité, aby proměnné prostředí se nastavují v příslušné oboru hello – počítače nebo uživatele.</span><span class="sxs-lookup"><span data-stu-id="d5120-234">At times it is important that environment variables are set at hello appropriate scope – machine or user.</span></span> <span data-ttu-id="d5120-235">Odkazovat [sem] [ 1] Další informace o nastavení proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="d5120-235">Refer [here][1] for more information on setting environment variables.</span></span>

### <a name="access-toolocations-where-hello-custom-scripts-are-stored"></a><span data-ttu-id="d5120-236">Přístup k toolocations, kde jsou uloženy vlastní skripty hello</span><span class="sxs-lookup"><span data-stu-id="d5120-236">Access toolocations where hello custom scripts are stored</span></span>
<span data-ttu-id="d5120-237">Skripty použité toocustomize tooeither clusteru musí být v hello výchozí účet úložiště pro hello cluster nebo ve veřejném kontejneru jen pro čtení na jiný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5120-237">Scripts used toocustomize a cluster needs tooeither be in hello default storage account for hello cluster or in a public read-only container on any other storage account.</span></span> <span data-ttu-id="d5120-238">Pokud skript odkazuje na prostředky, které se nacházejí jinde požadavky toobe ve veřejně přístupné (alespoň veřejné jen pro čtení).</span><span class="sxs-lookup"><span data-stu-id="d5120-238">If your script accesses resources located elsewhere these need toobe in a publicly accessible (at least public read-only).</span></span> <span data-ttu-id="d5120-239">Pro instanci může tooaccess soubor a uložte ji pomocí příkazu hello SaveFile HDI.</span><span class="sxs-lookup"><span data-stu-id="d5120-239">For instance you might want tooaccess a file and save it using hello SaveFile-HDI command.</span></span>

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

<span data-ttu-id="d5120-240">V tomto příkladu můžete zajistit, aby byl tento kontejner hello 'somecontainer' v účtu úložiště 'somestorageaccount' veřejně přístupná.</span><span class="sxs-lookup"><span data-stu-id="d5120-240">In this example, you must ensure that hello container 'somecontainer' in storage account 'somestorageaccount' is publicly accessible.</span></span> <span data-ttu-id="d5120-241">Skript hello, jinak hodnota vyhodí výjimku "Nebyl nalezen" a selhání.</span><span class="sxs-lookup"><span data-stu-id="d5120-241">Otherwise, hello script throws a ‘Not Found’ exception and fail.</span></span>

### <a name="pass-parameters-toohello-add-azurermhdinsightscriptaction-cmdlet"></a><span data-ttu-id="d5120-242">Předávání parametrů toohello přidat AzureRmHDInsightScriptAction rutiny</span><span class="sxs-lookup"><span data-stu-id="d5120-242">Pass parameters toohello Add-AzureRmHDInsightScriptAction cmdlet</span></span>
<span data-ttu-id="d5120-243">toopass více rutiny toohello AzureRmHDInsightScriptAction přidat parametry, musíte všechny parametry pro tooformat hello řetězec hodnotu toocontain hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="d5120-243">toopass multiple parameters toohello Add-AzureRmHDInsightScriptAction cmdlet, you need tooformat hello string value toocontain all parameters for hello script.</span></span> <span data-ttu-id="d5120-244">Například:</span><span class="sxs-lookup"><span data-stu-id="d5120-244">For example:</span></span>

    "-CertifcateUri wasb:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

<span data-ttu-id="d5120-245">nebo</span><span class="sxs-lookup"><span data-stu-id="d5120-245">or</span></span>

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a><span data-ttu-id="d5120-246">Throw – výjimka pro nasazení clusteru se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="d5120-246">Throw exception for failed cluster deployment</span></span>
<span data-ttu-id="d5120-247">Tooget přesně oznámení faktu hello, pokud přizpůsobení tohoto clusteru se nezdařilo podle očekávání, je důležité toothrow výjimku a vytváření clusteru hello nezdaří.</span><span class="sxs-lookup"><span data-stu-id="d5120-247">If you want tooget accurately notified of hello fact that cluster customization did not succeed as expected, it is important toothrow an exception and fail hello cluster creation.</span></span> <span data-ttu-id="d5120-248">Můžete například chcete tooprocess soubor, pokud existuje a zpracovat hello chyba případy, kdy hello soubor neexistuje.</span><span class="sxs-lookup"><span data-stu-id="d5120-248">For instance, you might want tooprocess a file if it exists and handle hello error case where hello file does not exist.</span></span> <span data-ttu-id="d5120-249">To by zajistěte, aby skript hello ukončí řádně a se správně označuje stav hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5120-249">This would ensure that hello script exits gracefully and hello state of hello cluster is correctly known.</span></span> <span data-ttu-id="d5120-250">Hello následující fragment kódu poskytuje příklad tooachieve toto:</span><span class="sxs-lookup"><span data-stu-id="d5120-250">hello following snippet gives an example of how tooachieve this:</span></span>

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

<span data-ttu-id="d5120-251">V tento fragment kódu Pokud soubor hello neexistoval, by vedlo tooa stavu hello skriptu ve skutečnosti řádně ukončí po tisku hello chybová zpráva, kde hello clusteru dosáhne stavu spuštěno za předpokladu, že ji "úspěšně" dokončit proces přizpůsobení clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5120-251">In this snippet, if hello file did not exist, it would lead tooa state where hello script actually exits gracefully after printing hello error message, and hello cluster reaches running state assuming it "successfully" completed cluster customization process.</span></span> <span data-ttu-id="d5120-252">Toobe přesně oznámení faktu hello, pokud přizpůsobení tohoto clusteru v podstatě se nezdařilo z důvodu chybějícího souboru očekávaným způsobem, je vhodnější toothrow výjimku a selhání hello clusteru přizpůsobení krok.</span><span class="sxs-lookup"><span data-stu-id="d5120-252">If you want toobe accurately notified of hello fact that cluster customization essentially did not succeed as expected because of a missing file, it is more appropriate toothrow an exception and fail hello cluster customization step.</span></span> <span data-ttu-id="d5120-253">tooachieve to hello následující fragment kódu ukázka místo toho musíte použít.</span><span class="sxs-lookup"><span data-stu-id="d5120-253">tooachieve this you must use hello following sample code snippet instead.</span></span>

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a><span data-ttu-id="d5120-254">Kontrolní seznam pro nasazení akce skriptu</span><span class="sxs-lookup"><span data-stu-id="d5120-254">Checklist for deploying a script action</span></span>
<span data-ttu-id="d5120-255">Zde jsou hello kroky, které jsme trvalo při přípravě toodeploy tyto skripty:</span><span class="sxs-lookup"><span data-stu-id="d5120-255">Here are hello steps we took when preparing toodeploy these scripts:</span></span>

1. <span data-ttu-id="d5120-256">Uveďte hello soubory, které obsahují vlastní skripty hello na místě, které je přístupné uzly clusteru hello během nasazení.</span><span class="sxs-lookup"><span data-stu-id="d5120-256">Put hello files that contain hello custom scripts in a place that is accessible by hello cluster nodes during deployment.</span></span> <span data-ttu-id="d5120-257">To může být libovolná z výchozí hello nebo další účty úložiště zadaný v době hello nasazení clusteru nebo jiných veřejně přístupná úložiště kontejneru.</span><span class="sxs-lookup"><span data-stu-id="d5120-257">This can be any of hello default or additional Storage accounts specified at hello time of cluster deployment, or any other publicly accessible storage container.</span></span>
2. <span data-ttu-id="d5120-258">Přidat kontroly do skriptů toomake jistotu, že spouštění idempotently, tak, aby hello skriptu lze provést několikrát u hello stejného uzlu.</span><span class="sxs-lookup"><span data-stu-id="d5120-258">Add checks into scripts toomake sure that they execute idempotently, so that hello script can be executed multiple times on hello same node.</span></span>
3. <span data-ttu-id="d5120-259">Použití hello **Write-Output** tooSTDOUT tooprint rutiny prostředí Azure PowerShell, jakož i STDERR.</span><span class="sxs-lookup"><span data-stu-id="d5120-259">Use hello **Write-Output** Azure PowerShell cmdlet tooprint tooSTDOUT as well as STDERR.</span></span> <span data-ttu-id="d5120-260">Nepoužívejte **Write-Host**.</span><span class="sxs-lookup"><span data-stu-id="d5120-260">Do not use **Write-Host**.</span></span>
4. <span data-ttu-id="d5120-261">Použijte složku dočasného souboru, jako je například $env: dočasné, tookeep hello stažený soubor používá hello skripty a pak vyčištění je po mají spouštět skripty.</span><span class="sxs-lookup"><span data-stu-id="d5120-261">Use a temporary file folder, such as $env:TEMP, tookeep hello downloaded file used by hello scripts and then clean them up after scripts have executed.</span></span>
5. <span data-ttu-id="d5120-262">Nainstalujte jenom na D:\ nebo C:\apps vlastní software.</span><span class="sxs-lookup"><span data-stu-id="d5120-262">Install custom software only at D:\ or C:\apps.</span></span> <span data-ttu-id="d5120-263">Jiných umístění na jednotce C: hello nepoužívejte, protože se jedná o vyhrazené.</span><span class="sxs-lookup"><span data-stu-id="d5120-263">Other locations on hello C: drive should not be used as they are reserved.</span></span> <span data-ttu-id="d5120-264">Všimněte si, že instalaci souborů na jednotce C: hello mimo složku C:\apps hello může vést instalace selhání během reimages hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="d5120-264">Note that installing files on hello C: drive outside of hello C:\apps folder may result in setup failures during reimages of hello node.</span></span>
6. <span data-ttu-id="d5120-265">V případě hello, nastavení na úrovni operačního systému nebo Hadoop služby konfigurační soubory se změnily můžete služby HDInsight toorestart tak, aby se vyzvedávat všechna nastavení, úrovni operačního systému, jako je například nastavit ve skriptech hello proměnné prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="d5120-265">In hello event that OS-level settings or Hadoop service configuration files were changed, you may want toorestart HDInsight services so that they can pick up any OS-level settings, such as hello environment variables set in hello scripts.</span></span>

## <a name="debug-custom-scripts"></a><span data-ttu-id="d5120-266">Ladění vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="d5120-266">Debug custom scripts</span></span>
<span data-ttu-id="d5120-267">protokoly chyb skriptu Hello ukládají spolu s další výstupu v hello výchozí úložiště účet, který jste zadali pro hello clusteru při jeho vytváření.</span><span class="sxs-lookup"><span data-stu-id="d5120-267">hello script error logs are stored, along with other output, in hello default Storage account that you specified for hello cluster at its creation.</span></span> <span data-ttu-id="d5120-268">Hello protokoly se ukládají v tabulce s názvem hello *u < \cluster-name-fragment >< \time-stamp > setuplog*.</span><span class="sxs-lookup"><span data-stu-id="d5120-268">hello logs are stored in a table with hello name *u<\cluster-name-fragment><\time-stamp>setuplog*.</span></span> <span data-ttu-id="d5120-269">Toto jsou agregovaná protokoly, které mají záznamy ze všech uzlů hello (hlavního uzlu a pracovní uzly) na které hello skript se spustí v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="d5120-269">These are aggregated logs that have records from all of hello nodes (head node and worker nodes) on which hello script runs in hello cluster.</span></span>
<span data-ttu-id="d5120-270">Snadný způsob toocheck hello protokoly je toouse nástroje HDInsight pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d5120-270">An easy way toocheck hello logs is toouse HDInsight Tools for Visual Studio.</span></span> <span data-ttu-id="d5120-271">Instalace nástrojů hello, najdete v části [začněte používat nástroje Visual Studio Hadoop pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)</span><span class="sxs-lookup"><span data-stu-id="d5120-271">For installing hello tools, see [Get started using Visual Studio Hadoop tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)</span></span>

<span data-ttu-id="d5120-272">**toocheck hello protokolu pomocí sady Visual Studio**</span><span class="sxs-lookup"><span data-stu-id="d5120-272">**toocheck hello log using Visual Studio**</span></span>

1. <span data-ttu-id="d5120-273">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d5120-273">Open Visual Studio.</span></span>
2. <span data-ttu-id="d5120-274">Klikněte na tlačítko **zobrazení**a potom klikněte na **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="d5120-274">Click **View**, and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="d5120-275">Klikněte pravým tlačítkem na "Azure", klikněte na tlačítko připojit příliš**předplatná Microsoft Azure**a pak zadejte svoje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="d5120-275">Right-click "Azure", click Connect too**Microsoft Azure Subscriptions**, and then enter your credentials.</span></span>
4. <span data-ttu-id="d5120-276">Rozbalte položku **úložiště**, rozbalte účet úložiště Azure hello používá jako výchozí systém souborů hello, rozbalte položku **tabulky**a potom dvakrát klikněte na název tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="d5120-276">Expand **Storage**, expand hello Azure storage account used as hello default file system, expand **Tables**, and then double-click hello table name.</span></span>

<span data-ttu-id="d5120-277">Můžete také vzdáleného do toosee uzly clusteru hello STDOUT a STDERR pro vlastní skripty.</span><span class="sxs-lookup"><span data-stu-id="d5120-277">You can also remote into hello cluster nodes toosee both STDOUT and STDERR for custom scripts.</span></span> <span data-ttu-id="d5120-278">Hello protokoly na každém uzlu jsou konkrétním uzlu pouze toothat a se protokolují do **C:\HDInsightLogs\DeploymentAgent.log**.</span><span class="sxs-lookup"><span data-stu-id="d5120-278">hello logs on each node are specific only toothat node and are logged into **C:\HDInsightLogs\DeploymentAgent.log**.</span></span> <span data-ttu-id="d5120-279">Tyto soubory protokolu zaznamenejte všechny výstupy z hello vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="d5120-279">These log files record all outputs from hello custom script.</span></span> <span data-ttu-id="d5120-280">Fragment kódu příklad protokolu pro akci skriptu Spark vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="d5120-280">An example log snippet for a Spark script action looks like this:</span></span>

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


<span data-ttu-id="d5120-281">Tento protokol je jasné, že akce skriptu Spark hello provedl na hello virtuálního počítače s názvem HEADNODE0 a že žádné výjimky došlo k během provádění hello.</span><span class="sxs-lookup"><span data-stu-id="d5120-281">In this log, it is clear that hello Spark script action has been executed on hello VM named HEADNODE0 and that no exceptions were thrown during hello execution.</span></span>

<span data-ttu-id="d5120-282">V hello události, k níž dojde k chybě provádění je výstup hello popisující ho také obsažený v tomto souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="d5120-282">In hello event that an execution failure occurs, hello output describing it is also contained in this log file.</span></span> <span data-ttu-id="d5120-283">Hello informací uvedených v těchto protokolech by měl být užitečné při ladění skriptu problémy, které by mohlo dojít.</span><span class="sxs-lookup"><span data-stu-id="d5120-283">hello information provided in these logs should be helpful in debugging script problems that may arise.</span></span>

## <a name="see-also"></a><span data-ttu-id="d5120-284">Viz také</span><span class="sxs-lookup"><span data-stu-id="d5120-284">See also</span></span>
* <span data-ttu-id="d5120-285">[Přizpůsobení clusterů HDInsight pomocí akce skriptu][hdinsight-cluster-customize]</span><span class="sxs-lookup"><span data-stu-id="d5120-285">[Customize HDInsight clusters using Script Action][hdinsight-cluster-customize]</span></span>
* <span data-ttu-id="d5120-286">[Nainstalovat a používat Spark v HDInsight clustery][hdinsight-install-spark]</span><span class="sxs-lookup"><span data-stu-id="d5120-286">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]</span></span>
* <span data-ttu-id="d5120-287">[Nainstalovat a používat R na clustery HDInsight][hdinsight-r-scripts]</span><span class="sxs-lookup"><span data-stu-id="d5120-287">[Install and use R on HDInsight clusters][hdinsight-r-scripts]</span></span>
* <span data-ttu-id="d5120-288">[Nainstalovat a používat Solr v clusterech HDInsight](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="d5120-288">[Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span>
* <span data-ttu-id="d5120-289">[Nainstalovat a používat Giraph v clusterech HDInsight](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="d5120-289">[Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span>

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
