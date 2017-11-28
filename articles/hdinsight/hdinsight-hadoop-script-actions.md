---
title: "Vývoj akcí skriptů v prostředí HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak přizpůsobit clustery Hadoop pomocí akce skriptu. Akce skriptu lze nainstalovat další software spuštěných v clusteru s Hadoop nebo změnit konfiguraci aplikace nainstalované v clusteru."
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
ms.openlocfilehash: 0e182e6b43fd2d17524c1da36cf4c204bb1b865a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a><span data-ttu-id="5681b-104">Vývoj skriptů akce skriptu pro clustery se systémem HDInsight Windows</span><span class="sxs-lookup"><span data-stu-id="5681b-104">Develop Script Action scripts for HDInsight Windows-based clusters</span></span>
<span data-ttu-id="5681b-105">Zjistěte, jak k psaní skriptů akce skriptu pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5681b-105">Learn how to write Script Action scripts for HDInsight.</span></span> <span data-ttu-id="5681b-106">Informace o použití akce skriptu skriptů najdete v tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="5681b-106">For information on using Script Action scripts, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md).</span></span> <span data-ttu-id="5681b-107">Stejný článek napsán pro clustery HDInsight se systémem Linux, najdete v části [vyvíjet akce skriptu skripty pro HDInsight](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="5681b-107">For the same article written for Linux-based HDInsight clusters, see [Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions-linux.md).</span></span>



> [!IMPORTANT]
> <span data-ttu-id="5681b-108">Kroky v tomto dokumentu fungovat pouze pro clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="5681b-108">The steps in this document only work for Windows-based HDInsight clusters.</span></span> <span data-ttu-id="5681b-109">HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="5681b-109">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="5681b-110">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="5681b-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="5681b-111">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="5681b-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="5681b-112">Informace o použití akce skriptu s clustery se systémem Linux najdete v tématu [vývoj akcí skriptů v prostředí HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="5681b-112">For information on using script actions with Linux-based clusters, see [Script action development with HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).</span></span>
>
>



<span data-ttu-id="5681b-113">Akce skriptu lze nainstalovat další software spuštěných v clusteru s Hadoop nebo změnit konfiguraci aplikace nainstalované v clusteru.</span><span class="sxs-lookup"><span data-stu-id="5681b-113">Script Action can be used to install additional software running on a Hadoop cluster or to change the configuration of applications installed on a cluster.</span></span> <span data-ttu-id="5681b-114">Akce skriptů jsou skripty, které jsou spuštěny na uzlech clusteru při nasazených clusterů HDInsight, a jsou prováděna po dokončení konfigurace HDInsight uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="5681b-114">Script actions are scripts that run on the cluster nodes when HDInsight clusters are deployed, and they are executed once nodes in the cluster complete HDInsight configuration.</span></span> <span data-ttu-id="5681b-115">Akce skriptu se spustí v části oprávnění k účtu správce systému a poskytuje úplná přístupová práva pro uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="5681b-115">A script action is executed under system admin account privileges and provides full access rights to the cluster nodes.</span></span> <span data-ttu-id="5681b-116">Každý cluster lze zadat seznam akcí skriptů spouštění v pořadí, ve kterém jsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="5681b-116">Each cluster can be provided with a list of script actions to be executed in the order in which they are specified.</span></span>

> [!NOTE]
> <span data-ttu-id="5681b-117">Pokud se setkáte se následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="5681b-117">If you experience the following error message:</span></span>
>
> <span data-ttu-id="5681b-118">System.Management.Automation.CommandNotFoundException; ExceptionMessage: Termín 'Uložit-HDIFile' nebyl rozpoznán jako název rutiny, funkce, soubor skriptu nebo spustitelného programu.</span><span class="sxs-lookup"><span data-stu-id="5681b-118">System.Management.Automation.CommandNotFoundException; ExceptionMessage : The term 'Save-HDIFile' is not recognized as the name of a cmdlet, function, script file, or operable program.</span></span> <span data-ttu-id="5681b-119">Zkontrolujte, zda název, nebo pokud byl zahrnut cestu, ověřte, zda je cesta správná a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="5681b-119">Check the spelling of the name, or if a path was included, verify that the path is correct and try again.</span></span>
> <span data-ttu-id="5681b-120">Je to proto, že jste nezahrnuli pomocné metody.</span><span class="sxs-lookup"><span data-stu-id="5681b-120">It is because you didn't include the helper methods.</span></span>  <span data-ttu-id="5681b-121">V tématu [pomocné metody pro vlastní skripty](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).</span><span class="sxs-lookup"><span data-stu-id="5681b-121">See [Helper methods for custom scripts](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).</span></span>
>
>

## <a name="sample-scripts"></a><span data-ttu-id="5681b-122">Ukázkové skripty</span><span class="sxs-lookup"><span data-stu-id="5681b-122">Sample scripts</span></span>
<span data-ttu-id="5681b-123">Akce skriptu pro vytváření clusterů HDInsight v operačním systému Windows, je skript prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5681b-123">For creating HDInsight clusters on Windows operating system, the Script Action is Azure PowerShell script.</span></span> <span data-ttu-id="5681b-124">Následující skript je ukázkou pro konfiguraci lokality konfigurační soubory:</span><span class="sxs-lookup"><span data-stu-id="5681b-124">The following script is a sample for configuring the site configuration files:</span></span>

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
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
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

<span data-ttu-id="5681b-125">Skript používá čtyři parametry, název konfiguračního souboru, vlastnosti, kterou chcete upravit, hodnotu, kterou chcete nastavit a popis.</span><span class="sxs-lookup"><span data-stu-id="5681b-125">The script takes four parameters, the configuration file name, the property you want to modify, the value you want to set, and a description.</span></span> <span data-ttu-id="5681b-126">Například:</span><span class="sxs-lookup"><span data-stu-id="5681b-126">For example:</span></span>

    hive-site.xml hive.metastore.client.socket.timeout 90

<span data-ttu-id="5681b-127">Tyto parametry nastaví hive.metastore.client.socket.timeout hodnotu na 90 v souboru hive-site.xml.</span><span class="sxs-lookup"><span data-stu-id="5681b-127">These parameters sets the hive.metastore.client.socket.timeout value to 90 in the hive-site.xml file.</span></span>  <span data-ttu-id="5681b-128">Výchozí hodnota je 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="5681b-128">The default value is 60 seconds.</span></span>

<span data-ttu-id="5681b-129">Tento vzorový skript naleznete také v [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).</span><span class="sxs-lookup"><span data-stu-id="5681b-129">This sample script can also be found at [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).</span></span>

<span data-ttu-id="5681b-130">HDInsight nabízí několik skriptů k instalaci dalších součástí v clusterech HDInsight:</span><span class="sxs-lookup"><span data-stu-id="5681b-130">HDInsight provides several scripts to install additional components on HDInsight clusters:</span></span>

| <span data-ttu-id="5681b-131">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5681b-131">Name</span></span> | <span data-ttu-id="5681b-132">Skript</span><span class="sxs-lookup"><span data-stu-id="5681b-132">Script</span></span> |
| --- | --- |
| <span data-ttu-id="5681b-133">**Nainstalujte Spark**</span><span class="sxs-lookup"><span data-stu-id="5681b-133">**Install Spark**</span></span> |<span data-ttu-id="5681b-134">https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1.</span><span class="sxs-lookup"><span data-stu-id="5681b-134">https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1.</span></span> <span data-ttu-id="5681b-135">V tématu [instalací a použitím clustery Spark v HDInsight][hdinsight-install-spark].</span><span class="sxs-lookup"><span data-stu-id="5681b-135">See [Install and use Spark on HDInsight clusters][hdinsight-install-spark].</span></span> |
| <span data-ttu-id="5681b-136">**Nainstalujte jazyk R**</span><span class="sxs-lookup"><span data-stu-id="5681b-136">**Install R**</span></span> |<span data-ttu-id="5681b-137">https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1.</span><span class="sxs-lookup"><span data-stu-id="5681b-137">https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1.</span></span> <span data-ttu-id="5681b-138">V tématu [instalací a použitím R v clusterech HDInsight][hdinsight-r-scripts].</span><span class="sxs-lookup"><span data-stu-id="5681b-138">See [Install and use R on HDInsight clusters][hdinsight-r-scripts].</span></span> |
| <span data-ttu-id="5681b-139">**Nainstalujte Solr**</span><span class="sxs-lookup"><span data-stu-id="5681b-139">**Install Solr**</span></span> |<span data-ttu-id="5681b-140">https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="5681b-140">https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1.</span></span> <span data-ttu-id="5681b-141">V tématu [instalace a použití clusterů v HDInsight Solr](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="5681b-141">See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span> |
| <span data-ttu-id="5681b-142">- **Nainstalujte Giraph**</span><span class="sxs-lookup"><span data-stu-id="5681b-142">- **Install Giraph**</span></span> |<span data-ttu-id="5681b-143">https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="5681b-143">https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1.</span></span> <span data-ttu-id="5681b-144">V tématu [instalace a použití clusterů v HDInsight Giraph](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="5681b-144">See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span> |

<span data-ttu-id="5681b-145">Akce skriptu se dá nasadit na portálu Azure, Azure PowerShell nebo pomocí sady .NET SDK HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5681b-145">Script Action can be deployed from the Azure portal, Azure PowerShell or by using the HDInsight .NET SDK.</span></span>  <span data-ttu-id="5681b-146">Další informace najdete v tématu [HDInsight přizpůsobit clustery pomocí akce skriptu][hdinsight-cluster-customize].</span><span class="sxs-lookup"><span data-stu-id="5681b-146">For more information, see [Customize HDInsight clusters using Script Action][hdinsight-cluster-customize].</span></span>

> [!NOTE]
> <span data-ttu-id="5681b-147">Ukázkové skripty pracovat pouze s verze clusteru HDInsight verze 3.1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="5681b-147">The sample scripts work only with HDInsight cluster version 3.1 or above.</span></span> <span data-ttu-id="5681b-148">Další informace o verzích clusterů HDInsight, naleznete v části [verze clusteru HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="5681b-148">For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>
>
>

## <a name="helper-methods-for-custom-scripts"></a><span data-ttu-id="5681b-149">Pomocné metody pro vlastní skripty</span><span class="sxs-lookup"><span data-stu-id="5681b-149">Helper methods for custom scripts</span></span>
<span data-ttu-id="5681b-150">Pomocné metody akcí skriptů jsou nástroje, které můžete použít při zápisu vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="5681b-150">Script Action helper methods are utilities that you can use while writing custom scripts.</span></span> <span data-ttu-id="5681b-151">Tyto metody jsou definovány v [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1)a můžou být součástí skripty pomocí následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="5681b-151">These methods are defined in [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1), and can be included in your scripts using the following sample:</span></span>

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

<span data-ttu-id="5681b-152">Zde jsou pomocné metody, které jsou poskytovány tento skript:</span><span class="sxs-lookup"><span data-stu-id="5681b-152">Here are the helper methods that are provided by this script:</span></span>

| <span data-ttu-id="5681b-153">Pomocná metoda</span><span class="sxs-lookup"><span data-stu-id="5681b-153">Helper method</span></span> | <span data-ttu-id="5681b-154">Popis</span><span class="sxs-lookup"><span data-stu-id="5681b-154">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5681b-155">**Uložit HDIFile**</span><span class="sxs-lookup"><span data-stu-id="5681b-155">**Save-HDIFile**</span></span> |<span data-ttu-id="5681b-156">Stažení souboru z zadaný identifikátor URI (Uniform Resource) do umístění na místní disk, který je přidružený k uzlu virtuálního počítače Azure přiřadili ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="5681b-156">Download a file from the specified Uniform Resource Identifier (URI) to a location on the local disk that is associated with the Azure VM node assigned to the cluster.</span></span> |
| <span data-ttu-id="5681b-157">**Rozbalte položku HDIZippedFile**</span><span class="sxs-lookup"><span data-stu-id="5681b-157">**Expand-HDIZippedFile**</span></span> |<span data-ttu-id="5681b-158">Rozbalte soubor ZIP.</span><span class="sxs-lookup"><span data-stu-id="5681b-158">Unzip a zipped file.</span></span> |
| <span data-ttu-id="5681b-159">**Vyvolání HDICmdScript**</span><span class="sxs-lookup"><span data-stu-id="5681b-159">**Invoke-HDICmdScript**</span></span> |<span data-ttu-id="5681b-160">Spusťte skript z cmd.exe.</span><span class="sxs-lookup"><span data-stu-id="5681b-160">Run a script from cmd.exe.</span></span> |
| <span data-ttu-id="5681b-161">**Zápis HDILog**</span><span class="sxs-lookup"><span data-stu-id="5681b-161">**Write-HDILog**</span></span> |<span data-ttu-id="5681b-162">Zapište výstup z vlastní skript používané pro akci skriptu.</span><span class="sxs-lookup"><span data-stu-id="5681b-162">Write output from the custom script used for a script action.</span></span> |
| <span data-ttu-id="5681b-163">**Get-Services**</span><span class="sxs-lookup"><span data-stu-id="5681b-163">**Get-Services**</span></span> |<span data-ttu-id="5681b-164">Získáte seznam služeb, které jsou spuštěny na počítači, kde se skript spustí.</span><span class="sxs-lookup"><span data-stu-id="5681b-164">Get a list of services running on the machine where the script executes.</span></span> |
| <span data-ttu-id="5681b-165">**Get-Service**</span><span class="sxs-lookup"><span data-stu-id="5681b-165">**Get-Service**</span></span> |<span data-ttu-id="5681b-166">S názvem konkrétní služby jako vstup, získat podrobné informace pro konkrétní službu (název služby, zpracovat ID, stavu, atd.) v počítači, kde se skript spustí.</span><span class="sxs-lookup"><span data-stu-id="5681b-166">With the specific service name as input, get detailed information for a specific service (service name, process ID, state, etc.) on the machine where the script executes.</span></span> |
| <span data-ttu-id="5681b-167">**Get-HDIServices**</span><span class="sxs-lookup"><span data-stu-id="5681b-167">**Get-HDIServices**</span></span> |<span data-ttu-id="5681b-168">Získejte seznam HDInsight služby spuštěné v počítači, kde se skript spustí.</span><span class="sxs-lookup"><span data-stu-id="5681b-168">Get a list of HDInsight services running on the computer where the script executes.</span></span> |
| <span data-ttu-id="5681b-169">**Get-HDIService**</span><span class="sxs-lookup"><span data-stu-id="5681b-169">**Get-HDIService**</span></span> |<span data-ttu-id="5681b-170">S konkrétním názvem služby HDInsight jako vstup, získat podrobné informace pro konkrétní službu (název služby, zpracovat ID, stavu, atd.) v počítači, kde se skript spustí.</span><span class="sxs-lookup"><span data-stu-id="5681b-170">With the specific HDInsight service name as input, get detailed information for a specific service (service name, process ID, state, etc.) on the machine where the script executes.</span></span> |
| <span data-ttu-id="5681b-171">**Get-ServicesRunning**</span><span class="sxs-lookup"><span data-stu-id="5681b-171">**Get-ServicesRunning**</span></span> |<span data-ttu-id="5681b-172">Získání seznamu služeb, které jsou spuštěné v počítači, kde se skript spustí.</span><span class="sxs-lookup"><span data-stu-id="5681b-172">Get a list of services that are running on the computer where the script executes.</span></span> |
| <span data-ttu-id="5681b-173">**Get-ServiceRunning**</span><span class="sxs-lookup"><span data-stu-id="5681b-173">**Get-ServiceRunning**</span></span> |<span data-ttu-id="5681b-174">Zkontrolujte, zda specifické služby (podle názvu) je spuštěn v počítači, kde se skript spustí.</span><span class="sxs-lookup"><span data-stu-id="5681b-174">Check if a specific service (by name) is running on the computer where the script executes.</span></span> |
| <span data-ttu-id="5681b-175">**Get-HDIServicesRunning**</span><span class="sxs-lookup"><span data-stu-id="5681b-175">**Get-HDIServicesRunning**</span></span> |<span data-ttu-id="5681b-176">Získejte seznam HDInsight služby spuštěné v počítači, kde se skript spustí.</span><span class="sxs-lookup"><span data-stu-id="5681b-176">Get a list of HDInsight services running on the computer where the script executes.</span></span> |
| <span data-ttu-id="5681b-177">**Get-HDIServiceRunning**</span><span class="sxs-lookup"><span data-stu-id="5681b-177">**Get-HDIServiceRunning**</span></span> |<span data-ttu-id="5681b-178">Zkontrolujte, zda specifické služby HDInsight (podle názvu) je spuštěn v počítači, kde se skript spustí.</span><span class="sxs-lookup"><span data-stu-id="5681b-178">Check if a specific HDInsight service (by name) is running on the computer where the script executes.</span></span> |
| <span data-ttu-id="5681b-179">**Get-HDIHadoopVersion**</span><span class="sxs-lookup"><span data-stu-id="5681b-179">**Get-HDIHadoopVersion**</span></span> |<span data-ttu-id="5681b-180">Získá verzi Hadoop nainstalovaná na počítači, kde se skript spustí.</span><span class="sxs-lookup"><span data-stu-id="5681b-180">Get the version of Hadoop installed on the computer where the script executes.</span></span> |
| <span data-ttu-id="5681b-181">**Test IsHDIHeadNode**</span><span class="sxs-lookup"><span data-stu-id="5681b-181">**Test-IsHDIHeadNode**</span></span> |<span data-ttu-id="5681b-182">Zkontrolujte, jestli je počítač, kde se skript spustí hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="5681b-182">Check if the computer where the script executes is a head node.</span></span> |
| <span data-ttu-id="5681b-183">**Test IsActiveHDIHeadNode**</span><span class="sxs-lookup"><span data-stu-id="5681b-183">**Test-IsActiveHDIHeadNode**</span></span> |<span data-ttu-id="5681b-184">Zkontrolujte, jestli je počítač, kde se skript spustí active hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="5681b-184">Check if the computer where the script executes is an active head node.</span></span> |
| <span data-ttu-id="5681b-185">**Test IsHDIDataNode**</span><span class="sxs-lookup"><span data-stu-id="5681b-185">**Test-IsHDIDataNode**</span></span> |<span data-ttu-id="5681b-186">Zkontrolujte, jestli je počítač, kde se skript spustí datový uzel.</span><span class="sxs-lookup"><span data-stu-id="5681b-186">Check if the computer where the script executes is a data node.</span></span> |
| <span data-ttu-id="5681b-187">**Upravit HDIConfigFile**</span><span class="sxs-lookup"><span data-stu-id="5681b-187">**Edit-HDIConfigFile**</span></span> |<span data-ttu-id="5681b-188">Upravte konfigurační soubory hive-site.xml, core-site.xml, hdfs-site.xml, mapred-site.xml nebo yarn-site.xml.</span><span class="sxs-lookup"><span data-stu-id="5681b-188">Edit the config files hive-site.xml, core-site.xml, hdfs-site.xml, mapred-site.xml, or yarn-site.xml.</span></span> |

## <a name="best-practices-for-script-development"></a><span data-ttu-id="5681b-189">Osvědčené postupy pro vývoj skriptů</span><span class="sxs-lookup"><span data-stu-id="5681b-189">Best practices for script development</span></span>
<span data-ttu-id="5681b-190">Při vývoji vlastních skriptů pro cluster služby HDInsight, existuje několik doporučených postupech pro mějte na paměti:</span><span class="sxs-lookup"><span data-stu-id="5681b-190">When you develop a custom script for an HDInsight cluster, there are several best practices to keep in mind:</span></span>

* <span data-ttu-id="5681b-191">Kontrola verze Hadoop</span><span class="sxs-lookup"><span data-stu-id="5681b-191">Check for the Hadoop version</span></span>

    <span data-ttu-id="5681b-192">Pouze HDInsight verze 3.1 (Hadoop 2.4) a vyšší podporu pomocí akce skriptu k instalaci vlastní součásti v clusteru.</span><span class="sxs-lookup"><span data-stu-id="5681b-192">Only HDInsight version 3.1 (Hadoop 2.4) and above support using Script Action to install custom components on a cluster.</span></span> <span data-ttu-id="5681b-193">Ve vašem vlastního skriptu, je nutné použít **Get-HDIHadoopVersion** pomocnou metodu, zkontrolujte verzi Hadoop, než budete pokračovat v provádění další úloh ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="5681b-193">In your custom script, you must use the **Get-HDIHadoopVersion** helper method to check the Hadoop version before proceeding with performing other tasks in the script.</span></span>
* <span data-ttu-id="5681b-194">Zadejte stabilní odkazy na zdroje skriptu</span><span class="sxs-lookup"><span data-stu-id="5681b-194">Provide stable links to script resources</span></span>

    <span data-ttu-id="5681b-195">Uživatelé měli ujistit, všechny skripty a další artefaktů použít do vlastního nastavení clusteru s podporou zůstaly dostupné v celém dobu životnosti clusteru a že verze těchto souborů se po dobu trvání nemění.</span><span class="sxs-lookup"><span data-stu-id="5681b-195">Users should make sure that all the scripts and other artifacts used in the customization of a cluster remain available throughout the lifetime of the cluster and that the versions of these files do not change for the duration.</span></span> <span data-ttu-id="5681b-196">Pokud obnovování uzly v clusteru se vyžaduje, je nutné tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="5681b-196">These resources are required if the reimaging of nodes in the cluster is required.</span></span> <span data-ttu-id="5681b-197">Osvědčeným postupem je ke stažení a archivaci vše v účtu úložiště, který uživatelské ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="5681b-197">The best practice is to download and archive everything in a Storage account that the user controls.</span></span> <span data-ttu-id="5681b-198">To může být výchozí účet úložiště nebo některé z dalších účtů úložiště, zadaný v době nasazení pro vlastní cluster.</span><span class="sxs-lookup"><span data-stu-id="5681b-198">This can be the default Storage account or any of the additional Storage accounts specified at the time of deployment for a customized cluster.</span></span>
    <span data-ttu-id="5681b-199">V Spark a R přizpůsobit clusteru ukázky zadaný v dokumentaci, například jsme provedli místní kopii prostředky v rámci tohoto účtu úložiště: https://hdiconfigactions.blob.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="5681b-199">In the Spark and R customized cluster samples provided in the documentation, for example, we have made a local copy of the resources in this Storage account: https://hdiconfigactions.blob.core.windows.net/.</span></span>
* <span data-ttu-id="5681b-200">Zajistěte, aby byl skript přizpůsobení clusteru idempotent</span><span class="sxs-lookup"><span data-stu-id="5681b-200">Ensure that the cluster customization script is idempotent</span></span>

    <span data-ttu-id="5681b-201">Měli očekávat, že uzly clusteru služby HDInsight, je obnovit z Image po dobu životnosti clusteru.</span><span class="sxs-lookup"><span data-stu-id="5681b-201">You must expect that the nodes of an HDInsight cluster is reimaged during the cluster lifetime.</span></span> <span data-ttu-id="5681b-202">Spuštění skriptu přizpůsobení clusteru vždy, když je obnovit z Image clusteru.</span><span class="sxs-lookup"><span data-stu-id="5681b-202">The cluster customization script is run whenever a cluster is reimaged.</span></span> <span data-ttu-id="5681b-203">Tento skript musí být navržena pro uzpůsobeny idempotent v tom smyslu, že při obnovování, skript by měl zajištění, že clusteru je vráceny do stejného stavu, který byl právě po skript spustili poprvé původně vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="5681b-203">This script must be designed to be idempotent in the sense that upon reimaging, the script should ensure that the cluster is returned to the same customized state that it was in just after the script ran for the first time when the cluster was initially created.</span></span> <span data-ttu-id="5681b-204">Například pokud vlastní skript nainstalován při prvním spuštění aplikace v D:\AppLocation, pak na každé následné spuštění při obnovování, skript by měl zkontrolujte, zda aplikace existuje v umístění D:\AppLocation předtím, než budete pokračovat v dalších krocích skript.</span><span class="sxs-lookup"><span data-stu-id="5681b-204">For example, if a custom script installed an application at D:\AppLocation on its first run, then on each subsequent run, upon reimaging, the script should check whether the application exists at the D:\AppLocation location before proceeding with other steps in the script.</span></span>
* <span data-ttu-id="5681b-205">Vlastní součásti nainstalovat na optimální umístění</span><span class="sxs-lookup"><span data-stu-id="5681b-205">Install custom components in the optimal location</span></span>

    <span data-ttu-id="5681b-206">Pokud uzly clusteru se obnoví z Image, jednotku C:\ prostředků a systémové jednotce D:\ můžete naformátována, což vede ke ztrátě dat a aplikací nainstalovaných na těchto jednotkách.</span><span class="sxs-lookup"><span data-stu-id="5681b-206">When cluster nodes are reimaged, the C:\ resource drive and D:\ system drive can be reformatted, resulting in the loss of data and applications that had been installed on those drives.</span></span> <span data-ttu-id="5681b-207">To může také dojít, pokud do virtuálních počítačů (VM) Azure uzlu, který je součástí clusteru přestane fungovat a bude nahrazen nový uzel.</span><span class="sxs-lookup"><span data-stu-id="5681b-207">This could also happen if an Azure virtual machine (VM) node that is part of the cluster goes down and is replaced by a new node.</span></span> <span data-ttu-id="5681b-208">Součásti můžete nainstalovat na jednotku D:\, nebo v umístění C:\apps v clusteru.</span><span class="sxs-lookup"><span data-stu-id="5681b-208">You can install components on the D:\ drive or in the C:\apps location on the cluster.</span></span> <span data-ttu-id="5681b-209">Všech jiných umístění na jednotce C:\ jsou vyhrazené.</span><span class="sxs-lookup"><span data-stu-id="5681b-209">All other locations on the C:\ drive are reserved.</span></span> <span data-ttu-id="5681b-210">Zadejte umístění, kde aplikace nebo knihovny se nainstalují ve skriptu přizpůsobení clusteru.</span><span class="sxs-lookup"><span data-stu-id="5681b-210">Specify the location where applications or libraries are to be installed in the cluster customization script.</span></span>
* <span data-ttu-id="5681b-211">Zajištění vysoké dostupnosti architektury clusteru</span><span class="sxs-lookup"><span data-stu-id="5681b-211">Ensure high availability of the cluster architecture</span></span>

    <span data-ttu-id="5681b-212">HDInsight má aktivní – pasivní architekturu pro vysokou dostupnost, ve kterém jednou z hlavního uzlu je v aktivním režimu (které jsou spuštěny služby HDInsight) a z hlavního uzlu je v pohotovostním režimu (v HDInsight, které nejsou spuštěny služby).</span><span class="sxs-lookup"><span data-stu-id="5681b-212">HDInsight has an active-passive architecture for high availability, in which one head node is in active mode (where the HDInsight services are running) and the other head node is in standby mode (in which HDInsight services are not running).</span></span> <span data-ttu-id="5681b-213">Uzly přepínače aktivní a pasivní režim, pokud jsou přerušení služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5681b-213">The nodes switch active and passive modes if HDInsight services are interrupted.</span></span> <span data-ttu-id="5681b-214">Pokud akce skriptu se používá k instalaci služby na obou head uzlů pro vysokou dostupnost, Všimněte si, že není možné automaticky převzít tyto služby uživatel nainstaloval mechanismus převzetí služeb při selhání HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5681b-214">If a script action is used to install services on both head nodes for high availability, note that the HDInsight failover mechanism is not able to automatically fail over these user-installed services.</span></span> <span data-ttu-id="5681b-215">Proto uživatel nainstaloval služeb v HDInsight hlavních uzlech, které jsou očekávané k zajištění vysoké dostupnosti musí mít vlastní mechanismus převzetí služeb při selhání, pokud v režimu aktivní pasivní nebo v režimu aktivní aktivní.</span><span class="sxs-lookup"><span data-stu-id="5681b-215">So user-installed services on HDInsight head nodes that are expected to be highly available must either have their own failover mechanism if in active-passive mode or be in active-active mode.</span></span>

    <span data-ttu-id="5681b-216">Příkaz akce skriptu HDInsight spustí na obou hlavních uzlech při roli Hlavní uzel je zadán jako hodnota v *ClusterRoleCollection* parametr.</span><span class="sxs-lookup"><span data-stu-id="5681b-216">An HDInsight Script Action command runs on both head nodes when the head-node role is specified as a value in the *ClusterRoleCollection* parameter.</span></span> <span data-ttu-id="5681b-217">Proto při návrhu vlastní skript, ujistěte se, že váš skript známa tento instalační program.</span><span class="sxs-lookup"><span data-stu-id="5681b-217">So when you design a custom script, make sure that your script is aware of this setup.</span></span> <span data-ttu-id="5681b-218">Nespouštějte k potížím, kde jsou stejné služby instalaci a spuštění na obou hlavních uzlech a jejich skončili neslučitelných mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="5681b-218">You should not run into problems where the same services are installed and started on both of the head nodes and they end up competing with each other.</span></span> <span data-ttu-id="5681b-219">Navíc mějte na paměti, že dojde ke ztrátě dat během obnovování, takže softwaru nainstalované prostřednictvím akce skriptu musí být odolné vůči tyto události.</span><span class="sxs-lookup"><span data-stu-id="5681b-219">Also, be aware that data is lost during reimaging, so software installed via Script Action has to be resilient to such events.</span></span> <span data-ttu-id="5681b-220">Aplikace by měl být pro práci s vysokou dostupností data, která se distribuuje do mnoha uzly.</span><span class="sxs-lookup"><span data-stu-id="5681b-220">Applications should be designed to work with highly available data that is distributed across many nodes.</span></span> <span data-ttu-id="5681b-221">Všimněte si, že až 1/5 uzlů v clusteru lze obnovit z Image ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="5681b-221">Note that as many as 1/5 of the nodes in a cluster can be reimaged at the same time.</span></span>
* <span data-ttu-id="5681b-222">Konfigurace vlastní součásti, které budou používat úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="5681b-222">Configure the custom components to use Azure Blob storage</span></span>

    <span data-ttu-id="5681b-223">Vlastní komponenty, které instalujete na uzlech clusteru může mít výchozí konfiguraci, kterou chcete používat Hadoop Distributed File System (HDFS) úložiště.</span><span class="sxs-lookup"><span data-stu-id="5681b-223">The custom components that you install on the cluster nodes might have a default configuration to use Hadoop Distributed File System (HDFS) storage.</span></span> <span data-ttu-id="5681b-224">Měli byste změnit konfiguraci místo toho používat úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="5681b-224">You should change the configuration to use Azure Blob storage instead.</span></span> <span data-ttu-id="5681b-225">Na obnovení z Image clusteru systému souborů HDFS získá formátu a by ztratíte všechna data, která je uložena existuje.</span><span class="sxs-lookup"><span data-stu-id="5681b-225">On a cluster reimage, the HDFS file system gets formatted and you would lose any data that is stored there.</span></span> <span data-ttu-id="5681b-226">Použití úložiště objektů Blob v Azure místo toho zajišťuje, aby vaše data se uchovávají.</span><span class="sxs-lookup"><span data-stu-id="5681b-226">Using Azure Blob storage instead ensures that your data is retained.</span></span>

## <a name="common-usage-patterns"></a><span data-ttu-id="5681b-227">Obecné vzory využití</span><span class="sxs-lookup"><span data-stu-id="5681b-227">Common usage patterns</span></span>
<span data-ttu-id="5681b-228">Tato část obsahuje pokyny k implementaci některých běžných vzorů využití, které se mohou vyskytnout při zápisu vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="5681b-228">This section provides guidance on implementing some of the common usage patterns that you might run into while writing your own custom script.</span></span>

### <a name="configure-environment-variables"></a><span data-ttu-id="5681b-229">Nakonfigurujte proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="5681b-229">Configure environment variables</span></span>
<span data-ttu-id="5681b-230">Často v vývoj akcí skriptů, si myslíte, že třeba nutnost nastavení proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="5681b-230">Often in script action development, you feel the need to set environment variables.</span></span> <span data-ttu-id="5681b-231">Například je nejpravděpodobnější scénář při stahování binární z externího webu, nainstalujte ji na clusteru a přidejte umístění, kde je instalována do proměnné prostředí vaší 'PATH'.</span><span class="sxs-lookup"><span data-stu-id="5681b-231">For instance, a most likely scenario is when you download a binary from an external site, install it on the cluster, and add the location of where it is installed to your ‘PATH’ environment variable.</span></span> <span data-ttu-id="5681b-232">Následující fragment kódu ukazuje, jak nastavení proměnných prostředí ve vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="5681b-232">The following snippet shows you how to set environment variables in the custom script.</span></span>

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

<span data-ttu-id="5681b-233">Tento příkaz nastaví proměnnou prostředí **MDS_RUNNER_CUSTOM_CLUSTER** na hodnotu "true" a také nastaví rozsah této proměnné můžete být celého systému.</span><span class="sxs-lookup"><span data-stu-id="5681b-233">This statement sets the environment variable **MDS_RUNNER_CUSTOM_CLUSTER** to the value 'true' and also sets the scope of this variable to be machine-wide.</span></span> <span data-ttu-id="5681b-234">V některých případech je důležité, aby proměnné prostředí se nastavují v příslušné oboru – počítače nebo uživatele.</span><span class="sxs-lookup"><span data-stu-id="5681b-234">At times it is important that environment variables are set at the appropriate scope – machine or user.</span></span> <span data-ttu-id="5681b-235">Odkazovat [sem] [ 1] Další informace o nastavení proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="5681b-235">Refer [here][1] for more information on setting environment variables.</span></span>

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a><span data-ttu-id="5681b-236">Přístup k umístění, kde jsou uloženy vlastní skripty</span><span class="sxs-lookup"><span data-stu-id="5681b-236">Access to locations where the custom scripts are stored</span></span>
<span data-ttu-id="5681b-237">Skripty použít pro přizpůsobení cluster musí buď nacházet ve výchozí účet úložiště pro cluster nebo ve veřejném kontejneru jen pro čtení na jiný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="5681b-237">Scripts used to customize a cluster needs to either be in the default storage account for the cluster or in a public read-only container on any other storage account.</span></span> <span data-ttu-id="5681b-238">Pokud skript odkazuje na prostředky, které se nacházejí jinde tyto musí být v veřejně přístupná (alespoň veřejné jen pro čtení).</span><span class="sxs-lookup"><span data-stu-id="5681b-238">If your script accesses resources located elsewhere these need to be in a publicly accessible (at least public read-only).</span></span> <span data-ttu-id="5681b-239">Můžete například chtít přístup k souboru a uložte ho pomocí příkazu SaveFile HDI.</span><span class="sxs-lookup"><span data-stu-id="5681b-239">For instance you might want to access a file and save it using the SaveFile-HDI command.</span></span>

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

<span data-ttu-id="5681b-240">V tomto příkladu je nutné zajistit, že kontejner 'somecontainer' v účtu úložiště 'somestorageaccount' je veřejně přístupná.</span><span class="sxs-lookup"><span data-stu-id="5681b-240">In this example, you must ensure that the container 'somecontainer' in storage account 'somestorageaccount' is publicly accessible.</span></span> <span data-ttu-id="5681b-241">Skript, jinak hodnota vyhodí výjimku "Nebyl nalezen" a selhání.</span><span class="sxs-lookup"><span data-stu-id="5681b-241">Otherwise, the script throws a ‘Not Found’ exception and fail.</span></span>

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a><span data-ttu-id="5681b-242">Předat parametry do rutiny přidat AzureRmHDInsightScriptAction</span><span class="sxs-lookup"><span data-stu-id="5681b-242">Pass parameters to the Add-AzureRmHDInsightScriptAction cmdlet</span></span>
<span data-ttu-id="5681b-243">Chcete-li předat do rutiny přidat AzureRmHDInsightScriptAction několik parametrů, je potřeba formátu řetězcovou hodnotu tak, aby obsahovala všechny parametry pro skript.</span><span class="sxs-lookup"><span data-stu-id="5681b-243">To pass multiple parameters to the Add-AzureRmHDInsightScriptAction cmdlet, you need to format the string value to contain all parameters for the script.</span></span> <span data-ttu-id="5681b-244">Například:</span><span class="sxs-lookup"><span data-stu-id="5681b-244">For example:</span></span>

    "-CertifcateUri wasb:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

<span data-ttu-id="5681b-245">nebo</span><span class="sxs-lookup"><span data-stu-id="5681b-245">or</span></span>

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a><span data-ttu-id="5681b-246">Throw – výjimka pro nasazení clusteru se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="5681b-246">Throw exception for failed cluster deployment</span></span>
<span data-ttu-id="5681b-247">Pokud chcete získat přesně informováni o nebylo úspěšné skutečnost, že přizpůsobení clusteru podle očekávání, je nutné vyvolat výjimku a selhání vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="5681b-247">If you want to get accurately notified of the fact that cluster customization did not succeed as expected, it is important to throw an exception and fail the cluster creation.</span></span> <span data-ttu-id="5681b-248">Můžete například chtít zpracovat soubor, pokud existuje a zpracování chyby případu, kdy soubor neexistuje.</span><span class="sxs-lookup"><span data-stu-id="5681b-248">For instance, you might want to process a file if it exists and handle the error case where the file does not exist.</span></span> <span data-ttu-id="5681b-249">To by zajistěte, aby skript ukončí řádně a stav clusteru správně je známý.</span><span class="sxs-lookup"><span data-stu-id="5681b-249">This would ensure that the script exits gracefully and the state of the cluster is correctly known.</span></span> <span data-ttu-id="5681b-250">Následující fragment kódu poskytuje příklad toho, jak to můžete udělat:</span><span class="sxs-lookup"><span data-stu-id="5681b-250">The following snippet gives an example of how to achieve this:</span></span>

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

<span data-ttu-id="5681b-251">V tento fragment kódu Pokud soubor neexistuje, by vedlo ke stavu, kde skript ve skutečnosti řádně ukončí po tisku chybovou zprávu, a cluster dosáhne stavu spuštěno za předpokladu, že ji "úspěšně" dokončit proces přizpůsobení clusteru.</span><span class="sxs-lookup"><span data-stu-id="5681b-251">In this snippet, if the file did not exist, it would lead to a state where the script actually exits gracefully after printing the error message, and the cluster reaches running state assuming it "successfully" completed cluster customization process.</span></span> <span data-ttu-id="5681b-252">Pokud chcete lépe informováni o skutečnost, že cluster přizpůsobení v podstatě se nezdařilo z důvodu chybějícího souboru očekávaným způsobem, je vhodnější a vyvolána výjimka, selžou krok přizpůsobení clusteru.</span><span class="sxs-lookup"><span data-stu-id="5681b-252">If you want to be accurately notified of the fact that cluster customization essentially did not succeed as expected because of a missing file, it is more appropriate to throw an exception and fail the cluster customization step.</span></span> <span data-ttu-id="5681b-253">K dosažení tohoto cíle musíte použít následující fragment kódu ukázka.</span><span class="sxs-lookup"><span data-stu-id="5681b-253">To achieve this you must use the following sample code snippet instead.</span></span>

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a><span data-ttu-id="5681b-254">Kontrolní seznam pro nasazení akce skriptu</span><span class="sxs-lookup"><span data-stu-id="5681b-254">Checklist for deploying a script action</span></span>
<span data-ttu-id="5681b-255">Zde jsou kroky, které jsme trvalo při přípravě nasazení těchto skriptů:</span><span class="sxs-lookup"><span data-stu-id="5681b-255">Here are the steps we took when preparing to deploy these scripts:</span></span>

1. <span data-ttu-id="5681b-256">Uveďte soubory, které obsahují vlastní skripty na místě, která je přístupná na uzlech clusteru během nasazení.</span><span class="sxs-lookup"><span data-stu-id="5681b-256">Put the files that contain the custom scripts in a place that is accessible by the cluster nodes during deployment.</span></span> <span data-ttu-id="5681b-257">To může být libovolná z výchozí nebo další účty úložiště zadaný v době nasazení clusteru nebo jiných veřejně přístupná úložiště kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5681b-257">This can be any of the default or additional Storage accounts specified at the time of cluster deployment, or any other publicly accessible storage container.</span></span>
2. <span data-ttu-id="5681b-258">Přidejte kontroly na skripty a ujistěte se, že spouštění idempotently, tak, aby skript můžete spustit několikrát na stejném uzlu.</span><span class="sxs-lookup"><span data-stu-id="5681b-258">Add checks into scripts to make sure that they execute idempotently, so that the script can be executed multiple times on the same node.</span></span>
3. <span data-ttu-id="5681b-259">Použití **Write-Output** rutiny Azure Powershellu tisknout do STDOUT a také STDERR.</span><span class="sxs-lookup"><span data-stu-id="5681b-259">Use the **Write-Output** Azure PowerShell cmdlet to print to STDOUT as well as STDERR.</span></span> <span data-ttu-id="5681b-260">Nepoužívejte **Write-Host**.</span><span class="sxs-lookup"><span data-stu-id="5681b-260">Do not use **Write-Host**.</span></span>
4. <span data-ttu-id="5681b-261">Použijte složku dočasného souboru, jako je například $env: dočasné zachovat stažený soubor používá skripty a vyčistit je po mají spouštět skripty.</span><span class="sxs-lookup"><span data-stu-id="5681b-261">Use a temporary file folder, such as $env:TEMP, to keep the downloaded file used by the scripts and then clean them up after scripts have executed.</span></span>
5. <span data-ttu-id="5681b-262">Nainstalujte jenom na D:\ nebo C:\apps vlastní software.</span><span class="sxs-lookup"><span data-stu-id="5681b-262">Install custom software only at D:\ or C:\apps.</span></span> <span data-ttu-id="5681b-263">Jiných umístění na jednotce C: není vhodné používat, protože se jedná o vyhrazené.</span><span class="sxs-lookup"><span data-stu-id="5681b-263">Other locations on the C: drive should not be used as they are reserved.</span></span> <span data-ttu-id="5681b-264">Všimněte si, že instalaci souborů na jednotce C: mimo složku C:\apps může vést instalace selhání během reimages uzlu.</span><span class="sxs-lookup"><span data-stu-id="5681b-264">Note that installing files on the C: drive outside of the C:\apps folder may result in setup failures during reimages of the node.</span></span>
6. <span data-ttu-id="5681b-265">V případě, že nastavení na úrovni operačního systému nebo Hadoop služby konfigurační soubory se změnily, můžete tak, aby se vyzvedávat všechna nastavení, úrovni operačního systému, jako je například proměnné prostředí, které jsou nastavené ve skriptech restartovat služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5681b-265">In the event that OS-level settings or Hadoop service configuration files were changed, you may want to restart HDInsight services so that they can pick up any OS-level settings, such as the environment variables set in the scripts.</span></span>

## <a name="debug-custom-scripts"></a><span data-ttu-id="5681b-266">Ladění vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="5681b-266">Debug custom scripts</span></span>
<span data-ttu-id="5681b-267">Spolu s další výstupu v výchozí účet úložiště, který jste zadali pro clusteru při jeho vytváření jsou uložené v souborech protokolů chyb skriptu.</span><span class="sxs-lookup"><span data-stu-id="5681b-267">The script error logs are stored, along with other output, in the default Storage account that you specified for the cluster at its creation.</span></span> <span data-ttu-id="5681b-268">Protokoly jsou uložené v tabulce s názvem *u < \cluster-name-fragment >< \time-stamp > setuplog*.</span><span class="sxs-lookup"><span data-stu-id="5681b-268">The logs are stored in a table with the name *u<\cluster-name-fragment><\time-stamp>setuplog*.</span></span> <span data-ttu-id="5681b-269">Toto jsou agregovaná protokoly, které mají záznamy ze všech uzlů (hlavního uzlu a pracovní uzly), na kterých bude skript spuštěn v clusteru.</span><span class="sxs-lookup"><span data-stu-id="5681b-269">These are aggregated logs that have records from all of the nodes (head node and worker nodes) on which the script runs in the cluster.</span></span>
<span data-ttu-id="5681b-270">Snadný způsob, jak v protokolech je používat nástroje HDInsight pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5681b-270">An easy way to check the logs is to use HDInsight Tools for Visual Studio.</span></span> <span data-ttu-id="5681b-271">Instalace nástrojů, najdete v části [začněte používat nástroje Visual Studio Hadoop pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)</span><span class="sxs-lookup"><span data-stu-id="5681b-271">For installing the tools, see [Get started using Visual Studio Hadoop tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)</span></span>

<span data-ttu-id="5681b-272">**Zkontrolujte protokol pomocí sady Visual Studio**</span><span class="sxs-lookup"><span data-stu-id="5681b-272">**To check the log using Visual Studio**</span></span>

1. <span data-ttu-id="5681b-273">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5681b-273">Open Visual Studio.</span></span>
2. <span data-ttu-id="5681b-274">Klikněte na tlačítko **zobrazení**a potom klikněte na **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="5681b-274">Click **View**, and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="5681b-275">Klikněte pravým tlačítkem na "Azure", klikněte na tlačítko Připojit k **předplatná Microsoft Azure**a pak zadejte svoje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="5681b-275">Right-click "Azure", click Connect to **Microsoft Azure Subscriptions**, and then enter your credentials.</span></span>
4. <span data-ttu-id="5681b-276">Rozbalte položku **úložiště**, rozbalte účet úložiště Azure používat jako výchozí systém souborů, rozbalte položku **tabulky**a potom dvakrát klikněte na název tabulky.</span><span class="sxs-lookup"><span data-stu-id="5681b-276">Expand **Storage**, expand the Azure storage account used as the default file system, expand **Tables**, and then double-click the table name.</span></span>

<span data-ttu-id="5681b-277">Můžete také vzdáleného do uzlů clusteru zobrazíte STDOUT a STDERR pro vlastní skripty.</span><span class="sxs-lookup"><span data-stu-id="5681b-277">You can also remote into the cluster nodes to see both STDOUT and STDERR for custom scripts.</span></span> <span data-ttu-id="5681b-278">Protokoly na každém uzlu jsou určené jenom pro tento uzel a jsou zaznamenány do **C:\HDInsightLogs\DeploymentAgent.log**.</span><span class="sxs-lookup"><span data-stu-id="5681b-278">The logs on each node are specific only to that node and are logged into **C:\HDInsightLogs\DeploymentAgent.log**.</span></span> <span data-ttu-id="5681b-279">Tyto soubory protokolu zaznamenejte všechny výstupy z vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="5681b-279">These log files record all outputs from the custom script.</span></span> <span data-ttu-id="5681b-280">Fragment kódu příklad protokolu pro akci skriptu Spark vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="5681b-280">An example log snippet for a Spark script action looks like this:</span></span>

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


<span data-ttu-id="5681b-281">Tento protokol je jasné, že akce skriptu Spark byla provedena ve virtuálním počítači s názvem HEADNODE0 a že žádné výjimky došlo k během provádění.</span><span class="sxs-lookup"><span data-stu-id="5681b-281">In this log, it is clear that the Spark script action has been executed on the VM named HEADNODE0 and that no exceptions were thrown during the execution.</span></span>

<span data-ttu-id="5681b-282">V případě, že dojde k chybě provádění, výstup popisující je také součástí tohoto souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="5681b-282">In the event that an execution failure occurs, the output describing it is also contained in this log file.</span></span> <span data-ttu-id="5681b-283">Informací uvedených v těchto protokolech by měl být užitečné při ladění skriptu problémy, které by mohlo dojít.</span><span class="sxs-lookup"><span data-stu-id="5681b-283">The information provided in these logs should be helpful in debugging script problems that may arise.</span></span>

## <a name="see-also"></a><span data-ttu-id="5681b-284">Viz také</span><span class="sxs-lookup"><span data-stu-id="5681b-284">See also</span></span>
* <span data-ttu-id="5681b-285">[Přizpůsobení clusterů HDInsight pomocí akce skriptu][hdinsight-cluster-customize]</span><span class="sxs-lookup"><span data-stu-id="5681b-285">[Customize HDInsight clusters using Script Action][hdinsight-cluster-customize]</span></span>
* <span data-ttu-id="5681b-286">[Nainstalovat a používat Spark v HDInsight clustery][hdinsight-install-spark]</span><span class="sxs-lookup"><span data-stu-id="5681b-286">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]</span></span>
* <span data-ttu-id="5681b-287">[Nainstalovat a používat R na clustery HDInsight][hdinsight-r-scripts]</span><span class="sxs-lookup"><span data-stu-id="5681b-287">[Install and use R on HDInsight clusters][hdinsight-r-scripts]</span></span>
* <span data-ttu-id="5681b-288">[Nainstalovat a používat Solr v clusterech HDInsight](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="5681b-288">[Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span>
* <span data-ttu-id="5681b-289">[Nainstalovat a používat Giraph v clusterech HDInsight](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="5681b-289">[Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span>

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
