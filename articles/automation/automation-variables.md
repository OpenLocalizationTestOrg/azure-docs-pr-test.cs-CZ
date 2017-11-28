---
title: "aaaVariable prostředky ve službě Azure Automation | Microsoft Docs"
description: "Proměnné prostředky jsou hodnoty, které jsou k dispozici tooall runboocích a konfiguracích DSC ve službě Azure Automation.  Tento článek vysvětluje podrobnosti hello proměnných a jak toowork s nimi v textové a grafické vytváření."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: b880c15f-46f5-4881-8e98-e034cc5a66ec
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/09/2017
ms.author: magoedte;bwren
ms.openlocfilehash: f9daa49fc1dc883ffb218a9adf26e36df1d6bb27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="variable-assets-in-azure-automation"></a><span data-ttu-id="da113-104">Proměnné prostředky ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="da113-104">Variable assets in Azure Automation</span></span>

<span data-ttu-id="da113-105">Proměnné prostředky jsou hodnoty, které jsou k dispozici tooall runboocích a konfiguracích DSC v účtu automation.</span><span class="sxs-lookup"><span data-stu-id="da113-105">Variable assets are values that are available tooall runbooks and DSC configurations in your automation account.</span></span> <span data-ttu-id="da113-106">Budou se dají vytvořit, upravit a načíst prostřednictvím hello portál Azure, prostředí Windows PowerShell a v rámci sady runbook nebo konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="da113-106">They can be created, modified, and retrieved from hello Azure portal, Windows PowerShell, and from within a runbook or DSC configuration.</span></span> <span data-ttu-id="da113-107">Proměnné služeb automatizace jsou užitečné pro hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="da113-107">Automation variables are useful for hello following scenarios:</span></span>

- <span data-ttu-id="da113-108">Sdílení hodnoty mezi několika runbooky nebo konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="da113-108">Share a value between multiple runbooks or DSC configurations.</span></span>

- <span data-ttu-id="da113-109">Sdílení hodnoty mezi několika úlohami hello stejné sady runbook nebo konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="da113-109">Share a value between multiple jobs from hello same runbook or DSC configuration.</span></span>

- <span data-ttu-id="da113-110">Správa hodnoty z portálu hello nebo z příkazového řádku prostředí Windows PowerShell hello, který je používán sady runbook nebo konfigurace DSC, jako je například sada běžné položky konfigurace jako konkrétní seznam názvů virtuálních počítačů, určité skupiny zdrojů, název domény Active Directory, atd.</span><span class="sxs-lookup"><span data-stu-id="da113-110">Manage a value from hello portal or from hello Windows PowerShell command line that is used by runbooks or DSC configurations, such as a set of common configuration items like specific list of VM names, a specific resource group,  an AD domain name, etc.</span></span>  

<span data-ttu-id="da113-111">Proměnné služeb automatizace jsou nastavené jako trvalé, takže jsou toobe k dispozici i v případě, že sada runbook hello nebo konfigurace DSC se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="da113-111">Automation variables are persisted so that they continue toobe available even if hello runbook or DSC configuration fails.</span></span>  <span data-ttu-id="da113-112">To také umožňuje hodnotu toobe, nastavte jednom runbooku, který je pak použije v jiném nebo hello používá stejné sady runbook nebo hello konfigurace DSC při příštím spuštění.</span><span class="sxs-lookup"><span data-stu-id="da113-112">This also allows a value toobe set by one runbook that is then used by another, or is used by hello same runbook or DSC configuration hello next time that it is run.</span></span>

<span data-ttu-id="da113-113">Při vytvoření proměnné můžete určit, že se uloží šifrované.</span><span class="sxs-lookup"><span data-stu-id="da113-113">When a variable is created, you can specify that it be stored encrypted.</span></span>  <span data-ttu-id="da113-114">Když je proměnná zašifrovaná, bezpečně se uloží ve službě Azure Automation a jeho hodnotu nelze načíst z hello [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) rutinu, která se dodává jako součást modulu Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="da113-114">When a variable is encrypted, it is stored securely in Azure Automation, and its value cannot be retrieved from hello [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) cmdlet that ships as part of hello Azure PowerShell module.</span></span>  <span data-ttu-id="da113-115">Hello jediným způsobem, že šifrovaná hodnota se dá načíst je z hello **Get-AutomationVariable** aktivity v sady runbook nebo konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="da113-115">hello only way that an encrypted value can be retrieved is from hello **Get-AutomationVariable** activity in a runbook or DSC configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="da113-116">Zabezpečené prostředky ve službě Azure Automation zahrnovat přihlašovací údaje, připojení, certifikátů a zašifrované proměnné.</span><span class="sxs-lookup"><span data-stu-id="da113-116">Secure assets in Azure Automation include credentials, certificates, connections, and encrypted variables.</span></span> <span data-ttu-id="da113-117">Tyto prostředky jsou zašifrovány a uložené v Azure Automation jednotlivých účtů automation pomocí jedinečný klíč, který se vygeneruje hello.</span><span class="sxs-lookup"><span data-stu-id="da113-117">These assets are encrypted and stored in hello Azure Automation using a unique key that is generated for each automation account.</span></span> <span data-ttu-id="da113-118">Tento klíč se šifruje pomocí hlavního certifikátu a uloží ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="da113-118">This key is encrypted by a master certificate and stored in Azure Automation.</span></span> <span data-ttu-id="da113-119">Před uložením o zabezpečený prostředek, hello klíč pro účet automation hello se dešifruje pomocí hlavního certifikátu hello a pak se použije tooencrypt hello asset.</span><span class="sxs-lookup"><span data-stu-id="da113-119">Before storing a secure asset, hello key for hello automation account is decrypted using hello master certificate and then used tooencrypt hello asset.</span></span>

## <a name="variable-types"></a><span data-ttu-id="da113-120">Typy proměnných</span><span class="sxs-lookup"><span data-stu-id="da113-120">Variable types</span></span>

<span data-ttu-id="da113-121">Při vytváření proměnné pomocí portálu Azure hello, musíte zadat datový typ z rozevíracího seznamu hello, takže hello portálu můžete zobrazit hello vhodný ovládací prvek pro zadání hodnoty proměnné hello.</span><span class="sxs-lookup"><span data-stu-id="da113-121">When you create a variable with hello Azure portal, you must specify a data type from hello drop-down list so hello portal can display hello appropriate control for entering hello variable value.</span></span> <span data-ttu-id="da113-122">Proměnná Hello není s omezeným přístupem toothis datového typu, ale musíte nastavit hello proměnné pomocí prostředí Windows PowerShell, pokud chcete, aby toospecify hodnotu jiného typu.</span><span class="sxs-lookup"><span data-stu-id="da113-122">hello variable is not restricted toothis data type, but you must set hello variable using Windows PowerShell if you want toospecify a value of a different type.</span></span> <span data-ttu-id="da113-123">Pokud zadáte **není definována**, pak hello hello proměnné se hodnota příliš**$null**, a je nutné nastavit hodnotu hello s hello [Set-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) rutiny nebo **Set-AutomationVariable** aktivity.</span><span class="sxs-lookup"><span data-stu-id="da113-123">If you specify **Not defined**, then hello value of hello variable will be set too**$null**, and you must set hello value with hello [Set-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) cmdlet or **Set-AutomationVariable** activity.</span></span>  <span data-ttu-id="da113-124">Nelze vytvořit nebo změnit hello hodnotu pro komplexní proměnná typu hello portálu, ale můžete zadat hodnotu libovolného typu pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="da113-124">You cannot create or change hello value for a complex variable type in hello portal, but you can provide a value of any type using Windows PowerShell.</span></span> <span data-ttu-id="da113-125">Komplexní typy bude vrácen jako [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx).</span><span class="sxs-lookup"><span data-stu-id="da113-125">Complex types will be returned as a [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx).</span></span>

<span data-ttu-id="da113-126">Vytváření zatřiďovací tabulky nebo pole a jeho uložením toohello proměnné můžete uložit více hodnot tooa jednu proměnnou.</span><span class="sxs-lookup"><span data-stu-id="da113-126">You can store multiple values tooa single variable by creating an array or hashtable and saving it toohello variable.</span></span>

<span data-ttu-id="da113-127">seznam proměnných typy, které jsou dostupné ve službě Automation se Hello následující:</span><span class="sxs-lookup"><span data-stu-id="da113-127">hello following are a list of variable types available in Automation:</span></span>

* <span data-ttu-id="da113-128">Řetězec</span><span class="sxs-lookup"><span data-stu-id="da113-128">String</span></span>
* <span data-ttu-id="da113-129">Integer</span><span class="sxs-lookup"><span data-stu-id="da113-129">Integer</span></span>
* <span data-ttu-id="da113-130">Data a času</span><span class="sxs-lookup"><span data-stu-id="da113-130">DateTime</span></span>
* <span data-ttu-id="da113-131">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="da113-131">Boolean</span></span>
* <span data-ttu-id="da113-132">Hodnotu Null</span><span class="sxs-lookup"><span data-stu-id="da113-132">Null</span></span>

## <a name="cmdlets-and-workflow-activities"></a><span data-ttu-id="da113-133">Rutiny a pracovní postup aktivity</span><span class="sxs-lookup"><span data-stu-id="da113-133">Cmdlets and workflow activities</span></span>

<span data-ttu-id="da113-134">Hello rutiny v následující tabulce hello jsou použité toocreate a správě proměnných Automation pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="da113-134">hello cmdlets in hello following table are used toocreate and manage Automation variables with Windows PowerShell.</span></span> <span data-ttu-id="da113-135">Se dodávají jako součást hello [modul Azure PowerShell](../powershell-install-configure.md) která je k dispozici pro použití v runbooků služeb automatizace a konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="da113-135">They ship as part of hello [Azure PowerShell module](../powershell-install-configure.md) which is available for use in Automation runbooks and DSC configuration.</span></span>

|<span data-ttu-id="da113-136">Rutiny</span><span class="sxs-lookup"><span data-stu-id="da113-136">Cmdlets</span></span>|<span data-ttu-id="da113-137">Popis</span><span class="sxs-lookup"><span data-stu-id="da113-137">Description</span></span>|
|:---|:---|
|[<span data-ttu-id="da113-138">Get-AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="da113-138">Get-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt603849.aspx)|<span data-ttu-id="da113-139">Načte hodnotu existující proměnné hello.</span><span class="sxs-lookup"><span data-stu-id="da113-139">Retrieves hello value of an existing variable.</span></span>|
|[<span data-ttu-id="da113-140">Nové AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="da113-140">New-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt603613.aspx)|<span data-ttu-id="da113-141">Vytvoří novou proměnnou a nastaví její hodnotu.</span><span class="sxs-lookup"><span data-stu-id="da113-141">Creates a new variable and sets its value.</span></span>|
|[<span data-ttu-id="da113-142">Odebrat AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="da113-142">Remove-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt619354.aspx)|<span data-ttu-id="da113-143">Odebere existující proměnnou.</span><span class="sxs-lookup"><span data-stu-id="da113-143">Removes an existing variable.</span></span>|
|[<span data-ttu-id="da113-144">Set-AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="da113-144">Set-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt603601.aspx)|<span data-ttu-id="da113-145">Nastaví hello hodnotu pro existující proměnnou.</span><span class="sxs-lookup"><span data-stu-id="da113-145">Sets hello value for an existing variable.</span></span>|

<span data-ttu-id="da113-146">aktivity pracovního postupu Hello v následující tabulce hello jsou použité tooaccess automatizace proměnné v runbooku.</span><span class="sxs-lookup"><span data-stu-id="da113-146">hello workflow activities in hello following table are used tooaccess Automation variables in a runbook.</span></span> <span data-ttu-id="da113-147">Jsou dostupné pouze pro použití v runbooku nebo konfigurace DSC a se nedodává jako součást modulu Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="da113-147">They are only available for use in a runbook or DSC configuration, and do not ship as part of hello Azure PowerShell module.</span></span>

|<span data-ttu-id="da113-148">Aktivity pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="da113-148">Workflow Activities</span></span>|<span data-ttu-id="da113-149">Popis</span><span class="sxs-lookup"><span data-stu-id="da113-149">Description</span></span>|
|:---|:---|
|<span data-ttu-id="da113-150">Get-AutomationVariable</span><span class="sxs-lookup"><span data-stu-id="da113-150">Get-AutomationVariable</span></span>|<span data-ttu-id="da113-151">Načte hodnotu existující proměnné hello.</span><span class="sxs-lookup"><span data-stu-id="da113-151">Retrieves hello value of an existing variable.</span></span>|
|<span data-ttu-id="da113-152">Set-AutomationVariable</span><span class="sxs-lookup"><span data-stu-id="da113-152">Set-AutomationVariable</span></span>|<span data-ttu-id="da113-153">Nastaví hello hodnotu pro existující proměnnou.</span><span class="sxs-lookup"><span data-stu-id="da113-153">Sets hello value for an existing variable.</span></span>|

> [!NOTE] 
> <span data-ttu-id="da113-154">Byste neměli používat proměnné v hello – název parametru **Get-AutomationVariable** v sady runbook nebo konfigurace DSC, protože to může zkomplikovat zjišťování závislostí mezi runbooky nebo konfigurace DSC a automatizace proměnné v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="da113-154">You should avoid using variables in hello –Name parameter of **Get-AutomationVariable**  in a runbook or DSC configuration since this can complicate discovering dependencies between runbooks or DSC configuration, and Automation variables at design time.</span></span>

## <a name="creating-a-new-automation-variable"></a><span data-ttu-id="da113-155">Vytvoření nové proměnné Automation</span><span class="sxs-lookup"><span data-stu-id="da113-155">Creating a new Automation variable</span></span>

### <a name="toocreate-a-new-variable-with-hello-azure-portal"></a><span data-ttu-id="da113-156">toocreate novou proměnnou s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="da113-156">toocreate a new variable with hello Azure portal</span></span>

1. <span data-ttu-id="da113-157">Z vašeho účtu Automation, klikněte na tlačítko hello **prostředky** dlaždici a potom na hello **prostředky** vyberte **proměnné**.</span><span class="sxs-lookup"><span data-stu-id="da113-157">From your Automation account, click hello **Assets** tile and then on hello **Assets** blade, select **Variables**.</span></span>
2. <span data-ttu-id="da113-158">Na hello **proměnné** dlaždice, vyberte **přidat proměnnou**.</span><span class="sxs-lookup"><span data-stu-id="da113-158">On hello **Variables** tile, select **Add a variable**.</span></span>
3. <span data-ttu-id="da113-159">Dokončete hello možností na hello **nové proměnné** a klikněte na **vytvořit** uložení nové proměnné hello.</span><span class="sxs-lookup"><span data-stu-id="da113-159">Complete hello options on hello **New Variable** blade and click **Create** save hello new variable.</span></span>

### <a name="toocreate-a-new-variable-with-windows-powershell"></a><span data-ttu-id="da113-160">toocreate nové proměnné pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="da113-160">toocreate a new variable with Windows PowerShell</span></span>

<span data-ttu-id="da113-161">Hello [New-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) rutina vytvoří novou proměnnou a nastaví počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="da113-161">hello [New-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) cmdlet creates a new variable and sets its initial value.</span></span> <span data-ttu-id="da113-162">Můžete načíst pomocí hodnota hello [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx).</span><span class="sxs-lookup"><span data-stu-id="da113-162">You can retrieve hello value using [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx).</span></span> <span data-ttu-id="da113-163">Pokud hodnota hello je jednoduchý typ, je vrácena stejného typu.</span><span class="sxs-lookup"><span data-stu-id="da113-163">If hello value is a simple type, then that same type is returned.</span></span> <span data-ttu-id="da113-164">Pokud se jedná o komplexní typ, pak **PSCustomObject** je vrácen.</span><span class="sxs-lookup"><span data-stu-id="da113-164">If it’s a complex type, then a **PSCustomObject** is returned.</span></span>

<span data-ttu-id="da113-165">Hello následující ukázkové příkazy Zobrazit jak toocreate proměnné typu řetězec a pak se vraťte jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="da113-165">hello following sample commands show how toocreate a variable of type string and then return its value.</span></span>

    New-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" 
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
    –Encrypted $false –Value 'My String'
    $string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value

<span data-ttu-id="da113-166">Hello následující ukázkové příkazy Zobrazit jak toocreate proměnná s komplexní typ a pak se vraťte jeho vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="da113-166">hello following sample commands show how toocreate a variable with a complex type and then return its properties.</span></span> <span data-ttu-id="da113-167">V takovém případě objektu virtuálního počítače z **Get-AzureRmVm** se používá.</span><span class="sxs-lookup"><span data-stu-id="da113-167">In this case, a virtual machine object from **Get-AzureRmVm** is used.</span></span>

    $vm = Get-AzureRmVm -ResourceGroupName "ResourceGroup01" –Name "VM01"
    New-AzureRmAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm
    
    $vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
    $vmName = $vmValue.Name
    $vmIpAddress = $vmValue.IpAddress



## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a><span data-ttu-id="da113-168">Použití proměnné v runbooku nebo konfigurace DSC</span><span class="sxs-lookup"><span data-stu-id="da113-168">Using a variable in a runbook or DSC configuration</span></span>

<span data-ttu-id="da113-169">Použití hello **Set-AutomationVariable** aktivity tooset hello hodnoty proměnné automatizace sady runbook nebo konfigurace DSC a hello **Get-AutomationVariable** tooretrieve ho.</span><span class="sxs-lookup"><span data-stu-id="da113-169">Use hello **Set-AutomationVariable** activity tooset hello value of an Automation variable in a runbook or DSC configuration, and hello **Get-AutomationVariable** tooretrieve it.</span></span>  <span data-ttu-id="da113-170">Neměli byste používat hello **Set-AzureAutomationVariable** nebo **Get-AzureAutomationVariable** rutiny v runbooku nebo konfigurace DSC vzhledem k tomu, že jsou míň efektivní než hello aktivity pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="da113-170">You shouldn't use hello **Set-AzureAutomationVariable** or  **Get-AzureAutomationVariable** cmdlets in a runbook or DSC configuration since they are less efficient than hello workflow activities.</span></span>  <span data-ttu-id="da113-171">Také nelze načíst hodnotu hello zabezpečené proměnných s **Get-AzureAutomationVariable**.</span><span class="sxs-lookup"><span data-stu-id="da113-171">You also cannot retrieve hello value of secure variables with **Get-AzureAutomationVariable**.</span></span>  <span data-ttu-id="da113-172">Hello pouze způsob toocreate nové proměnné z v rámci sady runbook nebo konfigurace DSC je toouse hello [New-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="da113-172">hello only way toocreate a new variable from within a runbook or DSC configuration is toouse hello [New-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx)  cmdlet.</span></span>


### <a name="textual-runbook-samples"></a><span data-ttu-id="da113-173">Ukázky textové runbooků</span><span class="sxs-lookup"><span data-stu-id="da113-173">Textual runbook samples</span></span>

#### <a name="setting-and-retrieving-a-simple-value-from-a-variable"></a><span data-ttu-id="da113-174">Nastavení nebo načtení jednoduché hodnotu z proměnné</span><span class="sxs-lookup"><span data-stu-id="da113-174">Setting and retrieving a simple value from a variable</span></span>

<span data-ttu-id="da113-175">Hello následující ukázkové příkazy Zobrazit jak tooset a načíst proměnnou v textové runbooku.</span><span class="sxs-lookup"><span data-stu-id="da113-175">hello following sample commands show how tooset and retrieve a variable in a textual runbook.</span></span> <span data-ttu-id="da113-176">V této ukázce se předpokládá, že proměnné celočíselného typu s názvem *NumberOfIterations* a *NumberOfRunnings* a proměnná řetězcového typu nazvaná *SampleMessage* již byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="da113-176">In this sample, it is assumed that variables of type integer named *NumberOfIterations* and *NumberOfRunnings* and a variable of type string named *SampleMessage* have already been created.</span></span>

    $NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
    $NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
    $SampleMessage = Get-AutomationVariable -Name 'SampleMessage'
    
    Write-Output "Runbook has been run $NumberOfRunnings times."
    
    for ($i = 1; $i -le $NumberOfIterations; $i++) {
       Write-Output "$i`: $SampleMessage"
    }
    Set-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)

#### <a name="setting-and-retrieving-a-complex-object-in-a-variable"></a><span data-ttu-id="da113-177">Nastavení nebo načtení komplexní objekt v proměnné</span><span class="sxs-lookup"><span data-stu-id="da113-177">Setting and retrieving a complex object in a variable</span></span>

<span data-ttu-id="da113-178">Hello následující ukázkový kód ukazuje, jak tooupdate proměnná s komplexní hodnoty v textové runbooku.</span><span class="sxs-lookup"><span data-stu-id="da113-178">hello following sample code shows how tooupdate a variable with a complex value in a textual runbook.</span></span> <span data-ttu-id="da113-179">V této ukázce je načíst virtuální počítač Azure s **Get-AzureVM** a uložené tooan existující automatizace proměnnou.</span><span class="sxs-lookup"><span data-stu-id="da113-179">In this sample, an Azure virtual machine is retrieved with **Get-AzureVM** and saved tooan existing Automation variable.</span></span>  <span data-ttu-id="da113-180">Jak je popsáno v [typy proměnných](#variable-types), to je uloženo jako PSCustomObject.</span><span class="sxs-lookup"><span data-stu-id="da113-180">As explained in [Variable types](#variable-types), this is stored as a PSCustomObject.</span></span>

    $vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
    Set-AutomationVariable -Name "MyComplexVariable" -Value $vm


<span data-ttu-id="da113-181">V hello následující kód je hodnota hello načtena z hello proměnné a slouží toostart hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="da113-181">In hello following code, hello value is retrieved from hello variable and used toostart hello virtual machine.</span></span>

    $vmObject = Get-AutomationVariable -Name "MyComplexVariable"
    if ($vmObject.PowerState -eq 'Stopped') {
       Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
    }


#### <a name="setting-and-retrieving-a-collection-in-a-variable"></a><span data-ttu-id="da113-182">Nastavení nebo načtení kolekce v proměnné</span><span class="sxs-lookup"><span data-stu-id="da113-182">Setting and retrieving a collection in a variable</span></span>

<span data-ttu-id="da113-183">Hello následující ukázkový kód ukazuje, jak toouse proměnná s kolekce komplexní hodnoty v textové runbooku.</span><span class="sxs-lookup"><span data-stu-id="da113-183">hello following sample code shows how toouse a variable with a collection of complex values in a textual runbook.</span></span> <span data-ttu-id="da113-184">V této ukázce se načítají více virtuálními počítači Azure s **Get-AzureVM** a uložené tooan existující automatizace proměnnou.</span><span class="sxs-lookup"><span data-stu-id="da113-184">In this sample, multiple Azure virtual machines are retrieved with **Get-AzureVM** and saved tooan existing Automation variable.</span></span>  <span data-ttu-id="da113-185">Jak je popsáno v [typy proměnných](#variable-types), to je uloženo jako soubor PSCustomObjects.</span><span class="sxs-lookup"><span data-stu-id="da113-185">As explained in [Variable types](#variable-types), this is stored as a collection of PSCustomObjects.</span></span>

    $vms = Get-AzureVM | Where -FilterScript {$_.Name -match "my"}     
    Set-AutomationVariable -Name 'MyComplexVariable' -Value $vms

<span data-ttu-id="da113-186">V následujícím kódu hello hello kolekce se načítají hello proměnné a používat toostart každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="da113-186">In hello following code, hello collection is retrieved from hello variable and used toostart each virtual machine.</span></span>

    $vmValues = Get-AutomationVariable -Name "MyComplexVariable"
    ForEach ($vmValue in $vmValues)
    {
       if ($vmValue.PowerState -eq 'Stopped') {
          Start-AzureVM -ServiceName $vmValue.ServiceName -Name $vmValue.Name
       }
    }


### <a name="graphical-runbook-samples"></a><span data-ttu-id="da113-187">Ukázky grafický runbook</span><span class="sxs-lookup"><span data-stu-id="da113-187">Graphical runbook samples</span></span>

<span data-ttu-id="da113-188">V grafický runbook přidáte hello **Get-AutomationVariable** nebo **Set-AutomationVariable** kliknutím pravým tlačítkem myši na hello proměnné v podokně knihovna hello hello grafického editoru a výběrem hello aktivity, které chcete.</span><span class="sxs-lookup"><span data-stu-id="da113-188">In a graphical runbook, you add hello **Get-AutomationVariable** or **Set-AutomationVariable** by right-clicking on hello variable in hello Library pane of hello graphical editor and selecting hello activity you want.</span></span>

![Přidejte proměnnou toocanvas](media/automation-variables/runbook-variable-add-canvas.png)

#### <a name="setting-values-in-a-variable"></a><span data-ttu-id="da113-190">Nastavení hodnot v proměnné</span><span class="sxs-lookup"><span data-stu-id="da113-190">Setting values in a variable</span></span>
<span data-ttu-id="da113-191">Hello následující obrázek znázorňuje ukázkové aktivity tooupdate proměnné s hodnotou jednoduché v grafický runbook.</span><span class="sxs-lookup"><span data-stu-id="da113-191">hello following image shows sample activities tooupdate a variable with a simple value in a graphical runbook.</span></span> <span data-ttu-id="da113-192">V této ukázce je načíst jediný virtuální počítač Azure s **Get-AzureRmVM** a název počítače hello po uložení tooan existující automatizace proměnné typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="da113-192">In this sample, a single Azure virtual machine is retrieved with **Get-AzureRmVM** and hello computer name is saved tooan existing Automation variable with a type of String.</span></span>  <span data-ttu-id="da113-193">Nezávisle na tom, zda hello [odkaz je kanál, nebo pořadí](automation-graphical-authoring-intro.md#links-and-workflow) vzhledem k tomu, že pouze Očekáváme, že jeden objekt ve výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="da113-193">It doesn't matter whether hello [link is a pipeline or sequence](automation-graphical-authoring-intro.md#links-and-workflow) since we only expect a single object in hello output.</span></span>

![Sada jednoduché proměnné](media/automation-variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a><span data-ttu-id="da113-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="da113-195">Next Steps</span></span>

* <span data-ttu-id="da113-196">toolearn Další informace o připojení aktivity společně v vytváření grafického obsahu, najdete v části [odkazy v vytváření grafického obsahu](automation-graphical-authoring-intro.md#links-and-workflow)</span><span class="sxs-lookup"><span data-stu-id="da113-196">toolearn more about connecting activities together in graphical authoring, see [Links in graphical authoring](automation-graphical-authoring-intro.md#links-and-workflow)</span></span>
* <span data-ttu-id="da113-197">tooget kroky s grafickými runbooky najdete v části [Můj první grafický runbook](automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="da113-197">tooget started with Graphical runbooks, see [My first graphical runbook](automation-first-runbook-graphical.md)</span></span> 

