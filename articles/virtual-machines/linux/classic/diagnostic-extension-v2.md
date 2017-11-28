---
title: "virtuální počítač s Linuxem pomocí rozšíření virtuálního počítače aaaMonitoring | Microsoft Docs"
description: "Zjistěte, jak toouse hello rozšíření diagnostiky Linux toomonitor hello výkon a diagnostických dat virtuálního počítače s Linuxem v Azure."
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
ms.openlocfilehash: cf7bfebca8c0367941f7a975417f60fe2e2dab25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-linux-diagnostic-extension-toomonitor-hello-performance-and-diagnostic-data-of-a-linux-vm"></a><span data-ttu-id="18df2-103">Použít hello rozšíření diagnostiky Linux toomonitor hello výkonu a diagnostických dat virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="18df2-103">Use hello Linux Diagnostic Extension toomonitor hello performance and diagnostic data of a Linux VM</span></span>

<span data-ttu-id="18df2-104">Tento dokument popisuje 2.3 verzi hello rozšíření diagnostiky Linux.</span><span class="sxs-lookup"><span data-stu-id="18df2-104">This document describes version 2.3 of hello Linux Diagnostic Extension.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18df2-105">Tato verze je zastaralá a může být publikování kdykoli po 30. června 2018.</span><span class="sxs-lookup"><span data-stu-id="18df2-105">This version is deprecated, and it may be unpublished any time after June 30, 2018.</span></span> <span data-ttu-id="18df2-106">Nahradila ji verze 3.0.</span><span class="sxs-lookup"><span data-stu-id="18df2-106">It has been replaced by version 3.0.</span></span> <span data-ttu-id="18df2-107">Další informace najdete v tématu hello [dokumentace pro verzi 3.0 hello rozšíření diagnostiky Linux](../diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="18df2-107">For more information, see hello [documentation for version 3.0 of hello Linux Diagnostic Extension](../diagnostic-extension.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="18df2-108">Úvod</span><span class="sxs-lookup"><span data-stu-id="18df2-108">Introduction</span></span>

<span data-ttu-id="18df2-109">(**Poznámka**: hello rozšíření diagnostiky Linux je open source na [Githubu](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) kde hello nejaktuálnější informace o rozšíření hello prvním publikování.</span><span class="sxs-lookup"><span data-stu-id="18df2-109">(**Note**: hello Linux Diagnostic Extension is open-sourced on [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) where hello most current information on hello extension is first published.</span></span> <span data-ttu-id="18df2-110">Můžete chtít toocheck hello [GitHub stránce](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) první.)</span><span class="sxs-lookup"><span data-stu-id="18df2-110">You might want toocheck hello [GitHub page](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) first.)</span></span>

<span data-ttu-id="18df2-111">Hello rozšíření diagnostiky Linux pomáhá hello monitorování uživatel virtuální počítače Linux, které jsou spuštěné v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="18df2-111">hello Linux Diagnostic Extension helps a user monitor hello Linux VMs that are running on Microsoft Azure.</span></span> <span data-ttu-id="18df2-112">Má hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="18df2-112">It has hello following capabilities:</span></span>

* <span data-ttu-id="18df2-113">Shromažďuje a odesílá informace o výkonu systému hello z tabulky úložiště hello virtuálního počítače s Linuxem toohello uživatele, včetně informací o diagnostiky a syslog.</span><span class="sxs-lookup"><span data-stu-id="18df2-113">Collects and uploads hello system performance information from hello Linux VM toohello user's storage table, including diagnostic and syslog information.</span></span>
* <span data-ttu-id="18df2-114">Umožňuje uživatelům toocustomize hello data metriky, které bude shromážděna a nahrát.</span><span class="sxs-lookup"><span data-stu-id="18df2-114">Enables users toocustomize hello data metrics that will be collected and uploaded.</span></span>
* <span data-ttu-id="18df2-115">Umožňuje uživatelům tooupload protokol příslušné soubory tooa určené úložiště tabulky.</span><span class="sxs-lookup"><span data-stu-id="18df2-115">Enables users tooupload specified log files tooa designated storage table.</span></span>

<span data-ttu-id="18df2-116">V aktuální verzi hello 2.3 hello data zahrnují:</span><span class="sxs-lookup"><span data-stu-id="18df2-116">In hello current version 2.3, hello data includes:</span></span>

* <span data-ttu-id="18df2-117">Všechny Linux Rsyslog protokoly, včetně systému, zabezpečení a protokoly aplikací.</span><span class="sxs-lookup"><span data-stu-id="18df2-117">All Linux Rsyslog logs, including system, security, and application logs.</span></span>
* <span data-ttu-id="18df2-118">Všechna data systému, které je zadáno v [hello řešení System Center křížové platformy lokality](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="18df2-118">All system data that's specified on [hello System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span>
* <span data-ttu-id="18df2-119">Soubory protokolu definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="18df2-119">User-specified log files.</span></span>

<span data-ttu-id="18df2-120">Toto rozšíření funguje s hello classic a modelech nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="18df2-120">This extension works with both hello classic and Resource Manager deployment models.</span></span>

### <a name="current-version-of-hello-extension-and-deprecation-of-old-versions"></a><span data-ttu-id="18df2-121">Aktuální verze hello rozšíření a vyřazení staré verze</span><span class="sxs-lookup"><span data-stu-id="18df2-121">Current version of hello extension and deprecation of old versions</span></span>

<span data-ttu-id="18df2-122">nejnovější verze rozšíření hello Hello je **2.3**, a **všechny starší verze (2.0, 2.1 a 2.2) se již nepoužívá a Nepublikováno konce tohoto roku (2017)**.</span><span class="sxs-lookup"><span data-stu-id="18df2-122">hello latest version of hello extension is **2.3**, and **any old versions (2.0, 2.1, and 2.2) will be deprecated and unpublished by end of this year (2017)**.</span></span> <span data-ttu-id="18df2-123">Pokud jste nainstalovali hello rozšíření diagnostiky Linux automatické podverze upgradu zakázána, doporučujeme odinstalovat rozšíření hello a znovu ji nainstalujte automatické podverze upgradu povoleno.</span><span class="sxs-lookup"><span data-stu-id="18df2-123">If you installed hello Linux Diagnostic extension with automatic minor version upgrade disabled, it's strongly recommended that you uninstall hello extension and reinstall it with automatic minor version upgrade enabled.</span></span> <span data-ttu-id="18df2-124">Na klasické virtuální počítače (ASM) můžete tím dosáhnout zadáním '2.*' jako verze hello, pokud instalujete rozšíření hello prostřednictvím příkazového řádku Azure XPLAT nebo Powershellu.</span><span class="sxs-lookup"><span data-stu-id="18df2-124">On classic (ASM) VMs, you can achieve this by specifying '2.*' as hello version if you are installing hello extension through Azure XPLAT CLI or Powershell.</span></span> <span data-ttu-id="18df2-125">Na virtuálních počítačů ARM, lze dosáhnout zahrnutím ' "autoUpgradeMinorVersion": true, v hello šablony nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="18df2-125">On ARM VMs, you can achieve this by including '"autoUpgradeMinorVersion": true' in hello VM deployment template.</span></span> <span data-ttu-id="18df2-126">Všechny nové instalace rozšíření hello navíc by měl mít vedlejší verze aktualizace hello automatického upgradu zapnuta možnost.</span><span class="sxs-lookup"><span data-stu-id="18df2-126">Also, any new installation of hello extension should have hello auto minor version upgrade option turned on.</span></span>

## <a name="enable-hello-extension"></a><span data-ttu-id="18df2-127">Povolit rozšíření hello</span><span class="sxs-lookup"><span data-stu-id="18df2-127">Enable hello extension</span></span>

<span data-ttu-id="18df2-128">Toto rozšíření můžete povolit pomocí hello [portál Azure](https://portal.azure.com/#), prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure skripty.</span><span class="sxs-lookup"><span data-stu-id="18df2-128">You can enable this extension by using hello [Azure portal](https://portal.azure.com/#), Azure PowerShell, or Azure CLI scripts.</span></span>

<span data-ttu-id="18df2-129">tooview a konfigurovat systém hello a údaje o výkonu přímo z hello portálu Azure, postupujte podle [na hello Azure blog tyto kroky](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).</span><span class="sxs-lookup"><span data-stu-id="18df2-129">tooview and configure hello system and performance data directly from hello Azure portal, follow [these steps on hello Azure blog](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).</span></span>

<span data-ttu-id="18df2-130">Tento článek se zaměřuje na tooenable a rozšíření hello nakonfigurovat pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="18df2-130">This article focuses on how tooenable and configure hello extension by using Azure CLI commands.</span></span> <span data-ttu-id="18df2-131">To vám umožní tooread a zobrazení hello data přímo z tabulky úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="18df2-131">This allows you tooread and view hello data directly from hello storage table.</span></span>

<span data-ttu-id="18df2-132">Všimněte si, že hello konfigurace metody, které jsou zde popsány nebudou fungovat pro hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="18df2-132">Note that hello configuration methods that are described here won't work for hello Azure portal.</span></span> <span data-ttu-id="18df2-133">tooview a nakonfigurovat hello výkon systému a data přímo z hello portálu Azure, musí být povolené rozšíření hello prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="18df2-133">tooview and configure hello system and performance data directly from hello Azure portal, hello extension must be enabled through hello portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="18df2-134">Požadavky</span><span class="sxs-lookup"><span data-stu-id="18df2-134">Prerequisites</span></span>

* <span data-ttu-id="18df2-135">**Azure Linux Agent verze 2.0.6 nebo novější**.</span><span class="sxs-lookup"><span data-stu-id="18df2-135">**Azure Linux Agent version 2.0.6 or later**.</span></span>

  <span data-ttu-id="18df2-136">Všimněte si, že většina Galerie Image virtuálních počítačů Linux Azure zahrnují verze 2.0.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="18df2-136">Note that most Azure VM Linux gallery images include version 2.0.6 or later.</span></span> <span data-ttu-id="18df2-137">Můžete spustit **příkaz WAAgent-verze** tooconfirm, která verze je nainstalovaná na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="18df2-137">You can run **WAAgent -version** tooconfirm which version is installed on hello VM.</span></span> <span data-ttu-id="18df2-138">Pokud hello virtuální počítač běží na verzi, která je starší než 2.0.6, můžete podle [tyto pokyny na Githubu](https://github.com/Azure/WALinuxAgent "pokyny") tooupdate ho.</span><span class="sxs-lookup"><span data-stu-id="18df2-138">If hello VM is running a version that's earlier than 2.0.6, you can follow [these instructions on GitHub](https://github.com/Azure/WALinuxAgent "instructions") tooupdate it.</span></span>
* <span data-ttu-id="18df2-139">**Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="18df2-139">**Azure CLI**.</span></span> <span data-ttu-id="18df2-140">Postupujte podle [tyto pokyny pro instalaci rozhraní příkazového řádku](../../../cli-install-nodejs.md) tooset prostředí příkazového řádku Azure CLI hello na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="18df2-140">Follow [this guidance for installing CLI](../../../cli-install-nodejs.md) tooset up hello Azure CLI environment on your machine.</span></span> <span data-ttu-id="18df2-141">Po instalaci rozhraní příkazového řádku Azure, můžete použít hello **azure** příkaz vaše rozhraní příkazového řádku (Bash, Terminálové nebo příkazového řádku) tooaccess hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="18df2-141">After Azure CLI is installed, you can use hello **azure** command from your command-line interface (Bash, Terminal, or command prompt) tooaccess hello Azure CLI commands.</span></span> <span data-ttu-id="18df2-142">Například:</span><span class="sxs-lookup"><span data-stu-id="18df2-142">For example:</span></span>

  * <span data-ttu-id="18df2-143">Spustit **sadu rozšíření virtuálního počítače azure – Nápověda** podrobnou nápovědu informace.</span><span class="sxs-lookup"><span data-stu-id="18df2-143">Run **azure vm extension set --help** for detailed help information.</span></span>
  * <span data-ttu-id="18df2-144">Spustit **přihlášení k azure** toosign v tooAzure.</span><span class="sxs-lookup"><span data-stu-id="18df2-144">Run **azure login** toosign in tooAzure.</span></span>
  * <span data-ttu-id="18df2-145">Spustit **seznamu virtuálních počítačů azure** toolist všechny hello virtuálních počítačů, které máte v Azure.</span><span class="sxs-lookup"><span data-stu-id="18df2-145">Run **azure vm list** toolist all hello virtual machines that you have on Azure.</span></span>
* <span data-ttu-id="18df2-146">Úložiště toostore hello data účtu.</span><span class="sxs-lookup"><span data-stu-id="18df2-146">A storage account toostore hello data.</span></span> <span data-ttu-id="18df2-147">Budete potřebovat název účtu úložiště, který byl vytvořen dříve a k přístupu klíče tooupload hello data tooyour úložiště.</span><span class="sxs-lookup"><span data-stu-id="18df2-147">You will need a storage account name that was created previously and an access key tooupload hello data tooyour storage.</span></span>

## <a name="use-hello-azure-cli-command-tooenable-hello-linux-diagnostic-extension"></a><span data-ttu-id="18df2-148">Použít hello rozhraní příkazového řádku Azure příkaz tooenable hello Linux rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="18df2-148">Use hello Azure CLI command tooenable hello Linux Diagnostic Extension</span></span>

### <a name="scenario-1-enable-hello-extension-with-hello-default-data-set"></a><span data-ttu-id="18df2-149">Scénář 1.</span><span class="sxs-lookup"><span data-stu-id="18df2-149">Scenario 1.</span></span> <span data-ttu-id="18df2-150">Povolit rozšíření hello s hello výchozí datové sady</span><span class="sxs-lookup"><span data-stu-id="18df2-150">Enable hello extension with hello default data set</span></span>

<span data-ttu-id="18df2-151">Ve verzi 2.3 nebo novější hello výchozí data, která se budou shromažďovat zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="18df2-151">In version 2.3 or later, hello default data that will be collected includes:</span></span>

* <span data-ttu-id="18df2-152">Všechny informace Rsyslog (včetně systému, zabezpečení a protokoly aplikací).</span><span class="sxs-lookup"><span data-stu-id="18df2-152">All Rsyslog information (including system, security, and application logs).</span></span>  
* <span data-ttu-id="18df2-153">Základní sady dat základ systému.</span><span class="sxs-lookup"><span data-stu-id="18df2-153">A core set of basis system data.</span></span> <span data-ttu-id="18df2-154">Všimněte si, že hello úplné datové sady je popsáno na hello [řešení System Center křížové platformy lokality](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="18df2-154">Note that hello full data set is described on hello [System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span>
  <span data-ttu-id="18df2-155">Pokud chcete, aby tooenable doplňující data, pokračujte kroky hello ve scénářích 2 a 3.</span><span class="sxs-lookup"><span data-stu-id="18df2-155">If you want tooenable extra data, continue with hello steps in Scenarios 2 and 3.</span></span>

<span data-ttu-id="18df2-156">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="18df2-156">Step 1.</span></span> <span data-ttu-id="18df2-157">Vytvořte soubor s názvem PrivateConfig.json s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="18df2-157">Create a file named PrivateConfig.json with hello following content:</span></span>

    {
        "storageAccountName" : "hello storage account tooreceive data",
        "storageAccountKey" : "hello key of hello account"
    }

<span data-ttu-id="18df2-158">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="18df2-158">Step 2.</span></span> <span data-ttu-id="18df2-159">Spustit  **rozšíření virtuálního počítače azure nastavit vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* – privátní config-path PrivateConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="18df2-159">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* --private-config-path PrivateConfig.json**.</span></span>

### <a name="scenario-2-customize-hello-performance-monitor-metrics"></a><span data-ttu-id="18df2-160">Scénář 2.</span><span class="sxs-lookup"><span data-stu-id="18df2-160">Scenario 2.</span></span> <span data-ttu-id="18df2-161">Přizpůsobení metriky monitorování výkonu hello</span><span class="sxs-lookup"><span data-stu-id="18df2-161">Customize hello performance monitor metrics</span></span>

<span data-ttu-id="18df2-162">Tato část popisuje, jak toocustomize hello výkon a diagnostických dat tabulky.</span><span class="sxs-lookup"><span data-stu-id="18df2-162">This section describes how toocustomize hello performance and diagnostic data table.</span></span>

<span data-ttu-id="18df2-163">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="18df2-163">Step 1.</span></span> <span data-ttu-id="18df2-164">Vytvořte soubor s názvem PrivateConfig.json s hello obsah, který je popsáno ve scénáři 1.</span><span class="sxs-lookup"><span data-stu-id="18df2-164">Create a file named PrivateConfig.json with hello content that was described in Scenario 1.</span></span> <span data-ttu-id="18df2-165">Vytvořte také soubor s názvem PublicConfig.json.</span><span class="sxs-lookup"><span data-stu-id="18df2-165">Also create a file named PublicConfig.json.</span></span> <span data-ttu-id="18df2-166">Zadejte hello konkrétní data, která chcete toocollect.</span><span class="sxs-lookup"><span data-stu-id="18df2-166">Specify hello particular data you want toocollect.</span></span>

<span data-ttu-id="18df2-167">Pro všechny podporované zprostředkovatele a proměnné odkazují hello [řešení System Center křížové platformy lokality](https://scx.codeplex.com/wikipage?title=xplatproviders).</span><span class="sxs-lookup"><span data-stu-id="18df2-167">For all supported providers and variables, reference hello [System Center Cross Platform Solutions site](https://scx.codeplex.com/wikipage?title=xplatproviders).</span></span> <span data-ttu-id="18df2-168">Můžete mít více dotazů a uložit je do více tabulek přidáním další dotazy toohello skriptu.</span><span class="sxs-lookup"><span data-stu-id="18df2-168">You can have multiple queries and store them in multiple tables by appending more queries toohello script.</span></span>

<span data-ttu-id="18df2-169">Ve výchozím nastavení je vždy shromažďují hello Rsyslog data.</span><span class="sxs-lookup"><span data-stu-id="18df2-169">By default, hello Rsyslog data is always collected.</span></span>

    {
          "perfCfg":
          [
              {
                  "query" : "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
                  "table" : "LinuxMemory"
              }
          ]
    }


<span data-ttu-id="18df2-170">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="18df2-170">Step 2.</span></span> <span data-ttu-id="18df2-171">Spustit  **rozšíření virtuálního počítače azure nastavit vm_name LinuxDiagnostic Microsoft.OSTCExtensions: 2.*' – privátní config-path PrivateConfig.json – veřejné config-path PublicConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="18df2-171">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json**.</span></span>

### <a name="scenario-3-upload-your-own-log-files"></a><span data-ttu-id="18df2-172">Scénář 3.</span><span class="sxs-lookup"><span data-stu-id="18df2-172">Scenario 3.</span></span> <span data-ttu-id="18df2-173">Odeslání souborů protokolu</span><span class="sxs-lookup"><span data-stu-id="18df2-173">Upload your own log files</span></span>

<span data-ttu-id="18df2-174">Tato část popisuje, jak toocollect a nahrání konkrétní protokolové soubory tooyour účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="18df2-174">This section describes how toocollect and upload specific log files tooyour storage account.</span></span> <span data-ttu-id="18df2-175">Je nutné toospecify obou hello tooyour protokolu souborů a hello název cesty hello tabulky, kde se má toostore protokolu.</span><span class="sxs-lookup"><span data-stu-id="18df2-175">You need toospecify both hello path tooyour log file and hello name of hello table where you want toostore your log.</span></span> <span data-ttu-id="18df2-176">Přidáním více souborů nebo tabulky položky toohello skriptu můžete vytvořit více souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="18df2-176">You can create multiple log files by adding multiple file/table entries toohello script.</span></span>

<span data-ttu-id="18df2-177">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="18df2-177">Step 1.</span></span> <span data-ttu-id="18df2-178">Vytvořte soubor s názvem PrivateConfig.json s hello obsah, který je popsáno ve scénáři 1.</span><span class="sxs-lookup"><span data-stu-id="18df2-178">Create a file named PrivateConfig.json with hello content that was described in Scenario 1.</span></span> <span data-ttu-id="18df2-179">Pak vytvořte jiný soubor s názvem PublicConfig.json s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="18df2-179">Then create another file named PublicConfig.json with hello following content:</span></span>

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

<span data-ttu-id="18df2-180">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="18df2-180">Step 2.</span></span> <span data-ttu-id="18df2-181">Spusťte `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json`.</span><span class="sxs-lookup"><span data-stu-id="18df2-181">Run `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json`.</span></span>

<span data-ttu-id="18df2-182">Všimněte si, že s tímto nastavením na hello rozšíření verze předchozí too2.3, všechny protokoly zapisují příliš`/var/log/mysql.err` může být příliš duplikován`/var/log/syslog` (nebo `/var/log/messages` v závislosti na hello Linux distro) také.</span><span class="sxs-lookup"><span data-stu-id="18df2-182">Note that with this setting on hello extension versions prior too2.3, all logs written too`/var/log/mysql.err` might be duplicated too`/var/log/syslog` (or `/var/log/messages` depending on hello Linux distro) as well.</span></span> <span data-ttu-id="18df2-183">Pokud chcete tooavoid tento duplicitní protokolování, můžete je vyloučit protokolování `local6` protokolů zařízení ve vaší konfiguraci rsyslog.</span><span class="sxs-lookup"><span data-stu-id="18df2-183">If you'd like tooavoid this duplicate logging, you can exclude logging of `local6` facility logs in your rsyslog configuration.</span></span> <span data-ttu-id="18df2-184">Závisí na hello Linux distro, ale u systému Ubuntu 14.04, je soubor toomodify hello `/etc/rsyslog.d/50-default.conf` a můžete nahradit hello řádku `*.*;auth,authpriv.none -/var/log/syslog` příliš`*.*;auth,authpriv,local6.none -/var/log/syslog`.</span><span class="sxs-lookup"><span data-stu-id="18df2-184">It depends on hello Linux distro, but on an Ubuntu 14.04 system, hello file toomodify is `/etc/rsyslog.d/50-default.conf` and you can replace hello line `*.*;auth,authpriv.none -/var/log/syslog` too`*.*;auth,authpriv,local6.none -/var/log/syslog`.</span></span> <span data-ttu-id="18df2-185">Tento problém vyřešen v hello nejnovější opravy hotfix verze 2.3 (2.3.9007), takže pokud máte rozšíření hello verze 2.3, tento problém došlo k neočekávané chybě.</span><span class="sxs-lookup"><span data-stu-id="18df2-185">This issue is fixed in hello latest hotfix release of 2.3 (2.3.9007), so if you have hello extension version 2.3, this issue should not happen.</span></span> <span data-ttu-id="18df2-186">Pokud k tomu ještě i po restartování virtuálního počítače, kontaktujte nás a Pomozte nám Poradce při potížích se hello nejnovější opravy hotfix není automaticky instalován.</span><span class="sxs-lookup"><span data-stu-id="18df2-186">If it still does even after restarting your VM, please contact us and help us troubleshoot why hello latest hotfix version is not installed automatically.</span></span>

### <a name="scenario-4-stop-hello-extension-from-collecting-any-logs"></a><span data-ttu-id="18df2-187">Scénář 4.</span><span class="sxs-lookup"><span data-stu-id="18df2-187">Scenario 4.</span></span> <span data-ttu-id="18df2-188">Zastavit shromažďování žádné protokoly rozšíření hello</span><span class="sxs-lookup"><span data-stu-id="18df2-188">Stop hello extension from collecting any logs</span></span>

<span data-ttu-id="18df2-189">Tato část popisuje, jak rozšíření hello toostop z shromažďování protokolů.</span><span class="sxs-lookup"><span data-stu-id="18df2-189">This section describes how toostop hello extension from collecting logs.</span></span> <span data-ttu-id="18df2-190">Všimněte si, že hello monitorování procesu agenta bude dál spuštěný a funkční i přes tuto změnu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="18df2-190">Note that hello monitoring agent process will be still up and running even with this reconfiguration.</span></span> <span data-ttu-id="18df2-191">Pokud chcete monitorování procesu agenta zcela hello toostop, můžete tak učinit zakázáním rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="18df2-191">If you'd like toostop hello monitoring agent process completely, you can do so by disabling hello extension.</span></span> <span data-ttu-id="18df2-192">Hello příkaz toodisable hello rozšíření je `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.</span><span class="sxs-lookup"><span data-stu-id="18df2-192">hello command toodisable hello extension is `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.</span></span>

<span data-ttu-id="18df2-193">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="18df2-193">Step 1.</span></span> <span data-ttu-id="18df2-194">Vytvořte soubor s názvem PrivateConfig.json s hello obsah, který je popsáno ve scénáři 1.</span><span class="sxs-lookup"><span data-stu-id="18df2-194">Create a file named PrivateConfig.json with hello content that was described in Scenario 1.</span></span> <span data-ttu-id="18df2-195">Vytvořte jiný soubor s názvem PublicConfig.json s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="18df2-195">Create another file named PublicConfig.json with hello following content:</span></span>

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


<span data-ttu-id="18df2-196">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="18df2-196">Step 2.</span></span> <span data-ttu-id="18df2-197">Spustit  **rozšíření virtuálního počítače azure nastavit vm_name LinuxDiagnostic Microsoft.OSTCExtensions: 2.*' – privátní config-path PrivateConfig.json – veřejné config-path PublicConfig.json**.</span><span class="sxs-lookup"><span data-stu-id="18df2-197">Run **azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json**.</span></span>

## <a name="review-your-data"></a><span data-ttu-id="18df2-198">Zkontrolujte vaše data</span><span class="sxs-lookup"><span data-stu-id="18df2-198">Review your data</span></span>

<span data-ttu-id="18df2-199">Hello výkonu a diagnostických dat jsou uložené v tabulce Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="18df2-199">hello performance and diagnostic data are stored in an Azure Storage table.</span></span> <span data-ttu-id="18df2-200">Zkontrolujte [jak toouse Azure Table Storage z Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) toolearn jak tooaccess hello dat v úložišti hello tabulky pomocí rozhraní příkazového řádku Azure skriptů.</span><span class="sxs-lookup"><span data-stu-id="18df2-200">Review [How toouse Azure Table Storage from Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) toolearn how tooaccess hello data in hello storage table by using Azure CLI scripts.</span></span>

<span data-ttu-id="18df2-201">Kromě toho můžete použít následující uživatelské rozhraní nástroje tooaccess hello dat:</span><span class="sxs-lookup"><span data-stu-id="18df2-201">In addition, you can use following UI tools tooaccess hello data:</span></span>

1. <span data-ttu-id="18df2-202">Průzkumníka serveru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="18df2-202">Visual Studio Server Explorer.</span></span> <span data-ttu-id="18df2-203">Přejděte tooyour účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="18df2-203">Go tooyour storage account.</span></span> <span data-ttu-id="18df2-204">Po dobu asi 5 minut spuštění hello virtuálních počítačů, uvidíte hello čtyři výchozí tabulky: "LinuxCpu", "LinuxDisk", "LinuxMemory" a "Linuxsyslog".</span><span class="sxs-lookup"><span data-stu-id="18df2-204">After hello VM runs for about five minutes, you'll see hello four default tables: “LinuxCpu”, ”LinuxDisk”, ”LinuxMemory”, and ”Linuxsyslog”.</span></span> <span data-ttu-id="18df2-205">Dvakrát klikněte na hello tabulky názvy tooview hello data.</span><span class="sxs-lookup"><span data-stu-id="18df2-205">Double-click hello table names tooview hello data.</span></span>
1. <span data-ttu-id="18df2-206">[Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").</span><span class="sxs-lookup"><span data-stu-id="18df2-206">[Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").</span></span>

![Bitové kopie](./media/diagnostic-extension/no1.png)

<span data-ttu-id="18df2-208">Pokud jste povolili fileCfg nebo perfCfg (jak je popsáno v scénáře 2 a 3), můžete použít Průzkumníka serveru Visual Studia a Azure Storage Explorer tooview jiné než výchozí data.</span><span class="sxs-lookup"><span data-stu-id="18df2-208">If you've enabled fileCfg or perfCfg (as described in Scenarios 2 and 3), you can use Visual Studio Server Explorer and Azure Storage Explorer tooview non-default data.</span></span>

## <a name="known-issues"></a><span data-ttu-id="18df2-209">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="18df2-209">Known issues</span></span>

* <span data-ttu-id="18df2-210">Hello Rsyslog informace a zákazník zadaný soubor protokolu lze přistupovat pouze pomocí skriptů.</span><span class="sxs-lookup"><span data-stu-id="18df2-210">hello Rsyslog information and customer-specified log file can only be accessed via scripting.</span></span>
