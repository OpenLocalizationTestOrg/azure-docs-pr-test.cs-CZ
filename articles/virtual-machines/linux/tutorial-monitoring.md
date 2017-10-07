---
title: "virtuální počítače s Linuxem aaaMonitor v Azure | Microsoft Docs"
description: "Zjistěte, jak toomonitor spouštění diagnostiky a metriky výkonu na virtuální počítač s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 282da0f03ab0bf37bd44750c22813ca8d1c89560
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomonitor-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="dcef8-103">Jak toomonitor virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="dcef8-103">How toomonitor a Linux virtual machine in Azure</span></span>

<span data-ttu-id="dcef8-104">tooensure, virtuální počítače (VM) v Azure běží správně, můžete zkontrolovat Diagnostika spouštění a metriky výkonu.</span><span class="sxs-lookup"><span data-stu-id="dcef8-104">tooensure your virtual machines (VMs) in Azure are running correctly, you can review boot diagnostics and performance metrics.</span></span> <span data-ttu-id="dcef8-105">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="dcef8-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dcef8-106">Povolit Diagnostika spouštění na hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="dcef8-106">Enable boot diagnostics on hello VM</span></span>
> * <span data-ttu-id="dcef8-107">Zobrazení diagnostiky spouštění</span><span class="sxs-lookup"><span data-stu-id="dcef8-107">View boot diagnostics</span></span>
> * <span data-ttu-id="dcef8-108">Zobrazit hostitele metriky</span><span class="sxs-lookup"><span data-stu-id="dcef8-108">View host metrics</span></span>
> * <span data-ttu-id="dcef8-109">Povolit rozšíření diagnostiky na hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="dcef8-109">Enable diagnostics extension on hello VM</span></span>
> * <span data-ttu-id="dcef8-110">Zobrazit metriky virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="dcef8-110">View VM metrics</span></span>
> * <span data-ttu-id="dcef8-111">Vytvářet výstrahy na základě diagnostiky metriky</span><span class="sxs-lookup"><span data-stu-id="dcef8-111">Create alerts based on diagnostic metrics</span></span>
> * <span data-ttu-id="dcef8-112">Nastavit pokročilé monitorování</span><span class="sxs-lookup"><span data-stu-id="dcef8-112">Set up advanced monitoring</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="dcef8-113">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="dcef8-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="dcef8-114">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="dcef8-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="dcef8-115">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="dcef8-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-vm"></a><span data-ttu-id="dcef8-116">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="dcef8-116">Create VM</span></span>

<span data-ttu-id="dcef8-117">toosee diagnostiky a metriky v akci, je nutné virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="dcef8-117">toosee diagnostics and metrics in action, you need a VM.</span></span> <span data-ttu-id="dcef8-118">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="dcef8-118">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="dcef8-119">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupMonitor* v hello *eastus* umístění.</span><span class="sxs-lookup"><span data-stu-id="dcef8-119">hello following example creates a resource group named *myResourceGroupMonitor* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

<span data-ttu-id="dcef8-120">Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](https://docs.microsoft.com/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="dcef8-120">Now create a VM with [az vm create](https://docs.microsoft.com/cli/azure/vm#create).</span></span> <span data-ttu-id="dcef8-121">Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="dcef8-121">hello following example creates a VM named *myVM*:</span></span>

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a><span data-ttu-id="dcef8-122">Povolit spuštění diagnostiky</span><span class="sxs-lookup"><span data-stu-id="dcef8-122">Enable boot diagnostics</span></span>

<span data-ttu-id="dcef8-123">Jak spustit virtuální počítače s Linuxem, rozšíření diagnostiky spouštěcí hello zaznamená spouštěcí výstup a uloží je v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="dcef8-123">As Linux VMs boot, hello boot diagnostic extension captures boot output and stores it in Azure storage.</span></span> <span data-ttu-id="dcef8-124">Tato data mohou být problémy spouštěcí použité tootroubleshoot virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="dcef8-124">This data can be used tootroubleshoot VM boot issues.</span></span> <span data-ttu-id="dcef8-125">Diagnostika spouštění nepovolí automaticky při vytváření virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure hello.</span><span class="sxs-lookup"><span data-stu-id="dcef8-125">Boot diagnostics are not automatically enabled when you create a Linux VM using hello Azure CLI.</span></span>

<span data-ttu-id="dcef8-126">Před povolením Diagnostika spouštění, musí účet úložiště toobe vytvořené pro ukládání spouštěcí protokoly.</span><span class="sxs-lookup"><span data-stu-id="dcef8-126">Before enabling boot diagnostics, a storage account needs toobe created for storing boot logs.</span></span> <span data-ttu-id="dcef8-127">Účty úložiště musí mít globálně jedinečného názvu, být v rozmezí 3 až 24 znaků a musí obsahovat pouze čísla a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="dcef8-127">Storage accounts must have a globally unique name, be between 3 and 24 characters, and must contain only numbers and lowercase letters.</span></span> <span data-ttu-id="dcef8-128">Vytvořit účet úložiště s hello [vytvořit účet úložiště az](/cli/azure/storage/account#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="dcef8-128">Create a storage account with hello [az storage account create](/cli/azure/storage/account#create) command.</span></span> <span data-ttu-id="dcef8-129">V tomto příkladu je náhodný řetězec použité toocreate název účtu úložiště jedinečný.</span><span class="sxs-lookup"><span data-stu-id="dcef8-129">In this example, a random string is used toocreate a unique storage account name.</span></span> 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

<span data-ttu-id="dcef8-130">Při povolování Diagnostika spouštění, je potřeba kontejner úložiště objektů blob toohello URI hello.</span><span class="sxs-lookup"><span data-stu-id="dcef8-130">When enabling boot diagnostics, hello URI toohello blob storage container is needed.</span></span> <span data-ttu-id="dcef8-131">Hello následující dotazy příkaz hello tooreturn účet úložiště tento identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="dcef8-131">hello following command queries hello storage account tooreturn this URI.</span></span> <span data-ttu-id="dcef8-132">Hello hodnota identifikátoru URI je uložený v názvech proměnných *bloburi*, která je použita v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="dcef8-132">hello URI value is stored in a variable names *bloburi*, which is used in hello next step.</span></span>

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

<span data-ttu-id="dcef8-133">Teď povolit Diagnostika spouštění s [povolit az virtuálního počítače – Diagnostika spouštění](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#enable).</span><span class="sxs-lookup"><span data-stu-id="dcef8-133">Now enable boot diagnostics with [az vm boot-diagnostics enable](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#enable).</span></span> <span data-ttu-id="dcef8-134">Hello `--storage` hodnota je hello blob URI shromážděných v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="dcef8-134">hello `--storage` value is hello blob URI collected in hello previous step.</span></span>

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a><span data-ttu-id="dcef8-135">Zobrazení diagnostiky spouštění</span><span class="sxs-lookup"><span data-stu-id="dcef8-135">View boot diagnostics</span></span>

<span data-ttu-id="dcef8-136">Pokud jsou povolené Diagnostika spouštění, pokaždé, když zastavit a spustit hello virtuálních počítačů, informace o procesu spouštění hello se zapíše tooa souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="dcef8-136">When boot diagnostics are enabled, each time you stop and start hello VM, information about hello boot process is written tooa log file.</span></span> <span data-ttu-id="dcef8-137">V tomto příkladu nejprve zrušit přidělení hello virtuálních počítačů s hello [az OM deallocate](/cli/azure/vm#deallocate) příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="dcef8-137">For this example, first deallocate hello VM with hello [az vm deallocate](/cli/azure/vm#deallocate) command as follows:</span></span>

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

<span data-ttu-id="dcef8-138">Nyní spustit hello virtuálních počítačů s hello [spuštění virtuálního počítače az]( /cli/azure/vm#stop) příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="dcef8-138">Now start hello VM with hello [az vm start]( /cli/azure/vm#stop) command as follows:</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

<span data-ttu-id="dcef8-139">Můžete získat hello spouštěcí diagnostických dat pro *Můjvp* s hello [Diagnostika spouštění virtuálních počítačů az – get spouštěcí protokolu](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#get-boot-log) příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="dcef8-139">You can get hello boot diagnostic data for *myVM* with hello [az vm boot-diagnostics get-boot-log](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#get-boot-log) command as follows:</span></span>

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a><span data-ttu-id="dcef8-140">Zobrazit hostitele metriky</span><span class="sxs-lookup"><span data-stu-id="dcef8-140">View host metrics</span></span>

<span data-ttu-id="dcef8-141">Virtuální počítač s Linuxem má vyhrazený hostitel v Azure, který komunikuje se službou.</span><span class="sxs-lookup"><span data-stu-id="dcef8-141">A Linux VM has a dedicated host in Azure that it interacts with.</span></span> <span data-ttu-id="dcef8-142">Metriky se pro hostitele hello automaticky shromažďovat a lze zobrazit v portálu Azure hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dcef8-142">Metrics are automatically collected for hello host and can be viewed in hello Azure portal as follows:</span></span>

1. <span data-ttu-id="dcef8-143">V hello portálu Azure, klikněte na **skupiny prostředků**, vyberte **myResourceGroupMonitor**a potom vyberte **Můjvp** v seznamu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="dcef8-143">In hello Azure portal, click **Resource Groups**, select **myResourceGroupMonitor**, and then select **myVM** in hello resource list.</span></span>
1. <span data-ttu-id="dcef8-144">toosee jak provádí hello hostitele virtuálních počítačů, klikněte na tlačítko **metriky** v okně hello virtuální počítač, pak vyberte některé z hello *[hostitel]* metriky v části **dostupné metriky**.</span><span class="sxs-lookup"><span data-stu-id="dcef8-144">toosee how hello host VM is performing, click **Metrics** on hello VM blade, then select any of hello *[Host]* metrics under **Available metrics**.</span></span>

    ![Zobrazit hostitele metriky](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a><span data-ttu-id="dcef8-146">Instalace rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="dcef8-146">Install diagnostics extension</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dcef8-147">Tento dokument popisuje 2.3 verzi hello rozšíření diagnostiky Linux, která se už nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="dcef8-147">This document describes version 2.3 of hello Linux Diagnostic Extension, which has been deprecated.</span></span> <span data-ttu-id="dcef8-148">Verze 2.3 bude až do 30. června 2018 podporována.</span><span class="sxs-lookup"><span data-stu-id="dcef8-148">Version 2.3 will be supported until June 30, 2018.</span></span>
>
> <span data-ttu-id="dcef8-149">Místo toho lze povolit verze 3.0 hello rozšíření diagnostiky Linux.</span><span class="sxs-lookup"><span data-stu-id="dcef8-149">Version 3.0 of hello Linux Diagnostic Extension can be enabled instead.</span></span> <span data-ttu-id="dcef8-150">Další informace najdete v tématu [hello dokumentaci](./diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="dcef8-150">For more information, see [hello documentation](./diagnostic-extension.md).</span></span>

<span data-ttu-id="dcef8-151">metriky základní hostitele Hello jsou k dispozici, ale toosee podrobnější a metriky specifické pro virtuální počítač, můžete tooneed tooinstall hello Azure rozšíření diagnostiky na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="dcef8-151">hello basic host metrics are available, but toosee more granular and VM-specific metrics, you tooneed tooinstall hello Azure diagnostics extension on hello VM.</span></span> <span data-ttu-id="dcef8-152">Hello rozšíření diagnostiky Azure umožňuje další funkce monitorování a Diagnostika toobe data načíst z hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="dcef8-152">hello Azure diagnostics extension allows additional monitoring and diagnostics data toobe retrieved from hello VM.</span></span> <span data-ttu-id="dcef8-153">Můžete zobrazit tyto metriky výkonu a vytvářet výstrahy založené na tom, jak se provádí hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="dcef8-153">You can view these performance metrics and create alerts based on how hello VM performs.</span></span> <span data-ttu-id="dcef8-154">rozšíření diagnostiky Hello se instaluje prostřednictvím portálu Azure hello takto:</span><span class="sxs-lookup"><span data-stu-id="dcef8-154">hello diagnostic extension is installed through hello Azure portal as follows:</span></span>

1. <span data-ttu-id="dcef8-155">V hello portálu Azure, klikněte na **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="dcef8-155">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
1. <span data-ttu-id="dcef8-156">Klikněte na tlačítko **diagnostiku nastavení**.</span><span class="sxs-lookup"><span data-stu-id="dcef8-156">Click **Diagnosis settings**.</span></span> <span data-ttu-id="dcef8-157">seznam Hello ukazuje, že *spouštění diagnostiky* jsou již povolené z předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="dcef8-157">hello list shows that *Boot diagnostics* are already enabled from hello previous section.</span></span> <span data-ttu-id="dcef8-158">Klikněte na zaškrtávací pole hello *základní metriky*.</span><span class="sxs-lookup"><span data-stu-id="dcef8-158">Click hello check box for *Basic metrics*.</span></span>
1. <span data-ttu-id="dcef8-159">V hello *účet úložiště* vyhledejte vyberte hello tooand *mydiagdata [1234]* účet vytvořený v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="dcef8-159">In hello *Storage account* section, browse tooand select hello *mydiagdata[1234]* account created in hello previous section.</span></span>
1. <span data-ttu-id="dcef8-160">Klikněte na tlačítko hello **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dcef8-160">Click hello **Save** button.</span></span>

    ![Zobrazit diagnostické metriky](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a><span data-ttu-id="dcef8-162">Zobrazit metriky virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="dcef8-162">View VM metrics</span></span>

<span data-ttu-id="dcef8-163">Hello virtuálních počítačů metriky lze zobrazit v hello stejným způsobem, že jste si zobrazili hello hostitele virtuálních počítačů metriky:</span><span class="sxs-lookup"><span data-stu-id="dcef8-163">You can view hello VM metrics in hello same way that you viewed hello host VM metrics:</span></span>

1. <span data-ttu-id="dcef8-164">V hello portálu Azure, klikněte na **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="dcef8-164">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
1. <span data-ttu-id="dcef8-165">toosee jak provádí hello virtuálních počítačů, klikněte na tlačítko **metriky** na hello okno virtuálních počítačů a potom vyberte libovolné hello diagnostiky metrik pod **dostupné metriky**.</span><span class="sxs-lookup"><span data-stu-id="dcef8-165">toosee how hello VM is performing, click **Metrics** on hello VM blade, and then select any of hello diagnostics metrics under **Available metrics**.</span></span>

    ![Zobrazit metriky virtuálního počítače](./media/tutorial-monitoring/monitor-vm-metrics.png)


## <a name="create-alerts"></a><span data-ttu-id="dcef8-167">Vytváření upozornění</span><span class="sxs-lookup"><span data-stu-id="dcef8-167">Create alerts</span></span>

<span data-ttu-id="dcef8-168">Můžete vytvořit na základě metriky výkonu specifických výstrah.</span><span class="sxs-lookup"><span data-stu-id="dcef8-168">You can create alerts based on specific performance metrics.</span></span> <span data-ttu-id="dcef8-169">Výstrahy můžou být použité toonotify, které jste při průměrné využití procesoru překračuje prahovou hodnotu nebo volné místo na disku klesne pod určitou částku, například.</span><span class="sxs-lookup"><span data-stu-id="dcef8-169">Alerts can be used toonotify you when average CPU usage exceeds a certain threshold or available free disk space drops below a certain amount, for example.</span></span> <span data-ttu-id="dcef8-170">Výstrahy se zobrazují v hello portál Azure nebo lze odeslat e-mailem.</span><span class="sxs-lookup"><span data-stu-id="dcef8-170">Alerts are displayed in hello Azure portal or can be sent via email.</span></span> <span data-ttu-id="dcef8-171">V odpovědi tooalerts generován můžete spustit taky runbooků služeb automatizace Azure nebo Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="dcef8-171">You can also trigger Azure Automation runbooks or Azure Logic Apps in response tooalerts being generated.</span></span>

<span data-ttu-id="dcef8-172">Hello následující příklad vytvoří výstrahu při průměrné využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="dcef8-172">hello following example creates an alert for average CPU usage.</span></span>

1. <span data-ttu-id="dcef8-173">V hello portálu Azure, klikněte na **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="dcef8-173">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
2. <span data-ttu-id="dcef8-174">Klikněte na tlačítko **výstrah pravidla** v okně hello virtuálních počítačů, klikněte **přidat metriky upozornění** napříč hello horní části okna hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="dcef8-174">Click **Alert rules** on hello VM blade, then click **Add metric alert** across hello top of hello alerts blade.</span></span>
4. <span data-ttu-id="dcef8-175">Zadejte **název** výstrahy, jako například *myAlertRule*</span><span class="sxs-lookup"><span data-stu-id="dcef8-175">Provide a **Name** for your alert, such as *myAlertRule*</span></span>
5. <span data-ttu-id="dcef8-176">tootrigger výstrahu, pokud procento využití procesoru překračuje 1.0 pro pět minut, ponechte hello všechny ostatní výchozí nastavení vybrané.</span><span class="sxs-lookup"><span data-stu-id="dcef8-176">tootrigger an alert when CPU percentage exceeds 1.0 for five minutes, leave all hello other defaults selected.</span></span>
6. <span data-ttu-id="dcef8-177">V případě potřeby zaškrtněte políčko hello pro *e-mailu vlastníci, přispěvatelé a čtenáři* toosend e-mailové oznámení.</span><span class="sxs-lookup"><span data-stu-id="dcef8-177">Optionally, check hello box for *Email owners, contributors, and readers* toosend email notification.</span></span> <span data-ttu-id="dcef8-178">výchozí akce Hello je toopresent oznámení portálu hello.</span><span class="sxs-lookup"><span data-stu-id="dcef8-178">hello default action is toopresent a notification in hello portal.</span></span>
7. <span data-ttu-id="dcef8-179">Klikněte na tlačítko hello **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dcef8-179">Click hello **OK** button.</span></span>


## <a name="advanced-monitoring"></a><span data-ttu-id="dcef8-180">Pokročilé sledování</span><span class="sxs-lookup"><span data-stu-id="dcef8-180">Advanced monitoring</span></span> 

<span data-ttu-id="dcef8-181">Můžete provést rozšířené monitorování vašeho virtuálního počítače pomocí [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span><span class="sxs-lookup"><span data-stu-id="dcef8-181">You can do more advanced monitoring of your VM by using [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span></span> <span data-ttu-id="dcef8-182">Pokud jste tak již neučinili, můžete si zaregistrovat [bezplatnou zkušební verzi](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) služby Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="dcef8-182">If you haven't already done so, you can sign up for a [free trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) of Operations Management Suite.</span></span>

<span data-ttu-id="dcef8-183">Až budete mít přístup k portálu OMS toohello, najdete v okně Nastavení hello hello identifikátor klíče a pracovního prostoru pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="dcef8-183">When you have access toohello OMS portal, you can find hello workspace key and workspace identifier on hello Settings blade.</span></span> <span data-ttu-id="dcef8-184">Nahraďte < klíč pracovního prostoru > a < id pracovního prostoru > hello hodnotami pro z vaší OMS prostoru a pak můžete použít **nastavení rozšíření virtuálního az** tooadd hello OMS rozšíření toohello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="dcef8-184">Replace <workspace-key> and <workspace-id> with hello values for from your OMS workspace and then you can use **az vm extension set** tooadd hello OMS extension toohello VM:</span></span>

```azurecli-interactive 
az vm extension set \
  --resource-group myResourceGroupMonitor \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.3 \
  --protected-settings '{"workspaceKey": "<workspace-key>"}' \
  --settings '{"workspaceId": "<workspace-id>"}'
```

<span data-ttu-id="dcef8-185">V okně hledání protokolů hello portálu OMS hello, měli byste vidět *Můjvp* například informace zobrazené v hello následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="dcef8-185">On hello Log Search blade of hello OMS portal, you should see *myVM* such as what is shown in hello following picture:</span></span>

![Okno OMS](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a><span data-ttu-id="dcef8-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dcef8-187">Next steps</span></span>

<span data-ttu-id="dcef8-188">V tomto kurzu jste nakonfigurovali a zkontrolovat virtuálních počítačů pomocí Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="dcef8-188">In this tutorial, you configured and reviewed VMs with Azure Security Center.</span></span> <span data-ttu-id="dcef8-189">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="dcef8-189">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dcef8-190">Povolit Diagnostika spouštění na hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="dcef8-190">Enable boot diagnostics on hello VM</span></span>
> * <span data-ttu-id="dcef8-191">Zobrazení diagnostiky spouštění</span><span class="sxs-lookup"><span data-stu-id="dcef8-191">View boot diagnostics</span></span>
> * <span data-ttu-id="dcef8-192">Zobrazit hostitele metriky</span><span class="sxs-lookup"><span data-stu-id="dcef8-192">View host metrics</span></span>
> * <span data-ttu-id="dcef8-193">Povolit rozšíření diagnostiky na hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="dcef8-193">Enable diagnostics extension on hello VM</span></span>
> * <span data-ttu-id="dcef8-194">Zobrazit metriky virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="dcef8-194">View VM metrics</span></span>
> * <span data-ttu-id="dcef8-195">Vytvářet výstrahy na základě diagnostiky metriky</span><span class="sxs-lookup"><span data-stu-id="dcef8-195">Create alerts based on diagnostic metrics</span></span>
> * <span data-ttu-id="dcef8-196">Nastavit pokročilé monitorování</span><span class="sxs-lookup"><span data-stu-id="dcef8-196">Set up advanced monitoring</span></span>

<span data-ttu-id="dcef8-197">Posunutí další kurz toolearn toohello o službě Azure security center.</span><span class="sxs-lookup"><span data-stu-id="dcef8-197">Advance toohello next tutorial toolearn about Azure security center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="dcef8-198">Správa zabezpečení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="dcef8-198">Manage VM security</span></span>](./tutorial-azure-security.md)