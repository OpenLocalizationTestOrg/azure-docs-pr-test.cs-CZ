---
title: "Monitorování virtuálního počítače s Linuxem pomocí rozšíření virtuálního počítače | Microsoft Docs"
description: "Další informace o použití rozšíření diagnostiky Linux k monitorování výkonu a diagnostických dat virtuálního počítače s Linuxem v Azure."
services: virtual-machines-linux
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f54a11c5-5a0e-40ff-af6c-e60bd464058b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: Ning
ms.openlocfilehash: b8c6e2e22d8478b6e92e7b7942f15d37a840fed3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-linux-diagnostic-extension-to-monitor-the-performance-and-diagnostic-data-of-a-linux-vm"></a><span data-ttu-id="f84db-103">Použití diagnostického rozšíření Linuxu pro monitorování údajů o výkonu a diagnostických dat virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="f84db-103">Use the Linux Diagnostic Extension to monitor the performance and diagnostic data of a Linux VM</span></span>

<span data-ttu-id="f84db-104">Tento dokument popisuje 2.3 verzi rozšíření diagnostiky Linux.</span><span class="sxs-lookup"><span data-stu-id="f84db-104">This document describes version 2.3 of the Linux Diagnostic Extension.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f84db-105">Tato verze je zastaralá a může být publikování kdykoli po 30. června 2018.</span><span class="sxs-lookup"><span data-stu-id="f84db-105">This version is deprecated, and it may be unpublished any time after June 30, 2018.</span></span> <span data-ttu-id="f84db-106">Nahradila ji verze 3.0.</span><span class="sxs-lookup"><span data-stu-id="f84db-106">It has been replaced by version 3.0.</span></span> <span data-ttu-id="f84db-107">Další informace najdete v tématu [dokumentace pro verzi 3.0 rozšíření diagnostiky Linux](../diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="f84db-107">For more information, see the [documentation for version 3.0 of the Linux Diagnostic Extension](../diagnostic-extension.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="f84db-108">Úvod</span><span class="sxs-lookup"><span data-stu-id="f84db-108">Introduction</span></span>

<span data-ttu-id="f84db-109">(**Poznámka**: rozšíření diagnostiky Linux je open source na [Githubu](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) kde nejaktuálnější informace o rozšíření prvním publikování.</span><span class="sxs-lookup"><span data-stu-id="f84db-109">(**Note**: The Linux Diagnostic Extension is open-sourced on [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) where the most current information on the extension is first published.</span></span> <span data-ttu-id="f84db-110">Můžete chtít zkontrolovat [GitHub stránce](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) první.)</span><span class="sxs-lookup"><span data-stu-id="f84db-110">You might want to check the [GitHub page](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) first.)</span></span>

<span data-ttu-id="f84db-111">Rozšíření diagnostiky Linux pomáhá uživatele monitorování virtuálních počítačů Linux, které jsou spuštěné v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f84db-111">The Linux Diagnostic Extension helps a user monitor the Linux VMs that are running on Microsoft Azure.</span></span> <span data-ttu-id="f84db-112">Má následující funkce:</span><span class="sxs-lookup"><span data-stu-id="f84db-112">It has the following capabilities:</span></span>

* <span data-ttu-id="f84db-113">Shromažďuje a odesílá informace o výkonu systému z virtuálního počítače s Linuxem do tabulky úložiště uživatele, včetně informací o diagnostiky a syslog.</span><span class="sxs-lookup"><span data-stu-id="f84db-113">Collects and uploads the system performance information from the Linux VM to the user's storage table, including diagnostic and syslog information.</span></span>
* <span data-ttu-id="f84db-114">Umožňuje uživatelům umožnit přizpůsobení metriky dat, které bude shromážděna a nahrát.</span><span class="sxs-lookup"><span data-stu-id="f84db-114">Enables users to customize the data metrics that will be collected and uploaded.</span></span>
* <span data-ttu-id="f84db-115">Umožňuje uživatelům odesílat zadané soubory protokolu do tabulky určené úložiště.</span><span class="sxs-lookup"><span data-stu-id="f84db-115">Enables users to upload specified log files to a designated storage table.</span></span>

<span data-ttu-id="f84db-116">V aktuální verzi 2.3 data obsahují:</span><span class="sxs-lookup"><span data-stu-id="f84db-116">In the current version 2.3, the data includes:</span></span>

* <span data-ttu-id="f84db-117">Všechny Linux Rsyslog protokoly, včetně systému, zabezpečení a protokoly aplikací.</span><span class="sxs-lookup"><span data-stu-id="f84db-117">All Linux Rsyslog logs, including system, security, and application logs.</span></span>
* <span data-ttu-id="f84db-118">Všechna data systému, které je zadáno v [webu řešení System Center křížové platformy](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="f84db-118">All system data that's specified on [the System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span>
* <span data-ttu-id="f84db-119">Soubory protokolu definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="f84db-119">User-specified log files.</span></span>

<span data-ttu-id="f84db-120">Toto rozšíření pracuje s classic i modelech nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f84db-120">This extension works with both the classic and Resource Manager deployment models.</span></span>

### <a name="current-version-of-the-extension-and-deprecation-of-old-versions"></a><span data-ttu-id="f84db-121">Aktuální verze rozšíření a vyřazení staré verze</span><span class="sxs-lookup"><span data-stu-id="f84db-121">Current version of the extension and deprecation of old versions</span></span>

<span data-ttu-id="f84db-122">Nejnovější verze rozšíření je **2.3**, a **všechny starší verze (2.0, 2.1 a 2.2) se již nepoužívá a Nepublikováno konce tohoto roku (2017)**.</span><span class="sxs-lookup"><span data-stu-id="f84db-122">The latest version of the extension is **2.3**, and **any old versions (2.0, 2.1, and 2.2) will be deprecated and unpublished by end of this year (2017)**.</span></span> <span data-ttu-id="f84db-123">Pokud jste nainstalovali rozšíření diagnostiky Linux automatické podverze upgradu zakázána, doporučujeme odinstalovat rozšíření a znovu ji nainstalujte automatické podverze upgradu povoleno.</span><span class="sxs-lookup"><span data-stu-id="f84db-123">If you installed the Linux Diagnostic extension with automatic minor version upgrade disabled, it's strongly recommended that you uninstall the extension and reinstall it with automatic minor version upgrade enabled.</span></span> <span data-ttu-id="f84db-124">Na klasické virtuální počítače (ASM) můžete tím dosáhnout zadáním '2.*, jako je verze při instalaci rozšíření prostřednictvím příkazového řádku Azure XPLAT nebo Powershellu.</span><span class="sxs-lookup"><span data-stu-id="f84db-124">On classic (ASM) VMs, you can achieve this by specifying '2.*' as the version if you are installing the extension through Azure XPLAT CLI or Powershell.</span></span> <span data-ttu-id="f84db-125">Na virtuálních počítačů ARM, lze dosáhnout zahrnutím ' "autoUpgradeMinorVersion": true, v nasazení šablony virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f84db-125">On ARM VMs, you can achieve this by including '"autoUpgradeMinorVersion": true' in the VM deployment template.</span></span> <span data-ttu-id="f84db-126">Všechny nové instalace rozšíření by měl mít také, podverze automatického upgradu zapnuta možnost.</span><span class="sxs-lookup"><span data-stu-id="f84db-126">Also, any new installation of the extension should have the auto minor version upgrade option turned on.</span></span>

## <a name="enable-the-extension"></a><span data-ttu-id="f84db-127">Povolení rozšíření</span><span class="sxs-lookup"><span data-stu-id="f84db-127">Enable the extension</span></span>

<span data-ttu-id="f84db-128">Toto rozšíření můžete povolit pomocí [portál Azure](https://portal.azure.com/#), prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure skripty.</span><span class="sxs-lookup"><span data-stu-id="f84db-128">You can enable this extension by using the [Azure portal](https://portal.azure.com/#), Azure PowerShell, or Azure CLI scripts.</span></span>

<span data-ttu-id="f84db-129">Chcete-li zobrazit a nakonfigurovat výkon systému a data přímo z portálu Azure, postupujte podle [na Azure blog tyto kroky](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).</span><span class="sxs-lookup"><span data-stu-id="f84db-129">To view and configure the system and performance data directly from the Azure portal, follow [these steps on the Azure blog](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).</span></span>

<span data-ttu-id="f84db-130">Tento článek se zaměřuje na tom, jak povolit a konfigurovat rozšíření pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="f84db-130">This article focuses on how to enable and configure the extension by using Azure CLI commands.</span></span> <span data-ttu-id="f84db-131">To umožňuje číst a zobrazovat data přímo z tabulky úložiště.</span><span class="sxs-lookup"><span data-stu-id="f84db-131">This allows you to read and view the data directly from the storage table.</span></span>

<span data-ttu-id="f84db-132">Všimněte si, že konfigurace metody, které jsou zde popsané nebudou fungovat pro portál Azure.</span><span class="sxs-lookup"><span data-stu-id="f84db-132">Note that the configuration methods that are described here won't work for the Azure portal.</span></span> <span data-ttu-id="f84db-133">Pokud chcete zobrazit a konfigurovat výkon systému a data přímo z portálu Azure, musí být povolena rozšíření prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="f84db-133">To view and configure the system and performance data directly from the Azure portal, the extension must be enabled through the portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f84db-134">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f84db-134">Prerequisites</span></span>

* <span data-ttu-id="f84db-135">**Azure Linux Agent verze 2.0.6 nebo novější**.</span><span class="sxs-lookup"><span data-stu-id="f84db-135">**Azure Linux Agent version 2.0.6 or later**.</span></span>

  <span data-ttu-id="f84db-136">Všimněte si, že většina Galerie Image virtuálních počítačů Linux Azure zahrnují verze 2.0.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f84db-136">Note that most Azure VM Linux gallery images include version 2.0.6 or later.</span></span> <span data-ttu-id="f84db-137">Můžete spustit **příkaz WAAgent-verze** k potvrzení, která verze je nainstalovaná ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="f84db-137">You can run **WAAgent -version** to confirm which version is installed on the VM.</span></span> <span data-ttu-id="f84db-138">Pokud je virtuální počítač spuštěný na verzi, která je starší než 2.0.6, můžete podle [tyto pokyny na Githubu](https://github.com/Azure/WALinuxAgent "pokyny") jej aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="f84db-138">If the VM is running a version that's earlier than 2.0.6, you can follow [these instructions on GitHub](https://github.com/Azure/WALinuxAgent "instructions") to update it.</span></span>
* <span data-ttu-id="f84db-139">**Rozhraní příkazového řádku Azure**.</span><span class="sxs-lookup"><span data-stu-id="f84db-139">**Azure CLI**.</span></span> <span data-ttu-id="f84db-140">Postupujte podle [tyto pokyny pro instalaci rozhraní příkazového řádku](../../../cli-install-nodejs.md) nastavení prostředí příkazového řádku Azure CLI na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="f84db-140">Follow [this guidance for installing CLI](../../../cli-install-nodejs.md) to set up the Azure CLI environment on your machine.</span></span> <span data-ttu-id="f84db-141">Po instalaci rozhraní příkazového řádku Azure, můžete použít **azure** příkaz vaše rozhraní příkazového řádku (Bash, Terminálové nebo příkazového řádku) pro přístup k příkazy rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="f84db-141">After Azure CLI is installed, you can use the **azure** command from your command-line interface (Bash, Terminal, or command prompt) to access the Azure CLI commands.</span></span> <span data-ttu-id="f84db-142">Například:</span><span class="sxs-lookup"><span data-stu-id="f84db-142">For example:</span></span>

  * <span data-ttu-id="f84db-143">Spustit **sadu rozšíření virtuálního počítače azure – Nápověda** podrobnou nápovědu informace.</span><span class="sxs-lookup"><span data-stu-id="f84db-143">Run **azure vm extension set --help** for detailed help information.</span></span>
  * <span data-ttu-id="f84db-144">Spustit **přihlášení k azure** pro přihlášení k Azure.</span><span class="sxs-lookup"><span data-stu-id="f84db-144">Run **azure login** to sign in to Azure.</span></span>
  * <span data-ttu-id="f84db-145">Spustit **seznamu virtuálních počítačů azure** seznam všech virtuálních počítačů, které máte v Azure.</span><span class="sxs-lookup"><span data-stu-id="f84db-145">Run **azure vm list** to list all the virtual machines that you have on Azure.</span></span>
* <span data-ttu-id="f84db-146">Účet úložiště pro ukládání dat.</span><span class="sxs-lookup"><span data-stu-id="f84db-146">A storage account to store the data.</span></span> <span data-ttu-id="f84db-147">Budete potřebovat název účtu úložiště, který byl vytvořen dříve a přístupový klíč chcete nahrát data do úložiště.</span><span class="sxs-lookup"><span data-stu-id="f84db-147">You will need a storage account name that was created previously and an access key to upload the data to your storage.</span></span>

## <a name="use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension"></a><span data-ttu-id="f84db-148">Povolit rozšíření diagnostiky Linux pomocí příkazu příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f84db-148">Use the Azure CLI command to enable the Linux Diagnostic Extension</span></span>

### <a name="scenario-1-enable-the-extension-with-the-default-data-set"></a><span data-ttu-id="f84db-149">Scénář 1.</span><span class="sxs-lookup"><span data-stu-id="f84db-149">Scenario 1.</span></span> <span data-ttu-id="f84db-150">Povolit rozšíření s výchozí sadou dat</span><span class="sxs-lookup"><span data-stu-id="f84db-150">Enable the extension with the default data set</span></span>

<span data-ttu-id="f84db-151">Ve verzi 2.3 nebo novější výchozí data, která se budou shromažďovat zahrnují:</span><span class="sxs-lookup"><span data-stu-id="f84db-151">In version 2.3 or later, the default data that will be collected includes:</span></span>

* <span data-ttu-id="f84db-152">Všechny informace Rsyslog (včetně systému, zabezpečení a protokoly aplikací).</span><span class="sxs-lookup"><span data-stu-id="f84db-152">All Rsyslog information (including system, security, and application logs).</span></span>  
* <span data-ttu-id="f84db-153">Základní sady dat základ systému.</span><span class="sxs-lookup"><span data-stu-id="f84db-153">A core set of basis system data.</span></span> <span data-ttu-id="f84db-154">Všimněte si, že úplné datové sady je popsáno na [řešení System Center křížové platformy lokality](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="f84db-154">Note that the full data set is described on the [System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span>
  <span data-ttu-id="f84db-155">Pokud chcete povolit doplňující data, pokračujte kroky v scénáře 2 a 3.</span><span class="sxs-lookup"><span data-stu-id="f84db-155">If you want to enable extra data, continue with the steps in Scenarios 2 and 3.</span></span>

<span data-ttu-id="f84db-156">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="f84db-156">Step 1.</span></span> <span data-ttu-id="f84db-157">Vytvořte soubor s názvem PrivateConfig.json s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="f84db-157">Create a file named PrivateConfig.json with the following content:</span></span>

    {
        "storageAccountName" : "the storage account to receive data",
        "storageAccountKey" : "the key of the account"
    }

<span data-ttu-id="f84db-158">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="f84db-158">Step 2.</span></span> <span data-ttu-id="f84db-159">Spustit  **rozšíření virtuálního počítače azure nastavit vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* – privátní config-path PrivateConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="f84db-159">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* --private-config-path PrivateConfig.json**.</span></span>

### <a name="scenario-2-customize-the-performance-monitor-metrics"></a><span data-ttu-id="f84db-160">Scénář 2.</span><span class="sxs-lookup"><span data-stu-id="f84db-160">Scenario 2.</span></span> <span data-ttu-id="f84db-161">Přizpůsobení metriky sledování výkonu</span><span class="sxs-lookup"><span data-stu-id="f84db-161">Customize the performance monitor metrics</span></span>

<span data-ttu-id="f84db-162">Tato část popisuje, jak přizpůsobit výkonu a diagnostických dat tabulky.</span><span class="sxs-lookup"><span data-stu-id="f84db-162">This section describes how to customize the performance and diagnostic data table.</span></span>

<span data-ttu-id="f84db-163">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="f84db-163">Step 1.</span></span> <span data-ttu-id="f84db-164">Vytvořte soubor s názvem PrivateConfig.json s obsahem, který je popsáno ve scénáři 1.</span><span class="sxs-lookup"><span data-stu-id="f84db-164">Create a file named PrivateConfig.json with the content that was described in Scenario 1.</span></span> <span data-ttu-id="f84db-165">Vytvořte také soubor s názvem PublicConfig.json.</span><span class="sxs-lookup"><span data-stu-id="f84db-165">Also create a file named PublicConfig.json.</span></span> <span data-ttu-id="f84db-166">Zadejte konkrétní data, která chcete shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="f84db-166">Specify the particular data you want to collect.</span></span>

<span data-ttu-id="f84db-167">Pro všechny podporované zprostředkovatele a proměnné odkazují [řešení System Center křížové platformy lokality](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="f84db-167">For all supported providers and variables, reference the [System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span> <span data-ttu-id="f84db-168">Můžete mít více dotazů a uložit je do více tabulek přidáním další dotazy do skriptu.</span><span class="sxs-lookup"><span data-stu-id="f84db-168">You can have multiple queries and store them in multiple tables by appending more queries to the script.</span></span>

<span data-ttu-id="f84db-169">Ve výchozím nastavení je vždy shromažďují Rsyslog data.</span><span class="sxs-lookup"><span data-stu-id="f84db-169">By default, the Rsyslog data is always collected.</span></span>

    {
          "perfCfg":
          [
              {
                  "query" : "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
                  "table" : "LinuxMemory"
              }
          ]
    }


<span data-ttu-id="f84db-170">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="f84db-170">Step 2.</span></span> <span data-ttu-id="f84db-171">Spustit  **rozšíření virtuálního počítače azure nastavit vm_name LinuxDiagnostic Microsoft.OSTCExtensions: 2.*' – privátní config-path PrivateConfig.json – veřejné config-path PublicConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="f84db-171">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json**.</span></span>

### <a name="scenario-3-upload-your-own-log-files"></a><span data-ttu-id="f84db-172">Scénář 3.</span><span class="sxs-lookup"><span data-stu-id="f84db-172">Scenario 3.</span></span> <span data-ttu-id="f84db-173">Odeslání souborů protokolu</span><span class="sxs-lookup"><span data-stu-id="f84db-173">Upload your own log files</span></span>

<span data-ttu-id="f84db-174">Tato část popisuje, jak ke sběru a odeslat konkrétní soubory do svého účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f84db-174">This section describes how to collect and upload specific log files to your storage account.</span></span> <span data-ttu-id="f84db-175">Je třeba zadat cestu k souboru protokolu a název tabulky, kam chcete uložit protokolu.</span><span class="sxs-lookup"><span data-stu-id="f84db-175">You need to specify both the path to your log file and the name of the table where you want to store your log.</span></span> <span data-ttu-id="f84db-176">Přidáním více položek souboru nebo tabulky do skriptu můžete vytvořit více souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="f84db-176">You can create multiple log files by adding multiple file/table entries to the script.</span></span>

<span data-ttu-id="f84db-177">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="f84db-177">Step 1.</span></span> <span data-ttu-id="f84db-178">Vytvořte soubor s názvem PrivateConfig.json s obsahem, který je popsáno ve scénáři 1.</span><span class="sxs-lookup"><span data-stu-id="f84db-178">Create a file named PrivateConfig.json with the content that was described in Scenario 1.</span></span> <span data-ttu-id="f84db-179">Pak vytvořte jiný soubor s názvem PublicConfig.json s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="f84db-179">Then create another file named PublicConfig.json with the following content:</span></span>

```json
{
    "fileCfg" :
    [
        {
            "file" : "/var/log/mysql.err",
            "table" : "mysqlerr"
            }
    ]
}
```

<span data-ttu-id="f84db-180">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="f84db-180">Step 2.</span></span> <span data-ttu-id="f84db-181">Spusťte `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json`.</span><span class="sxs-lookup"><span data-stu-id="f84db-181">Run `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json`.</span></span>

<span data-ttu-id="f84db-182">Všimněte si, že s tímto nastavením na rozšíření verze starší než 2.3 všechny protokoly zapisují do `/var/log/mysql.err` může být duplicitní k `/var/log/syslog` (nebo `/var/log/messages` v závislosti na Linux distro) také.</span><span class="sxs-lookup"><span data-stu-id="f84db-182">Note that with this setting on the extension versions prior to 2.3, all logs written to `/var/log/mysql.err` might be duplicated to `/var/log/syslog` (or `/var/log/messages` depending on the Linux distro) as well.</span></span> <span data-ttu-id="f84db-183">Pokud chcete, aby se zabránilo duplicitním protokolování, můžete je vyloučit protokolování `local6` protokolů zařízení ve vaší konfiguraci rsyslog.</span><span class="sxs-lookup"><span data-stu-id="f84db-183">If you'd like to avoid this duplicate logging, you can exclude logging of `local6` facility logs in your rsyslog configuration.</span></span> <span data-ttu-id="f84db-184">Závisí na Linux distro, ale u systému Ubuntu 14.04, je soubor k úpravě `/etc/rsyslog.d/50-default.conf` a můžete nahradit řádek `*.*;auth,authpriv.none -/var/log/syslog` k `*.*;auth,authpriv,local6.none -/var/log/syslog`.</span><span class="sxs-lookup"><span data-stu-id="f84db-184">It depends on the Linux distro, but on an Ubuntu 14.04 system, the file to modify is `/etc/rsyslog.d/50-default.conf` and you can replace the line `*.*;auth,authpriv.none -/var/log/syslog` to `*.*;auth,authpriv,local6.none -/var/log/syslog`.</span></span> <span data-ttu-id="f84db-185">Tento problém vyřešen v nejnovější opravy hotfix verzi 2.3 (2.3.9007), takže pokud máte verzi rozšíření 2.3, tento problém došlo k neočekávané chybě.</span><span class="sxs-lookup"><span data-stu-id="f84db-185">This issue is fixed in the latest hotfix release of 2.3 (2.3.9007), so if you have the extension version 2.3, this issue should not happen.</span></span> <span data-ttu-id="f84db-186">Pokud k tomu ještě i po restartování virtuálního počítače, kontaktujte nás a Pomozte nám Poradce při potížích se není automaticky nainstalovaná nejnovější verze opravy hotfix.</span><span class="sxs-lookup"><span data-stu-id="f84db-186">If it still does even after restarting your VM, please contact us and help us troubleshoot why the latest hotfix version is not installed automatically.</span></span>

### <a name="scenario-4-stop-the-extension-from-collecting-any-logs"></a><span data-ttu-id="f84db-187">Scénář 4.</span><span class="sxs-lookup"><span data-stu-id="f84db-187">Scenario 4.</span></span> <span data-ttu-id="f84db-188">Zastavit rozšíření z shromažďování žádné protokoly</span><span class="sxs-lookup"><span data-stu-id="f84db-188">Stop the extension from collecting any logs</span></span>

<span data-ttu-id="f84db-189">Tato část popisuje postup zastavení rozšíření z shromažďování protokolů.</span><span class="sxs-lookup"><span data-stu-id="f84db-189">This section describes how to stop the extension from collecting logs.</span></span> <span data-ttu-id="f84db-190">Všimněte si, že proces agenta monitorování bude dál spuštěný a funkční i přes tuto změnu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f84db-190">Note that the monitoring agent process will be still up and running even with this reconfiguration.</span></span> <span data-ttu-id="f84db-191">Pokud chcete úplně zastavit proces agenta monitorování, můžete tak učinit zakázáním rozšíření.</span><span class="sxs-lookup"><span data-stu-id="f84db-191">If you'd like to stop the monitoring agent process completely, you can do so by disabling the extension.</span></span> <span data-ttu-id="f84db-192">Příkaz rozšíření zakázat je `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.</span><span class="sxs-lookup"><span data-stu-id="f84db-192">The command to disable the extension is `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.</span></span>

<span data-ttu-id="f84db-193">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="f84db-193">Step 1.</span></span> <span data-ttu-id="f84db-194">Vytvořte soubor s názvem PrivateConfig.json s obsahem, který je popsáno ve scénáři 1.</span><span class="sxs-lookup"><span data-stu-id="f84db-194">Create a file named PrivateConfig.json with the content that was described in Scenario 1.</span></span> <span data-ttu-id="f84db-195">Vytvořte jiný soubor s názvem PublicConfig.json s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="f84db-195">Create another file named PublicConfig.json with the following content:</span></span>

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


<span data-ttu-id="f84db-196">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="f84db-196">Step 2.</span></span> <span data-ttu-id="f84db-197">Spustit  **rozšíření virtuálního počítače azure nastavit vm_name LinuxDiagnostic Microsoft.OSTCExtensions: 2.*' – privátní config-path PrivateConfig.json – veřejné config-path PublicConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="f84db-197">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json**.</span></span>

## <a name="review-your-data"></a><span data-ttu-id="f84db-198">Zkontrolujte vaše data</span><span class="sxs-lookup"><span data-stu-id="f84db-198">Review your data</span></span>

<span data-ttu-id="f84db-199">Výkon a diagnostických dat jsou uložené v tabulce Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="f84db-199">The performance and diagnostic data are stored in an Azure Storage table.</span></span> <span data-ttu-id="f84db-200">Zkontrolujte [jak používat Azure Table Storage z Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) se dozvíte, jak k přístupu k datům v tabulce úložiště pomocí rozhraní příkazového řádku Azure skriptů.</span><span class="sxs-lookup"><span data-stu-id="f84db-200">Review [How to use Azure Table Storage from Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) to learn how to access the data in the storage table by using Azure CLI scripts.</span></span>

<span data-ttu-id="f84db-201">Kromě toho můžete tyto nástroje uživatelského rozhraní pro přístup k datům:</span><span class="sxs-lookup"><span data-stu-id="f84db-201">In addition, you can use following UI tools to access the data:</span></span>

1. <span data-ttu-id="f84db-202">Průzkumníka serveru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f84db-202">Visual Studio Server Explorer.</span></span> <span data-ttu-id="f84db-203">Přejděte na svůj účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="f84db-203">Go to your storage account.</span></span> <span data-ttu-id="f84db-204">Po dobu asi 5 minut spuštění virtuálního počítače, uvidíte čtyři výchozí tabulky: "LinuxCpu", "LinuxDisk", "LinuxMemory" a "Linuxsyslog".</span><span class="sxs-lookup"><span data-stu-id="f84db-204">After the VM runs for about five minutes, you'll see the four default tables: “LinuxCpu”, ”LinuxDisk”, ”LinuxMemory”, and ”Linuxsyslog”.</span></span> <span data-ttu-id="f84db-205">Dvakrát klikněte na názvy tabulek, které chcete zobrazit data.</span><span class="sxs-lookup"><span data-stu-id="f84db-205">Double-click the table names to view the data.</span></span>
1. <span data-ttu-id="f84db-206">[Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").</span><span class="sxs-lookup"><span data-stu-id="f84db-206">[Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").</span></span>

![Bitové kopie](./media/diagnostic-extension/no1.png)

<span data-ttu-id="f84db-208">Pokud jste povolili fileCfg nebo perfCfg (jak je popsáno v scénáře 2 a 3), můžete použít Průzkumníka serveru Visual Studia a Azure Storage Explorer pro zobrazení dat jiné než výchozí.</span><span class="sxs-lookup"><span data-stu-id="f84db-208">If you've enabled fileCfg or perfCfg (as described in Scenarios 2 and 3), you can use Visual Studio Server Explorer and Azure Storage Explorer to view non-default data.</span></span>

## <a name="known-issues"></a><span data-ttu-id="f84db-209">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="f84db-209">Known issues</span></span>

* <span data-ttu-id="f84db-210">Rsyslog informace a zákazník zadaný soubor protokolu můžete přistupovat pouze pomocí skriptů.</span><span class="sxs-lookup"><span data-stu-id="f84db-210">The Rsyslog information and customer-specified log file can only be accessed via scripting.</span></span>
