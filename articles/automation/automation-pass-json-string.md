---
title: "Objekt JSON předat runbook služby Azure Automation | Microsoft Docs"
description: "Jak předat parametry sady runbook jako objekt JSON"
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
keywords: "prostředí PowerShell, sady runbook, json, služby azure automation"
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 06/15/2017
ms.author: eslesar
ms.openlocfilehash: eac0e95a46731b9d396ea0590e629d61ca6a7d70
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="pass-a-json-object-to-an-azure-automation-runbook"></a><span data-ttu-id="eaa2b-104">Předat objekt JSON do runbooku automatizace Azure</span><span class="sxs-lookup"><span data-stu-id="eaa2b-104">Pass a JSON object to an Azure Automation runbook</span></span>

<span data-ttu-id="eaa2b-105">Může být užitečné k ukládání dat, který chcete předat do sady runbook v souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-105">It can be useful to store data that you want to pass to a runbook in a JSON file.</span></span>
<span data-ttu-id="eaa2b-106">Například může vytvořit soubor JSON, který obsahuje všechny parametry, které chcete předat k sadě runbook.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-106">For example, you might create a JSON file that contains all of the parameters you want to pass to a runbook.</span></span>
<span data-ttu-id="eaa2b-107">K tomuto účelu, budete muset převést na řetězec ve formátu JSON a pak převést řetězec na objektu prostředí PowerShell před předáním její obsah do runbooku.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-107">To do this, you have to convert the JSON to a string and then convert the string to a PowerShell object before passing its contents to the runbook.</span></span>

<span data-ttu-id="eaa2b-108">V tomto příkladu vytvoříme skript prostředí PowerShell, který volá [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) pro spuštění sady runbook PowerShell předávání obsah JSON do runbooku.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-108">In this example, we'll create a PowerShell script that calls [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) to start a PowerShell runbook, passing the contents of the JSON to the runbook.</span></span>
<span data-ttu-id="eaa2b-109">Powershellový runbook spustí virtuální počítač Azure, získávání parametrů pro virtuální počítač z formátu JSON, který byl předán v.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-109">The PowerShell runbook starts an Azure VM, getting the parameters for the VM from the JSON that was passed in.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eaa2b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="eaa2b-110">Prerequisites</span></span>
<span data-ttu-id="eaa2b-111">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="eaa2b-111">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="eaa2b-112">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-112">Azure subscription.</span></span> <span data-ttu-id="eaa2b-113">Pokud nemáte účet, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo <a href="/pricing/free-account/" target="_blank">[si zaregistrovat bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="eaa2b-113">If you don't have one yet, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or <a href="/pricing/free-account/" target="_blank">[sign up for a free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="eaa2b-114">[Účet Automation](automation-sec-configure-azure-runas-account.md), abyste si mohli runbook podržet a mohli ověřovat prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-114">[Automation account](automation-sec-configure-azure-runas-account.md) to hold the runbook and authenticate to Azure resources.</span></span>  <span data-ttu-id="eaa2b-115">Tento účet musí mít oprávnění ke spuštění a zastavení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-115">This account must have permission to start and stop the virtual machine.</span></span>
* <span data-ttu-id="eaa2b-116">Virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-116">An Azure virtual machine.</span></span> <span data-ttu-id="eaa2b-117">Počítač zastavíme a spustíme, proto to nesmí být produkční virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-117">We stop and start this machine so it should not be a production VM.</span></span>
* <span data-ttu-id="eaa2b-118">Azure Powershell nainstalovaný v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-118">Azure Powershell installed on a local machine.</span></span> <span data-ttu-id="eaa2b-119">V tématu [nainstalovat a nakonfigurovat Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) informace o tom, jak získat prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-119">See [Install and configure Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) for information about how to get Azure PowerShell.</span></span>

## <a name="create-the-json-file"></a><span data-ttu-id="eaa2b-120">Vytvořte soubor JSON</span><span class="sxs-lookup"><span data-stu-id="eaa2b-120">Create the JSON file</span></span>

<span data-ttu-id="eaa2b-121">Zadejte následující testu do textového souboru a uložte ho jako `test.json` někde v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-121">Type the following test in a text file, and save it as `test.json` somewhere on your local computer.</span></span>

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

## <a name="create-the-runbook"></a><span data-ttu-id="eaa2b-122">Vytvoření sady runbook</span><span class="sxs-lookup"><span data-stu-id="eaa2b-122">Create the runbook</span></span>

<span data-ttu-id="eaa2b-123">Vytvořte nový runbook prostředí PowerShell s názvem "Test-Json" ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-123">Create a new PowerShell runbook named "Test-Json" in Azure Automation.</span></span>
<span data-ttu-id="eaa2b-124">Naučte se vytvořit novou sadu runbook Powershellu, najdete v tématu [Můj první Powershellový runbook](automation-first-runbook-textual-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="eaa2b-124">To learn how to create a new PowerShell runbook, see [My first PowerShell runbook](automation-first-runbook-textual-powershell.md).</span></span>

<span data-ttu-id="eaa2b-125">Přijmout JSON data, sada runbook vyžaduje objekt jako vstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-125">To accept the JSON data, the runbook must take an object as an input parameter.</span></span>

<span data-ttu-id="eaa2b-126">Sady runbook pak můžete použít vlastnosti definované ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-126">The runbook can then use the properties defined in the JSON.</span></span>

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect to Azure account   
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object to actual JSON
$json = $json | ConvertFrom-Json

# Use the values from the JSON object as the parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
 ```

 <span data-ttu-id="eaa2b-127">Uložte a publikovat tuto sadu runbook v účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-127">Save and publish this runbook in your Automation account.</span></span>

## <a name="call-the-runbook-from-powershell"></a><span data-ttu-id="eaa2b-128">Volání sady runbook z prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="eaa2b-128">Call the runbook from PowerShell</span></span>

<span data-ttu-id="eaa2b-129">Nyní můžete volat sadu runbook z místního počítače pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-129">Now you can call the runbook from your local machine by using Azure PowerShell.</span></span>
<span data-ttu-id="eaa2b-130">Spusťte následující příkazy prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="eaa2b-130">Run the following PowerShell commands:</span></span>

1. <span data-ttu-id="eaa2b-131">Přihlaste se k Azure:</span><span class="sxs-lookup"><span data-stu-id="eaa2b-131">Log in to Azure:</span></span>
   ```powershell
   Login-AzureRmAccount
   ```
    <span data-ttu-id="eaa2b-132">Zobrazí se výzva k zadání přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-132">You are prompted to enter your Azure credentials.</span></span>
1. <span data-ttu-id="eaa2b-133">Získat obsah souboru JSON a převeďte ho na řetězec:</span><span class="sxs-lookup"><span data-stu-id="eaa2b-133">Get the contents of the JSON file and convert it to a string:</span></span>
    ```powershell
    $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
    ```
    <span data-ttu-id="eaa2b-134">`JsonPath`je cesta, kam jste uložili soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-134">`JsonPath` is the path where you saved the JSON file.</span></span>
1. <span data-ttu-id="eaa2b-135">Převést řetězec obsah `$json` na objekt prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="eaa2b-135">Convert the string contents of `$json` to a PowerShell object:</span></span>
   ```powershell
   $JsonParams = @{"json"=$json}
   ```
1. <span data-ttu-id="eaa2b-136">Vytvořit tabulku hash pro parametry `Start-AzureRmAutomstionRunbook`:</span><span class="sxs-lookup"><span data-stu-id="eaa2b-136">Create a hashtable for the parameters for `Start-AzureRmAutomstionRunbook`:</span></span>
   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```
   <span data-ttu-id="eaa2b-137">Všimněte si, že nastavíte hodnotu `Parameters` objekt prostředí PowerShell, který obsahuje hodnoty ze souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-137">Notice that you are setting the value of `Parameters` to the PowerShell object that contains the values from the JSON file.</span></span> 
1. <span data-ttu-id="eaa2b-138">Spuštění runbooku</span><span class="sxs-lookup"><span data-stu-id="eaa2b-138">Start the runbook</span></span>
   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

<span data-ttu-id="eaa2b-139">Sada runbook používá hodnoty ze souboru JSON pro spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="eaa2b-139">The runbook uses the values from the JSON file to start a VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eaa2b-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eaa2b-140">Next steps</span></span>

* <span data-ttu-id="eaa2b-141">Další informace o úpravách prostředí PowerShell a pracovní postup prostředí PowerShell sad runbook s textový editor, najdete v části [úpravy textovou sady runbook ve službě Azure Automation](automation-edit-textual-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="eaa2b-141">To learn more about editing PowerShell and PowerShell Workflow runbooks with a textual editor, see [Editing textual runbooks in Azure Automation](automation-edit-textual-runbook.md)</span></span> 
* <span data-ttu-id="eaa2b-142">Další informace o vytváření a import sad runbook najdete v tématu [vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="eaa2b-142">To learn more about creating and importing runbooks, see [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md)</span></span>


