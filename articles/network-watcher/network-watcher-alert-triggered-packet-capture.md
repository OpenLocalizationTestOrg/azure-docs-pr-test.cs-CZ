---
title: "monitorování pomocí výstrah a Azure Functions sítě aaaUse paketu zachycení toodo proaktivní | Microsoft Docs"
description: "Tento článek popisuje, jak toocreate výstraha aktivuje zachytáváním paketů s sledovací proces sítě Azure"
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
ms.openlocfilehash: 4722a831f3a9d5537c0e6f53daba4dfc35d0cf24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-packet-capture-for-proactive-network-monitoring-with-alerts-and-azure-functions"></a><span data-ttu-id="1e351-103">Použít zachytáváním paketů pro monitorování proaktivní sítě pomocí výstrah a Azure Functions</span><span class="sxs-lookup"><span data-stu-id="1e351-103">Use packet capture for proactive network monitoring with alerts and Azure Functions</span></span>

<span data-ttu-id="1e351-104">Zachycení dat ze sítě sledovacích procesů paketu vytvoří zaznamenání relace tootrack provozu do a z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1e351-104">Network Watcher packet capture creates capture sessions tootrack traffic in and out of virtual machines.</span></span> <span data-ttu-id="1e351-105">Hello zachycení soubor může mít filtr, který je definován tootrack pouze hello přenosy, které chcete toomonitor.</span><span class="sxs-lookup"><span data-stu-id="1e351-105">hello capture file can have a filter that is defined tootrack only hello traffic that you want toomonitor.</span></span> <span data-ttu-id="1e351-106">Tato data jsou pak uloženy v storage blob nebo místně na počítači hosta hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-106">This data is then stored in a storage blob or locally on hello guest machine.</span></span>

<span data-ttu-id="1e351-107">Tato funkce můžete spustit vzdáleně z jiných automatizace scénáře, jako je Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="1e351-107">This capability can be started remotely from other automation scenarios such as Azure Functions.</span></span> <span data-ttu-id="1e351-108">Poskytuje zachytávání paketů hello schopností toorun proaktivní zachycení na základě definované sítě anomálií.</span><span class="sxs-lookup"><span data-stu-id="1e351-108">Packet capture gives you hello capability toorun proactive captures based on defined network anomalies.</span></span> <span data-ttu-id="1e351-109">Mezi další použití patří shromažďování statistiku sítě, získání informací o síti vniknutí, ladění komunikaci klienta se serverem a další.</span><span class="sxs-lookup"><span data-stu-id="1e351-109">Other uses include gathering network statistics, getting information about network intrusions, debugging client-server communications, and more.</span></span>

<span data-ttu-id="1e351-110">Prostředky, které jsou nasazené v Azure spustit 24 hodin denně 7.</span><span class="sxs-lookup"><span data-stu-id="1e351-110">Resources that are deployed in Azure run 24/7.</span></span> <span data-ttu-id="1e351-111">Vás a vašich zaměstnanců nelze monitorovat aktivně hello stavu všech prostředků, 24 nebo 7.</span><span class="sxs-lookup"><span data-stu-id="1e351-111">You and your staff cannot actively monitor hello status of all resources 24/7.</span></span> <span data-ttu-id="1e351-112">Co se například stane, pokud se vyskytne problém na 2: 00?</span><span class="sxs-lookup"><span data-stu-id="1e351-112">For example, what happens if an issue occurs at 2 AM?</span></span>

<span data-ttu-id="1e351-113">Pomocí sledovací proces sítě, výstrahy a funkce z v rámci hello Azure ekosystém můžete proaktivně odpoví hello dat a nástroje toosolve problémy ve vaší síti.</span><span class="sxs-lookup"><span data-stu-id="1e351-113">By using Network Watcher, alerting, and functions from within hello Azure ecosystem, you can proactively respond with hello data and tools toosolve problems in your network.</span></span>

![Scénář][scenario]

## <a name="prerequisites"></a><span data-ttu-id="1e351-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1e351-115">Prerequisites</span></span>

* <span data-ttu-id="1e351-116">nejnovější verze Hello [prostředí Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="1e351-116">hello latest version of [Azure PowerShell](/powershell/azure/install-azurerm-ps).</span></span>
* <span data-ttu-id="1e351-117">Existující instanci sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="1e351-117">An existing instance of Network Watcher.</span></span> <span data-ttu-id="1e351-118">Pokud jste ještě nemáte, [vytvořit instanci sledovací proces sítě](network-watcher-create.md).</span><span class="sxs-lookup"><span data-stu-id="1e351-118">If you don't already have one, [create an instance of Network Watcher](network-watcher-create.md).</span></span>
* <span data-ttu-id="1e351-119">Existující virtuální počítač v hello stejné oblasti jako sledovací proces sítě s hello [rozšíření Windows](../virtual-machines/windows/extensions-nwa.md) nebo [rozšíření virtuálního počítače Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="1e351-119">An existing virtual machine in hello same region as Network Watcher with hello [Windows extension](../virtual-machines/windows/extensions-nwa.md) or [Linux virtual machine extension](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="1e351-120">Scénář</span><span class="sxs-lookup"><span data-stu-id="1e351-120">Scenario</span></span>

<span data-ttu-id="1e351-121">V tomto příkladu váš virtuální počítač odesílá víc segmentů TCP než obvykle a chcete toobe výstrahy.</span><span class="sxs-lookup"><span data-stu-id="1e351-121">In this example, your VM is sending more TCP segments than usual, and you want toobe alerted.</span></span> <span data-ttu-id="1e351-122">Segmentů TCP slouží jako příklad zde, ale žádné výstrahy podmínku můžete použít.</span><span class="sxs-lookup"><span data-stu-id="1e351-122">TCP segments are used as an example here, but you can use any alert condition.</span></span>

<span data-ttu-id="1e351-123">Když se zobrazí výstraha, budete chtít toounderstand data na úrovni paketů tooreceive proč komunikace zvýšilo.</span><span class="sxs-lookup"><span data-stu-id="1e351-123">When you are alerted, you want tooreceive packet-level data toounderstand why communication has increased.</span></span> <span data-ttu-id="1e351-124">Potom můžete provést kroky tooreturn hello virtuálního počítače tooregular komunikace.</span><span class="sxs-lookup"><span data-stu-id="1e351-124">Then you can take steps tooreturn hello virtual machine tooregular communication.</span></span>

<span data-ttu-id="1e351-125">Tento scénář předpokládá, že máte existující instanci sledovací proces sítě a skupinu prostředků s platným virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="1e351-125">This scenario assumes that you have an existing instance of Network Watcher and a resource group with a valid virtual machine.</span></span>

<span data-ttu-id="1e351-126">Hello následujícím seznamu je přehled hello pracovního postupu, který probíhá:</span><span class="sxs-lookup"><span data-stu-id="1e351-126">hello following list is an overview of hello workflow that takes place:</span></span>

1. <span data-ttu-id="1e351-127">Výstrahy na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="1e351-127">An alert is triggered on your VM.</span></span>
1. <span data-ttu-id="1e351-128">Výstraha Hello volání funkce Azure prostřednictvím webhook, jehož.</span><span class="sxs-lookup"><span data-stu-id="1e351-128">hello alert calls your Azure function via a webhook.</span></span>
1. <span data-ttu-id="1e351-129">Funkce Azure zpracovává hello výstrahy a spustí relaci zachytávání paketů sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="1e351-129">Your Azure function processes hello alert and starts a Network Watcher packet capture session.</span></span>
1. <span data-ttu-id="1e351-130">zachytáváním paketů Hello běží na hello virtuálních počítačů a shromažďuje provoz.</span><span class="sxs-lookup"><span data-stu-id="1e351-130">hello packet capture runs on hello VM and collects traffic.</span></span>
1. <span data-ttu-id="1e351-131">Hello paketu zachycení soubor odešle tooa účtu úložiště ke kontrole a diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="1e351-131">hello packet capture file is uploaded tooa storage account for review and diagnosis.</span></span>

<span data-ttu-id="1e351-132">tooautomate tento proces jsme vytvořte a připojte se výstrahu na naše tootrigger virtuálních počítačů v případě incidentu hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-132">tooautomate this process, we create and connect an alert on our VM tootrigger when hello incident occurs.</span></span> <span data-ttu-id="1e351-133">Také jsme do sledovací proces sítě vytvořit toocall funkce.</span><span class="sxs-lookup"><span data-stu-id="1e351-133">We also create a function toocall into Network Watcher.</span></span>

<span data-ttu-id="1e351-134">Tento scénář hello následující:</span><span class="sxs-lookup"><span data-stu-id="1e351-134">This scenario does hello following:</span></span>

* <span data-ttu-id="1e351-135">Vytvoří Azure funkci, která se spouští zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="1e351-135">Creates an Azure function that starts a packet capture.</span></span>
* <span data-ttu-id="1e351-136">Vytvoří pravidlo výstrahy na virtuálním počítači a nakonfiguruje hello pravidlo výstrahy toocall hello Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="1e351-136">Creates an alert rule on a virtual machine and configures hello alert rule toocall hello Azure function.</span></span>

## <a name="create-an-azure-function"></a><span data-ttu-id="1e351-137">Vytvoření Azure funkce</span><span class="sxs-lookup"><span data-stu-id="1e351-137">Create an Azure function</span></span>

<span data-ttu-id="1e351-138">prvním krokem Hello je toocreate výstrahu Azure funkce tooprocess hello a vytvořte zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="1e351-138">hello first step is toocreate an Azure function tooprocess hello alert and create a packet capture.</span></span>

1. <span data-ttu-id="1e351-139">V hello [portál Azure](https://portal.azure.com), vyberte **nový** > **výpočetní** > **aplikaci funkce**.</span><span class="sxs-lookup"><span data-stu-id="1e351-139">In hello [Azure portal](https://portal.azure.com), select **New** > **Compute** > **Function App**.</span></span>

    ![Vytvoření aplikace – funkce][1-1]

2. <span data-ttu-id="1e351-141">Na hello **aplikaci funkce** okno, zadejte následující hodnoty hello a potom vyberte **OK** toocreate hello aplikace:</span><span class="sxs-lookup"><span data-stu-id="1e351-141">On hello **Function App** blade, enter hello following values, and then select **OK** toocreate hello app:</span></span>

    |<span data-ttu-id="1e351-142">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="1e351-142">**Setting**</span></span> | <span data-ttu-id="1e351-143">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="1e351-143">**Value**</span></span> | <span data-ttu-id="1e351-144">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="1e351-144">**Details**</span></span> |
    |---|---|---|
    |<span data-ttu-id="1e351-145">**Název aplikace**</span><span class="sxs-lookup"><span data-stu-id="1e351-145">**App name**</span></span>|<span data-ttu-id="1e351-146">PacketCaptureExample</span><span class="sxs-lookup"><span data-stu-id="1e351-146">PacketCaptureExample</span></span>|<span data-ttu-id="1e351-147">Název Hello hello funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="1e351-147">hello name of hello function app.</span></span>|
    |<span data-ttu-id="1e351-148">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="1e351-148">**Subscription**</span></span>|<span data-ttu-id="1e351-149">[Předplatné] hello předplatné, pro které toocreate hello funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="1e351-149">[Your subscription]hello subscription for which toocreate hello function app.</span></span>||
    |<span data-ttu-id="1e351-150">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="1e351-150">**Resource Group**</span></span>|<span data-ttu-id="1e351-151">PacketCaptureRG</span><span class="sxs-lookup"><span data-stu-id="1e351-151">PacketCaptureRG</span></span>|<span data-ttu-id="1e351-152">Hello prostředků skupiny toocontain hello funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="1e351-152">hello resource group toocontain hello function app.</span></span>|
    |<span data-ttu-id="1e351-153">**Hostování plán**</span><span class="sxs-lookup"><span data-stu-id="1e351-153">**Hosting Plan**</span></span>|<span data-ttu-id="1e351-154">Plán spotřeba</span><span class="sxs-lookup"><span data-stu-id="1e351-154">Consumption Plan</span></span>| <span data-ttu-id="1e351-155">Typ Hello naplánujte vaše aplikace používá funkce.</span><span class="sxs-lookup"><span data-stu-id="1e351-155">hello type of plan your function app uses.</span></span> <span data-ttu-id="1e351-156">Možnosti jsou spotřeby nebo plán služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="1e351-156">Options are Consumption or Azure App Service plan.</span></span> |
    |<span data-ttu-id="1e351-157">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="1e351-157">**Location**</span></span>|<span data-ttu-id="1e351-158">Střed USA</span><span class="sxs-lookup"><span data-stu-id="1e351-158">Central US</span></span>| <span data-ttu-id="1e351-159">Hello oblast, ve které toocreate hello funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="1e351-159">hello region in which toocreate hello function app.</span></span>|
    |<span data-ttu-id="1e351-160">**Účet úložiště**</span><span class="sxs-lookup"><span data-stu-id="1e351-160">**Storage Account**</span></span>|<span data-ttu-id="1e351-161">{automaticky vygenerovanou}</span><span class="sxs-lookup"><span data-stu-id="1e351-161">{autogenerated}</span></span>| <span data-ttu-id="1e351-162">Hello účtu úložiště, které potřebuje funkce Azure pro úložiště pro obecné účely.</span><span class="sxs-lookup"><span data-stu-id="1e351-162">hello storage account that Azure Functions needs for general-purpose storage.</span></span>|

3. <span data-ttu-id="1e351-163">Na hello **PacketCaptureExample funkce aplikace** vyberte **funkce** > **vlastní funkce**  >  **+**.</span><span class="sxs-lookup"><span data-stu-id="1e351-163">On hello **PacketCaptureExample Function Apps** blade, select **Functions** > **Custom function** >**+**.</span></span>

4. <span data-ttu-id="1e351-164">Vyberte **HttpTrigger-Powershell**a pak zadejte zbývající informace hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-164">Select **HttpTrigger-Powershell**, and then enter hello remaining information.</span></span> <span data-ttu-id="1e351-165">Nakonec toocreate hello funkce, vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1e351-165">Finally, toocreate hello function, select **Create**.</span></span>

    |<span data-ttu-id="1e351-166">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="1e351-166">**Setting**</span></span> | <span data-ttu-id="1e351-167">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="1e351-167">**Value**</span></span> | <span data-ttu-id="1e351-168">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="1e351-168">**Details**</span></span> |
    |---|---|---|
    |<span data-ttu-id="1e351-169">**Scénář**</span><span class="sxs-lookup"><span data-stu-id="1e351-169">**Scenario**</span></span>|<span data-ttu-id="1e351-170">experimentální</span><span class="sxs-lookup"><span data-stu-id="1e351-170">Experimental</span></span>|<span data-ttu-id="1e351-171">Typ scénáře</span><span class="sxs-lookup"><span data-stu-id="1e351-171">Type of scenario</span></span>|
    |<span data-ttu-id="1e351-172">**Pojmenujte svoji funkci**</span><span class="sxs-lookup"><span data-stu-id="1e351-172">**Name your function**</span></span>|<span data-ttu-id="1e351-173">AlertPacketCapturePowerShell</span><span class="sxs-lookup"><span data-stu-id="1e351-173">AlertPacketCapturePowerShell</span></span>|<span data-ttu-id="1e351-174">Název funkce hello</span><span class="sxs-lookup"><span data-stu-id="1e351-174">Name of hello function</span></span>|
    |<span data-ttu-id="1e351-175">**Úroveň oprávnění**</span><span class="sxs-lookup"><span data-stu-id="1e351-175">**Authorization level**</span></span>|<span data-ttu-id="1e351-176">Funkce</span><span class="sxs-lookup"><span data-stu-id="1e351-176">Function</span></span>|<span data-ttu-id="1e351-177">Úroveň oprávnění pro funkci hello</span><span class="sxs-lookup"><span data-stu-id="1e351-177">Authorization level for hello function</span></span>|

![Příklad funkce][functions1]

> [!NOTE]
> <span data-ttu-id="1e351-179">Šablona Hello prostředí PowerShell je experimentální a nemá plnou podporu.</span><span class="sxs-lookup"><span data-stu-id="1e351-179">hello PowerShell template is experimental and does not have full support.</span></span>

<span data-ttu-id="1e351-180">Úpravy jsou požadovány pro tento příklad a jsou vysvětlené v hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="1e351-180">Customizations are required for this example and are explained in hello following steps.</span></span>

### <a name="add-modules"></a><span data-ttu-id="1e351-181">Přidání modulů</span><span class="sxs-lookup"><span data-stu-id="1e351-181">Add modules</span></span>

<span data-ttu-id="1e351-182">toouse rutiny prostředí PowerShell sledovací proces sítě, nahrajte hello nejnovější prostředí PowerShell modulu toohello funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="1e351-182">toouse Network Watcher PowerShell cmdlets, upload hello latest PowerShell module toohello function app.</span></span>

1. <span data-ttu-id="1e351-183">Na místním počítači s hello nejnovější nainstalované moduly prostředí Azure PowerShell spusťte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="1e351-183">On your local machine with hello latest Azure PowerShell modules installed, run hello following PowerShell command:</span></span>

    ```powershell
    (Get-Module AzureRM.Network).Path
    ```

    <span data-ttu-id="1e351-184">Tento příklad poskytuje hello místní cesty moduly prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1e351-184">This example gives you hello local path of your Azure PowerShell modules.</span></span> <span data-ttu-id="1e351-185">Tyto složky se používají v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="1e351-185">These folders are used in a later step.</span></span> <span data-ttu-id="1e351-186">Hello moduly, které se používají v tomto scénáři jsou:</span><span class="sxs-lookup"><span data-stu-id="1e351-186">hello modules that are used in this scenario are:</span></span>

    * <span data-ttu-id="1e351-187">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="1e351-187">AzureRM.Network</span></span>

    * <span data-ttu-id="1e351-188">AzureRM.Profile</span><span class="sxs-lookup"><span data-stu-id="1e351-188">AzureRM.Profile</span></span>

    * <span data-ttu-id="1e351-189">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="1e351-189">AzureRM.Resources</span></span>

    ![Složky prostředí PowerShell][functions5]

1. <span data-ttu-id="1e351-191">Vyberte **funkce nastavení aplikace** > **přejděte tooApp Editor služby**.</span><span class="sxs-lookup"><span data-stu-id="1e351-191">Select **Function app settings** > **Go tooApp Service Editor**.</span></span>

    ![Nastavení aplikace – funkce][functions2]

1. <span data-ttu-id="1e351-193">Klikněte pravým tlačítkem na hello **AlertPacketCapturePowershell** složku a pak vytvořte složku s názvem **azuremodules**.</span><span class="sxs-lookup"><span data-stu-id="1e351-193">Right-click hello **AlertPacketCapturePowershell** folder, and then create a folder called **azuremodules**.</span></span> 

4. <span data-ttu-id="1e351-194">Vytvořte podsložku pro každý modul, který potřebujete.</span><span class="sxs-lookup"><span data-stu-id="1e351-194">Create a subfolder for each module that you need.</span></span>

    ![Složky a podsložky][functions3]

    * <span data-ttu-id="1e351-196">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="1e351-196">AzureRM.Network</span></span>

    * <span data-ttu-id="1e351-197">AzureRM.Profile</span><span class="sxs-lookup"><span data-stu-id="1e351-197">AzureRM.Profile</span></span>

    * <span data-ttu-id="1e351-198">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="1e351-198">AzureRM.Resources</span></span>

1. <span data-ttu-id="1e351-199">Klikněte pravým tlačítkem na hello **AzureRM.Network** podsložku a potom vyberte **nahrání souborů**.</span><span class="sxs-lookup"><span data-stu-id="1e351-199">Right-click hello **AzureRM.Network** subfolder, and then select **Upload Files**.</span></span> 

6. <span data-ttu-id="1e351-200">Přejděte tooyour Azure moduly.</span><span class="sxs-lookup"><span data-stu-id="1e351-200">Go tooyour Azure modules.</span></span> <span data-ttu-id="1e351-201">V místní hello **AzureRM.Network** složky, vyberte všechny hello soubory ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-201">In hello local **AzureRM.Network** folder, select all hello files in hello folder.</span></span> <span data-ttu-id="1e351-202">Potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="1e351-202">Then select **OK**.</span></span> 

7. <span data-ttu-id="1e351-203">Opakujte tyto kroky pro **AzureRM.Profile** a **AzureRM.Resources**.</span><span class="sxs-lookup"><span data-stu-id="1e351-203">Repeat these steps for **AzureRM.Profile** and **AzureRM.Resources**.</span></span>

    ![Nahrání souborů][functions6]

1. <span data-ttu-id="1e351-205">Po dokončení, každé složky by měly mít soubory modulu prostředí PowerShell hello z místního počítače.</span><span class="sxs-lookup"><span data-stu-id="1e351-205">After you've finished, each folder should have hello PowerShell module files from your local machine.</span></span>

    ![Soubory prostředí PowerShell][functions7]

### <a name="authentication"></a><span data-ttu-id="1e351-207">Authentication</span><span class="sxs-lookup"><span data-stu-id="1e351-207">Authentication</span></span>

<span data-ttu-id="1e351-208">rutiny prostředí PowerShell hello toouse, je třeba ověřit.</span><span class="sxs-lookup"><span data-stu-id="1e351-208">toouse hello PowerShell cmdlets, you must authenticate.</span></span> <span data-ttu-id="1e351-209">Nakonfigurujte ověřování v aplikaci funkce hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-209">You configure authentication in hello function app.</span></span> <span data-ttu-id="1e351-210">tooconfigure ověřování, je nutné nakonfigurovat proměnné prostředí a nahrát aplikaci funkce toohello šifrovaný soubor klíče.</span><span class="sxs-lookup"><span data-stu-id="1e351-210">tooconfigure authentication, you must configure environment variables and upload an encrypted key file toohello function app.</span></span>

> [!NOTE]
> <span data-ttu-id="1e351-211">Tento scénář obsahuje pouze jeden z příkladů jak tooimplement ověřování s Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="1e351-211">This scenario provides just one example of how tooimplement authentication with Azure Functions.</span></span> <span data-ttu-id="1e351-212">Existují jiné způsoby toodo to.</span><span class="sxs-lookup"><span data-stu-id="1e351-212">There are other ways toodo this.</span></span>

#### <a name="encrypted-credentials"></a><span data-ttu-id="1e351-213">Zašifrované přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="1e351-213">Encrypted credentials</span></span>

<span data-ttu-id="1e351-214">Hello následující skript prostředí PowerShell vytvoří soubor klíče s názvem **PassEncryptKey.key**.</span><span class="sxs-lookup"><span data-stu-id="1e351-214">hello following PowerShell script creates a key file called **PassEncryptKey.key**.</span></span> <span data-ttu-id="1e351-215">Nabízí taky zašifrovaná verze hello hesla, která se dodává.</span><span class="sxs-lookup"><span data-stu-id="1e351-215">It also provides an encrypted version of hello password that's supplied.</span></span> <span data-ttu-id="1e351-216">Toto heslo je hello stejné heslo, která je definována pro hello aplikaci Azure Active Directory, která se používá k ověřování.</span><span class="sxs-lookup"><span data-stu-id="1e351-216">This password is hello same password that is defined for hello Azure Active Directory application that's used for authentication.</span></span>

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

<span data-ttu-id="1e351-217">V hello Editor služby aplikace hello funkce aplikace, vytvořte složku s názvem **klíče** pod **AlertPacketCapturePowerShell**.</span><span class="sxs-lookup"><span data-stu-id="1e351-217">In hello App Service Editor of hello function app, create a folder called **keys** under **AlertPacketCapturePowerShell**.</span></span> <span data-ttu-id="1e351-218">Nahrajte hello **PassEncryptKey.key** soubor, který jste vytvořili v předchozí ukázce prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-218">Then upload hello **PassEncryptKey.key** file that you created in hello previous PowerShell sample.</span></span>

![Funkce klíč][functions8]

### <a name="retrieve-values-for-environment-variables"></a><span data-ttu-id="1e351-220">Načtení hodnoty pro proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="1e351-220">Retrieve values for environment variables</span></span>

<span data-ttu-id="1e351-221">poslední požadavek Hello je tooset až hello proměnné prostředí, které jsou nezbytné tooaccess hello hodnoty pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="1e351-221">hello final requirement is tooset up hello environment variables that are necessary tooaccess hello values for authentication.</span></span> <span data-ttu-id="1e351-222">Hello následující seznam uvádí hello proměnné prostředí, které jsou vytvořené:</span><span class="sxs-lookup"><span data-stu-id="1e351-222">hello following list shows hello environment variables that are created:</span></span>

* <span data-ttu-id="1e351-223">AzureClientID</span><span class="sxs-lookup"><span data-stu-id="1e351-223">AzureClientID</span></span>

* <span data-ttu-id="1e351-224">AzureTenant</span><span class="sxs-lookup"><span data-stu-id="1e351-224">AzureTenant</span></span>

* <span data-ttu-id="1e351-225">AzureCredPassword</span><span class="sxs-lookup"><span data-stu-id="1e351-225">AzureCredPassword</span></span>


#### <a name="azureclientid"></a><span data-ttu-id="1e351-226">AzureClientID</span><span class="sxs-lookup"><span data-stu-id="1e351-226">AzureClientID</span></span>

<span data-ttu-id="1e351-227">ID klienta Hello je hello ID aplikace aplikace v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1e351-227">hello client ID is hello Application ID of an application in Azure Active Directory.</span></span>

1. <span data-ttu-id="1e351-228">Pokud ještě nemáte toouse aplikace, spusťte následující příklad toocreate hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1e351-228">If you don't already have an application toouse, run hello following example toocreate an application.</span></span>

    ```powershell
    $app = New-AzureRmADApplication -DisplayName "ExampleAutomationAccount_MF" -HomePage "https://exampleapp.com" -IdentifierUris "https://exampleapp1.com/ExampleFunctionsAccount" -Password "<same password as defined earlier>"
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    Start-Sleep 15
    New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $app.ApplicationId
    ```

   > [!NOTE]
   > <span data-ttu-id="1e351-229">Hello heslo, které použijete při vytvoření aplikace hello by měla být hello stejné heslo, které jste předtím vytvořili při ukládání souboru klíče hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-229">hello password that you use when creating hello application should be hello same password that you created earlier when saving hello key file.</span></span>

1. <span data-ttu-id="1e351-230">V hello portálu Azure, vyberte **odběry**.</span><span class="sxs-lookup"><span data-stu-id="1e351-230">In hello Azure portal, select **Subscriptions**.</span></span> <span data-ttu-id="1e351-231">Vyberte toouse hello předplatného a pak vyberte **přístup k ovládacímu prvku (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="1e351-231">Select hello subscription toouse, and then select **Access control (IAM)**.</span></span>

    ![Funkce IAM][functions9]

1. <span data-ttu-id="1e351-233">Zvolte účet toouse hello a potom vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="1e351-233">Choose hello account toouse, and then select **Properties**.</span></span> <span data-ttu-id="1e351-234">Zkopírujte hello ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="1e351-234">Copy hello Application ID.</span></span>

    ![ID aplikace funkce][functions10]

#### <a name="azuretenant"></a><span data-ttu-id="1e351-236">AzureTenant</span><span class="sxs-lookup"><span data-stu-id="1e351-236">AzureTenant</span></span>

<span data-ttu-id="1e351-237">Získejte ID klienta hello spuštěním hello následující ukázka prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="1e351-237">Obtain hello tenant ID  by running hello following PowerShell sample:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "<subscriptionName>").TenantId
```

#### <a name="azurecredpassword"></a><span data-ttu-id="1e351-238">AzureCredPassword</span><span class="sxs-lookup"><span data-stu-id="1e351-238">AzureCredPassword</span></span>

<span data-ttu-id="1e351-239">Hello hodnota proměnné prostředí AzureCredPassword hello je hello hodnotu, kterou můžete získat z systémem hello následující ukázka prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1e351-239">hello value of hello AzureCredPassword environment variable is hello value that you get from running hello following PowerShell sample.</span></span> <span data-ttu-id="1e351-240">V tomto příkladu je hello stejný jako ten, které se zobrazí v předchozím hello **šifrovat přihlašovací údaje** části.</span><span class="sxs-lookup"><span data-stu-id="1e351-240">This example is hello same one that's shown in hello preceding **Encrypted credentials** section.</span></span> <span data-ttu-id="1e351-241">Hello hodnotu, která je potřeba je hello výstup hello `$Encryptedpassword` proměnné.</span><span class="sxs-lookup"><span data-stu-id="1e351-241">hello value that's needed is hello output of hello `$Encryptedpassword` variable.</span></span>  <span data-ttu-id="1e351-242">Toto je služba hello hlavní se heslo, které jste zašifrovali pomocí skriptu prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-242">This is hello service principal password that you encrypted by using hello PowerShell script.</span></span>

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

### <a name="store-hello-environment-variables"></a><span data-ttu-id="1e351-243">Proměnné prostředí hello úložiště</span><span class="sxs-lookup"><span data-stu-id="1e351-243">Store hello environment variables</span></span>

1. <span data-ttu-id="1e351-244">Přejděte toohello funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="1e351-244">Go toohello function app.</span></span> <span data-ttu-id="1e351-245">Potom vyberte **funkce nastavení aplikace** > **konfigurovat nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1e351-245">Then select **Function app settings** > **Configure app settings**.</span></span>

    ![Konfigurace nastavení aplikace][functions11]

1. <span data-ttu-id="1e351-247">Přidání proměnné prostředí hello a jejich hodnoty toohello aplikace nastavení a potom vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1e351-247">Add hello environment variables and their values toohello app settings, and then select **Save**.</span></span>

    ![Nastavení aplikace][functions12]

### <a name="add-powershell-toohello-function"></a><span data-ttu-id="1e351-249">Přidání funkce toohello prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1e351-249">Add PowerShell toohello function</span></span>

<span data-ttu-id="1e351-250">Je teď čas toomake volání sledovací proces sítě z v rámci hello Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="1e351-250">It's now time toomake calls into Network Watcher from within hello Azure function.</span></span> <span data-ttu-id="1e351-251">V závislosti na požadavcích hello hello implementací této funkce se může lišit.</span><span class="sxs-lookup"><span data-stu-id="1e351-251">Depending on hello requirements, hello implementation of this function can vary.</span></span> <span data-ttu-id="1e351-252">Obecný tok hello hello kódu však vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="1e351-252">However, hello general flow of hello code is as follows:</span></span>

1. <span data-ttu-id="1e351-253">Proces vstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="1e351-253">Process input parameters.</span></span>
2. <span data-ttu-id="1e351-254">Paket existující dotaz zaznamená tooverify omezení a vyřešit konflikty názvů.</span><span class="sxs-lookup"><span data-stu-id="1e351-254">Query existing packet captures tooverify limits and resolve name conflicts.</span></span>
3. <span data-ttu-id="1e351-255">Vytvořte zachytáváním paketů s příslušnými parametry.</span><span class="sxs-lookup"><span data-stu-id="1e351-255">Create a packet capture with appropriate parameters.</span></span>
4. <span data-ttu-id="1e351-256">Dotazování paketu zachytit pravidelně, dokud nebude dokončeno.</span><span class="sxs-lookup"><span data-stu-id="1e351-256">Poll packet capture periodically until it's complete.</span></span>
5. <span data-ttu-id="1e351-257">Upozorněte uživatele hello, relaci zachytávání paketů hello je dokončena.</span><span class="sxs-lookup"><span data-stu-id="1e351-257">Notify hello user that hello packet capture session is complete.</span></span>

<span data-ttu-id="1e351-258">Hello následující příklad je prostředí PowerShell kód, který můžete použít ve funkci hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-258">hello following example is PowerShell code that can be used in hello function.</span></span> <span data-ttu-id="1e351-259">Existují hodnoty, které je třeba toobe nahrazena pro **subscriptionId**, **resourceGroupName**, a **storageAccountName**.</span><span class="sxs-lookup"><span data-stu-id="1e351-259">There are values that need toobe replaced for **subscriptionId**, **resourceGroupName**, and **storageAccountName**.</span></span>

```powershell
            #Import Azure PowerShell modules required toomake calls tooNetwork Watcher
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Profile\AzureRM.Profile.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Network\AzureRM.Network.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Resources\AzureRM.Resources.psd1" -Global

            #Process alert request body
            $requestBody = Get-Content $req -Raw | ConvertFrom-Json

            #Storage account ID toosave captures in
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


            #Get hello VM that fired hello alert
            if($requestBody.context.resourceType -eq "Microsoft.Compute/virtualMachines")
            {
                Write-Output ("Subscription ID: {0}" -f $requestBody.context.subscriptionId)
                Write-Output ("Resource Group:  {0}" -f $requestBody.context.resourceGroupName)
                Write-Output ("Resource Name:  {0}" -f $requestBody.context.resourceName)
                Write-Output ("Resource Type:  {0}" -f $requestBody.context.resourceType)

                #Get hello Network Watcher in hello VM's region
                $nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $requestBody.context.resourceRegion}
                $networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

                #Get existing packetCaptures
                $packetCaptures = Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher

                #Remove existing packet capture created by hello function (if it exists)
                $packetCaptures | %{if($_.Name -eq $packetCaptureName)
                { 
                    Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName $packetCaptureName
                }}

                #Initiate packet capture on hello VM that fired hello alert
                if ((Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher).Count -lt $packetCaptureLimit){
                    echo "Initiating Packet Capture"
                    New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $requestBody.context.resourceId -PacketCaptureName $packetCaptureName -StorageAccountId $storageaccountid -TimeLimitInSeconds $packetCaptureDuration
                    Out-File -Encoding Ascii -FilePath $res -inputObject "Packet Capture created on ${requestBody.context.resourceID}"
                }
            } 
 ``` 
#### <a name="retrieve-hello-function-url"></a><span data-ttu-id="1e351-260">Načíst URL pro hello – funkce</span><span class="sxs-lookup"><span data-stu-id="1e351-260">Retrieve hello function URL</span></span> 
1. <span data-ttu-id="1e351-261">Po vytvoření funkce, nakonfigurujte vaše adresa URL výstrahy toocall hello, který je spojen s hello funkce.</span><span class="sxs-lookup"><span data-stu-id="1e351-261">After you've created your function, configure your alert toocall hello URL that's associated with hello function.</span></span> <span data-ttu-id="1e351-262">tooget tuto hodnotu, adresa URL funkce hello kopírování z aplikace funkce.</span><span class="sxs-lookup"><span data-stu-id="1e351-262">tooget this value, copy hello function URL from your function app.</span></span>

    ![Hledání URL funkce hello][functions13]

2. <span data-ttu-id="1e351-264">Zkopírujte hello funkce URL pro aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="1e351-264">Copy hello function URL for your function app.</span></span>

    ![Kopírování URL funkce hello][2]

<span data-ttu-id="1e351-266">Pokud budete potřebovat vlastní vlastnosti v hello datovou část požadavku POST webhooku hello, podívejte se příliš[konfigurace webhook, jehož na výstrahu Azure metriky](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="1e351-266">If you require custom properties in hello payload of hello webhook POST request, refer too[Configure a webhook on an Azure metric alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

## <a name="configure-an-alert-on-a-vm"></a><span data-ttu-id="1e351-267">Konfigurace výstrahy na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="1e351-267">Configure an alert on a VM</span></span>

<span data-ttu-id="1e351-268">Výstrahy můžou být nakonfigurované toonotify jednotlivce, když konkrétní metrika překračuje prahovou hodnotu, která je přiřazena tooit.</span><span class="sxs-lookup"><span data-stu-id="1e351-268">Alerts can be configured toonotify individuals when a specific metric crosses a threshold that's assigned tooit.</span></span> <span data-ttu-id="1e351-269">V tomto příkladu je výstraha hello na hello segmentů TCP, které se odesílají, ale hello může být pro výstraha mnoho jiné metriky.</span><span class="sxs-lookup"><span data-stu-id="1e351-269">In this example, hello alert is on hello TCP segments that are sent, but hello alert can be triggered for many other metrics.</span></span> <span data-ttu-id="1e351-270">Výstraha v tomto příkladu je nakonfigurované toocall funkci webhooku toocall hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-270">In this example, an alert is configured toocall a webhook toocall hello function.</span></span>

### <a name="create-hello-alert-rule"></a><span data-ttu-id="1e351-271">Vytvořit pravidlo výstrahy hello</span><span class="sxs-lookup"><span data-stu-id="1e351-271">Create hello alert rule</span></span>

<span data-ttu-id="1e351-272">Přejděte tooan existující virtuální počítač a poté přidejte pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="1e351-272">Go tooan existing virtual machine, and then add an alert rule.</span></span> <span data-ttu-id="1e351-273">Podrobnější dokumentaci týkající se konfigurace výstrahy naleznete na [vytvoření výstrahy v Azure monitorování pro služby Azure – portál Azure](../monitoring-and-diagnostics/insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1e351-273">More detailed documentation about configuring alerts can be found at [Create alerts in Azure Monitor for Azure services - Azure portal](../monitoring-and-diagnostics/insights-alerts-portal.md).</span></span> <span data-ttu-id="1e351-274">Zadejte následující hodnoty v hello hello **pravidlo výstrahy** a pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="1e351-274">Enter hello following values in hello **Alert rule** blade, and then select **OK**.</span></span>

  |<span data-ttu-id="1e351-275">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="1e351-275">**Setting**</span></span> | <span data-ttu-id="1e351-276">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="1e351-276">**Value**</span></span> | <span data-ttu-id="1e351-277">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="1e351-277">**Details**</span></span> |
  |---|---|---|
  |<span data-ttu-id="1e351-278">**Název**</span><span class="sxs-lookup"><span data-stu-id="1e351-278">**Name**</span></span>|<span data-ttu-id="1e351-279">TCP_Segments_Sent_Exceeded</span><span class="sxs-lookup"><span data-stu-id="1e351-279">TCP_Segments_Sent_Exceeded</span></span>|<span data-ttu-id="1e351-280">Název pravidla výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-280">Name of hello alert rule.</span></span>|
  |<span data-ttu-id="1e351-281">**Popis**</span><span class="sxs-lookup"><span data-stu-id="1e351-281">**Description**</span></span>|<span data-ttu-id="1e351-282">Segmentů TCP odeslané překročil prahovou hodnotu</span><span class="sxs-lookup"><span data-stu-id="1e351-282">TCP segments sent exceeded threshold</span></span>|<span data-ttu-id="1e351-283">Hello popis pro pravidlo výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-283">hello description for hello alert rule.</span></span>||
  |<span data-ttu-id="1e351-284">**Metrika**</span><span class="sxs-lookup"><span data-stu-id="1e351-284">**Metric**</span></span>|<span data-ttu-id="1e351-285">Odeslané segmenty TCP</span><span class="sxs-lookup"><span data-stu-id="1e351-285">TCP segments sent</span></span>| <span data-ttu-id="1e351-286">Hello metriky toouse tootrigger hello výstraha.</span><span class="sxs-lookup"><span data-stu-id="1e351-286">hello metric toouse tootrigger hello alert.</span></span> |
  |<span data-ttu-id="1e351-287">**Podmínka**</span><span class="sxs-lookup"><span data-stu-id="1e351-287">**Condition**</span></span>|<span data-ttu-id="1e351-288">Více než</span><span class="sxs-lookup"><span data-stu-id="1e351-288">Greater than</span></span>| <span data-ttu-id="1e351-289">Hello podmínku toouse při vyhodnocování metrika hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-289">hello condition toouse when evaluating hello metric.</span></span>|
  |<span data-ttu-id="1e351-290">**Prahová hodnota**</span><span class="sxs-lookup"><span data-stu-id="1e351-290">**Threshold**</span></span>|<span data-ttu-id="1e351-291">100</span><span class="sxs-lookup"><span data-stu-id="1e351-291">100</span></span>| <span data-ttu-id="1e351-292">Hodnota Hello hello metriku, která aktivuje výstrahu hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-292">hello  value of hello metric that triggers hello alert.</span></span> <span data-ttu-id="1e351-293">Tato hodnota je třeba nastavit tooa platnou hodnotu pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="1e351-293">This value should be set tooa valid value for your environment.</span></span>|
  |<span data-ttu-id="1e351-294">**Období**</span><span class="sxs-lookup"><span data-stu-id="1e351-294">**Period**</span></span>|<span data-ttu-id="1e351-295">Přes hello posledních pět minut</span><span class="sxs-lookup"><span data-stu-id="1e351-295">Over hello last five minutes</span></span>| <span data-ttu-id="1e351-296">Určuje interval hello v které toolook pro hello prahovou hodnotu na metrika hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-296">Determines hello period in which toolook for hello threshold on hello metric.</span></span>|
  |<span data-ttu-id="1e351-297">**Webhooku**</span><span class="sxs-lookup"><span data-stu-id="1e351-297">**Webhook**</span></span>|<span data-ttu-id="1e351-298">[URL webhooku se nenačetla z funkce aplikace]</span><span class="sxs-lookup"><span data-stu-id="1e351-298">[webhook URL from function app]</span></span>| <span data-ttu-id="1e351-299">Adresa URL webhooku Hello z hello funkce aplikace, který byl vytvořen v předchozích krocích hello.</span><span class="sxs-lookup"><span data-stu-id="1e351-299">hello webhook URL from hello function app that was created in hello previous steps.</span></span>|

> [!NOTE]
> <span data-ttu-id="1e351-300">Metrika segmentů TCP Hello není povoleno ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="1e351-300">hello TCP segments metric is not enabled by default.</span></span> <span data-ttu-id="1e351-301">Další informace o další metriky tooenable navštivte stránky [zapínání monitorování a Diagnostika](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="1e351-301">Learn more about how tooenable additional metrics by visiting [Enable monitoring and diagnostics](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).</span></span>

## <a name="review-hello-results"></a><span data-ttu-id="1e351-302">Zkontrolujte výsledky hello</span><span class="sxs-lookup"><span data-stu-id="1e351-302">Review hello results</span></span>

<span data-ttu-id="1e351-303">Po hello kritéria pro výstrahy aktivační události hello vytvoří se zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="1e351-303">After hello criteria for hello alert triggers, a packet capture is created.</span></span> <span data-ttu-id="1e351-304">Přejděte tooNetwork sledovacích procesů a poté vyberte **zachytáváním paketů**.</span><span class="sxs-lookup"><span data-stu-id="1e351-304">Go tooNetwork Watcher, and then select **Packet capture**.</span></span> <span data-ttu-id="1e351-305">Na této stránce můžete vybrat hello paketu zachycení soubor odkaz toodownload hello zachytáváním paketů.</span><span class="sxs-lookup"><span data-stu-id="1e351-305">On this page, you can select hello packet capture file link toodownload hello packet capture.</span></span>

![Zobrazení zachytávání paketů][functions14]

<span data-ttu-id="1e351-307">Pokud soubor zachycení hello je uložený místně, můžete ji načíst přihlášením toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1e351-307">If hello capture file is stored locally, you can retrieve it by signing in toohello virtual machine.</span></span>

<span data-ttu-id="1e351-308">Pokyny týkající se stahování souborů z účtů úložiště Azure najdete v tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="1e351-308">For instructions about downloading files from Azure storage accounts, see [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="1e351-309">Jiný nástroj, můžete je [Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="1e351-309">Another tool you can use is [Storage Explorer](http://storageexplorer.com/).</span></span>

<span data-ttu-id="1e351-310">Po stažení vaší zachycení, můžete ji zobrazit pomocí libovolného nástroje, který může číst **CAP** souboru.</span><span class="sxs-lookup"><span data-stu-id="1e351-310">After your capture has been downloaded, you can view it by using any tool that can read a **.cap** file.</span></span> <span data-ttu-id="1e351-311">Tady jsou odkazy tootwo tyto nástroje:</span><span class="sxs-lookup"><span data-stu-id="1e351-311">Following are links tootwo of these tools:</span></span>

- [<span data-ttu-id="1e351-312">Microsoft Message Analyzer</span><span class="sxs-lookup"><span data-stu-id="1e351-312">Microsoft Message Analyzer</span></span>](https://technet.microsoft.com/library/jj649776.aspx)
- [<span data-ttu-id="1e351-313">WireShark</span><span class="sxs-lookup"><span data-stu-id="1e351-313">WireShark</span></span>](https://www.wireshark.org/)

## <a name="next-steps"></a><span data-ttu-id="1e351-314">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e351-314">Next steps</span></span>

<span data-ttu-id="1e351-315">Zjistěte, jak tooview vaše paketu zaznamená navštivte stránky [analýza zachytávání paketů s Wireshark](network-watcher-deep-packet-inspection.md).</span><span class="sxs-lookup"><span data-stu-id="1e351-315">Learn how tooview your packet captures by visiting [Packet capture analysis with Wireshark](network-watcher-deep-packet-inspection.md).</span></span>


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
