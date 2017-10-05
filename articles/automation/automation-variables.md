---
title: "Proměnné prostředky ve službě Azure Automation | Microsoft Docs"
description: "Proměnné prostředky jsou hodnoty, které jsou k dispozici pro všechny runbooky a konfigurace DSC ve službě Azure Automation.  Tento článek vysvětluje podrobnosti proměnné a postupy pro práci s nimi v textové a grafické vytváření."
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
ms.openlocfilehash: dc00e1e5fa8df5cb55e7e2672137d1df44133773
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="variable-assets-in-azure-automation"></a><span data-ttu-id="4226f-104">Proměnné prostředky ve službě Azure Automation</span><span class="sxs-lookup"><span data-stu-id="4226f-104">Variable assets in Azure Automation</span></span>

<span data-ttu-id="4226f-105">Proměnné prostředky jsou hodnoty, které jsou k dispozici pro všechny runbooky a konfigurace DSC v účtu automation.</span><span class="sxs-lookup"><span data-stu-id="4226f-105">Variable assets are values that are available to all runbooks and DSC configurations in your automation account.</span></span> <span data-ttu-id="4226f-106">Budou se dají vytvořit, upravit a načíst z portálu Azure, prostředí Windows PowerShell a v rámci sady runbook nebo konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="4226f-106">They can be created, modified, and retrieved from the Azure portal, Windows PowerShell, and from within a runbook or DSC configuration.</span></span> <span data-ttu-id="4226f-107">Proměnné služeb automatizace jsou užitečné pro následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="4226f-107">Automation variables are useful for the following scenarios:</span></span>

- <span data-ttu-id="4226f-108">Sdílení hodnoty mezi několika runbooky nebo konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="4226f-108">Share a value between multiple runbooks or DSC configurations.</span></span>

- <span data-ttu-id="4226f-109">Sdílení hodnoty mezi několika úlohami ze stejné sady runbook nebo konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="4226f-109">Share a value between multiple jobs from the same runbook or DSC configuration.</span></span>

- <span data-ttu-id="4226f-110">Správa hodnoty z portálu nebo z příkazového řádku prostředí Windows PowerShell, který je používán sady runbook nebo konfigurace DSC, jako je například sada běžné položky konfigurace jako konkrétní seznam názvů virtuálních počítačů, určité skupiny zdrojů, název domény Active Directory, atd.</span><span class="sxs-lookup"><span data-stu-id="4226f-110">Manage a value from the portal or from the Windows PowerShell command line that is used by runbooks or DSC configurations, such as a set of common configuration items like specific list of VM names, a specific resource group,  an AD domain name, etc.</span></span>  

<span data-ttu-id="4226f-111">Proměnné služeb automatizace jsou trvalé, takže budou nadále k dispozici i v případě, že sada runbook nebo konfigurace DSC se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4226f-111">Automation variables are persisted so that they continue to be available even if the runbook or DSC configuration fails.</span></span>  <span data-ttu-id="4226f-112">To také umožňuje nastavit jednom runbooku, který je pak použije v jiném, nebo se používá stejné sady runbook nebo konfigurace DSC při příštím spuštění hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4226f-112">This also allows a value to be set by one runbook that is then used by another, or is used by the same runbook or DSC configuration the next time that it is run.</span></span>

<span data-ttu-id="4226f-113">Při vytvoření proměnné můžete určit, že se uloží šifrované.</span><span class="sxs-lookup"><span data-stu-id="4226f-113">When a variable is created, you can specify that it be stored encrypted.</span></span>  <span data-ttu-id="4226f-114">Když je proměnná zašifrovaná, bezpečně se uloží ve službě Azure Automation a jeho hodnotu nelze načíst z [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) rutinu, která se dodává jako součást modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4226f-114">When a variable is encrypted, it is stored securely in Azure Automation, and its value cannot be retrieved from the [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) cmdlet that ships as part of the Azure PowerShell module.</span></span>  <span data-ttu-id="4226f-115">Jediným způsobem, že šifrovaná hodnota se dá načíst je z **Get-AutomationVariable** aktivity v sady runbook nebo konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="4226f-115">The only way that an encrypted value can be retrieved is from the **Get-AutomationVariable** activity in a runbook or DSC configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="4226f-116">Zabezpečené prostředky ve službě Azure Automation zahrnovat přihlašovací údaje, připojení, certifikátů a zašifrované proměnné.</span><span class="sxs-lookup"><span data-stu-id="4226f-116">Secure assets in Azure Automation include credentials, certificates, connections, and encrypted variables.</span></span> <span data-ttu-id="4226f-117">Tyto prostředky jsou zašifrovány a uložené ve službě Azure Automation pomocí jedinečný klíč, který se vygeneruje pro každý účet automation.</span><span class="sxs-lookup"><span data-stu-id="4226f-117">These assets are encrypted and stored in the Azure Automation using a unique key that is generated for each automation account.</span></span> <span data-ttu-id="4226f-118">Tento klíč se šifruje pomocí hlavního certifikátu a uloží ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="4226f-118">This key is encrypted by a master certificate and stored in Azure Automation.</span></span> <span data-ttu-id="4226f-119">Před ukládání o zabezpečený prostředek, klíč pro účet služby automation jsou dešifrována pomocí hlavního certifikátu a pak se použije k zašifrování asset.</span><span class="sxs-lookup"><span data-stu-id="4226f-119">Before storing a secure asset, the key for the automation account is decrypted using the master certificate and then used to encrypt the asset.</span></span>

## <a name="variable-types"></a><span data-ttu-id="4226f-120">Typy proměnných</span><span class="sxs-lookup"><span data-stu-id="4226f-120">Variable types</span></span>

<span data-ttu-id="4226f-121">Při vytváření proměnné pomocí portálu Azure, musíte zadat datový typ z rozevíracího seznamu, tak na portálu můžete zobrazit příslušný ovládací prvek pro zadání hodnotu proměnné.</span><span class="sxs-lookup"><span data-stu-id="4226f-121">When you create a variable with the Azure portal, you must specify a data type from the drop-down list so the portal can display the appropriate control for entering the variable value.</span></span> <span data-ttu-id="4226f-122">Proměnná není omezen na tento typ dat, ale musíte nastavit proměnnou pomocí prostředí Windows PowerShell, pokud chcete zadat hodnotu jiného typu.</span><span class="sxs-lookup"><span data-stu-id="4226f-122">The variable is not restricted to this data type, but you must set the variable using Windows PowerShell if you want to specify a value of a different type.</span></span> <span data-ttu-id="4226f-123">Pokud zadáte **není definována**, bude nastavena hodnotu proměnné **$null**, a je nutné nastavit hodnotu s [Set-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) rutiny nebo **Set-AutomationVariable** aktivity.</span><span class="sxs-lookup"><span data-stu-id="4226f-123">If you specify **Not defined**, then the value of the variable will be set to **$null**, and you must set the value with the [Set-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) cmdlet or **Set-AutomationVariable** activity.</span></span>  <span data-ttu-id="4226f-124">Nelze vytvořit nebo změnit hodnotu pro proměnnou komplexní typ na portálu, ale můžete zadat hodnotu libovolného typu pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4226f-124">You cannot create or change the value for a complex variable type in the portal, but you can provide a value of any type using Windows PowerShell.</span></span> <span data-ttu-id="4226f-125">Komplexní typy bude vrácen jako [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx).</span><span class="sxs-lookup"><span data-stu-id="4226f-125">Complex types will be returned as a [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx).</span></span>

<span data-ttu-id="4226f-126">Vytváření zatřiďovací tabulky nebo pole a jeho uložením do proměnné můžete ukládat víc hodnot do jedné proměnné.</span><span class="sxs-lookup"><span data-stu-id="4226f-126">You can store multiple values to a single variable by creating an array or hashtable and saving it to the variable.</span></span>

<span data-ttu-id="4226f-127">Následuje seznam proměnných typy, které jsou dostupné ve službě Automation:</span><span class="sxs-lookup"><span data-stu-id="4226f-127">The following are a list of variable types available in Automation:</span></span>

* <span data-ttu-id="4226f-128">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4226f-128">String</span></span>
* <span data-ttu-id="4226f-129">Integer</span><span class="sxs-lookup"><span data-stu-id="4226f-129">Integer</span></span>
* <span data-ttu-id="4226f-130">Data a času</span><span class="sxs-lookup"><span data-stu-id="4226f-130">DateTime</span></span>
* <span data-ttu-id="4226f-131">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="4226f-131">Boolean</span></span>
* <span data-ttu-id="4226f-132">Hodnotu Null</span><span class="sxs-lookup"><span data-stu-id="4226f-132">Null</span></span>

## <a name="cmdlets-and-workflow-activities"></a><span data-ttu-id="4226f-133">Rutiny a pracovní postup aktivity</span><span class="sxs-lookup"><span data-stu-id="4226f-133">Cmdlets and workflow activities</span></span>

<span data-ttu-id="4226f-134">Rutiny v následující tabulce se používají k vytváření a správě proměnných Automation pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4226f-134">The cmdlets in the following table are used to create and manage Automation variables with Windows PowerShell.</span></span> <span data-ttu-id="4226f-135">Se dodávají jako součást [modul Azure PowerShell](../powershell-install-configure.md) která je k dispozici pro použití v runbooků služeb automatizace a konfigurace DSC.</span><span class="sxs-lookup"><span data-stu-id="4226f-135">They ship as part of the [Azure PowerShell module](../powershell-install-configure.md) which is available for use in Automation runbooks and DSC configuration.</span></span>

|<span data-ttu-id="4226f-136">Rutiny</span><span class="sxs-lookup"><span data-stu-id="4226f-136">Cmdlets</span></span>|<span data-ttu-id="4226f-137">Popis</span><span class="sxs-lookup"><span data-stu-id="4226f-137">Description</span></span>|
|:---|:---|
|[<span data-ttu-id="4226f-138">Get-AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="4226f-138">Get-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt603849.aspx)|<span data-ttu-id="4226f-139">Načte hodnotu existující proměnné.</span><span class="sxs-lookup"><span data-stu-id="4226f-139">Retrieves the value of an existing variable.</span></span>|
|[<span data-ttu-id="4226f-140">Nové AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="4226f-140">New-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt603613.aspx)|<span data-ttu-id="4226f-141">Vytvoří novou proměnnou a nastaví její hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4226f-141">Creates a new variable and sets its value.</span></span>|
|[<span data-ttu-id="4226f-142">Odebrat AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="4226f-142">Remove-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt619354.aspx)|<span data-ttu-id="4226f-143">Odebere existující proměnnou.</span><span class="sxs-lookup"><span data-stu-id="4226f-143">Removes an existing variable.</span></span>|
|[<span data-ttu-id="4226f-144">Set-AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="4226f-144">Set-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt603601.aspx)|<span data-ttu-id="4226f-145">Nastaví hodnotu existující proměnné.</span><span class="sxs-lookup"><span data-stu-id="4226f-145">Sets the value for an existing variable.</span></span>|

<span data-ttu-id="4226f-146">Aktivity pracovního postupu v následující tabulce se používají pro přístup k proměnným automatizace v sadě runbook.</span><span class="sxs-lookup"><span data-stu-id="4226f-146">The workflow activities in the following table are used to access Automation variables in a runbook.</span></span> <span data-ttu-id="4226f-147">Jsou dostupné pouze pro použití v runbooku nebo konfigurace DSC a se nedodává jako součást modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4226f-147">They are only available for use in a runbook or DSC configuration, and do not ship as part of the Azure PowerShell module.</span></span>

|<span data-ttu-id="4226f-148">Aktivity pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="4226f-148">Workflow Activities</span></span>|<span data-ttu-id="4226f-149">Popis</span><span class="sxs-lookup"><span data-stu-id="4226f-149">Description</span></span>|
|:---|:---|
|<span data-ttu-id="4226f-150">Get-AutomationVariable</span><span class="sxs-lookup"><span data-stu-id="4226f-150">Get-AutomationVariable</span></span>|<span data-ttu-id="4226f-151">Načte hodnotu existující proměnné.</span><span class="sxs-lookup"><span data-stu-id="4226f-151">Retrieves the value of an existing variable.</span></span>|
|<span data-ttu-id="4226f-152">Set-AutomationVariable</span><span class="sxs-lookup"><span data-stu-id="4226f-152">Set-AutomationVariable</span></span>|<span data-ttu-id="4226f-153">Nastaví hodnotu existující proměnné.</span><span class="sxs-lookup"><span data-stu-id="4226f-153">Sets the value for an existing variable.</span></span>|

> [!NOTE] 
> <span data-ttu-id="4226f-154">Byste neměli používat proměnné v parametru – Name **Get-AutomationVariable** v sady runbook nebo konfigurace DSC, protože to může zkomplikovat zjišťování závislostí mezi runbooky nebo konfigurace DSC a proměnné služeb automatizace v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="4226f-154">You should avoid using variables in the –Name parameter of **Get-AutomationVariable**  in a runbook or DSC configuration since this can complicate discovering dependencies between runbooks or DSC configuration, and Automation variables at design time.</span></span>

## <a name="creating-a-new-automation-variable"></a><span data-ttu-id="4226f-155">Vytvoření nové proměnné Automation</span><span class="sxs-lookup"><span data-stu-id="4226f-155">Creating a new Automation variable</span></span>

### <a name="to-create-a-new-variable-with-the-azure-portal"></a><span data-ttu-id="4226f-156">Chcete-li vytvořit nové proměnné pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4226f-156">To create a new variable with the Azure portal</span></span>

1. <span data-ttu-id="4226f-157">Z vašeho účtu Automation, klikněte na tlačítko **prostředky** dlaždici a potom na **prostředky** vyberte **proměnné**.</span><span class="sxs-lookup"><span data-stu-id="4226f-157">From your Automation account, click the **Assets** tile and then on the **Assets** blade, select **Variables**.</span></span>
2. <span data-ttu-id="4226f-158">Na **proměnné** dlaždice, vyberte **přidat proměnnou**.</span><span class="sxs-lookup"><span data-stu-id="4226f-158">On the **Variables** tile, select **Add a variable**.</span></span>
3. <span data-ttu-id="4226f-159">Dokončete možností na **nové proměnné** a klikněte na **vytvořit** uložení nové proměnné.</span><span class="sxs-lookup"><span data-stu-id="4226f-159">Complete the options on the **New Variable** blade and click **Create** save the new variable.</span></span>

### <a name="to-create-a-new-variable-with-windows-powershell"></a><span data-ttu-id="4226f-160">Chcete-li vytvořit nové proměnné pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="4226f-160">To create a new variable with Windows PowerShell</span></span>

<span data-ttu-id="4226f-161">[New-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) rutina vytvoří novou proměnnou a nastaví počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4226f-161">The [New-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) cmdlet creates a new variable and sets its initial value.</span></span> <span data-ttu-id="4226f-162">Můžete načíst pomocí hodnota [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx).</span><span class="sxs-lookup"><span data-stu-id="4226f-162">You can retrieve the value using [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx).</span></span> <span data-ttu-id="4226f-163">Pokud je hodnota jednoduchého typu, je vrácena stejného typu.</span><span class="sxs-lookup"><span data-stu-id="4226f-163">If the value is a simple type, then that same type is returned.</span></span> <span data-ttu-id="4226f-164">Pokud se jedná o komplexní typ, pak **PSCustomObject** je vrácen.</span><span class="sxs-lookup"><span data-stu-id="4226f-164">If it’s a complex type, then a **PSCustomObject** is returned.</span></span>

<span data-ttu-id="4226f-165">Následující vzorové příkazy znázorňují postup vytvoření proměnné typu řetězec a pak se vraťte jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4226f-165">The following sample commands show how to create a variable of type string and then return its value.</span></span>

    New-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" 
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
    –Encrypted $false –Value 'My String'
    $string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value

<span data-ttu-id="4226f-166">Následující vzorové příkazy znázorňují postup vytvoření proměnné s komplexní typ a pak se vraťte jeho vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4226f-166">The following sample commands show how to create a variable with a complex type and then return its properties.</span></span> <span data-ttu-id="4226f-167">V takovém případě objektu virtuálního počítače z **Get-AzureRmVm** se používá.</span><span class="sxs-lookup"><span data-stu-id="4226f-167">In this case, a virtual machine object from **Get-AzureRmVm** is used.</span></span>

    $vm = Get-AzureRmVm -ResourceGroupName "ResourceGroup01" –Name "VM01"
    New-AzureRmAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm
    
    $vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
    $vmName = $vmValue.Name
    $vmIpAddress = $vmValue.IpAddress



## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a><span data-ttu-id="4226f-168">Použití proměnné v runbooku nebo konfigurace DSC</span><span class="sxs-lookup"><span data-stu-id="4226f-168">Using a variable in a runbook or DSC configuration</span></span>

<span data-ttu-id="4226f-169">Použití **Set-AutomationVariable** aktivity do hodnoty proměnné automatizace sady runbook nebo konfigurace DSC a **Get-AutomationVariable** ho chcete zjistit.</span><span class="sxs-lookup"><span data-stu-id="4226f-169">Use the **Set-AutomationVariable** activity to set the value of an Automation variable in a runbook or DSC configuration, and the **Get-AutomationVariable** to retrieve it.</span></span>  <span data-ttu-id="4226f-170">Neměli byste používat **Set-AzureAutomationVariable** nebo **Get-AzureAutomationVariable** rutiny v runbooku nebo konfigurace DSC vzhledem k tomu, že jsou míň efektivní než aktivity pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="4226f-170">You shouldn't use the **Set-AzureAutomationVariable** or  **Get-AzureAutomationVariable** cmdlets in a runbook or DSC configuration since they are less efficient than the workflow activities.</span></span>  <span data-ttu-id="4226f-171">Také nelze načíst hodnotu proměnných zabezpečené se **Get-AzureAutomationVariable**.</span><span class="sxs-lookup"><span data-stu-id="4226f-171">You also cannot retrieve the value of secure variables with **Get-AzureAutomationVariable**.</span></span>  <span data-ttu-id="4226f-172">Jediným způsobem, jak vytvořit nové proměnné z v rámci sady runbook nebo konfigurace DSC se má používat [New-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="4226f-172">The only way to create a new variable from within a runbook or DSC configuration is to use the [New-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx)  cmdlet.</span></span>


### <a name="textual-runbook-samples"></a><span data-ttu-id="4226f-173">Ukázky textové runbooků</span><span class="sxs-lookup"><span data-stu-id="4226f-173">Textual runbook samples</span></span>

#### <a name="setting-and-retrieving-a-simple-value-from-a-variable"></a><span data-ttu-id="4226f-174">Nastavení nebo načtení jednoduché hodnotu z proměnné</span><span class="sxs-lookup"><span data-stu-id="4226f-174">Setting and retrieving a simple value from a variable</span></span>

<span data-ttu-id="4226f-175">Následující vzorové příkazy ukazují, jak nastavit a načíst proměnnou v textový.</span><span class="sxs-lookup"><span data-stu-id="4226f-175">The following sample commands show how to set and retrieve a variable in a textual runbook.</span></span> <span data-ttu-id="4226f-176">V této ukázce se předpokládá, že proměnné celočíselného typu s názvem *NumberOfIterations* a *NumberOfRunnings* a proměnná řetězcového typu nazvaná *SampleMessage* již byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="4226f-176">In this sample, it is assumed that variables of type integer named *NumberOfIterations* and *NumberOfRunnings* and a variable of type string named *SampleMessage* have already been created.</span></span>

    $NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
    $NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
    $SampleMessage = Get-AutomationVariable -Name 'SampleMessage'
    
    Write-Output "Runbook has been run $NumberOfRunnings times."
    
    for ($i = 1; $i -le $NumberOfIterations; $i++) {
       Write-Output "$i`: $SampleMessage"
    }
    Set-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)

#### <a name="setting-and-retrieving-a-complex-object-in-a-variable"></a><span data-ttu-id="4226f-177">Nastavení nebo načtení komplexní objekt v proměnné</span><span class="sxs-lookup"><span data-stu-id="4226f-177">Setting and retrieving a complex object in a variable</span></span>

<span data-ttu-id="4226f-178">Následující vzorový kód ukazuje, jak aktualizovat proměnnou s komplexní hodnoty v textové runbooku.</span><span class="sxs-lookup"><span data-stu-id="4226f-178">The following sample code shows how to update a variable with a complex value in a textual runbook.</span></span> <span data-ttu-id="4226f-179">V této ukázce je načíst virtuální počítač Azure s **Get-AzureVM** a uložit do existující automatizace proměnné.</span><span class="sxs-lookup"><span data-stu-id="4226f-179">In this sample, an Azure virtual machine is retrieved with **Get-AzureVM** and saved to an existing Automation variable.</span></span>  <span data-ttu-id="4226f-180">Jak je popsáno v [typy proměnných](#variable-types), to je uloženo jako PSCustomObject.</span><span class="sxs-lookup"><span data-stu-id="4226f-180">As explained in [Variable types](#variable-types), this is stored as a PSCustomObject.</span></span>

    $vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
    Set-AutomationVariable -Name "MyComplexVariable" -Value $vm


<span data-ttu-id="4226f-181">V následujícím kódu je hodnota načítají proměnnou a používá ke spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4226f-181">In the following code, the value is retrieved from the variable and used to start the virtual machine.</span></span>

    $vmObject = Get-AutomationVariable -Name "MyComplexVariable"
    if ($vmObject.PowerState -eq 'Stopped') {
       Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
    }


#### <a name="setting-and-retrieving-a-collection-in-a-variable"></a><span data-ttu-id="4226f-182">Nastavení nebo načtení kolekce v proměnné</span><span class="sxs-lookup"><span data-stu-id="4226f-182">Setting and retrieving a collection in a variable</span></span>

<span data-ttu-id="4226f-183">Následující vzorový kód ukazuje, jak používat proměnné s kolekcí komplexní hodnoty v textové runbooku.</span><span class="sxs-lookup"><span data-stu-id="4226f-183">The following sample code shows how to use a variable with a collection of complex values in a textual runbook.</span></span> <span data-ttu-id="4226f-184">V této ukázce se načítají více virtuálními počítači Azure s **Get-AzureVM** a uložit do existující automatizace proměnné.</span><span class="sxs-lookup"><span data-stu-id="4226f-184">In this sample, multiple Azure virtual machines are retrieved with **Get-AzureVM** and saved to an existing Automation variable.</span></span>  <span data-ttu-id="4226f-185">Jak je popsáno v [typy proměnných](#variable-types), to je uloženo jako soubor PSCustomObjects.</span><span class="sxs-lookup"><span data-stu-id="4226f-185">As explained in [Variable types](#variable-types), this is stored as a collection of PSCustomObjects.</span></span>

    $vms = Get-AzureVM | Where -FilterScript {$_.Name -match "my"}     
    Set-AutomationVariable -Name 'MyComplexVariable' -Value $vms

<span data-ttu-id="4226f-186">V následujícím kódu je kolekce načítají proměnnou a používá ke spuštění každého virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4226f-186">In the following code, the collection is retrieved from the variable and used to start each virtual machine.</span></span>

    $vmValues = Get-AutomationVariable -Name "MyComplexVariable"
    ForEach ($vmValue in $vmValues)
    {
       if ($vmValue.PowerState -eq 'Stopped') {
          Start-AzureVM -ServiceName $vmValue.ServiceName -Name $vmValue.Name
       }
    }


### <a name="graphical-runbook-samples"></a><span data-ttu-id="4226f-187">Ukázky grafický runbook</span><span class="sxs-lookup"><span data-stu-id="4226f-187">Graphical runbook samples</span></span>

<span data-ttu-id="4226f-188">V grafický runbook přidáte **Get-AutomationVariable** nebo **Set-AutomationVariable** kliknutím pravým tlačítkem myši na proměnnou v podokně knihovna grafického editoru a výběrem aktivity, které chcete.</span><span class="sxs-lookup"><span data-stu-id="4226f-188">In a graphical runbook, you add the **Get-AutomationVariable** or **Set-AutomationVariable** by right-clicking on the variable in the Library pane of the graphical editor and selecting the activity you want.</span></span>

![Přidání proměnné na plátno](media/automation-variables/runbook-variable-add-canvas.png)

#### <a name="setting-values-in-a-variable"></a><span data-ttu-id="4226f-190">Nastavení hodnot v proměnné</span><span class="sxs-lookup"><span data-stu-id="4226f-190">Setting values in a variable</span></span>
<span data-ttu-id="4226f-191">Následující obrázek znázorňuje ukázkové aktivity se aktualizovat proměnnou s hodnotou jednoduché v grafický runbook.</span><span class="sxs-lookup"><span data-stu-id="4226f-191">The following image shows sample activities to update a variable with a simple value in a graphical runbook.</span></span> <span data-ttu-id="4226f-192">V této ukázce je načíst jediný virtuální počítač Azure s **Get-AzureRmVM** a název počítače je uložen do existující automatizace proměnnou s typem řetězce.</span><span class="sxs-lookup"><span data-stu-id="4226f-192">In this sample, a single Azure virtual machine is retrieved with **Get-AzureRmVM** and the computer name is saved to an existing Automation variable with a type of String.</span></span>  <span data-ttu-id="4226f-193">Nezávisle na tom, zda [odkaz je kanál, nebo pořadí](automation-graphical-authoring-intro.md#links-and-workflow) vzhledem k tomu, že pouze Očekáváme, že jeden objekt ve výstupu.</span><span class="sxs-lookup"><span data-stu-id="4226f-193">It doesn't matter whether the [link is a pipeline or sequence](automation-graphical-authoring-intro.md#links-and-workflow) since we only expect a single object in the output.</span></span>

![Sada jednoduché proměnné](media/automation-variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a><span data-ttu-id="4226f-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4226f-195">Next Steps</span></span>

* <span data-ttu-id="4226f-196">Další informace o připojení aktivity společně v vytváření grafického obsahu najdete v tématu [odkazy v vytváření grafického obsahu](automation-graphical-authoring-intro.md#links-and-workflow)</span><span class="sxs-lookup"><span data-stu-id="4226f-196">To learn more about connecting activities together in graphical authoring, see [Links in graphical authoring](automation-graphical-authoring-intro.md#links-and-workflow)</span></span>
* <span data-ttu-id="4226f-197">První kroky s grafickými runbooky najdete v článku [Můj první grafický runbook](automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="4226f-197">To get started with Graphical runbooks, see [My first graphical runbook](automation-first-runbook-graphical.md)</span></span> 

