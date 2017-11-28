---
title: "protokoly aaaIntegrate z Azure Key Vault pomocí služby Event Hubs | Microsoft Docs"
description: "Kurz, který poskytuje hello potřebné kroky toomake Key Vault protokolů k dispozici tooa SIEM s využitím protokolu integrace se službou Azure"
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
ms.openlocfilehash: ada2fc846cc6bf09e12cc2c016815b27afef0d50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-log-integration-tutorial-process-azure-key-vault-events-by-using-event-hubs"></a><span data-ttu-id="fd584-103">Kurz pro Azure integrace protokolu: proces Azure Key Vault událostí pomocí služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="fd584-103">Azure Log Integration tutorial: Process Azure Key Vault events by using Event Hubs</span></span>

<span data-ttu-id="fd584-104">Můžete použít protokol integrace se službou Azure tooretrieve protokolují události a systém je k dispozici tooyour zabezpečení informací a událostí správy (SIEM).</span><span class="sxs-lookup"><span data-stu-id="fd584-104">You can use Azure Log Integration tooretrieve logged events and make them available tooyour security information and event management (SIEM) system.</span></span> <span data-ttu-id="fd584-105">Tento kurz ukazuje příklad jak integrace se službou Azure protokolu lze použít tooprocess protokoly, které jsou získány prostřednictvím Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="fd584-105">This tutorial shows an example of how Azure Log Integration can be used tooprocess logs that are acquired through Azure Event Hubs.</span></span>
 
<span data-ttu-id="fd584-106">Použijte tento kurz tooget seznámen se jak integrace se službou protokolu Azure a Event Hubs pracovní společně podle kroků hello příklady kroků a pochopení, jak každý krok podporuje hello řešení.</span><span class="sxs-lookup"><span data-stu-id="fd584-106">Use this tutorial tooget acquainted with how Azure Log Integration and Event Hubs work together by following hello example steps and understanding how each step supports hello solution.</span></span> <span data-ttu-id="fd584-107">Potom můžete provést co když jste se naučili v tomto poli toocreate vlastní toosupport kroky jedinečným požadavkům vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="fd584-107">Then you can take what you’ve learned here toocreate your own steps toosupport your company’s unique requirements.</span></span>

>[!WARNING]
<span data-ttu-id="fd584-108">Hello kroky a příkazy v tomto kurzu nejsou určený toobe zkopírovat a vložit.</span><span class="sxs-lookup"><span data-stu-id="fd584-108">hello steps and commands in this tutorial are not intended toobe copied and pasted.</span></span> <span data-ttu-id="fd584-109">Jsou jenom příklady.</span><span class="sxs-lookup"><span data-stu-id="fd584-109">They're examples only.</span></span> <span data-ttu-id="fd584-110">Nepoužívejte příkazy prostředí PowerShell hello "tak, jak je ve vašem prostředí za provozu.</span><span class="sxs-lookup"><span data-stu-id="fd584-110">Do not use hello PowerShell commands “as is” in your live environment.</span></span> <span data-ttu-id="fd584-111">Je nutné přizpůsobit podle vašeho prostředí jedinečné.</span><span class="sxs-lookup"><span data-stu-id="fd584-111">You must customize them based on your unique environment.</span></span>


<span data-ttu-id="fd584-112">Tento kurz vás provede procesem hello trvá centra událostí zaznamenané tooan aktivity Azure Key Vault a zpřístupnění jako systém SIEM tooyour soubory JSON.</span><span class="sxs-lookup"><span data-stu-id="fd584-112">This tutorial walks you through hello process of taking Azure Key Vault activity logged tooan event hub and making it available as JSON files tooyour SIEM system.</span></span> <span data-ttu-id="fd584-113">Pak můžete nakonfigurovat souborů JSON hello tooprocess systému SIEM.</span><span class="sxs-lookup"><span data-stu-id="fd584-113">You can then configure your SIEM system tooprocess hello JSON files.</span></span>

>[!NOTE]
><span data-ttu-id="fd584-114">Většina hello kroky v tomto kurzu je konfigurace trezorů klíčů, účty úložiště a event hubs.</span><span class="sxs-lookup"><span data-stu-id="fd584-114">Most of hello steps in this tutorial involve configuring key vaults, storage accounts, and event hubs.</span></span> <span data-ttu-id="fd584-115">konkrétní kroky integrace se službou Azure protokolu Hello jsou na hello konce tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="fd584-115">hello specific Azure Log Integration steps are at hello end of this tutorial.</span></span> <span data-ttu-id="fd584-116">Neprovádějte kroky v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="fd584-116">Do not perform these steps in a production environment.</span></span> <span data-ttu-id="fd584-117">Ty jsou určené pro prostředí laboratoře.</span><span class="sxs-lookup"><span data-stu-id="fd584-117">They are intended for a lab environment only.</span></span> <span data-ttu-id="fd584-118">Je nutné přizpůsobit hello kroky před jejich používání v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="fd584-118">You must customize hello steps before using them in production.</span></span>

<span data-ttu-id="fd584-119">Zadané informace společně pomáhá hello způsobem, že rozumíte hello důvodech každý krok.</span><span class="sxs-lookup"><span data-stu-id="fd584-119">Information provided along hello way helps you understand hello reasons behind each step.</span></span> <span data-ttu-id="fd584-120">Odkazy tooother články poskytují další podrobnosti na určité témata.</span><span class="sxs-lookup"><span data-stu-id="fd584-120">Links tooother articles give you more detail on certain topics.</span></span>

<span data-ttu-id="fd584-121">Další informace o hello služby, které uvádí v tomto kurzu najdete v části:</span><span class="sxs-lookup"><span data-stu-id="fd584-121">For more information about hello services that this tutorial mentions, see:</span></span> 

- [<span data-ttu-id="fd584-122">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="fd584-122">Azure Key Vault</span></span>](../key-vault/key-vault-whatis.md)
- [<span data-ttu-id="fd584-123">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="fd584-123">Azure Event Hubs</span></span>](../event-hubs/event-hubs-what-is-event-hubs.md)
- [<span data-ttu-id="fd584-124">Integrace protokolů Azure</span><span class="sxs-lookup"><span data-stu-id="fd584-124">Azure Log Integration</span></span>](security-azure-log-integration-overview.md)


## <a name="initial-setup"></a><span data-ttu-id="fd584-125">Počáteční nastavení</span><span class="sxs-lookup"><span data-stu-id="fd584-125">Initial setup</span></span>

<span data-ttu-id="fd584-126">Před dokončením hello kroky v tomto článku, je třeba hello následující:</span><span class="sxs-lookup"><span data-stu-id="fd584-126">Before you can complete hello steps in this article, you need hello following:</span></span>

1. <span data-ttu-id="fd584-127">Předplatné Azure a účet v tomto předplatném s právy správce.</span><span class="sxs-lookup"><span data-stu-id="fd584-127">An Azure subscription and account on that subscription with administrator rights.</span></span> <span data-ttu-id="fd584-128">Pokud nemáte předplatné, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="fd584-128">If you don't have a subscription, you can create a [free account](https://azure.microsoft.com/free/).</span></span>
 
2. <span data-ttu-id="fd584-129">Systém s toohello přístup k Internetu, který splňuje požadavky hello instalaci protokolu integrace se službou Azure.</span><span class="sxs-lookup"><span data-stu-id="fd584-129">A system with access toohello internet that meets hello requirements for installing Azure Log Integration.</span></span> <span data-ttu-id="fd584-130">Hello systém může být v cloudové službě nebo hostovaný místně.</span><span class="sxs-lookup"><span data-stu-id="fd584-130">hello system can be on a cloud service or hosted on-premises.</span></span>

3. <span data-ttu-id="fd584-131">[Integrace se službou Azure protokolu](https://www.microsoft.com/download/details.aspx?id=53324) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="fd584-131">[Azure Log Integration](https://www.microsoft.com/download/details.aspx?id=53324) installed.</span></span> <span data-ttu-id="fd584-132">tooinstall ho:</span><span class="sxs-lookup"><span data-stu-id="fd584-132">tooinstall it:</span></span>

   <span data-ttu-id="fd584-133">a.</span><span class="sxs-lookup"><span data-stu-id="fd584-133">a.</span></span> <span data-ttu-id="fd584-134">Používejte systém toohello tooconnect vzdálené plochy je uveden v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="fd584-134">Use Remote Desktop tooconnect toohello system mentioned in step 2.</span></span>   
   <span data-ttu-id="fd584-135">b.</span><span class="sxs-lookup"><span data-stu-id="fd584-135">b.</span></span> <span data-ttu-id="fd584-136">Zkopírujte hello integrace se službou Azure protokolu instalačního programu toohello systému.</span><span class="sxs-lookup"><span data-stu-id="fd584-136">Copy hello Azure Log Integration installer toohello system.</span></span> <span data-ttu-id="fd584-137">Můžete [stáhnout instalační soubory hello](https://www.microsoft.com/download/details.aspx?id=53324).</span><span class="sxs-lookup"><span data-stu-id="fd584-137">You can [download hello installation files](https://www.microsoft.com/download/details.aspx?id=53324).</span></span>   
   <span data-ttu-id="fd584-138">c.</span><span class="sxs-lookup"><span data-stu-id="fd584-138">c.</span></span> <span data-ttu-id="fd584-139">Spusťte instalační program hello a přijměte licenční podmínky softwaru společnosti Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="fd584-139">Start hello installer and accept hello Microsoft Software License Terms.</span></span>   
   <span data-ttu-id="fd584-140">d.</span><span class="sxs-lookup"><span data-stu-id="fd584-140">d.</span></span> <span data-ttu-id="fd584-141">Pokud bude poskytovat informace telemetrie, ponechejte zaškrtnuté zaškrtávací políčko hello.</span><span class="sxs-lookup"><span data-stu-id="fd584-141">If you will provide telemetry information, leave hello check box selected.</span></span> <span data-ttu-id="fd584-142">Pokud místo není odesílali tooMicrosoft informace o využití, zrušte zaškrtnutí políčka hello.</span><span class="sxs-lookup"><span data-stu-id="fd584-142">If you'd rather not send usage information tooMicrosoft, clear hello check box.</span></span>
   
   <span data-ttu-id="fd584-143">Další informace o protokolu integrace se službou Azure a jak tooinstall, najdete v části [integrace protokolu Azure s Azure Diagnostics protokolování a předávání událostí systému Windows](security-azure-log-integration-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="fd584-143">For more information about Azure Log Integration and how tooinstall it, see [Azure Log Integration with Azure Diagnostics logging and Windows Event Forwarding](security-azure-log-integration-get-started.md).</span></span>

4. <span data-ttu-id="fd584-144">Hello nejnovější verze prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fd584-144">hello latest PowerShell version.</span></span>
 
   <span data-ttu-id="fd584-145">Pokud máte nainstalovaný Windows Server 2016, pak máte alespoň PowerShell 5.0.</span><span class="sxs-lookup"><span data-stu-id="fd584-145">If you have Windows Server 2016 installed, then you have at least PowerShell 5.0.</span></span> <span data-ttu-id="fd584-146">Pokud používáte jinou verzi systému Windows Server, bude pravděpodobně starší verzi prostředí PowerShell nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="fd584-146">If you're using any other version of Windows Server, you might have an earlier version of PowerShell installed.</span></span> <span data-ttu-id="fd584-147">Můžete zkontrolovat hello verze zadáním ```get-host``` v okně prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fd584-147">You can check hello version by entering ```get-host``` in a PowerShell window.</span></span> <span data-ttu-id="fd584-148">Pokud nemáte nainstalovaný PowerShell 5.0, můžete [stažení](https://www.microsoft.com/download/details.aspx?id=50395).</span><span class="sxs-lookup"><span data-stu-id="fd584-148">If you don't have PowerShell 5.0 installed, you can [download it](https://www.microsoft.com/download/details.aspx?id=50395).</span></span>

   <span data-ttu-id="fd584-149">Až budete mít alespoň PowerShell 5.0, abyste mohli pokračovat tooinstall hello nejnovější verzi:</span><span class="sxs-lookup"><span data-stu-id="fd584-149">After you have at least PowerShell 5.0, you can proceed tooinstall hello latest version:</span></span>
   
   <span data-ttu-id="fd584-150">a.</span><span class="sxs-lookup"><span data-stu-id="fd584-150">a.</span></span> <span data-ttu-id="fd584-151">V okně prostředí PowerShell, zadejte hello ```Install-Module Azure``` příkaz.</span><span class="sxs-lookup"><span data-stu-id="fd584-151">In a PowerShell window, enter hello ```Install-Module Azure``` command.</span></span> <span data-ttu-id="fd584-152">Kroky instalace hello.</span><span class="sxs-lookup"><span data-stu-id="fd584-152">Complete hello installation steps.</span></span>    
   <span data-ttu-id="fd584-153">b.</span><span class="sxs-lookup"><span data-stu-id="fd584-153">b.</span></span> <span data-ttu-id="fd584-154">Zadejte hello ```Install-Module AzureRM``` příkaz.</span><span class="sxs-lookup"><span data-stu-id="fd584-154">Enter hello ```Install-Module AzureRM``` command.</span></span> <span data-ttu-id="fd584-155">Kroky instalace hello.</span><span class="sxs-lookup"><span data-stu-id="fd584-155">Complete hello installation steps.</span></span>

   <span data-ttu-id="fd584-156">Další informace najdete v tématu [nainstalovat Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span><span class="sxs-lookup"><span data-stu-id="fd584-156">For more information, see [Install Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span></span>


## <a name="create-supporting-infrastructure-elements"></a><span data-ttu-id="fd584-157">Vytváření podpůrné prvků infrastruktury</span><span class="sxs-lookup"><span data-stu-id="fd584-157">Create supporting infrastructure elements</span></span>

1. <span data-ttu-id="fd584-158">Otevřete okno prostředí PowerShell se zvýšenými oprávněními a přejděte příliš**C:\Program Files\Microsoft Azure protokolu integrace**.</span><span class="sxs-lookup"><span data-stu-id="fd584-158">Open an elevated PowerShell window and go too**C:\Program Files\Microsoft Azure Log Integration**.</span></span>
2. <span data-ttu-id="fd584-159">Importujte rutiny AzLog hello spuštěním skriptu hello LoadAzLogModule.ps1.</span><span class="sxs-lookup"><span data-stu-id="fd584-159">Import hello AzLog cmdlets by running hello script LoadAzLogModule.ps1.</span></span> <span data-ttu-id="fd584-160">Zadejte hello `.\LoadAzLogModule.ps1` příkaz.</span><span class="sxs-lookup"><span data-stu-id="fd584-160">Enter hello `.\LoadAzLogModule.ps1` command.</span></span> <span data-ttu-id="fd584-161">(Všimněte si hello ". \" v tomto příkazu.) Mělo by se vám zobrazit přibližně toto:</span><span class="sxs-lookup"><span data-stu-id="fd584-161">(Notice hello “.\” in that command.) You should see something like this:</span></span></br>

   ![Seznam načtených modulů](./media/security-azure-log-integration-keyvault-eventhub/loaded-modules.png)

3. <span data-ttu-id="fd584-163">Zadejte hello `Login-AzureRmAccount` příkaz.</span><span class="sxs-lookup"><span data-stu-id="fd584-163">Enter hello `Login-AzureRmAccount` command.</span></span> <span data-ttu-id="fd584-164">V okně přihlášení hello zadejte přihlašovací údaje hello hello odběru, který budete používat pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="fd584-164">In hello login window, enter hello credential information for hello subscription that you will use for this tutorial.</span></span>

   >[!NOTE]
   ><span data-ttu-id="fd584-165">Pokud je to hello poprvé, který jste přihlášení tooAzure z tohoto počítače, zobrazí se zpráva o povolení data o využití Microsoft toocollect prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fd584-165">If this is hello first time that you're logging in tooAzure from this machine, you will see a message about allowing Microsoft toocollect PowerShell usage data.</span></span> <span data-ttu-id="fd584-166">Doporučujeme povolit tuto kolekci dat, protože je použité tooimprove prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fd584-166">We recommend that you enable this data collection because it will be used tooimprove Azure PowerShell.</span></span>

4. <span data-ttu-id="fd584-167">Po úspěšném ověření jste přihlášeni a zobrazí informace o hello v hello následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="fd584-167">After successful authentication, you're logged in and you see hello information in hello following screenshot.</span></span> <span data-ttu-id="fd584-168">Poznamenejte si název odběru ID a předplatné hello, protože je budete potřebovat později toocomplete kroky.</span><span class="sxs-lookup"><span data-stu-id="fd584-168">Take note of hello subscription ID and subscription name, because you'll need them toocomplete later steps.</span></span>

   ![Okno prostředí PowerShell](./media/security-azure-log-integration-keyvault-eventhub/login-azurermaccount.png)
5. <span data-ttu-id="fd584-170">Vytvořte proměnné toostore hodnoty, které se použijí později.</span><span class="sxs-lookup"><span data-stu-id="fd584-170">Create variables toostore values that will be used later.</span></span> <span data-ttu-id="fd584-171">Zadejte všechny hello následující řádky prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fd584-171">Enter each of hello following PowerShell lines.</span></span> <span data-ttu-id="fd584-172">Můžete potřebovat tooadjust hello hodnoty toomatch prostředí.</span><span class="sxs-lookup"><span data-stu-id="fd584-172">You might need tooadjust hello values toomatch your environment.</span></span>
    - <span data-ttu-id="fd584-173">```$subscriptionName = ‘Visual Studio Ultimate with MSDN’```(Název odběru mohou lišit.</span><span class="sxs-lookup"><span data-stu-id="fd584-173">```$subscriptionName = ‘Visual Studio Ultimate with MSDN’``` (Your subscription name might be different.</span></span> <span data-ttu-id="fd584-174">Zobrazí se jako součást hello výstup hello předchozí příkaz.)</span><span class="sxs-lookup"><span data-stu-id="fd584-174">You can see it as part of hello output of hello previous command.)</span></span>
    - <span data-ttu-id="fd584-175">```$location = 'West US'```(Tato proměnná bude použité toopass hello umístění, kde by měl být vytvořen prostředky.</span><span class="sxs-lookup"><span data-stu-id="fd584-175">```$location = 'West US'``` (This variable will be used toopass hello location where resources should be created.</span></span> <span data-ttu-id="fd584-176">Můžete změnit této proměnné toobe libovolného umístění podle vaší volby.)</span><span class="sxs-lookup"><span data-stu-id="fd584-176">You can change this variable toobe any location of your choosing.)</span></span>
    - ```$random = Get-Random```
    - <span data-ttu-id="fd584-177">``` $name = 'azlogtest' + $random```(můžete použít jakýkoli název hello, ale měl by obsahovat pouze malá písmena a číslice.)</span><span class="sxs-lookup"><span data-stu-id="fd584-177">``` $name = 'azlogtest' + $random``` (hello name can be anything, but it should include only lowercase letters and numbers.)</span></span>
    - <span data-ttu-id="fd584-178">``` $storageName = $name```(Tato proměnná se použije pro název účtu úložiště hello.)</span><span class="sxs-lookup"><span data-stu-id="fd584-178">``` $storageName = $name``` (This variable will be used for hello storage account name.)</span></span>
    - <span data-ttu-id="fd584-179">```$rgname = $name ```(Tato proměnná se použije pro název skupiny prostředků hello.)</span><span class="sxs-lookup"><span data-stu-id="fd584-179">```$rgname = $name ``` (This variable will be used for hello resource group name.)</span></span>
    - <span data-ttu-id="fd584-180">``` $eventHubNameSpaceName = $name```(To je hello název oboru názvů centra událostí hello.)</span><span class="sxs-lookup"><span data-stu-id="fd584-180">``` $eventHubNameSpaceName = $name``` (This is hello name of hello event hub namespace.)</span></span>
6. <span data-ttu-id="fd584-181">Zadejte hello předplatné, které budete pracovat s:</span><span class="sxs-lookup"><span data-stu-id="fd584-181">Specify hello subscription that you will be working with:</span></span>
    
    ```Select-AzureRmSubscription -SubscriptionName $subscriptionName```
7. <span data-ttu-id="fd584-182">Vytvořte skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="fd584-182">Create a resource group:</span></span>
    
    ```$rg = New-AzureRmResourceGroup -Name $rgname -Location $location```
    
   <span data-ttu-id="fd584-183">Pokud zadáte `$rg` v tomto okamžiku byste měli vidět výstup podobný toothis snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="fd584-183">If you enter `$rg` at this point, you should see output similar toothis screenshot:</span></span>

   ![Výstup po vytvoření skupiny prostředků](./media/security-azure-log-integration-keyvault-eventhub/create-rg.png)
8. <span data-ttu-id="fd584-185">Vytvořte účet úložiště, které budou použité tookeep zaznamenávat informace o stavu:</span><span class="sxs-lookup"><span data-stu-id="fd584-185">Create a storage account that will be used tookeep track of state information:</span></span>
    
    ```$storage = New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename -Location $location -SkuName Standard_LRS```
9. <span data-ttu-id="fd584-186">Vytvoření oboru názvů centra událostí hello.</span><span class="sxs-lookup"><span data-stu-id="fd584-186">Create hello event hub namespace.</span></span> <span data-ttu-id="fd584-187">Toto je požadovaná toocreate centra událostí.</span><span class="sxs-lookup"><span data-stu-id="fd584-187">This is required toocreate an event hub.</span></span>
    
    ```$eventHubNameSpace = New-AzureRmEventHubNamespace -ResourceGroupName $rgname -NamespaceName $eventHubnamespaceName -Location $location```
10. <span data-ttu-id="fd584-188">Získání ID hello pravidlo, které budou použity s hello Statistika zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="fd584-188">Get hello rule ID that will be used with hello insights provider:</span></span>
    
    ```$sbruleid = $eventHubNameSpace.Id +'/authorizationrules/RootManageSharedAccessKey' ```
11. <span data-ttu-id="fd584-189">Získat všechny možné Azure umístění a hello názvy tooa proměnné, které je možné přidat do pozdějšího kroku:</span><span class="sxs-lookup"><span data-stu-id="fd584-189">Get all possible Azure locations and add hello names tooa variable that can be used in a later step:</span></span>
    
    <span data-ttu-id="fd584-190">a.</span><span class="sxs-lookup"><span data-stu-id="fd584-190">a.</span></span> ```$locationObjects = Get-AzureRMLocation```    
    <span data-ttu-id="fd584-191">b.</span><span class="sxs-lookup"><span data-stu-id="fd584-191">b.</span></span> ```$locations = @('global') + $locationobjects.location```
    
    <span data-ttu-id="fd584-192">Pokud zadáte `$locations` v tomto okamžiku zobrazí hello umístění názvy bez hello vrácený Get-AzureRmLocation dalších informací.</span><span class="sxs-lookup"><span data-stu-id="fd584-192">If you enter `$locations` at this point, you see hello location names without hello additional information returned by Get-AzureRmLocation.</span></span>
12. <span data-ttu-id="fd584-193">Vytvoření profilu protokolu správce Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="fd584-193">Create an Azure Resource Manager log profile:</span></span> 
    
    ```Add-AzureRmLogProfile -Name $name -ServiceBusRuleId $sbruleid -Locations $locations```
    
    <span data-ttu-id="fd584-194">Další informace o hello profil protokolů Azure najdete v tématu [přehled hello protokol činnosti Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="fd584-194">For more information about hello Azure log profile, see [Overview of hello Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).</span></span>

> [!NOTE]
> <span data-ttu-id="fd584-195">Při pokusu o toocreate profil protokolu, může se zobrazí chybové hlášení.</span><span class="sxs-lookup"><span data-stu-id="fd584-195">You might get an error message when you try toocreate a log profile.</span></span> <span data-ttu-id="fd584-196">Potom můžete zkontrolovat hello dokumentace pro Get-AzureRmLogProfile a AzureRmLogProfile odebrat.</span><span class="sxs-lookup"><span data-stu-id="fd584-196">You can then review hello documentation for Get-AzureRmLogProfile and Remove-AzureRmLogProfile.</span></span> <span data-ttu-id="fd584-197">Pokud spustíte Get-AzureRmLogProfile, zobrazí informace o profilu protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="fd584-197">If you run Get-AzureRmLogProfile, you see information about hello log profile.</span></span> <span data-ttu-id="fd584-198">Odstraněním hello stávající profil protokolu zadáním hello ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` příkaz.</span><span class="sxs-lookup"><span data-stu-id="fd584-198">You can delete hello existing log profile by entering hello ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` command.</span></span>
>
>![Správce prostředků profilu došlo k chybě](./media/security-azure-log-integration-keyvault-eventhub/rm-profile-error.png)

## <a name="create-a-key-vault"></a><span data-ttu-id="fd584-200">Vytvořte trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="fd584-200">Create a key vault</span></span>

1. <span data-ttu-id="fd584-201">Vytvoření trezoru klíčů hello:</span><span class="sxs-lookup"><span data-stu-id="fd584-201">Create hello key vault:</span></span>

   ```$kv = New-AzureRmKeyVault -VaultName $name -ResourceGroupName $rgname -Location $location ```

2. <span data-ttu-id="fd584-202">Konfigurace protokolování pro trezor klíčů hello:</span><span class="sxs-lookup"><span data-stu-id="fd584-202">Configure logging for hello key vault:</span></span>

   ```Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -ServiceBusRuleId $sbruleid -Enabled $true ```

## <a name="generate-log-activity"></a><span data-ttu-id="fd584-203">Generovat aktivitu protokolu</span><span class="sxs-lookup"><span data-stu-id="fd584-203">Generate log activity</span></span>

<span data-ttu-id="fd584-204">Požadavky nutné toobe odeslané tooKey aktivity protokolu toogenerate trezoru.</span><span class="sxs-lookup"><span data-stu-id="fd584-204">Requests need toobe sent tooKey Vault toogenerate log activity.</span></span> <span data-ttu-id="fd584-205">Akce, jako generování klíčů, ukládání tajné klíče, nebo čtení tajných klíčů z Key Vault vytvoří položky protokolu.</span><span class="sxs-lookup"><span data-stu-id="fd584-205">Actions like key generation, storing secrets, or reading secrets from Key Vault will create log entries.</span></span>

1. <span data-ttu-id="fd584-206">Zobrazí aktuální klíče úložiště hello:</span><span class="sxs-lookup"><span data-stu-id="fd584-206">Display hello current storage keys:</span></span>
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
2. <span data-ttu-id="fd584-207">Generovat nové **key2**:</span><span class="sxs-lookup"><span data-stu-id="fd584-207">Generate a new **key2**:</span></span>
    
   ```New-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname -KeyName key2```
3. <span data-ttu-id="fd584-208">Nezobrazovat hello klíče, zjistíte, že **key2** obsahuje jiné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="fd584-208">Display hello keys again and see that **key2** holds a different value:</span></span>
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
4. <span data-ttu-id="fd584-209">Nastavte a číst tajný toogenerate položky další protokolu:</span><span class="sxs-lookup"><span data-stu-id="fd584-209">Set and read a secret toogenerate additional log entries:</span></span>
    
   <span data-ttu-id="fd584-210">a.</span><span class="sxs-lookup"><span data-stu-id="fd584-210">a.</span></span> <span data-ttu-id="fd584-211">```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b.</span><span class="sxs-lookup"><span data-stu-id="fd584-211">```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b.</span></span> ```(Get-AzureKeyVaultSecret -VaultName $name -Name TestSecret).SecretValueText```

   ![Vrátí tajné](./media/security-azure-log-integration-keyvault-eventhub/keyvaultsecret.png)


## <a name="configure-azure-log-integration"></a><span data-ttu-id="fd584-213">Konfigurace integrace protokolů Azure</span><span class="sxs-lookup"><span data-stu-id="fd584-213">Configure Azure Log Integration</span></span>

<span data-ttu-id="fd584-214">Teď, když jste nakonfigurovali všechny hello požadované prvky toohave protokolování v Key Vault tooan centra událostí, je třeba tooconfigure integrace se službou Azure protokolu:</span><span class="sxs-lookup"><span data-stu-id="fd584-214">Now that you have configured all hello required elements toohave Key Vault logging tooan event hub, you need tooconfigure Azure Log Integration:</span></span>

1. ```$storage = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename```
2. ```$eventHubKey = Get-AzureRmEventHubNamespaceKey -ResourceGroupName $rgname -NamespaceName $eventHubNamespace.name -AuthorizationRuleName RootManageSharedAccessKey```
3. ```$storagekeys = Get-AzureRmStorageAccountKey -ResourceGroupName $rgname -Name $storagename```
4. ``` $storagekey = $storagekeys[0].Value```

<span data-ttu-id="fd584-215">Spusťte příkaz hello AzLog pro každé Centrum událostí:</span><span class="sxs-lookup"><span data-stu-id="fd584-215">Run hello AzLog command for each event hub:</span></span>

1. ```$eventhubs = Get-AzureRmEventHub -ResourceGroupName $rgname -NamespaceName $eventHubNamespaceName```
2. ```$eventhubs.Name | %{Add-AzLogEventSource -Name $sub' - '$_ -StorageAccount $storage.StorageAccountName -StorageKey $storageKey -EventHubConnectionString $eventHubKey.PrimaryConnectionString -EventHubName $_}```

<span data-ttu-id="fd584-216">Za minutu spuštěných hello poslední dva příkazy měli byste vidět generován soubory JSON.</span><span class="sxs-lookup"><span data-stu-id="fd584-216">After a minute or so of running hello last two commands, you should see JSON files being generated.</span></span> <span data-ttu-id="fd584-217">Je můžete potvrdit, že sledováním hello directory **C:\users\AzLog\EventHubJson**.</span><span class="sxs-lookup"><span data-stu-id="fd584-217">You can confirm that by monitoring hello directory **C:\users\AzLog\EventHubJson**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd584-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fd584-218">Next steps</span></span>

- [<span data-ttu-id="fd584-219">Integrace Azure protokolu – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="fd584-219">Azure Log Integration FAQ</span></span>](security-azure-log-integration-faq.md)
- [<span data-ttu-id="fd584-220">Začínáme s Azure protokolu integrace</span><span class="sxs-lookup"><span data-stu-id="fd584-220">Get started with Azure Log Integration</span></span>](security-azure-log-integration-get-started.md)
- [<span data-ttu-id="fd584-221">Protokoly z prostředků Azure integrovat do vašeho systému SIEM systémy</span><span class="sxs-lookup"><span data-stu-id="fd584-221">Integrate logs from Azure resources into your SIEM systems</span></span>](security-azure-log-integration-overview.md)
