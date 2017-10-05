---
title: "Monitorovat virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Naučte se monitorovat Diagnostika spouštění a metriky výkonu pro virtuální počítač s Linuxem v Azure"
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
ms.openlocfilehash: 3fe8390e88e609b57a462e066f972346f8e8730e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-monitor-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="17421-103">Postup sledování virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="17421-103">How to monitor a Linux virtual machine in Azure</span></span>

<span data-ttu-id="17421-104">K zajištění, že virtuální počítače (VM) v Azure běží správně, můžete zkontrolovat Diagnostika spouštění a metriky výkonu.</span><span class="sxs-lookup"><span data-stu-id="17421-104">To ensure your virtual machines (VMs) in Azure are running correctly, you can review boot diagnostics and performance metrics.</span></span> <span data-ttu-id="17421-105">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="17421-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="17421-106">Povolit Diagnostika spouštění ve virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="17421-106">Enable boot diagnostics on the VM</span></span>
> * <span data-ttu-id="17421-107">Zobrazení diagnostiky spouštění</span><span class="sxs-lookup"><span data-stu-id="17421-107">View boot diagnostics</span></span>
> * <span data-ttu-id="17421-108">Zobrazit hostitele metriky</span><span class="sxs-lookup"><span data-stu-id="17421-108">View host metrics</span></span>
> * <span data-ttu-id="17421-109">Povolit rozšíření diagnostiky na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="17421-109">Enable diagnostics extension on the VM</span></span>
> * <span data-ttu-id="17421-110">Zobrazit metriky virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="17421-110">View VM metrics</span></span>
> * <span data-ttu-id="17421-111">Vytvářet výstrahy na základě diagnostiky metriky</span><span class="sxs-lookup"><span data-stu-id="17421-111">Create alerts based on diagnostic metrics</span></span>
> * <span data-ttu-id="17421-112">Nastavit pokročilé monitorování</span><span class="sxs-lookup"><span data-stu-id="17421-112">Set up advanced monitoring</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="17421-113">Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="17421-113">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="17421-114">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="17421-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="17421-115">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="17421-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-vm"></a><span data-ttu-id="17421-116">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="17421-116">Create VM</span></span>

<span data-ttu-id="17421-117">Pokud chcete zobrazit diagnostiku a metriky v akci, je nutné virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="17421-117">To see diagnostics and metrics in action, you need a VM.</span></span> <span data-ttu-id="17421-118">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="17421-118">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="17421-119">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupMonitor* v *eastus* umístění.</span><span class="sxs-lookup"><span data-stu-id="17421-119">The following example creates a resource group named *myResourceGroupMonitor* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

<span data-ttu-id="17421-120">Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](https://docs.microsoft.com/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="17421-120">Now create a VM with [az vm create](https://docs.microsoft.com/cli/azure/vm#create).</span></span> <span data-ttu-id="17421-121">Následující příklad vytvoří virtuální počítač s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="17421-121">The following example creates a VM named *myVM*:</span></span>

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a><span data-ttu-id="17421-122">Povolit spuštění diagnostiky</span><span class="sxs-lookup"><span data-stu-id="17421-122">Enable boot diagnostics</span></span>

<span data-ttu-id="17421-123">Jak spustit virtuální počítače s Linuxem, rozšíření diagnostiky spouštěcí zaznamená spouštěcí výstup a uloží je v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="17421-123">As Linux VMs boot, the boot diagnostic extension captures boot output and stores it in Azure storage.</span></span> <span data-ttu-id="17421-124">Tato data můžete použít k odstraňování problémů spouštění virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="17421-124">This data can be used to troubleshoot VM boot issues.</span></span> <span data-ttu-id="17421-125">Diagnostika spouštění nepovolí automaticky při vytváření virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="17421-125">Boot diagnostics are not automatically enabled when you create a Linux VM using the Azure CLI.</span></span>

<span data-ttu-id="17421-126">Před povolením Diagnostika spouštění, musí být vytvořen pro ukládání protokolů spouštěcí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="17421-126">Before enabling boot diagnostics, a storage account needs to be created for storing boot logs.</span></span> <span data-ttu-id="17421-127">Účty úložiště musí mít globálně jedinečného názvu, být v rozmezí 3 až 24 znaků a musí obsahovat pouze čísla a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="17421-127">Storage accounts must have a globally unique name, be between 3 and 24 characters, and must contain only numbers and lowercase letters.</span></span> <span data-ttu-id="17421-128">Vytvořit účet úložiště s [vytvořit účet úložiště az](/cli/azure/storage/account#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="17421-128">Create a storage account with the [az storage account create](/cli/azure/storage/account#create) command.</span></span> <span data-ttu-id="17421-129">V tomto příkladu je náhodný řetězec použít k vytvoření název účtu úložiště jedinečný.</span><span class="sxs-lookup"><span data-stu-id="17421-129">In this example, a random string is used to create a unique storage account name.</span></span> 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

<span data-ttu-id="17421-130">Při povolování Diagnostika spouštění, je potřeba identifikátor URI pro kontejner úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="17421-130">When enabling boot diagnostics, the URI to the blob storage container is needed.</span></span> <span data-ttu-id="17421-131">Následující příkaz dotazuje účet úložiště, který chcete vrátit tento identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="17421-131">The following command queries the storage account to return this URI.</span></span> <span data-ttu-id="17421-132">Hodnota identifikátoru URI je uložen v názvech proměnných *bloburi*, která je použita v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="17421-132">The URI value is stored in a variable names *bloburi*, which is used in the next step.</span></span>

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

<span data-ttu-id="17421-133">Teď povolit Diagnostika spouštění s [povolit az virtuálního počítače – Diagnostika spouštění](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#enable).</span><span class="sxs-lookup"><span data-stu-id="17421-133">Now enable boot diagnostics with [az vm boot-diagnostics enable](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#enable).</span></span> <span data-ttu-id="17421-134">`--storage` Hodnota je objekt blob URI shromážděných v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="17421-134">The `--storage` value is the blob URI collected in the previous step.</span></span>

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a><span data-ttu-id="17421-135">Zobrazení diagnostiky spouštění</span><span class="sxs-lookup"><span data-stu-id="17421-135">View boot diagnostics</span></span>

<span data-ttu-id="17421-136">Pokud jsou povolené Diagnostika spouštění, pokaždé, když zastavení a spuštění virtuálního počítače, informace o procesu spouštění je zapsán do souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="17421-136">When boot diagnostics are enabled, each time you stop and start the VM, information about the boot process is written to a log file.</span></span> <span data-ttu-id="17421-137">V tomto příkladu nejprve zrušit přidělení virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate) příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="17421-137">For this example, first deallocate the VM with the [az vm deallocate](/cli/azure/vm#deallocate) command as follows:</span></span>

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

<span data-ttu-id="17421-138">Nyní spusťte virtuální počítač s [spuštění virtuálního počítače az]( /cli/azure/vm#stop) příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="17421-138">Now start the VM with the [az vm start]( /cli/azure/vm#stop) command as follows:</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

<span data-ttu-id="17421-139">Můžete získat data diagnostiky spouštění pro *Můjvp* s [Diagnostika spouštění virtuálních počítačů az – get spouštěcí protokolu](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#get-boot-log) příkaz takto:</span><span class="sxs-lookup"><span data-stu-id="17421-139">You can get the boot diagnostic data for *myVM* with the [az vm boot-diagnostics get-boot-log](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#get-boot-log) command as follows:</span></span>

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a><span data-ttu-id="17421-140">Zobrazit hostitele metriky</span><span class="sxs-lookup"><span data-stu-id="17421-140">View host metrics</span></span>

<span data-ttu-id="17421-141">Virtuální počítač s Linuxem má vyhrazený hostitel v Azure, který komunikuje se službou.</span><span class="sxs-lookup"><span data-stu-id="17421-141">A Linux VM has a dedicated host in Azure that it interacts with.</span></span> <span data-ttu-id="17421-142">Metriky jsou automaticky shromažďovat pro hostitele a lze je zobrazit na portálu Azure následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="17421-142">Metrics are automatically collected for the host and can be viewed in the Azure portal as follows:</span></span>

1. <span data-ttu-id="17421-143">Na portálu Azure klikněte na tlačítko **skupiny prostředků**, vyberte **myResourceGroupMonitor**a potom vyberte **Můjvp** v seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="17421-143">In the Azure portal, click **Resource Groups**, select **myResourceGroupMonitor**, and then select **myVM** in the resource list.</span></span>
1. <span data-ttu-id="17421-144">Chcete-li zjistit, jaký je výkon hostitelů virtuálních počítačů, klikněte na tlačítko **metriky** v okně virtuálního počítače, pak vyberte některé z *[hostitel]* metriky v části **dostupné metriky**.</span><span class="sxs-lookup"><span data-stu-id="17421-144">To see how the host VM is performing, click **Metrics** on the VM blade, then select any of the *[Host]* metrics under **Available metrics**.</span></span>

    ![Zobrazit hostitele metriky](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a><span data-ttu-id="17421-146">Instalace rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="17421-146">Install diagnostics extension</span></span>

> [!IMPORTANT]
> <span data-ttu-id="17421-147">Tento dokument popisuje 2.3 verzi rozšíření diagnostiky Linux, která se už nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="17421-147">This document describes version 2.3 of the Linux Diagnostic Extension, which has been deprecated.</span></span> <span data-ttu-id="17421-148">Verze 2.3 bude až do 30. června 2018 podporována.</span><span class="sxs-lookup"><span data-stu-id="17421-148">Version 2.3 will be supported until June 30, 2018.</span></span>
>
> <span data-ttu-id="17421-149">Místo toho lze povolit rozšíření diagnostiky Linux verze 3.0.</span><span class="sxs-lookup"><span data-stu-id="17421-149">Version 3.0 of the Linux Diagnostic Extension can be enabled instead.</span></span> <span data-ttu-id="17421-150">Další informace najdete v tématu [dokumentaci](./diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="17421-150">For more information, see [the documentation](./diagnostic-extension.md).</span></span>

<span data-ttu-id="17421-151">Metriky základní hostitele jsou dostupné, ale podrobnější a metriky specifické pro virtuální počítač, budete muset nainstalovat rozšíření diagnostiky Azure ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="17421-151">The basic host metrics are available, but to see more granular and VM-specific metrics, you to need to install the Azure diagnostics extension on the VM.</span></span> <span data-ttu-id="17421-152">Rozšíření diagnostiky Azure umožňuje další funkce monitorování a diagnostická data mají být načteny z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="17421-152">The Azure diagnostics extension allows additional monitoring and diagnostics data to be retrieved from the VM.</span></span> <span data-ttu-id="17421-153">Můžete zobrazit tyto metriky výkonu a vytvářet výstrahy založené na tom, jak se provádí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="17421-153">You can view these performance metrics and create alerts based on how the VM performs.</span></span> <span data-ttu-id="17421-154">Diagnostické rozšíření nainstalovaný prostřednictvím portálu Azure následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="17421-154">The diagnostic extension is installed through the Azure portal as follows:</span></span>

1. <span data-ttu-id="17421-155">Na portálu Azure klikněte na tlačítko **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="17421-155">In the Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in the resource list.</span></span>
1. <span data-ttu-id="17421-156">Klikněte na tlačítko **diagnostiku nastavení**.</span><span class="sxs-lookup"><span data-stu-id="17421-156">Click **Diagnosis settings**.</span></span> <span data-ttu-id="17421-157">V seznamu uvedena, který *spouštění diagnostiky* jsou již povolené z předchozí části.</span><span class="sxs-lookup"><span data-stu-id="17421-157">The list shows that *Boot diagnostics* are already enabled from the previous section.</span></span> <span data-ttu-id="17421-158">Klikněte na zaškrtávací políčko pro *základní metriky*.</span><span class="sxs-lookup"><span data-stu-id="17421-158">Click the check box for *Basic metrics*.</span></span>
1. <span data-ttu-id="17421-159">V *účet úložiště* , vyhledejte a vyberte *mydiagdata [1234]* účet vytvořený v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="17421-159">In the *Storage account* section, browse to and select the *mydiagdata[1234]* account created in the previous section.</span></span>
1. <span data-ttu-id="17421-160">Klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="17421-160">Click the **Save** button.</span></span>

    ![Zobrazit diagnostické metriky](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a><span data-ttu-id="17421-162">Zobrazit metriky virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="17421-162">View VM metrics</span></span>

<span data-ttu-id="17421-163">Metriky virtuálního počítače lze zobrazit stejným způsobem, že jste si zobrazili hostitele metriky virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="17421-163">You can view the VM metrics in the same way that you viewed the host VM metrics:</span></span>

1. <span data-ttu-id="17421-164">Na portálu Azure klikněte na tlačítko **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="17421-164">In the Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in the resource list.</span></span>
1. <span data-ttu-id="17421-165">Chcete-li zjistit, jaký je výkon virtuálního počítače, klikněte na tlačítko **metriky** v okně virtuálního počítače a potom vyberte některé z metriky diagnostiky v části **dostupné metriky**.</span><span class="sxs-lookup"><span data-stu-id="17421-165">To see how the VM is performing, click **Metrics** on the VM blade, and then select any of the diagnostics metrics under **Available metrics**.</span></span>

    ![Zobrazit metriky virtuálního počítače](./media/tutorial-monitoring/monitor-vm-metrics.png)


## <a name="create-alerts"></a><span data-ttu-id="17421-167">Vytváření upozornění</span><span class="sxs-lookup"><span data-stu-id="17421-167">Create alerts</span></span>

<span data-ttu-id="17421-168">Můžete vytvořit na základě metriky výkonu specifických výstrah.</span><span class="sxs-lookup"><span data-stu-id="17421-168">You can create alerts based on specific performance metrics.</span></span> <span data-ttu-id="17421-169">Výstrahy lze oznámí, že jste při průměrné využití procesoru překračuje prahovou hodnotu nebo volné místo na disku klesne pod určitou částku, například.</span><span class="sxs-lookup"><span data-stu-id="17421-169">Alerts can be used to notify you when average CPU usage exceeds a certain threshold or available free disk space drops below a certain amount, for example.</span></span> <span data-ttu-id="17421-170">Výstrahy jsou zobrazeny v portálu Azure nebo lze odeslat e-mailem.</span><span class="sxs-lookup"><span data-stu-id="17421-170">Alerts are displayed in the Azure portal or can be sent via email.</span></span> <span data-ttu-id="17421-171">Runbooky služby automatizace Azure nebo Azure Logic Apps můžete také aktivovat v reakci na výstrahy generován.</span><span class="sxs-lookup"><span data-stu-id="17421-171">You can also trigger Azure Automation runbooks or Azure Logic Apps in response to alerts being generated.</span></span>

<span data-ttu-id="17421-172">Následující příklad vytvoří výstrahu při průměrné využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="17421-172">The following example creates an alert for average CPU usage.</span></span>

1. <span data-ttu-id="17421-173">Na portálu Azure klikněte na tlačítko **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="17421-173">In the Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in the resource list.</span></span>
2. <span data-ttu-id="17421-174">Klikněte na tlačítko **výstrah pravidla** v okně virtuálního počítače klikněte **přidat metriky upozornění** v horní části okna výstrahy.</span><span class="sxs-lookup"><span data-stu-id="17421-174">Click **Alert rules** on the VM blade, then click **Add metric alert** across the top of the alerts blade.</span></span>
4. <span data-ttu-id="17421-175">Zadejte **název** výstrahy, jako například *myAlertRule*</span><span class="sxs-lookup"><span data-stu-id="17421-175">Provide a **Name** for your alert, such as *myAlertRule*</span></span>
5. <span data-ttu-id="17421-176">Spustí výstrahu, pokud procento využití procesoru překročí 1.0 pět minut, ponechte všechny ostatní výchozí nastavení vybrané.</span><span class="sxs-lookup"><span data-stu-id="17421-176">To trigger an alert when CPU percentage exceeds 1.0 for five minutes, leave all the other defaults selected.</span></span>
6. <span data-ttu-id="17421-177">Volitelně můžete zaškrtnout políčko pro *e-mailu vlastníci, přispěvatelé a čtenáři* k odesílání e-mailové oznámení.</span><span class="sxs-lookup"><span data-stu-id="17421-177">Optionally, check the box for *Email owners, contributors, and readers* to send email notification.</span></span> <span data-ttu-id="17421-178">Výchozí akce je k dispozici oznámení na portálu.</span><span class="sxs-lookup"><span data-stu-id="17421-178">The default action is to present a notification in the portal.</span></span>
7. <span data-ttu-id="17421-179">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="17421-179">Click the **OK** button.</span></span>


## <a name="advanced-monitoring"></a><span data-ttu-id="17421-180">Pokročilé sledování</span><span class="sxs-lookup"><span data-stu-id="17421-180">Advanced monitoring</span></span> 

<span data-ttu-id="17421-181">Můžete provést rozšířené monitorování vašeho virtuálního počítače pomocí [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span><span class="sxs-lookup"><span data-stu-id="17421-181">You can do more advanced monitoring of your VM by using [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span></span> <span data-ttu-id="17421-182">Pokud jste tak již neučinili, můžete si zaregistrovat [bezplatnou zkušební verzi](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) služby Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="17421-182">If you haven't already done so, you can sign up for a [free trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) of Operations Management Suite.</span></span>

<span data-ttu-id="17421-183">Až budete mít přístup k portálu OMS, můžete najít klíč pracovního prostoru a identifikátor prostoru v okně nastavení.</span><span class="sxs-lookup"><span data-stu-id="17421-183">When you have access to the OMS portal, you can find the workspace key and workspace identifier on the Settings blade.</span></span> <span data-ttu-id="17421-184">Nahrazení < klíč pracovního prostoru > a < id pracovního prostoru > hodnotami pro z vaší OMS prostoru a pak můžete použít **nastavení rozšíření virtuálního az** přidat příponu OMS na virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="17421-184">Replace <workspace-key> and <workspace-id> with the values for from your OMS workspace and then you can use **az vm extension set** to add the OMS extension to the VM:</span></span>

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

<span data-ttu-id="17421-185">V okně hledání protokolů na portálu OMS byste měli vidět *Můjvp* například informace zobrazené na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="17421-185">On the Log Search blade of the OMS portal, you should see *myVM* such as what is shown in the following picture:</span></span>

![Okno OMS](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a><span data-ttu-id="17421-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="17421-187">Next steps</span></span>

<span data-ttu-id="17421-188">V tomto kurzu jste nakonfigurovali a zkontrolovat virtuálních počítačů pomocí Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="17421-188">In this tutorial, you configured and reviewed VMs with Azure Security Center.</span></span> <span data-ttu-id="17421-189">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="17421-189">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="17421-190">Povolit Diagnostika spouštění ve virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="17421-190">Enable boot diagnostics on the VM</span></span>
> * <span data-ttu-id="17421-191">Zobrazení diagnostiky spouštění</span><span class="sxs-lookup"><span data-stu-id="17421-191">View boot diagnostics</span></span>
> * <span data-ttu-id="17421-192">Zobrazit hostitele metriky</span><span class="sxs-lookup"><span data-stu-id="17421-192">View host metrics</span></span>
> * <span data-ttu-id="17421-193">Povolit rozšíření diagnostiky na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="17421-193">Enable diagnostics extension on the VM</span></span>
> * <span data-ttu-id="17421-194">Zobrazit metriky virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="17421-194">View VM metrics</span></span>
> * <span data-ttu-id="17421-195">Vytvářet výstrahy na základě diagnostiky metriky</span><span class="sxs-lookup"><span data-stu-id="17421-195">Create alerts based on diagnostic metrics</span></span>
> * <span data-ttu-id="17421-196">Nastavit pokročilé monitorování</span><span class="sxs-lookup"><span data-stu-id="17421-196">Set up advanced monitoring</span></span>

<span data-ttu-id="17421-197">Přechodu na v dalším kurzu se dozvíte o službě Azure security center.</span><span class="sxs-lookup"><span data-stu-id="17421-197">Advance to the next tutorial to learn about Azure security center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="17421-198">Správa zabezpečení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="17421-198">Manage VM security</span></span>](./tutorial-azure-security.md)