---
title: "Integrovat protokoly z Azure Key Vault pomocí služby Event Hubs | Microsoft Docs"
description: "Kurz, který poskytuje nezbytných kroků k zpřístupnit protokoly Key Vault server SIEM s využitím protokolu integrace se službou Azure"
services: security
author: barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.topic: article
ms.date: 08/07/2017
ms.author: Barclayn
ms.custom: AzLog
ms.openlocfilehash: 3cd80817bf8b2ef2f66e9942eddc186a3eb5b5e4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-log-integration-tutorial-process-azure-key-vault-events-by-using-event-hubs"></a><span data-ttu-id="0895d-103">Kurz pro Azure integrace protokolu: proces Azure Key Vault událostí pomocí služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="0895d-103">Azure Log Integration tutorial: Process Azure Key Vault events by using Event Hubs</span></span>

<span data-ttu-id="0895d-104">Integrace se službou protokolu Azure slouží k načtení protokolované události a zpřístupněte je váš systém zabezpečení informací a událostí správy (SIEM).</span><span class="sxs-lookup"><span data-stu-id="0895d-104">You can use Azure Log Integration to retrieve logged events and make them available to your security information and event management (SIEM) system.</span></span> <span data-ttu-id="0895d-105">Tento kurz ukazuje příklad použití Azure integrace protokolu ke zpracování protokoly, které jsou získány prostřednictvím Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="0895d-105">This tutorial shows an example of how Azure Log Integration can be used to process logs that are acquired through Azure Event Hubs.</span></span>
 
<span data-ttu-id="0895d-106">Tento kurz použijte k se seznámit s jak integrace se službou protokolu Azure a Event Hubs spolupracují podle následujících kroků příklad a pochopíte, jak každý krok podporuje řešení.</span><span class="sxs-lookup"><span data-stu-id="0895d-106">Use this tutorial to get acquainted with how Azure Log Integration and Event Hubs work together by following the example steps and understanding how each step supports the solution.</span></span> <span data-ttu-id="0895d-107">Potom můžete využít, co jste se naučili tady vytvořit vlastní postup pro podporu jedinečným požadavkům vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="0895d-107">Then you can take what you’ve learned here to create your own steps to support your company’s unique requirements.</span></span>

>[!WARNING]
<span data-ttu-id="0895d-108">Kroky a příkazy v tomto kurzu nejsou určeny k kopírovat a vkládat data.</span><span class="sxs-lookup"><span data-stu-id="0895d-108">The steps and commands in this tutorial are not intended to be copied and pasted.</span></span> <span data-ttu-id="0895d-109">Jsou jenom příklady.</span><span class="sxs-lookup"><span data-stu-id="0895d-109">They're examples only.</span></span> <span data-ttu-id="0895d-110">Nepoužívejte příkazy prostředí PowerShell "tak, jak je ve vašem prostředí za provozu.</span><span class="sxs-lookup"><span data-stu-id="0895d-110">Do not use the PowerShell commands “as is” in your live environment.</span></span> <span data-ttu-id="0895d-111">Je nutné přizpůsobit podle vašeho prostředí jedinečné.</span><span class="sxs-lookup"><span data-stu-id="0895d-111">You must customize them based on your unique environment.</span></span>


<span data-ttu-id="0895d-112">Tento kurz vás provede procesem vezme zaprotokolované do centra událostí Azure Key Vault činnosti a zpřístupnit ho jako soubory JSON k vašemu systému SIEM.</span><span class="sxs-lookup"><span data-stu-id="0895d-112">This tutorial walks you through the process of taking Azure Key Vault activity logged to an event hub and making it available as JSON files to your SIEM system.</span></span> <span data-ttu-id="0895d-113">Potom můžete nakonfigurovat systém SIEM ke zpracování souborů JSON.</span><span class="sxs-lookup"><span data-stu-id="0895d-113">You can then configure your SIEM system to process the JSON files.</span></span>

>[!NOTE]
><span data-ttu-id="0895d-114">Většina kroky v tomto kurzu je konfigurace trezorů klíčů, účty úložiště a event hubs.</span><span class="sxs-lookup"><span data-stu-id="0895d-114">Most of the steps in this tutorial involve configuring key vaults, storage accounts, and event hubs.</span></span> <span data-ttu-id="0895d-115">Konkrétní postup integrace se službou Azure protokolu je na konci tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0895d-115">The specific Azure Log Integration steps are at the end of this tutorial.</span></span> <span data-ttu-id="0895d-116">Neprovádějte kroky v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0895d-116">Do not perform these steps in a production environment.</span></span> <span data-ttu-id="0895d-117">Ty jsou určené pro prostředí laboratoře.</span><span class="sxs-lookup"><span data-stu-id="0895d-117">They are intended for a lab environment only.</span></span> <span data-ttu-id="0895d-118">Je nutné přizpůsobit kroky před jejich používání v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0895d-118">You must customize the steps before using them in production.</span></span>

<span data-ttu-id="0895d-119">Informace uvedené na této cestě vám pomůže pochopit důvodech každý krok.</span><span class="sxs-lookup"><span data-stu-id="0895d-119">Information provided along the way helps you understand the reasons behind each step.</span></span> <span data-ttu-id="0895d-120">Odkazy na další články poskytují další podrobnosti na určité témata.</span><span class="sxs-lookup"><span data-stu-id="0895d-120">Links to other articles give you more detail on certain topics.</span></span>

<span data-ttu-id="0895d-121">Další informace o službách, které uvádí tento kurz najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="0895d-121">For more information about the services that this tutorial mentions, see:</span></span> 

- [<span data-ttu-id="0895d-122">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0895d-122">Azure Key Vault</span></span>](../key-vault/key-vault-whatis.md)
- [<span data-ttu-id="0895d-123">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="0895d-123">Azure Event Hubs</span></span>](../event-hubs/event-hubs-what-is-event-hubs.md)
- [<span data-ttu-id="0895d-124">Integrace protokolů Azure</span><span class="sxs-lookup"><span data-stu-id="0895d-124">Azure Log Integration</span></span>](security-azure-log-integration-overview.md)


## <a name="initial-setup"></a><span data-ttu-id="0895d-125">Počáteční nastavení</span><span class="sxs-lookup"><span data-stu-id="0895d-125">Initial setup</span></span>

<span data-ttu-id="0895d-126">Před provedením kroků v tomto článku, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="0895d-126">Before you can complete the steps in this article, you need the following:</span></span>

1. <span data-ttu-id="0895d-127">Předplatné Azure a účet v tomto předplatném s právy správce.</span><span class="sxs-lookup"><span data-stu-id="0895d-127">An Azure subscription and account on that subscription with administrator rights.</span></span> <span data-ttu-id="0895d-128">Pokud nemáte předplatné, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="0895d-128">If you don't have a subscription, you can create a [free account](https://azure.microsoft.com/free/).</span></span>
 
2. <span data-ttu-id="0895d-129">Systém s přístupem k Internetu, který splňuje požadavky na instalaci protokolu integrace se službou Azure.</span><span class="sxs-lookup"><span data-stu-id="0895d-129">A system with access to the internet that meets the requirements for installing Azure Log Integration.</span></span> <span data-ttu-id="0895d-130">V systému může být v cloudové službě nebo hostovaný místně.</span><span class="sxs-lookup"><span data-stu-id="0895d-130">The system can be on a cloud service or hosted on-premises.</span></span>

3. <span data-ttu-id="0895d-131">[Integrace se službou Azure protokolu](https://www.microsoft.com/download/details.aspx?id=53324) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="0895d-131">[Azure Log Integration](https://www.microsoft.com/download/details.aspx?id=53324) installed.</span></span> <span data-ttu-id="0895d-132">K její instalaci:</span><span class="sxs-lookup"><span data-stu-id="0895d-132">To install it:</span></span>

   <span data-ttu-id="0895d-133">a.</span><span class="sxs-lookup"><span data-stu-id="0895d-133">a.</span></span> <span data-ttu-id="0895d-134">Pomocí vzdálené plochy pro připojení k systému uveden v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="0895d-134">Use Remote Desktop to connect to the system mentioned in step 2.</span></span>   
   <span data-ttu-id="0895d-135">b.</span><span class="sxs-lookup"><span data-stu-id="0895d-135">b.</span></span> <span data-ttu-id="0895d-136">Zkopírujte instalační službu Azure protokolu integrace do systému.</span><span class="sxs-lookup"><span data-stu-id="0895d-136">Copy the Azure Log Integration installer to the system.</span></span> <span data-ttu-id="0895d-137">Můžete [stáhnout instalační soubory](https://www.microsoft.com/download/details.aspx?id=53324).</span><span class="sxs-lookup"><span data-stu-id="0895d-137">You can [download the installation files](https://www.microsoft.com/download/details.aspx?id=53324).</span></span>   
   <span data-ttu-id="0895d-138">c.</span><span class="sxs-lookup"><span data-stu-id="0895d-138">c.</span></span> <span data-ttu-id="0895d-139">Spusťte instalační program a přijměte licenční podmínky pro Software společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0895d-139">Start the installer and accept the Microsoft Software License Terms.</span></span>   
   <span data-ttu-id="0895d-140">d.</span><span class="sxs-lookup"><span data-stu-id="0895d-140">d.</span></span> <span data-ttu-id="0895d-141">Pokud bude poskytovat informace telemetrie, nechte zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="0895d-141">If you will provide telemetry information, leave the check box selected.</span></span> <span data-ttu-id="0895d-142">Pokud místo by není odeslat informace o využití do Microsoftu, zrušte zaškrtnutí políčka.</span><span class="sxs-lookup"><span data-stu-id="0895d-142">If you'd rather not send usage information to Microsoft, clear the check box.</span></span>
   
   <span data-ttu-id="0895d-143">Další informace o protokolu integrace se službou Azure a jak ji nainstalovat, najdete v části [integrace protokolu Azure s Azure Diagnostics protokolování a předávání událostí systému Windows](security-azure-log-integration-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0895d-143">For more information about Azure Log Integration and how to install it, see [Azure Log Integration with Azure Diagnostics logging and Windows Event Forwarding](security-azure-log-integration-get-started.md).</span></span>

4. <span data-ttu-id="0895d-144">Nejnovější verze prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0895d-144">The latest PowerShell version.</span></span>
 
   <span data-ttu-id="0895d-145">Pokud máte nainstalovaný Windows Server 2016, pak máte alespoň PowerShell 5.0.</span><span class="sxs-lookup"><span data-stu-id="0895d-145">If you have Windows Server 2016 installed, then you have at least PowerShell 5.0.</span></span> <span data-ttu-id="0895d-146">Pokud používáte jinou verzi systému Windows Server, bude pravděpodobně starší verzi prostředí PowerShell nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="0895d-146">If you're using any other version of Windows Server, you might have an earlier version of PowerShell installed.</span></span> <span data-ttu-id="0895d-147">Verze můžete zkontrolovat tak, že zadáte ```get-host``` v okně prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0895d-147">You can check the version by entering ```get-host``` in a PowerShell window.</span></span> <span data-ttu-id="0895d-148">Pokud nemáte nainstalovaný PowerShell 5.0, můžete [stažení](https://www.microsoft.com/download/details.aspx?id=50395).</span><span class="sxs-lookup"><span data-stu-id="0895d-148">If you don't have PowerShell 5.0 installed, you can [download it](https://www.microsoft.com/download/details.aspx?id=50395).</span></span>

   <span data-ttu-id="0895d-149">Až budete mít alespoň PowerShell 5.0, můžete přejít k instalaci nejnovější verze:</span><span class="sxs-lookup"><span data-stu-id="0895d-149">After you have at least PowerShell 5.0, you can proceed to install the latest version:</span></span>
   
   <span data-ttu-id="0895d-150">a.</span><span class="sxs-lookup"><span data-stu-id="0895d-150">a.</span></span> <span data-ttu-id="0895d-151">V okně prostředí PowerShell, zadejte ```Install-Module Azure``` příkaz.</span><span class="sxs-lookup"><span data-stu-id="0895d-151">In a PowerShell window, enter the ```Install-Module Azure``` command.</span></span> <span data-ttu-id="0895d-152">Kroky instalace.</span><span class="sxs-lookup"><span data-stu-id="0895d-152">Complete the installation steps.</span></span>    
   <span data-ttu-id="0895d-153">b.</span><span class="sxs-lookup"><span data-stu-id="0895d-153">b.</span></span> <span data-ttu-id="0895d-154">Zadejte ```Install-Module AzureRM``` příkaz.</span><span class="sxs-lookup"><span data-stu-id="0895d-154">Enter the ```Install-Module AzureRM``` command.</span></span> <span data-ttu-id="0895d-155">Kroky instalace.</span><span class="sxs-lookup"><span data-stu-id="0895d-155">Complete the installation steps.</span></span>

   <span data-ttu-id="0895d-156">Další informace najdete v tématu [nainstalovat Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span><span class="sxs-lookup"><span data-stu-id="0895d-156">For more information, see [Install Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span></span>


## <a name="create-supporting-infrastructure-elements"></a><span data-ttu-id="0895d-157">Vytváření podpůrné prvků infrastruktury</span><span class="sxs-lookup"><span data-stu-id="0895d-157">Create supporting infrastructure elements</span></span>

1. <span data-ttu-id="0895d-158">Otevřete okno prostředí PowerShell se zvýšenými oprávněními a přejděte na **C:\Program Files\Microsoft Azure protokolu integrace**.</span><span class="sxs-lookup"><span data-stu-id="0895d-158">Open an elevated PowerShell window and go to **C:\Program Files\Microsoft Azure Log Integration**.</span></span>
2. <span data-ttu-id="0895d-159">Importujte rutiny AzLog spuštěním skriptu LoadAzLogModule.ps1.</span><span class="sxs-lookup"><span data-stu-id="0895d-159">Import the AzLog cmdlets by running the script LoadAzLogModule.ps1.</span></span> <span data-ttu-id="0895d-160">Zadejte `.\LoadAzLogModule.ps1` příkaz.</span><span class="sxs-lookup"><span data-stu-id="0895d-160">Enter the `.\LoadAzLogModule.ps1` command.</span></span> <span data-ttu-id="0895d-161">(Všimněte si ". \" v tomto příkazu.) Mělo by se vám zobrazit přibližně toto:</span><span class="sxs-lookup"><span data-stu-id="0895d-161">(Notice the “.\” in that command.) You should see something like this:</span></span></br>

   ![Seznam načtených modulů](./media/security-azure-log-integration-keyvault-eventhub/loaded-modules.png)

3. <span data-ttu-id="0895d-163">Zadejte `Login-AzureRmAccount` příkaz.</span><span class="sxs-lookup"><span data-stu-id="0895d-163">Enter the `Login-AzureRmAccount` command.</span></span> <span data-ttu-id="0895d-164">V okně přihlášení zadejte přihlašovací údaje pro předplatné, které budete používat pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0895d-164">In the login window, enter the credential information for the subscription that you will use for this tutorial.</span></span>

   >[!NOTE]
   ><span data-ttu-id="0895d-165">Pokud je poprvé, který jste protokolování Azure z tohoto počítače, zobrazí se zpráva o povolení Microsoft ke shromažďování dat o použití prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0895d-165">If this is the first time that you're logging in to Azure from this machine, you will see a message about allowing Microsoft to collect PowerShell usage data.</span></span> <span data-ttu-id="0895d-166">Doporučujeme, abyste povolili tuto kolekci dat, protože se použije ke zlepšení prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0895d-166">We recommend that you enable this data collection because it will be used to improve Azure PowerShell.</span></span>

4. <span data-ttu-id="0895d-167">Po úspěšném ověření jste přihlášeni a přečtěte si informace na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="0895d-167">After successful authentication, you're logged in and you see the information in the following screenshot.</span></span> <span data-ttu-id="0895d-168">Poznamenejte si ID předplatného a název odběru, protože je k dokončení následujících krocích budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="0895d-168">Take note of the subscription ID and subscription name, because you'll need them to complete later steps.</span></span>

   ![Okno prostředí PowerShell](./media/security-azure-log-integration-keyvault-eventhub/login-azurermaccount.png)
5. <span data-ttu-id="0895d-170">Vytváření proměnných pro uložení hodnot, které se použijí později.</span><span class="sxs-lookup"><span data-stu-id="0895d-170">Create variables to store values that will be used later.</span></span> <span data-ttu-id="0895d-171">Zadejte, každý z následujících řádků prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0895d-171">Enter each of the following PowerShell lines.</span></span> <span data-ttu-id="0895d-172">Možná budete muset upravit hodnoty tak, aby odpovídaly vašemu prostředí.</span><span class="sxs-lookup"><span data-stu-id="0895d-172">You might need to adjust the values to match your environment.</span></span>
    - <span data-ttu-id="0895d-173">```$subscriptionName = ‘Visual Studio Ultimate with MSDN’```(Název odběru mohou lišit.</span><span class="sxs-lookup"><span data-stu-id="0895d-173">```$subscriptionName = ‘Visual Studio Ultimate with MSDN’``` (Your subscription name might be different.</span></span> <span data-ttu-id="0895d-174">Zobrazí se v rámci výstupu předchozí příkaz.)</span><span class="sxs-lookup"><span data-stu-id="0895d-174">You can see it as part of the output of the previous command.)</span></span>
    - <span data-ttu-id="0895d-175">```$location = 'West US'```(Tato proměnná se použije k předání umístění, kde by měl být vytvořen prostředky.</span><span class="sxs-lookup"><span data-stu-id="0895d-175">```$location = 'West US'``` (This variable will be used to pass the location where resources should be created.</span></span> <span data-ttu-id="0895d-176">Můžete změnit tuto proměnnou na libovolné místo dle vlastního výběru.)</span><span class="sxs-lookup"><span data-stu-id="0895d-176">You can change this variable to be any location of your choosing.)</span></span>
    - ```$random = Get-Random```
    - <span data-ttu-id="0895d-177">``` $name = 'azlogtest' + $random```(Název může být nic, ale měl by obsahovat pouze malá písmena a číslice.)</span><span class="sxs-lookup"><span data-stu-id="0895d-177">``` $name = 'azlogtest' + $random``` (The name can be anything, but it should include only lowercase letters and numbers.)</span></span>
    - <span data-ttu-id="0895d-178">``` $storageName = $name```(Tato proměnná se použije pro název účtu úložiště.)</span><span class="sxs-lookup"><span data-stu-id="0895d-178">``` $storageName = $name``` (This variable will be used for the storage account name.)</span></span>
    - <span data-ttu-id="0895d-179">```$rgname = $name ```(Tato proměnná se použije pro název skupiny prostředků.)</span><span class="sxs-lookup"><span data-stu-id="0895d-179">```$rgname = $name ``` (This variable will be used for the resource group name.)</span></span>
    - <span data-ttu-id="0895d-180">``` $eventHubNameSpaceName = $name```(To je název oboru názvů centra událostí.)</span><span class="sxs-lookup"><span data-stu-id="0895d-180">``` $eventHubNameSpaceName = $name``` (This is the name of the event hub namespace.)</span></span>
6. <span data-ttu-id="0895d-181">Určete předplatné, budete pracovat s:</span><span class="sxs-lookup"><span data-stu-id="0895d-181">Specify the subscription that you will be working with:</span></span>
    
    ```Select-AzureRmSubscription -SubscriptionName $subscriptionName```
7. <span data-ttu-id="0895d-182">Vytvořte skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="0895d-182">Create a resource group:</span></span>
    
    ```$rg = New-AzureRmResourceGroup -Name $rgname -Location $location```
    
   <span data-ttu-id="0895d-183">Pokud zadáte `$rg` v tomto okamžiku byste měli vidět výstup podobný na tomto snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="0895d-183">If you enter `$rg` at this point, you should see output similar to this screenshot:</span></span>

   ![Výstup po vytvoření skupiny prostředků](./media/security-azure-log-integration-keyvault-eventhub/create-rg.png)
8. <span data-ttu-id="0895d-185">Vytvořte účet úložiště, který se použije ke sledování informací o stavu:</span><span class="sxs-lookup"><span data-stu-id="0895d-185">Create a storage account that will be used to keep track of state information:</span></span>
    
    ```$storage = New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename -Location $location -SkuName Standard_LRS```
9. <span data-ttu-id="0895d-186">Vytvoření oboru názvů centra událostí.</span><span class="sxs-lookup"><span data-stu-id="0895d-186">Create the event hub namespace.</span></span> <span data-ttu-id="0895d-187">To je nutné k vytváření centra událostí.</span><span class="sxs-lookup"><span data-stu-id="0895d-187">This is required to create an event hub.</span></span>
    
    ```$eventHubNameSpace = New-AzureRmEventHubNamespace -ResourceGroupName $rgname -NamespaceName $eventHubnamespaceName -Location $location```
10. <span data-ttu-id="0895d-188">Získání ID pravidlo, které budou použity s poskytovateli statistiky:</span><span class="sxs-lookup"><span data-stu-id="0895d-188">Get the rule ID that will be used with the insights provider:</span></span>
    
    ```$sbruleid = $eventHubNameSpace.Id +'/authorizationrules/RootManageSharedAccessKey' ```
11. <span data-ttu-id="0895d-189">Získat všechny možné Azure umístění a přidat názvy do proměnné, která lze použít v pozdějším kroku:</span><span class="sxs-lookup"><span data-stu-id="0895d-189">Get all possible Azure locations and add the names to a variable that can be used in a later step:</span></span>
    
    <span data-ttu-id="0895d-190">a.</span><span class="sxs-lookup"><span data-stu-id="0895d-190">a.</span></span> ```$locationObjects = Get-AzureRMLocation```    
    <span data-ttu-id="0895d-191">b.</span><span class="sxs-lookup"><span data-stu-id="0895d-191">b.</span></span> ```$locations = @('global') + $locationobjects.location```
    
    <span data-ttu-id="0895d-192">Pokud zadáte `$locations` v tomto okamžiku zobrazí názvy umístění bez dalších informací vrácený Get-AzureRmLocation.</span><span class="sxs-lookup"><span data-stu-id="0895d-192">If you enter `$locations` at this point, you see the location names without the additional information returned by Get-AzureRmLocation.</span></span>
12. <span data-ttu-id="0895d-193">Vytvoření profilu protokolu správce Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="0895d-193">Create an Azure Resource Manager log profile:</span></span> 
    
    ```Add-AzureRmLogProfile -Name $name -ServiceBusRuleId $sbruleid -Locations $locations```
    
    <span data-ttu-id="0895d-194">Další informace o profilu protokolů Azure najdete v tématu [přehled protokol činnosti Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="0895d-194">For more information about the Azure log profile, see [Overview of the Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).</span></span>

> [!NOTE]
> <span data-ttu-id="0895d-195">Při pokusu o vytvoření profilu protokolu, může se zobrazí chybové hlášení.</span><span class="sxs-lookup"><span data-stu-id="0895d-195">You might get an error message when you try to create a log profile.</span></span> <span data-ttu-id="0895d-196">Potom můžete zkontrolovat v dokumentaci pro Get-AzureRmLogProfile a AzureRmLogProfile odebrat.</span><span class="sxs-lookup"><span data-stu-id="0895d-196">You can then review the documentation for Get-AzureRmLogProfile and Remove-AzureRmLogProfile.</span></span> <span data-ttu-id="0895d-197">Pokud spustíte Get-AzureRmLogProfile, zobrazí informace o profilu protokolu.</span><span class="sxs-lookup"><span data-stu-id="0895d-197">If you run Get-AzureRmLogProfile, you see information about the log profile.</span></span> <span data-ttu-id="0895d-198">Můžete odstranit stávající profil protokolu zadáním ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` příkaz.</span><span class="sxs-lookup"><span data-stu-id="0895d-198">You can delete the existing log profile by entering the ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` command.</span></span>
>
>![Správce prostředků profilu došlo k chybě](./media/security-azure-log-integration-keyvault-eventhub/rm-profile-error.png)

## <a name="create-a-key-vault"></a><span data-ttu-id="0895d-200">Vytvořte trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="0895d-200">Create a key vault</span></span>

1. <span data-ttu-id="0895d-201">Vytvoření trezoru klíčů:</span><span class="sxs-lookup"><span data-stu-id="0895d-201">Create the key vault:</span></span>

   ```$kv = New-AzureRmKeyVault -VaultName $name -ResourceGroupName $rgname -Location $location ```

2. <span data-ttu-id="0895d-202">Konfigurace protokolování pro key vault:</span><span class="sxs-lookup"><span data-stu-id="0895d-202">Configure logging for the key vault:</span></span>

   ```Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -ServiceBusRuleId $sbruleid -Enabled $true ```

## <a name="generate-log-activity"></a><span data-ttu-id="0895d-203">Generovat aktivitu protokolu</span><span class="sxs-lookup"><span data-stu-id="0895d-203">Generate log activity</span></span>

<span data-ttu-id="0895d-204">Požadavky nutné k odeslání do Key Vault pro generování aktivity protokolu.</span><span class="sxs-lookup"><span data-stu-id="0895d-204">Requests need to be sent to Key Vault to generate log activity.</span></span> <span data-ttu-id="0895d-205">Akce, jako generování klíčů, ukládání tajné klíče, nebo čtení tajných klíčů z Key Vault vytvoří položky protokolu.</span><span class="sxs-lookup"><span data-stu-id="0895d-205">Actions like key generation, storing secrets, or reading secrets from Key Vault will create log entries.</span></span>

1. <span data-ttu-id="0895d-206">Zobrazí aktuální klíče k úložišti:</span><span class="sxs-lookup"><span data-stu-id="0895d-206">Display the current storage keys:</span></span>
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
2. <span data-ttu-id="0895d-207">Generovat nové **key2**:</span><span class="sxs-lookup"><span data-stu-id="0895d-207">Generate a new **key2**:</span></span>
    
   ```New-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname -KeyName key2```
3. <span data-ttu-id="0895d-208">Nezobrazovat klíče, zjistíte, že **key2** obsahuje jiné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="0895d-208">Display the keys again and see that **key2** holds a different value:</span></span>
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
4. <span data-ttu-id="0895d-209">Nastavení a čtení tajný klíč pro generování položky další protokolu:</span><span class="sxs-lookup"><span data-stu-id="0895d-209">Set and read a secret to generate additional log entries:</span></span>
    
   <span data-ttu-id="0895d-210">a.</span><span class="sxs-lookup"><span data-stu-id="0895d-210">a.</span></span> <span data-ttu-id="0895d-211">```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b.</span><span class="sxs-lookup"><span data-stu-id="0895d-211">```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b.</span></span> ```(Get-AzureKeyVaultSecret -VaultName $name -Name TestSecret).SecretValueText```

   ![Vrátí tajné](./media/security-azure-log-integration-keyvault-eventhub/keyvaultsecret.png)


## <a name="configure-azure-log-integration"></a><span data-ttu-id="0895d-213">Konfigurace integrace protokolů Azure</span><span class="sxs-lookup"><span data-stu-id="0895d-213">Configure Azure Log Integration</span></span>

<span data-ttu-id="0895d-214">Teď, když jste nakonfigurovali všechny požadované prvky mít Key Vault protokolování do centra událostí, je třeba nakonfigurovat integraci Azure protokolu:</span><span class="sxs-lookup"><span data-stu-id="0895d-214">Now that you have configured all the required elements to have Key Vault logging to an event hub, you need to configure Azure Log Integration:</span></span>

1. ```$storage = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename```
2. ```$eventHubKey = Get-AzureRmEventHubNamespaceKey -ResourceGroupName $rgname -NamespaceName $eventHubNamespace.name -AuthorizationRuleName RootManageSharedAccessKey```
3. ```$storagekeys = Get-AzureRmStorageAccountKey -ResourceGroupName $rgname -Name $storagename```
4. ``` $storagekey = $storagekeys[0].Value```

<span data-ttu-id="0895d-215">Spusťte příkaz AzLog pro každé Centrum událostí:</span><span class="sxs-lookup"><span data-stu-id="0895d-215">Run the AzLog command for each event hub:</span></span>

1. ```$eventhubs = Get-AzureRmEventHub -ResourceGroupName $rgname -NamespaceName $eventHubNamespaceName```
2. ```$eventhubs.Name | %{Add-AzLogEventSource -Name $sub' - '$_ -StorageAccount $storage.StorageAccountName -StorageKey $storageKey -EventHubConnectionString $eventHubKey.PrimaryConnectionString -EventHubName $_}```

<span data-ttu-id="0895d-216">Za minutu z posledních dvou příkazů měli byste vidět generován soubory JSON.</span><span class="sxs-lookup"><span data-stu-id="0895d-216">After a minute or so of running the last two commands, you should see JSON files being generated.</span></span> <span data-ttu-id="0895d-217">Je můžete potvrdit, že sledováním adresáři **C:\users\AzLog\EventHubJson**.</span><span class="sxs-lookup"><span data-stu-id="0895d-217">You can confirm that by monitoring the directory **C:\users\AzLog\EventHubJson**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0895d-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0895d-218">Next steps</span></span>

- [<span data-ttu-id="0895d-219">Integrace Azure protokolu – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="0895d-219">Azure Log Integration FAQ</span></span>](security-azure-log-integration-faq.md)
- [<span data-ttu-id="0895d-220">Začínáme s Azure protokolu integrace</span><span class="sxs-lookup"><span data-stu-id="0895d-220">Get started with Azure Log Integration</span></span>](security-azure-log-integration-get-started.md)
- [<span data-ttu-id="0895d-221">Protokoly z prostředků Azure integrovat do vašeho systému SIEM systémy</span><span class="sxs-lookup"><span data-stu-id="0895d-221">Integrate logs from Azure resources into your SIEM systems</span></span>](security-azure-log-integration-overview.md)
