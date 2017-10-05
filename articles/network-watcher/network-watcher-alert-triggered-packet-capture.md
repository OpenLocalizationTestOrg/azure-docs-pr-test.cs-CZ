---
title: "Pomocí zachytáváním paketů provádět monitorování proaktivní sítě pomocí výstrah a Azure Functions | Microsoft Docs"
description: "Tento článek popisuje, jak vytvořit zaznamenání výstrahy spouštěná paketů s sledovací proces sítě Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 75e6e7c4-b3ba-4173-8815-b00d7d824e11
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b813172fc1fc1cc683f463f05370c95bfec10f8d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-packet-capture-for-proactive-network-monitoring-with-alerts-and-azure-functions"></a><span data-ttu-id="6eb2f-103">Použít zachytáváním paketů pro monitorování proaktivní sítě pomocí výstrah a Azure Functions</span><span class="sxs-lookup"><span data-stu-id="6eb2f-103">Use packet capture for proactive network monitoring with alerts and Azure Functions</span></span>

<span data-ttu-id="6eb2f-104">Zachycení dat ze sítě sledovacích procesů paketu vytvoří zaznamenání relace ke sledování provozu do a z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-104">Network Watcher packet capture creates capture sessions to track traffic in and out of virtual machines.</span></span> <span data-ttu-id="6eb2f-105">Zachycení soubor může mít filtr, který je definován sledovat pouze provoz, který chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-105">The capture file can have a filter that is defined to track only the traffic that you want to monitor.</span></span> <span data-ttu-id="6eb2f-106">Tato data jsou pak uloženy v storage blob nebo místně na počítači hosta.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-106">This data is then stored in a storage blob or locally on the guest machine.</span></span>

<span data-ttu-id="6eb2f-107">Tato funkce můžete spustit vzdáleně z jiných automatizace scénáře, jako je Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-107">This capability can be started remotely from other automation scenarios such as Azure Functions.</span></span> <span data-ttu-id="6eb2f-108">Zachytáváním paketů vám dává možnost spustit proaktivní zachycení založené na definované sítě anomálií.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-108">Packet capture gives you the capability to run proactive captures based on defined network anomalies.</span></span> <span data-ttu-id="6eb2f-109">Mezi další použití patří shromažďování statistiku sítě, získání informací o síti vniknutí, ladění komunikaci klienta se serverem a další.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-109">Other uses include gathering network statistics, getting information about network intrusions, debugging client-server communications, and more.</span></span>

<span data-ttu-id="6eb2f-110">Prostředky, které jsou nasazené v Azure spustit 24 hodin denně 7.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-110">Resources that are deployed in Azure run 24/7.</span></span> <span data-ttu-id="6eb2f-111">Vás a vašich zaměstnanců nelze monitorovat aktivně stavu všech prostředků, 24 nebo 7.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-111">You and your staff cannot actively monitor the status of all resources 24/7.</span></span> <span data-ttu-id="6eb2f-112">Co se například stane, pokud se vyskytne problém na 2: 00?</span><span class="sxs-lookup"><span data-stu-id="6eb2f-112">For example, what happens if an issue occurs at 2 AM?</span></span>

<span data-ttu-id="6eb2f-113">Pomocí sledovací proces sítě, výstrahy a funkce z v ekosystému Azure můžete proaktivně reagovat s daty a nástroje k řešení problémů ve vaší síti.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-113">By using Network Watcher, alerting, and functions from within the Azure ecosystem, you can proactively respond with the data and tools to solve problems in your network.</span></span>

![Scénář][scenario]

## <a name="prerequisites"></a><span data-ttu-id="6eb2f-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6eb2f-115">Prerequisites</span></span>

* <span data-ttu-id="6eb2f-116">Nejnovější verzi [prostředí Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="6eb2f-116">The latest version of [Azure PowerShell](/powershell/azure/install-azurerm-ps).</span></span>
* <span data-ttu-id="6eb2f-117">Existující instanci sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-117">An existing instance of Network Watcher.</span></span> <span data-ttu-id="6eb2f-118">Pokud jste ještě nemáte, [vytvořit instanci sledovací proces sítě](network-watcher-create.md).</span><span class="sxs-lookup"><span data-stu-id="6eb2f-118">If you don't already have one, [create an instance of Network Watcher](network-watcher-create.md).</span></span>
* <span data-ttu-id="6eb2f-119">Existující virtuální počítač ve stejné oblasti jako sledovací proces sítě s [rozšíření Windows](../virtual-machines/windows/extensions-nwa.md) nebo [rozšíření virtuálního počítače Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="6eb2f-119">An existing virtual machine in the same region as Network Watcher with the [Windows extension](../virtual-machines/windows/extensions-nwa.md) or [Linux virtual machine extension](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="6eb2f-120">Scénář</span><span class="sxs-lookup"><span data-stu-id="6eb2f-120">Scenario</span></span>

<span data-ttu-id="6eb2f-121">V tomto příkladu váš virtuální počítač odesílá víc segmentů TCP než obvykle a chcete být upozorněni.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-121">In this example, your VM is sending more TCP segments than usual, and you want to be alerted.</span></span> <span data-ttu-id="6eb2f-122">Segmentů TCP slouží jako příklad zde, ale žádné výstrahy podmínku můžete použít.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-122">TCP segments are used as an example here, but you can use any alert condition.</span></span>

<span data-ttu-id="6eb2f-123">Když se zobrazí výstraha, budete chtít přijímat data na úrovni paketů pochopit, proč má vyšší komunikace.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-123">When you are alerted, you want to receive packet-level data to understand why communication has increased.</span></span> <span data-ttu-id="6eb2f-124">Potom můžete provést kroky se vraťte do regulární komunikace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-124">Then you can take steps to return the virtual machine to regular communication.</span></span>

<span data-ttu-id="6eb2f-125">Tento scénář předpokládá, že máte existující instanci sledovací proces sítě a skupinu prostředků s platným virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-125">This scenario assumes that you have an existing instance of Network Watcher and a resource group with a valid virtual machine.</span></span>

<span data-ttu-id="6eb2f-126">V následujícím seznamu je přehled pracovního postupu, který probíhá:</span><span class="sxs-lookup"><span data-stu-id="6eb2f-126">The following list is an overview of the workflow that takes place:</span></span>

1. <span data-ttu-id="6eb2f-127">Výstrahy na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-127">An alert is triggered on your VM.</span></span>
1. <span data-ttu-id="6eb2f-128">Výstraha volání funkce Azure prostřednictvím webhook, jehož.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-128">The alert calls your Azure function via a webhook.</span></span>
1. <span data-ttu-id="6eb2f-129">Funkce Azure zpracovává výstrahy a spustí relaci zachytávání paketů sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-129">Your Azure function processes the alert and starts a Network Watcher packet capture session.</span></span>
1. <span data-ttu-id="6eb2f-130">Zachytávání paketů běží ve virtuálním počítači a shromažďuje provoz.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-130">The packet capture runs on the VM and collects traffic.</span></span>
1. <span data-ttu-id="6eb2f-131">Odeslání souboru zachytávání paketů do účtu úložiště ke kontrole a diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-131">The packet capture file is uploaded to a storage account for review and diagnosis.</span></span>

<span data-ttu-id="6eb2f-132">K automatizaci tohoto procesu, můžeme vytvořit a připojit výstrahu na našem virtuálních počítačů k aktivaci dojde-li k incidentu.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-132">To automate this process, we create and connect an alert on our VM to trigger when the incident occurs.</span></span> <span data-ttu-id="6eb2f-133">Také vytvoříme funkce k volání do sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-133">We also create a function to call into Network Watcher.</span></span>

<span data-ttu-id="6eb2f-134">Tento scénář provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="6eb2f-134">This scenario does the following:</span></span>

* <span data-ttu-id="6eb2f-135">Vytvoří Azure funkci, která se spouští zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-135">Creates an Azure function that starts a packet capture.</span></span>
* <span data-ttu-id="6eb2f-136">Vytvoří pravidlo výstrahy na virtuálním počítači a nakonfiguruje pravidlo výstrahy pro volání funkce Azure.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-136">Creates an alert rule on a virtual machine and configures the alert rule to call the Azure function.</span></span>

## <a name="create-an-azure-function"></a><span data-ttu-id="6eb2f-137">Vytvoření Azure funkce</span><span class="sxs-lookup"><span data-stu-id="6eb2f-137">Create an Azure function</span></span>

<span data-ttu-id="6eb2f-138">Prvním krokem je vytvoření funkce Azure ke zpracování upozornění a vytvořit zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-138">The first step is to create an Azure function to process the alert and create a packet capture.</span></span>

1. <span data-ttu-id="6eb2f-139">V [portál Azure](https://portal.azure.com), vyberte **nový** > **výpočetní** > **aplikaci funkce**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-139">In the [Azure portal](https://portal.azure.com), select **New** > **Compute** > **Function App**.</span></span>

    ![Vytvoření aplikace – funkce][1-1]

2. <span data-ttu-id="6eb2f-141">Na **aplikaci funkce** okno, zadejte následující hodnoty a potom vyberte **OK** k vytvoření aplikace:</span><span class="sxs-lookup"><span data-stu-id="6eb2f-141">On the **Function App** blade, enter the following values, and then select **OK** to create the app:</span></span>

    |<span data-ttu-id="6eb2f-142">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-142">**Setting**</span></span> | <span data-ttu-id="6eb2f-143">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-143">**Value**</span></span> | <span data-ttu-id="6eb2f-144">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-144">**Details**</span></span> |
    |---|---|---|
    |<span data-ttu-id="6eb2f-145">**Název aplikace**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-145">**App name**</span></span>|<span data-ttu-id="6eb2f-146">PacketCaptureExample</span><span class="sxs-lookup"><span data-stu-id="6eb2f-146">PacketCaptureExample</span></span>|<span data-ttu-id="6eb2f-147">Název funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-147">The name of the function app.</span></span>|
    |<span data-ttu-id="6eb2f-148">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-148">**Subscription**</span></span>|<span data-ttu-id="6eb2f-149">[Předplatné] Předplatné, pro který chcete vytvořit aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-149">[Your subscription]The subscription for which to create the function app.</span></span>||
    |<span data-ttu-id="6eb2f-150">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-150">**Resource Group**</span></span>|<span data-ttu-id="6eb2f-151">PacketCaptureRG</span><span class="sxs-lookup"><span data-stu-id="6eb2f-151">PacketCaptureRG</span></span>|<span data-ttu-id="6eb2f-152">Skupina prostředků tak, aby obsahovala aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-152">The resource group to contain the function app.</span></span>|
    |<span data-ttu-id="6eb2f-153">**Hostování plán**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-153">**Hosting Plan**</span></span>|<span data-ttu-id="6eb2f-154">Plán spotřeba</span><span class="sxs-lookup"><span data-stu-id="6eb2f-154">Consumption Plan</span></span>| <span data-ttu-id="6eb2f-155">Typ naplánujte vaše aplikace používá funkce.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-155">The type of plan your function app uses.</span></span> <span data-ttu-id="6eb2f-156">Možnosti jsou spotřeby nebo plán služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-156">Options are Consumption or Azure App Service plan.</span></span> |
    |<span data-ttu-id="6eb2f-157">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-157">**Location**</span></span>|<span data-ttu-id="6eb2f-158">Střed USA</span><span class="sxs-lookup"><span data-stu-id="6eb2f-158">Central US</span></span>| <span data-ttu-id="6eb2f-159">Oblast, ve které chcete vytvořit aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-159">The region in which to create the function app.</span></span>|
    |<span data-ttu-id="6eb2f-160">**Účet úložiště**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-160">**Storage Account**</span></span>|<span data-ttu-id="6eb2f-161">{automaticky vygenerovanou}</span><span class="sxs-lookup"><span data-stu-id="6eb2f-161">{autogenerated}</span></span>| <span data-ttu-id="6eb2f-162">Účet úložiště, který potřebuje funkce Azure pro úložiště pro obecné účely.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-162">The storage account that Azure Functions needs for general-purpose storage.</span></span>|

3. <span data-ttu-id="6eb2f-163">Na **PacketCaptureExample funkce aplikace** vyberte **funkce** > **vlastní funkce**  >  **+**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-163">On the **PacketCaptureExample Function Apps** blade, select **Functions** > **Custom function** >**+**.</span></span>

4. <span data-ttu-id="6eb2f-164">Vyberte **HttpTrigger-Powershell**a pak zadejte zbývající informace.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-164">Select **HttpTrigger-Powershell**, and then enter the remaining information.</span></span> <span data-ttu-id="6eb2f-165">Nakonec vytvořit funkce, vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-165">Finally, to create the function, select **Create**.</span></span>

    |<span data-ttu-id="6eb2f-166">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-166">**Setting**</span></span> | <span data-ttu-id="6eb2f-167">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-167">**Value**</span></span> | <span data-ttu-id="6eb2f-168">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-168">**Details**</span></span> |
    |---|---|---|
    |<span data-ttu-id="6eb2f-169">**Scénář**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-169">**Scenario**</span></span>|<span data-ttu-id="6eb2f-170">experimentální</span><span class="sxs-lookup"><span data-stu-id="6eb2f-170">Experimental</span></span>|<span data-ttu-id="6eb2f-171">Typ scénáře</span><span class="sxs-lookup"><span data-stu-id="6eb2f-171">Type of scenario</span></span>|
    |<span data-ttu-id="6eb2f-172">**Pojmenujte svoji funkci**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-172">**Name your function**</span></span>|<span data-ttu-id="6eb2f-173">AlertPacketCapturePowerShell</span><span class="sxs-lookup"><span data-stu-id="6eb2f-173">AlertPacketCapturePowerShell</span></span>|<span data-ttu-id="6eb2f-174">Název funkce</span><span class="sxs-lookup"><span data-stu-id="6eb2f-174">Name of the function</span></span>|
    |<span data-ttu-id="6eb2f-175">**Úroveň oprávnění**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-175">**Authorization level**</span></span>|<span data-ttu-id="6eb2f-176">Funkce</span><span class="sxs-lookup"><span data-stu-id="6eb2f-176">Function</span></span>|<span data-ttu-id="6eb2f-177">Úroveň oprávnění pro funkci</span><span class="sxs-lookup"><span data-stu-id="6eb2f-177">Authorization level for the function</span></span>|

![Příklad funkce][functions1]

> [!NOTE]
> <span data-ttu-id="6eb2f-179">Šablona prostředí PowerShell je experimentální a nemá plnou podporu.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-179">The PowerShell template is experimental and does not have full support.</span></span>

<span data-ttu-id="6eb2f-180">Úpravy jsou požadovány pro tento příklad a jsou vysvětlené v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-180">Customizations are required for this example and are explained in the following steps.</span></span>

### <a name="add-modules"></a><span data-ttu-id="6eb2f-181">Přidání modulů</span><span class="sxs-lookup"><span data-stu-id="6eb2f-181">Add modules</span></span>

<span data-ttu-id="6eb2f-182">Použití rutin prostředí PowerShell sledovací proces sítě, nahrajte do aplikaci funkce nejnovější modul prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-182">To use Network Watcher PowerShell cmdlets, upload the latest PowerShell module to the function app.</span></span>

1. <span data-ttu-id="6eb2f-183">Na místním počítači s nejnovější nainstalované moduly prostředí Azure PowerShell spusťte následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6eb2f-183">On your local machine with the latest Azure PowerShell modules installed, run the following PowerShell command:</span></span>

    ```powershell
    (Get-Module AzureRM.Network).Path
    ```

    <span data-ttu-id="6eb2f-184">Tento příklad poskytuje místní cesty moduly prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-184">This example gives you the local path of your Azure PowerShell modules.</span></span> <span data-ttu-id="6eb2f-185">Tyto složky se používají v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-185">These folders are used in a later step.</span></span> <span data-ttu-id="6eb2f-186">Moduly, které se používají v tomto scénáři jsou:</span><span class="sxs-lookup"><span data-stu-id="6eb2f-186">The modules that are used in this scenario are:</span></span>

    * <span data-ttu-id="6eb2f-187">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="6eb2f-187">AzureRM.Network</span></span>

    * <span data-ttu-id="6eb2f-188">AzureRM.Profile</span><span class="sxs-lookup"><span data-stu-id="6eb2f-188">AzureRM.Profile</span></span>

    * <span data-ttu-id="6eb2f-189">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="6eb2f-189">AzureRM.Resources</span></span>

    ![Složky prostředí PowerShell][functions5]

1. <span data-ttu-id="6eb2f-191">Vyberte **funkce nastavení aplikace** > **přejděte do editoru služby aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-191">Select **Function app settings** > **Go to App Service Editor**.</span></span>

    ![Nastavení aplikace – funkce][functions2]

1. <span data-ttu-id="6eb2f-193">Klikněte pravým tlačítkem myši **AlertPacketCapturePowershell** složku a pak vytvořte složku s názvem **azuremodules**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-193">Right-click the **AlertPacketCapturePowershell** folder, and then create a folder called **azuremodules**.</span></span> 

4. <span data-ttu-id="6eb2f-194">Vytvořte podsložku pro každý modul, který potřebujete.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-194">Create a subfolder for each module that you need.</span></span>

    ![Složky a podsložky][functions3]

    * <span data-ttu-id="6eb2f-196">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="6eb2f-196">AzureRM.Network</span></span>

    * <span data-ttu-id="6eb2f-197">AzureRM.Profile</span><span class="sxs-lookup"><span data-stu-id="6eb2f-197">AzureRM.Profile</span></span>

    * <span data-ttu-id="6eb2f-198">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="6eb2f-198">AzureRM.Resources</span></span>

1. <span data-ttu-id="6eb2f-199">Klikněte pravým tlačítkem myši **AzureRM.Network** podsložku a potom vyberte **nahrání souborů**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-199">Right-click the **AzureRM.Network** subfolder, and then select **Upload Files**.</span></span> 

6. <span data-ttu-id="6eb2f-200">Přejděte na Azure moduly.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-200">Go to your Azure modules.</span></span> <span data-ttu-id="6eb2f-201">Místní **AzureRM.Network** složky, vyberte všechny soubory ve složce.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-201">In the local **AzureRM.Network** folder, select all the files in the folder.</span></span> <span data-ttu-id="6eb2f-202">Potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-202">Then select **OK**.</span></span> 

7. <span data-ttu-id="6eb2f-203">Opakujte tyto kroky pro **AzureRM.Profile** a **AzureRM.Resources**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-203">Repeat these steps for **AzureRM.Profile** and **AzureRM.Resources**.</span></span>

    ![Nahrání souborů][functions6]

1. <span data-ttu-id="6eb2f-205">Po dokončení, každé složky by měly mít soubory modulu prostředí PowerShell z místního počítače.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-205">After you've finished, each folder should have the PowerShell module files from your local machine.</span></span>

    ![Soubory prostředí PowerShell][functions7]

### <a name="authentication"></a><span data-ttu-id="6eb2f-207">Authentication</span><span class="sxs-lookup"><span data-stu-id="6eb2f-207">Authentication</span></span>

<span data-ttu-id="6eb2f-208">Pokud chcete používat rutiny prostředí PowerShell, je třeba ověřit.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-208">To use the PowerShell cmdlets, you must authenticate.</span></span> <span data-ttu-id="6eb2f-209">Nakonfigurujte ověřování v aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-209">You configure authentication in the function app.</span></span> <span data-ttu-id="6eb2f-210">Postup konfigurace ověřování, musíte nakonfigurovat proměnné prostředí a nahrát soubor šifrované klíče do aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-210">To configure authentication, you must configure environment variables and upload an encrypted key file to the function app.</span></span>

> [!NOTE]
> <span data-ttu-id="6eb2f-211">Tento scénář obsahuje pouze jeden z příkladů jak implementovat ověřování s Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-211">This scenario provides just one example of how to implement authentication with Azure Functions.</span></span> <span data-ttu-id="6eb2f-212">Uděláte to tak i jinými způsoby.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-212">There are other ways to do this.</span></span>

#### <a name="encrypted-credentials"></a><span data-ttu-id="6eb2f-213">Zašifrované přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="6eb2f-213">Encrypted credentials</span></span>

<span data-ttu-id="6eb2f-214">Následující skript prostředí PowerShell vytvoří soubor klíče s názvem **PassEncryptKey.key**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-214">The following PowerShell script creates a key file called **PassEncryptKey.key**.</span></span> <span data-ttu-id="6eb2f-215">Nabízí taky zašifrovaná verze hesla, která se dodává.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-215">It also provides an encrypted version of the password that's supplied.</span></span> <span data-ttu-id="6eb2f-216">Toto heslo je stejné heslo, které je definována pro aplikaci Azure Active Directory, která se používá k ověřování.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-216">This password is the same password that is defined for the Azure Active Directory application that's used for authentication.</span></span>

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

<span data-ttu-id="6eb2f-217">V aplikaci služby editoru funkce aplikace, vytvořte složku s názvem **klíče** pod **AlertPacketCapturePowerShell**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-217">In the App Service Editor of the function app, create a folder called **keys** under **AlertPacketCapturePowerShell**.</span></span> <span data-ttu-id="6eb2f-218">Nahrajte **PassEncryptKey.key** soubor, který jste vytvořili v předchozí ukázce prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-218">Then upload the **PassEncryptKey.key** file that you created in the previous PowerShell sample.</span></span>

![Funkce klíč][functions8]

### <a name="retrieve-values-for-environment-variables"></a><span data-ttu-id="6eb2f-220">Načtení hodnoty pro proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="6eb2f-220">Retrieve values for environment variables</span></span>

<span data-ttu-id="6eb2f-221">Požadavek na konečné je nastavení proměnných prostředí, které jsou potřebné pro přístup k hodnotám pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-221">The final requirement is to set up the environment variables that are necessary to access the values for authentication.</span></span> <span data-ttu-id="6eb2f-222">V následujícím seznamu jsou proměnné prostředí, které jsou vytvořené:</span><span class="sxs-lookup"><span data-stu-id="6eb2f-222">The following list shows the environment variables that are created:</span></span>

* <span data-ttu-id="6eb2f-223">AzureClientID</span><span class="sxs-lookup"><span data-stu-id="6eb2f-223">AzureClientID</span></span>

* <span data-ttu-id="6eb2f-224">AzureTenant</span><span class="sxs-lookup"><span data-stu-id="6eb2f-224">AzureTenant</span></span>

* <span data-ttu-id="6eb2f-225">AzureCredPassword</span><span class="sxs-lookup"><span data-stu-id="6eb2f-225">AzureCredPassword</span></span>


#### <a name="azureclientid"></a><span data-ttu-id="6eb2f-226">AzureClientID</span><span class="sxs-lookup"><span data-stu-id="6eb2f-226">AzureClientID</span></span>

<span data-ttu-id="6eb2f-227">ID klienta je ID aplikace aplikace v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-227">The client ID is the Application ID of an application in Azure Active Directory.</span></span>

1. <span data-ttu-id="6eb2f-228">Pokud ještě nemáte aplikace pro použití, spusťte následující příklad k vytvoření aplikace.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-228">If you don't already have an application to use, run the following example to create an application.</span></span>

    ```powershell
    $app = New-AzureRmADApplication -DisplayName "ExampleAutomationAccount_MF" -HomePage "https://exampleapp.com" -IdentifierUris "https://exampleapp1.com/ExampleFunctionsAccount" -Password "<same password as defined earlier>"
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    Start-Sleep 15
    New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $app.ApplicationId
    ```

   > [!NOTE]
   > <span data-ttu-id="6eb2f-229">Heslo, které použijete při vytváření aplikace musí být stejné heslo, které jste předtím vytvořili při ukládání souboru klíče.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-229">The password that you use when creating the application should be the same password that you created earlier when saving the key file.</span></span>

1. <span data-ttu-id="6eb2f-230">Na portálu Azure vyberte **odběry**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-230">In the Azure portal, select **Subscriptions**.</span></span> <span data-ttu-id="6eb2f-231">Vyberte předplatné, které chcete použít a pak vyberte **přístup k ovládacímu prvku (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-231">Select the subscription to use, and then select **Access control (IAM)**.</span></span>

    ![Funkce IAM][functions9]

1. <span data-ttu-id="6eb2f-233">Vyberte účet, který chcete použít a potom vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-233">Choose the account to use, and then select **Properties**.</span></span> <span data-ttu-id="6eb2f-234">Zkopírujte ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-234">Copy the Application ID.</span></span>

    ![ID aplikace funkce][functions10]

#### <a name="azuretenant"></a><span data-ttu-id="6eb2f-236">AzureTenant</span><span class="sxs-lookup"><span data-stu-id="6eb2f-236">AzureTenant</span></span>

<span data-ttu-id="6eb2f-237">Získejte ID klienta tak, že spustíte následující ukázku v prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6eb2f-237">Obtain the tenant ID  by running the following PowerShell sample:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "<subscriptionName>").TenantId
```

#### <a name="azurecredpassword"></a><span data-ttu-id="6eb2f-238">AzureCredPassword</span><span class="sxs-lookup"><span data-stu-id="6eb2f-238">AzureCredPassword</span></span>

<span data-ttu-id="6eb2f-239">Hodnota proměnné prostředí AzureCredPassword je hodnota, kterou můžete získat z systémem následující ukázku v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-239">The value of the AzureCredPassword environment variable is the value that you get from running the following PowerShell sample.</span></span> <span data-ttu-id="6eb2f-240">Tento příklad je stejný jako ten, který je zobrazen v předchozím **šifrovat přihlašovací údaje** části.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-240">This example is the same one that's shown in the preceding **Encrypted credentials** section.</span></span> <span data-ttu-id="6eb2f-241">Hodnota, která je potřeba je výstup `$Encryptedpassword` proměnné.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-241">The value that's needed is the output of the `$Encryptedpassword` variable.</span></span>  <span data-ttu-id="6eb2f-242">Toto je hlavní heslo služby, který jste zašifrovali pomocí skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-242">This is the service principal password that you encrypted by using the PowerShell script.</span></span>

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

### <a name="store-the-environment-variables"></a><span data-ttu-id="6eb2f-243">Uložení proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="6eb2f-243">Store the environment variables</span></span>

1. <span data-ttu-id="6eb2f-244">Přejděte do aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-244">Go to the function app.</span></span> <span data-ttu-id="6eb2f-245">Potom vyberte **funkce nastavení aplikace** > **konfigurovat nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-245">Then select **Function app settings** > **Configure app settings**.</span></span>

    ![Konfigurace nastavení aplikace][functions11]

1. <span data-ttu-id="6eb2f-247">Přidání proměnné prostředí a jejich hodnoty nastavení aplikace a pak vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-247">Add the environment variables and their values to the app settings, and then select **Save**.</span></span>

    ![Nastavení aplikace][functions12]

### <a name="add-powershell-to-the-function"></a><span data-ttu-id="6eb2f-249">Přidání funkce prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6eb2f-249">Add PowerShell to the function</span></span>

<span data-ttu-id="6eb2f-250">Nyní je čas provádět volání do sledovací proces sítě z v rámci Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-250">It's now time to make calls into Network Watcher from within the Azure function.</span></span> <span data-ttu-id="6eb2f-251">V závislosti na požadavcích implementace této funkce se může lišit.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-251">Depending on the requirements, the implementation of this function can vary.</span></span> <span data-ttu-id="6eb2f-252">Obecný tok kódu však vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6eb2f-252">However, the general flow of the code is as follows:</span></span>

1. <span data-ttu-id="6eb2f-253">Proces vstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-253">Process input parameters.</span></span>
2. <span data-ttu-id="6eb2f-254">Ověřte omezení a vyřešit konflikty v názvech zaznamená paketu existující dotaz.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-254">Query existing packet captures to verify limits and resolve name conflicts.</span></span>
3. <span data-ttu-id="6eb2f-255">Vytvořte zachytáváním paketů s příslušnými parametry.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-255">Create a packet capture with appropriate parameters.</span></span>
4. <span data-ttu-id="6eb2f-256">Dotazování paketu zachytit pravidelně, dokud nebude dokončeno.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-256">Poll packet capture periodically until it's complete.</span></span>
5. <span data-ttu-id="6eb2f-257">Upozorněte uživatele, relace zachytávání paketů je dokončena.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-257">Notify the user that the packet capture session is complete.</span></span>

<span data-ttu-id="6eb2f-258">V následujícím příkladu je prostředí PowerShell kód, který můžete použít ve funkci.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-258">The following example is PowerShell code that can be used in the function.</span></span> <span data-ttu-id="6eb2f-259">Existují hodnoty, které je nutné nahradit pro **subscriptionId**, **resourceGroupName**, a **storageAccountName**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-259">There are values that need to be replaced for **subscriptionId**, **resourceGroupName**, and **storageAccountName**.</span></span>

```powershell
            #Import Azure PowerShell modules required to make calls to Network Watcher
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Profile\AzureRM.Profile.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Network\AzureRM.Network.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Resources\AzureRM.Resources.psd1" -Global

            #Process alert request body
            $requestBody = Get-Content $req -Raw | ConvertFrom-Json

            #Storage account ID to save captures in
            $storageaccountid = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}"

            #Packet capture vars
            $packetcapturename = "PSAzureFunction"
            $packetCaptureLimit = 10
            $packetCaptureDuration = 10

            #Credentials
            $tenant = $env:AzureTenant
            $pw = $env:AzureCredPassword
            $clientid = $env:AzureClientId
            $keypath = "D:\home\site\wwwroot\AlertPacketCapturePowerShell\keys\PassEncryptKey.key"

            #Authentication
            $secpassword = $pw | ConvertTo-SecureString -Key (Get-Content $keypath)
            $credential = New-Object System.Management.Automation.PSCredential ($clientid, $secpassword)
            Add-AzureRMAccount -ServicePrincipal -Tenant $tenant -Credential $credential #-WarningAction SilentlyContinue | out-null


            #Get the VM that fired the alert
            if($requestBody.context.resourceType -eq "Microsoft.Compute/virtualMachines")
            {
                Write-Output ("Subscription ID: {0}" -f $requestBody.context.subscriptionId)
                Write-Output ("Resource Group:  {0}" -f $requestBody.context.resourceGroupName)
                Write-Output ("Resource Name:  {0}" -f $requestBody.context.resourceName)
                Write-Output ("Resource Type:  {0}" -f $requestBody.context.resourceType)

                #Get the Network Watcher in the VM's region
                $nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $requestBody.context.resourceRegion}
                $networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

                #Get existing packetCaptures
                $packetCaptures = Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher

                #Remove existing packet capture created by the function (if it exists)
                $packetCaptures | %{if($_.Name -eq $packetCaptureName)
                { 
                    Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName $packetCaptureName
                }}

                #Initiate packet capture on the VM that fired the alert
                if ((Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher).Count -lt $packetCaptureLimit){
                    echo "Initiating Packet Capture"
                    New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $requestBody.context.resourceId -PacketCaptureName $packetCaptureName -StorageAccountId $storageaccountid -TimeLimitInSeconds $packetCaptureDuration
                    Out-File -Encoding Ascii -FilePath $res -inputObject "Packet Capture created on ${requestBody.context.resourceID}"
                }
            } 
 ``` 
#### <a name="retrieve-the-function-url"></a><span data-ttu-id="6eb2f-260">Načíst URL pro funkce</span><span class="sxs-lookup"><span data-stu-id="6eb2f-260">Retrieve the function URL</span></span> 
1. <span data-ttu-id="6eb2f-261">Po vytvoření funkce, nakonfigurujte upozornění k volání adresu URL, který je spojen s funkcí.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-261">After you've created your function, configure your alert to call the URL that's associated with the function.</span></span> <span data-ttu-id="6eb2f-262">Chcete-li získat tuto hodnotu, zkopírujte adresu URL funkce z vaší aplikace. funkce.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-262">To get this value, copy the function URL from your function app.</span></span>

    ![Adresa URL funkce hledání][functions13]

2. <span data-ttu-id="6eb2f-264">Zkopírujte adresu URL funkce pro vaši aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-264">Copy the function URL for your function app.</span></span>

    ![Kopírování adresu – funkce][2]

<span data-ttu-id="6eb2f-266">Pokud budete potřebovat vlastní vlastnosti v datové části požadavek POST webhooku, podívejte se na [konfigurace webhook, jehož na výstrahu Azure metriky](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="6eb2f-266">If you require custom properties in the payload of the webhook POST request, refer to [Configure a webhook on an Azure metric alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

## <a name="configure-an-alert-on-a-vm"></a><span data-ttu-id="6eb2f-267">Konfigurace výstrahy na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="6eb2f-267">Configure an alert on a VM</span></span>

<span data-ttu-id="6eb2f-268">Výstrahy můžete nakonfigurovat pro jednotlivce upozornění, když konkrétní metrika překračuje prahovou hodnotu, která je k němu přiřazen.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-268">Alerts can be configured to notify individuals when a specific metric crosses a threshold that's assigned to it.</span></span> <span data-ttu-id="6eb2f-269">V tomto příkladu je výstraha na segmentů TCP, které se odesílají, ale může být výstraha pro mnoho jiné metriky.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-269">In this example, the alert is on the TCP segments that are sent, but the alert can be triggered for many other metrics.</span></span> <span data-ttu-id="6eb2f-270">V tomto příkladu je nakonfigurované výstrahu volat webhook pro volání funkce.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-270">In this example, an alert is configured to call a webhook to call the function.</span></span>

### <a name="create-the-alert-rule"></a><span data-ttu-id="6eb2f-271">Vytvořit pravidlo výstrahy</span><span class="sxs-lookup"><span data-stu-id="6eb2f-271">Create the alert rule</span></span>

<span data-ttu-id="6eb2f-272">Přejít na existující virtuální počítač a poté přidejte pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-272">Go to an existing virtual machine, and then add an alert rule.</span></span> <span data-ttu-id="6eb2f-273">Podrobnější dokumentaci týkající se konfigurace výstrahy naleznete na [vytvoření výstrahy v Azure monitorování pro služby Azure – portál Azure](../monitoring-and-diagnostics/insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6eb2f-273">More detailed documentation about configuring alerts can be found at [Create alerts in Azure Monitor for Azure services - Azure portal](../monitoring-and-diagnostics/insights-alerts-portal.md).</span></span> <span data-ttu-id="6eb2f-274">Zadejte následující hodnoty v **pravidlo výstrahy** a pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-274">Enter the following values in the **Alert rule** blade, and then select **OK**.</span></span>

  |<span data-ttu-id="6eb2f-275">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-275">**Setting**</span></span> | <span data-ttu-id="6eb2f-276">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-276">**Value**</span></span> | <span data-ttu-id="6eb2f-277">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-277">**Details**</span></span> |
  |---|---|---|
  |<span data-ttu-id="6eb2f-278">**Název**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-278">**Name**</span></span>|<span data-ttu-id="6eb2f-279">TCP_Segments_Sent_Exceeded</span><span class="sxs-lookup"><span data-stu-id="6eb2f-279">TCP_Segments_Sent_Exceeded</span></span>|<span data-ttu-id="6eb2f-280">Název pravidla výstrahy.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-280">Name of the alert rule.</span></span>|
  |<span data-ttu-id="6eb2f-281">**Popis**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-281">**Description**</span></span>|<span data-ttu-id="6eb2f-282">Segmentů TCP odeslané překročil prahovou hodnotu</span><span class="sxs-lookup"><span data-stu-id="6eb2f-282">TCP segments sent exceeded threshold</span></span>|<span data-ttu-id="6eb2f-283">Popis pro pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-283">The description for the alert rule.</span></span>||
  |<span data-ttu-id="6eb2f-284">**Metrika**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-284">**Metric**</span></span>|<span data-ttu-id="6eb2f-285">Odeslané segmenty TCP</span><span class="sxs-lookup"><span data-stu-id="6eb2f-285">TCP segments sent</span></span>| <span data-ttu-id="6eb2f-286">Metrika sloužící k aktivaci upozornění.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-286">The metric to use to trigger the alert.</span></span> |
  |<span data-ttu-id="6eb2f-287">**Podmínka**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-287">**Condition**</span></span>|<span data-ttu-id="6eb2f-288">Více než</span><span class="sxs-lookup"><span data-stu-id="6eb2f-288">Greater than</span></span>| <span data-ttu-id="6eb2f-289">Podmínku, která má použít při vyhodnocení metriku.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-289">The condition to use when evaluating the metric.</span></span>|
  |<span data-ttu-id="6eb2f-290">**Prahová hodnota**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-290">**Threshold**</span></span>|<span data-ttu-id="6eb2f-291">100</span><span class="sxs-lookup"><span data-stu-id="6eb2f-291">100</span></span>| <span data-ttu-id="6eb2f-292">Hodnota metriku, která aktivuje výstrahu.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-292">The  value of the metric that triggers the alert.</span></span> <span data-ttu-id="6eb2f-293">Tato hodnota by měla nastavit na platnou hodnotu pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-293">This value should be set to a valid value for your environment.</span></span>|
  |<span data-ttu-id="6eb2f-294">**Období**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-294">**Period**</span></span>|<span data-ttu-id="6eb2f-295">Za posledních pět minut</span><span class="sxs-lookup"><span data-stu-id="6eb2f-295">Over the last five minutes</span></span>| <span data-ttu-id="6eb2f-296">Určuje dobu, ve kterém má být vyhledán prahovou hodnotu na metriky.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-296">Determines the period in which to look for the threshold on the metric.</span></span>|
  |<span data-ttu-id="6eb2f-297">**Webhooku**</span><span class="sxs-lookup"><span data-stu-id="6eb2f-297">**Webhook**</span></span>|<span data-ttu-id="6eb2f-298">[URL webhooku se nenačetla z funkce aplikace]</span><span class="sxs-lookup"><span data-stu-id="6eb2f-298">[webhook URL from function app]</span></span>| <span data-ttu-id="6eb2f-299">Adresa URL webhooku z funkce aplikace, který byl vytvořen v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-299">The webhook URL from the function app that was created in the previous steps.</span></span>|

> [!NOTE]
> <span data-ttu-id="6eb2f-300">Metrika segmentů TCP není povoleno ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-300">The TCP segments metric is not enabled by default.</span></span> <span data-ttu-id="6eb2f-301">Další informace o tom, jak povolit další metriky navštivte stránky [zapínání monitorování a Diagnostika](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="6eb2f-301">Learn more about how to enable additional metrics by visiting [Enable monitoring and diagnostics](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).</span></span>

## <a name="review-the-results"></a><span data-ttu-id="6eb2f-302">Zkontrolujte výsledky</span><span class="sxs-lookup"><span data-stu-id="6eb2f-302">Review the results</span></span>

<span data-ttu-id="6eb2f-303">Po kritéria pro výstrahy aktivačních událostí vytvoří se zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-303">After the criteria for the alert triggers, a packet capture is created.</span></span> <span data-ttu-id="6eb2f-304">Přejděte do sledovací proces sítě a potom vyberte **zachytáváním paketů**.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-304">Go to Network Watcher, and then select **Packet capture**.</span></span> <span data-ttu-id="6eb2f-305">Na této stránce můžete vybrat odkaz souboru zachytávání paketů stáhnout zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-305">On this page, you can select the packet capture file link to download the packet capture.</span></span>

![Zobrazení zachytávání paketů][functions14]

<span data-ttu-id="6eb2f-307">Pokud soubor snímek je uložený místně, můžete ji načíst po přihlášení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-307">If the capture file is stored locally, you can retrieve it by signing in to the virtual machine.</span></span>

<span data-ttu-id="6eb2f-308">Pokyny týkající se stahování souborů z účtů úložiště Azure najdete v tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="6eb2f-308">For instructions about downloading files from Azure storage accounts, see [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="6eb2f-309">Jiný nástroj, můžete je [Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="6eb2f-309">Another tool you can use is [Storage Explorer](http://storageexplorer.com/).</span></span>

<span data-ttu-id="6eb2f-310">Po stažení vaší zachycení, můžete ji zobrazit pomocí libovolného nástroje, který může číst **CAP** souboru.</span><span class="sxs-lookup"><span data-stu-id="6eb2f-310">After your capture has been downloaded, you can view it by using any tool that can read a **.cap** file.</span></span> <span data-ttu-id="6eb2f-311">Tady jsou odkazy na dva z těchto nástrojů:</span><span class="sxs-lookup"><span data-stu-id="6eb2f-311">Following are links to two of these tools:</span></span>

- [<span data-ttu-id="6eb2f-312">Microsoft Message Analyzer</span><span class="sxs-lookup"><span data-stu-id="6eb2f-312">Microsoft Message Analyzer</span></span>](https://technet.microsoft.com/library/jj649776.aspx)
- [<span data-ttu-id="6eb2f-313">WireShark</span><span class="sxs-lookup"><span data-stu-id="6eb2f-313">WireShark</span></span>](https://www.wireshark.org/)

## <a name="next-steps"></a><span data-ttu-id="6eb2f-314">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6eb2f-314">Next steps</span></span>

<span data-ttu-id="6eb2f-315">Zjistěte, jak zobrazit vaše paketu zachycení tak, že navštívíte [analýza zachytávání paketů s Wireshark](network-watcher-deep-packet-inspection.md).</span><span class="sxs-lookup"><span data-stu-id="6eb2f-315">Learn how to view your packet captures by visiting [Packet capture analysis with Wireshark](network-watcher-deep-packet-inspection.md).</span></span>


[1]: ./media/network-watcher-alert-triggered-packet-capture/figure1.png
[1-1]: ./media/network-watcher-alert-triggered-packet-capture/figure1-1.png
[2]: ./media/network-watcher-alert-triggered-packet-capture/figure2.png
[3]: ./media/network-watcher-alert-triggered-packet-capture/figure3.png
[functions1]:./media/network-watcher-alert-triggered-packet-capture/functions1.png
[functions2]:./media/network-watcher-alert-triggered-packet-capture/functions2.png
[functions3]:./media/network-watcher-alert-triggered-packet-capture/functions3.png
[functions4]:./media/network-watcher-alert-triggered-packet-capture/functions4.png
[functions5]:./media/network-watcher-alert-triggered-packet-capture/functions5.png
[functions6]:./media/network-watcher-alert-triggered-packet-capture/functions6.png
[functions7]:./media/network-watcher-alert-triggered-packet-capture/functions7.png
[functions8]:./media/network-watcher-alert-triggered-packet-capture/functions8.png
[functions9]:./media/network-watcher-alert-triggered-packet-capture/functions9.png
[functions10]:./media/network-watcher-alert-triggered-packet-capture/functions10.png
[functions11]:./media/network-watcher-alert-triggered-packet-capture/functions11.png
[functions12]:./media/network-watcher-alert-triggered-packet-capture/functions12.png
[functions13]:./media/network-watcher-alert-triggered-packet-capture/functions13.png
[functions14]:./media/network-watcher-alert-triggered-packet-capture/functions14.png
[scenario]:./media/network-watcher-alert-triggered-packet-capture/scenario.png
