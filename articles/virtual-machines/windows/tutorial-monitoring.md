---
title: "aaaAzure monitorování a virtuální počítače s Windows | Microsoft Docs"
description: "Kurz – monitorování virtuálního počítače s Windows pomocí Azure Powershellu"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/04/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 191dc5a30d41c25a9e38f8ec2a32efdc05e03015
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-windows-virtual-machine-with-azure-powershell"></a><span data-ttu-id="5b4fb-103">Monitorování virtuálního počítače s Windows pomocí Azure Powershellu</span><span class="sxs-lookup"><span data-stu-id="5b4fb-103">Monitor a Windows Virtual Machine with Azure PowerShell</span></span>

<span data-ttu-id="5b4fb-104">Azure monitorování používá spouštěcí toocollect agentů a údaje o výkonu z virtuálních počítačích Azure, uložení dat této v úložišti Azure a zpřístupněte ho prostřednictvím portálu, modul Azure PowerShell hello a hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-104">Azure monitoring uses agents toocollect boot and performance data from Azure VMs, store this data in Azure storage, and make it accessible through portal, hello Azure PowerShell module, and hello Azure CLI.</span></span> <span data-ttu-id="5b4fb-105">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="5b4fb-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5b4fb-106">Povolit Diagnostika spouštění na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="5b4fb-106">Enable boot diagnostics on a VM</span></span>
> * <span data-ttu-id="5b4fb-107">Zobrazení diagnostiky spouštění</span><span class="sxs-lookup"><span data-stu-id="5b4fb-107">View boot diagnostics</span></span>
> * <span data-ttu-id="5b4fb-108">Zobrazit metriky hostitele virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5b4fb-108">View VM host metrics</span></span>
> * <span data-ttu-id="5b4fb-109">Instalace rozšíření diagnostiky hello</span><span class="sxs-lookup"><span data-stu-id="5b4fb-109">Install hello diagnostics extension</span></span>
> * <span data-ttu-id="5b4fb-110">Zobrazit metriky virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5b4fb-110">View VM metrics</span></span>
> * <span data-ttu-id="5b4fb-111">Vytvořit výstrahu</span><span class="sxs-lookup"><span data-stu-id="5b4fb-111">Create an alert</span></span>
> * <span data-ttu-id="5b4fb-112">Nastavit pokročilé monitorování</span><span class="sxs-lookup"><span data-stu-id="5b4fb-112">Set up advanced monitoring</span></span>

<span data-ttu-id="5b4fb-113">Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-113">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="5b4fb-114">Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-114">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="5b4fb-115">Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="5b4fb-115">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

<span data-ttu-id="5b4fb-116">Příklad hello toocomplete v tomto kurzu, musí mít existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-116">toocomplete hello example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="5b4fb-117">V případě potřeby to [ukázka skriptu](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) můžete vytvořit za vás.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-117">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="5b4fb-118">Při práci prostřednictvím hello kurzu, nahraďte hello skupinu prostředků, název virtuálního počítače a umístění tam, kde je potřeba.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-118">When working through hello tutorial, replace hello resource group, VM name, and location where needed.</span></span>

## <a name="view-boot-diagnostics"></a><span data-ttu-id="5b4fb-119">Zobrazení diagnostiky spouštění</span><span class="sxs-lookup"><span data-stu-id="5b4fb-119">View boot diagnostics</span></span>

<span data-ttu-id="5b4fb-120">Jak spustit virtuální počítače s Windows, zaznamená agenta pro diagnostiku hello spouštěcí obrazovky výstupu, který lze použít pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-120">As Windows virtual machines boot up, hello boot diagnostic agent captures screen output that can be used for troubleshooting purpose.</span></span> <span data-ttu-id="5b4fb-121">Tato funkce je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-121">This capability is enabled by default.</span></span> <span data-ttu-id="5b4fb-122">Hello zachycení obrazovky, které snímky jsou uložené v účtu úložiště Azure, který se také vytvoří ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-122">hello captured screen shots are stored in an Azure storage account, which is also created by default.</span></span> 

<span data-ttu-id="5b4fb-123">Můžete získat hello spouštěcí diagnostických dat s hello [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) příkaz.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-123">You can get hello boot diagnostic data with hello [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) command.</span></span> <span data-ttu-id="5b4fb-124">V následujícím příkladu hello, Diagnostika spouštění jsou stažené toohello kořenovém hello * c:\* jednotky.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-124">In hello following example, boot diagnostics are downloaded toohello root of hello *c:\* drive.</span></span> 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a><span data-ttu-id="5b4fb-125">Zobrazit hostitele metriky</span><span class="sxs-lookup"><span data-stu-id="5b4fb-125">View host metrics</span></span>

<span data-ttu-id="5b4fb-126">Virtuální počítač s Windows má vyhrazený virtuální počítač hostitele v Azure, který komunikuje se službou.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-126">A Windows VM has a dedicated Host VM in Azure that it interacts with.</span></span> <span data-ttu-id="5b4fb-127">Metriky jsou automaticky shromažďovat pro hello hostitele a lze zobrazit v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-127">Metrics are automatically collected for hello Host and can be viewed in hello Azure portal.</span></span>

1. <span data-ttu-id="5b4fb-128">V hello portálu Azure, klikněte na **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-128">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
2. <span data-ttu-id="5b4fb-129">Klikněte na tlačítko **metriky** na hello okno virtuálních počítačů a potom vyberte libovolné hello metrik hostitele pod **dostupné metriky** toosee jak provádí hello hostitele virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-129">Click **Metrics** on hello VM blade, and then select any of hello Host metrics under **Available metrics** toosee how hello Host VM is performing.</span></span>

    ![Zobrazit hostitele metriky](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a><span data-ttu-id="5b4fb-131">Instalace rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="5b4fb-131">Install diagnostics extension</span></span>

<span data-ttu-id="5b4fb-132">metriky základní hostitele Hello jsou k dispozici, ale toosee podrobnější a metriky specifické pro virtuální počítač, můžete tooneed tooinstall hello Azure rozšíření diagnostiky na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-132">hello basic host metrics are available, but toosee more granular and VM-specific metrics, you tooneed tooinstall hello Azure diagnostics extension on hello VM.</span></span> <span data-ttu-id="5b4fb-133">Hello rozšíření diagnostiky Azure umožňuje další funkce monitorování a Diagnostika toobe data načíst z hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-133">hello Azure diagnostics extension allows additional monitoring and diagnostics data toobe retrieved from hello VM.</span></span> <span data-ttu-id="5b4fb-134">Můžete zobrazit tyto metriky výkonu a vytvářet výstrahy založené na tom, jak se provádí hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-134">You can view these performance metrics and create alerts based on how hello VM performs.</span></span> <span data-ttu-id="5b4fb-135">rozšíření diagnostiky Hello se instaluje prostřednictvím portálu Azure hello takto:</span><span class="sxs-lookup"><span data-stu-id="5b4fb-135">hello diagnostic extension is installed through hello Azure portal as follows:</span></span>

1. <span data-ttu-id="5b4fb-136">V hello portálu Azure, klikněte na **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-136">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
2. <span data-ttu-id="5b4fb-137">Klikněte na tlačítko **diagnostiku nastavení**.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-137">Click **Diagnosis settings**.</span></span> <span data-ttu-id="5b4fb-138">seznam Hello ukazuje, že *spouštění diagnostiky* jsou již povolené z předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-138">hello list shows that *Boot diagnostics* are already enabled from hello previous section.</span></span> <span data-ttu-id="5b4fb-139">Klikněte na zaškrtávací pole hello *základní metriky*.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-139">Click hello check box for *Basic metrics*.</span></span>
3. <span data-ttu-id="5b4fb-140">Klikněte na tlačítko hello **povolit sledování na úrovni hosta** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-140">Click hello **Enable guest-level monitoring** button.</span></span>

    ![Zobrazit diagnostické metriky](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a><span data-ttu-id="5b4fb-142">Zobrazit metriky virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5b4fb-142">View VM metrics</span></span>

<span data-ttu-id="5b4fb-143">Hello virtuálních počítačů metriky lze zobrazit v hello stejným způsobem, že jste si zobrazili hello hostitele virtuálních počítačů metriky:</span><span class="sxs-lookup"><span data-stu-id="5b4fb-143">You can view hello VM metrics in hello same way that you viewed hello host VM metrics:</span></span>

1. <span data-ttu-id="5b4fb-144">V hello portálu Azure, klikněte na **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-144">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
2. <span data-ttu-id="5b4fb-145">toosee jak provádí hello virtuálních počítačů, klikněte na tlačítko **metriky** na hello okno virtuálních počítačů a potom vyberte libovolné hello diagnostiky metrik pod **dostupné metriky**.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-145">toosee how hello VM is performing, click **Metrics** on hello VM blade, and then select any of hello diagnostics metrics under **Available metrics**.</span></span>

    ![Zobrazit metriky virtuálního počítače](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a><span data-ttu-id="5b4fb-147">Vytváření upozornění</span><span class="sxs-lookup"><span data-stu-id="5b4fb-147">Create alerts</span></span>

<span data-ttu-id="5b4fb-148">Můžete vytvořit na základě metriky výkonu specifických výstrah.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-148">You can create alerts based on specific performance metrics.</span></span> <span data-ttu-id="5b4fb-149">Výstrahy můžou být použité toonotify, které jste při průměrné využití procesoru překračuje prahovou hodnotu nebo volné místo na disku klesne pod určitou částku, například.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-149">Alerts can be used toonotify you when average CPU usage exceeds a certain threshold or available free disk space drops below a certain amount, for example.</span></span> <span data-ttu-id="5b4fb-150">Výstrahy se zobrazují v hello portál Azure nebo lze odeslat e-mailem.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-150">Alerts are displayed in hello Azure portal or can be sent via email.</span></span> <span data-ttu-id="5b4fb-151">V odpovědi tooalerts generován můžete spustit taky runbooků služeb automatizace Azure nebo Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-151">You can also trigger Azure Automation runbooks or Azure Logic Apps in response tooalerts being generated.</span></span>

<span data-ttu-id="5b4fb-152">Hello následující příklad vytvoří výstrahu při průměrné využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-152">hello following example creates an alert for average CPU usage.</span></span>

1. <span data-ttu-id="5b4fb-153">V hello portálu Azure, klikněte na **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-153">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
2. <span data-ttu-id="5b4fb-154">Klikněte na tlačítko **výstrah pravidla** v okně hello virtuálních počítačů, klikněte **přidat metriky upozornění** napříč hello horní části okna hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-154">Click **Alert rules** on hello VM blade, then click **Add metric alert** across hello top of hello alerts blade.</span></span>
4. <span data-ttu-id="5b4fb-155">Zadejte **název** výstrahy, jako například *myAlertRule*</span><span class="sxs-lookup"><span data-stu-id="5b4fb-155">Provide a **Name** for your alert, such as *myAlertRule*</span></span>
5. <span data-ttu-id="5b4fb-156">tootrigger výstrahu, pokud procento využití procesoru překračuje 1.0 pro pět minut, ponechte hello všechny ostatní výchozí nastavení vybrané.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-156">tootrigger an alert when CPU percentage exceeds 1.0 for five minutes, leave all hello other defaults selected.</span></span>
6. <span data-ttu-id="5b4fb-157">V případě potřeby zaškrtněte políčko hello pro *e-mailu vlastníci, přispěvatelé a čtenáři* toosend e-mailové oznámení.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-157">Optionally, check hello box for *Email owners, contributors, and readers* toosend email notification.</span></span> <span data-ttu-id="5b4fb-158">výchozí akce Hello je toopresent oznámení portálu hello.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-158">hello default action is toopresent a notification in hello portal.</span></span>
7. <span data-ttu-id="5b4fb-159">Klikněte na tlačítko hello **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-159">Click hello **OK** button.</span></span>

## <a name="advanced-monitoring"></a><span data-ttu-id="5b4fb-160">Pokročilé sledování</span><span class="sxs-lookup"><span data-stu-id="5b4fb-160">Advanced monitoring</span></span> 

<span data-ttu-id="5b4fb-161">Můžete provést rozšířené monitorování vašeho virtuálního počítače pomocí [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span><span class="sxs-lookup"><span data-stu-id="5b4fb-161">You can do more advanced monitoring of your VM by using [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span></span> <span data-ttu-id="5b4fb-162">Pokud jste tak již neučinili, můžete si zaregistrovat [bezplatnou zkušební verzi](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) služby Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-162">If you haven't already done so, you can sign up for a [free trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) of Operations Management Suite.</span></span>

<span data-ttu-id="5b4fb-163">Až budete mít přístup k portálu OMS toohello, najdete v okně Nastavení hello hello identifikátor klíče a pracovního prostoru pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-163">When you have access toohello OMS portal, you can find hello workspace key and workspace identifier on hello Settings blade.</span></span> <span data-ttu-id="5b4fb-164">Použití hello [Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) zadanými tootooadd hello OMS rozšíření toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-164">Use hello [Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) cmmand tootooadd hello OMS extension toohello VM.</span></span> <span data-ttu-id="5b4fb-165">Proměnná hello aktualizace hodnoty ve hello níže ukázka tooreflect můžete klíč pracovního prostoru OMS a prostoru ID.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-165">Update hello variable values in hello below sample tooreflect you OMS workspace key and workspace Id.</span></span>  

```powershell
$omsId = "<Replace with your OMS Id>"
$omsKey = "<Replace with your OMS key>"

Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName myVM `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $omsId} `
  -ProtectedSettings @{"workspaceKey" = $omsKey} `
  -Location eastus
```

<span data-ttu-id="5b4fb-166">Po několika minutách, měli byste vidět hello nový virtuální počítač v pracovní prostor OMS hello.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-166">After a few minutes, you should see hello new VM in hello OMS workspace.</span></span> 

![Okno OMS](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a><span data-ttu-id="5b4fb-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5b4fb-168">Next steps</span></span>
<span data-ttu-id="5b4fb-169">V tomto kurzu jste nakonfigurovali a zkontrolovat virtuálních počítačů pomocí Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-169">In this tutorial, you configured and reviewed VMs with Azure Security Center.</span></span> <span data-ttu-id="5b4fb-170">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="5b4fb-170">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5b4fb-171">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="5b4fb-171">Create a virtual network</span></span>
> * <span data-ttu-id="5b4fb-172">Vytvoření skupiny prostředků a virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5b4fb-172">Create a resource group and VM</span></span> 
> * <span data-ttu-id="5b4fb-173">Povolit Diagnostika spouštění na hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5b4fb-173">Enable boot diagnostics on hello VM</span></span>
> * <span data-ttu-id="5b4fb-174">Zobrazení diagnostiky spouštění</span><span class="sxs-lookup"><span data-stu-id="5b4fb-174">View boot diagnostics</span></span>
> * <span data-ttu-id="5b4fb-175">Zobrazit hostitele metriky</span><span class="sxs-lookup"><span data-stu-id="5b4fb-175">View host metrics</span></span>
> * <span data-ttu-id="5b4fb-176">Instalace rozšíření diagnostiky hello</span><span class="sxs-lookup"><span data-stu-id="5b4fb-176">Install hello diagnostics extension</span></span>
> * <span data-ttu-id="5b4fb-177">Zobrazit metriky virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5b4fb-177">View VM metrics</span></span>
> * <span data-ttu-id="5b4fb-178">Vytvořit výstrahu</span><span class="sxs-lookup"><span data-stu-id="5b4fb-178">Create an alert</span></span>
> * <span data-ttu-id="5b4fb-179">Nastavit pokročilé monitorování</span><span class="sxs-lookup"><span data-stu-id="5b4fb-179">Set up advanced monitoring</span></span>

<span data-ttu-id="5b4fb-180">Posunutí další kurz toolearn toohello o službě Azure security center.</span><span class="sxs-lookup"><span data-stu-id="5b4fb-180">Advance toohello next tutorial toolearn about Azure security center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5b4fb-181">Správa zabezpečení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5b4fb-181">Manage VM security</span></span>](./tutorial-azure-security.md)