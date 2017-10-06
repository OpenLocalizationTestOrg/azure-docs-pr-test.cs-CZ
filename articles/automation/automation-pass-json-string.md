---
title: aaaPass JSON objektu tooan runbook automatizace Azure. | Microsoft Docs
description: Jak toopass parametry tooa runbook jako objekt JSON
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
ms.openlocfilehash: 8229a16015d549927ead5496c70e9fb391d35498
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="pass-a-json-object-tooan-azure-automation-runbook"></a><span data-ttu-id="91569-104">Předat Azure Automation runbook tooan objekt JSON</span><span class="sxs-lookup"><span data-stu-id="91569-104">Pass a JSON object tooan Azure Automation runbook</span></span>

<span data-ttu-id="91569-105">Může být užitečné toostore dat, které chcete toopass tooa runbook v souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="91569-105">It can be useful toostore data that you want toopass tooa runbook in a JSON file.</span></span>
<span data-ttu-id="91569-106">Například může vytvořit soubor JSON, který obsahuje všechny parametry hello chcete toopass tooa runbook.</span><span class="sxs-lookup"><span data-stu-id="91569-106">For example, you might create a JSON file that contains all of hello parameters you want toopass tooa runbook.</span></span>
<span data-ttu-id="91569-107">toodo, můžete mít tooconvert hello JSON tooa řetězec a pak převést objekt prostředí PowerShell tooa řetězce hello před předáním runbook toohello jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="91569-107">toodo this, you have tooconvert hello JSON tooa string and then convert hello string tooa PowerShell object before passing its contents toohello runbook.</span></span>

<span data-ttu-id="91569-108">V tomto příkladu vytvoříme skript prostředí PowerShell, který volá [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart Powershellového runbooku, předávání hello obsah sady hello JSON toohello runbook.</span><span class="sxs-lookup"><span data-stu-id="91569-108">In this example, we'll create a PowerShell script that calls [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart a PowerShell runbook, passing hello contents of hello JSON toohello runbook.</span></span>
<span data-ttu-id="91569-109">Hello Powershellový runbook spustí virtuální počítač Azure, získávání parametrů hello hello virtuálních počítačů z formátu JSON, který byl předán v hello.</span><span class="sxs-lookup"><span data-stu-id="91569-109">hello PowerShell runbook starts an Azure VM, getting hello parameters for hello VM from hello JSON that was passed in.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="91569-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="91569-110">Prerequisites</span></span>
<span data-ttu-id="91569-111">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="91569-111">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="91569-112">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="91569-112">Azure subscription.</span></span> <span data-ttu-id="91569-113">Pokud nemáte účet, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo <a href="/pricing/free-account/" target="_blank">[si zaregistrovat bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="91569-113">If you don't have one yet, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or <a href="/pricing/free-account/" target="_blank">[sign up for a free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="91569-114">[Účet Automation](automation-sec-configure-azure-runas-account.md) toohold hello sady runbook a ověření tooAzure prostředky.</span><span class="sxs-lookup"><span data-stu-id="91569-114">[Automation account](automation-sec-configure-azure-runas-account.md) toohold hello runbook and authenticate tooAzure resources.</span></span>  <span data-ttu-id="91569-115">Tento účet musí mít oprávnění toostart a zastavit hello virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="91569-115">This account must have permission toostart and stop hello virtual machine.</span></span>
* <span data-ttu-id="91569-116">Virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="91569-116">An Azure virtual machine.</span></span> <span data-ttu-id="91569-117">Počítač zastavíme a spustíme, proto to nesmí být produkční virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="91569-117">We stop and start this machine so it should not be a production VM.</span></span>
* <span data-ttu-id="91569-118">Azure Powershell nainstalovaný v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="91569-118">Azure Powershell installed on a local machine.</span></span> <span data-ttu-id="91569-119">V tématu [nainstalovat a nakonfigurovat Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) informace o tooget prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="91569-119">See [Install and configure Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) for information about how tooget Azure PowerShell.</span></span>

## <a name="create-hello-json-file"></a><span data-ttu-id="91569-120">Vytvořte soubor JSON hello</span><span class="sxs-lookup"><span data-stu-id="91569-120">Create hello JSON file</span></span>

<span data-ttu-id="91569-121">Typ hello následující testování do textového souboru a uložte ho jako `test.json` někde v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="91569-121">Type hello following test in a text file, and save it as `test.json` somewhere on your local computer.</span></span>

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

## <a name="create-hello-runbook"></a><span data-ttu-id="91569-122">Vytvoření sady runbook hello</span><span class="sxs-lookup"><span data-stu-id="91569-122">Create hello runbook</span></span>

<span data-ttu-id="91569-123">Vytvořte nový runbook prostředí PowerShell s názvem "Test-Json" ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="91569-123">Create a new PowerShell runbook named "Test-Json" in Azure Automation.</span></span>
<span data-ttu-id="91569-124">toolearn jak toocreate novou sadu runbook Powershellu, najdete v části [Můj první Powershellový runbook](automation-first-runbook-textual-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="91569-124">toolearn how toocreate a new PowerShell runbook, see [My first PowerShell runbook](automation-first-runbook-textual-powershell.md).</span></span>

<span data-ttu-id="91569-125">tooaccept hello JSON data, vyžaduje hello runbook objekt jako vstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="91569-125">tooaccept hello JSON data, hello runbook must take an object as an input parameter.</span></span>

<span data-ttu-id="91569-126">Hello runbook pak můžete použít hello vlastnosti definované v hello JSON.</span><span class="sxs-lookup"><span data-stu-id="91569-126">hello runbook can then use hello properties defined in hello JSON.</span></span>

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect tooAzure account   
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object tooactual JSON
$json = $json | ConvertFrom-Json

# Use hello values from hello JSON object as hello parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
 ```

 <span data-ttu-id="91569-127">Uložte a publikovat tuto sadu runbook v účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="91569-127">Save and publish this runbook in your Automation account.</span></span>

## <a name="call-hello-runbook-from-powershell"></a><span data-ttu-id="91569-128">Volání sady runbook hello z prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="91569-128">Call hello runbook from PowerShell</span></span>

<span data-ttu-id="91569-129">Nyní můžete volat hello runbook z místního počítače pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="91569-129">Now you can call hello runbook from your local machine by using Azure PowerShell.</span></span>
<span data-ttu-id="91569-130">Spusťte následující příkazy prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="91569-130">Run hello following PowerShell commands:</span></span>

1. <span data-ttu-id="91569-131">Přihlaste se tooAzure:</span><span class="sxs-lookup"><span data-stu-id="91569-131">Log in tooAzure:</span></span>
   ```powershell
   Login-AzureRmAccount
   ```
    <span data-ttu-id="91569-132">Můžete se výzvami tooenter vaše přihlašovací údaje Azure.</span><span class="sxs-lookup"><span data-stu-id="91569-132">You are prompted tooenter your Azure credentials.</span></span>
1. <span data-ttu-id="91569-133">Získat hello obsah souboru JSON hello a převést ho tooa řetězec:</span><span class="sxs-lookup"><span data-stu-id="91569-133">Get hello contents of hello JSON file and convert it tooa string:</span></span>
    ```powershell
    $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
    ```
    <span data-ttu-id="91569-134">`JsonPath`je hello cesta, kam jste uložili soubor JSON hello.</span><span class="sxs-lookup"><span data-stu-id="91569-134">`JsonPath` is hello path where you saved hello JSON file.</span></span>
1. <span data-ttu-id="91569-135">Převést řetězec obsah hello `$json` tooa objekt prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="91569-135">Convert hello string contents of `$json` tooa PowerShell object:</span></span>
   ```powershell
   $JsonParams = @{"json"=$json}
   ```
1. <span data-ttu-id="91569-136">Vytvořit tabulku hash pro parametry hello `Start-AzureRmAutomstionRunbook`:</span><span class="sxs-lookup"><span data-stu-id="91569-136">Create a hashtable for hello parameters for `Start-AzureRmAutomstionRunbook`:</span></span>
   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```
   <span data-ttu-id="91569-137">Všimněte si, že nastavíte hodnotu hello `Parameters` toohello prostředí PowerShell objekt, který obsahuje hodnoty hello ze souboru JSON hello.</span><span class="sxs-lookup"><span data-stu-id="91569-137">Notice that you are setting hello value of `Parameters` toohello PowerShell object that contains hello values from hello JSON file.</span></span> 
1. <span data-ttu-id="91569-138">Spuštění sady runbook hello</span><span class="sxs-lookup"><span data-stu-id="91569-138">Start hello runbook</span></span>
   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

<span data-ttu-id="91569-139">Hello runbook používá hello hodnoty z hello JSON souboru toostart virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="91569-139">hello runbook uses hello values from hello JSON file toostart a VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91569-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="91569-140">Next steps</span></span>

* <span data-ttu-id="91569-141">toolearn Další informace o úpravy sady runbook Powershellu a pracovní postup prostředí PowerShell s textový editor, najdete v části [úpravy textovou sady runbook ve službě Azure Automation](automation-edit-textual-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="91569-141">toolearn more about editing PowerShell and PowerShell Workflow runbooks with a textual editor, see [Editing textual runbooks in Azure Automation](automation-edit-textual-runbook.md)</span></span> 
* <span data-ttu-id="91569-142">toolearn Další informace o vytvoření a import sad runbook, najdete v části [vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="91569-142">toolearn more about creating and importing runbooks, see [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md)</span></span>


