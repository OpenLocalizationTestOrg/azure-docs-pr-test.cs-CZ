---
title: "Stáhnout šablonu pro virtuální počítač Azure | Microsoft Docs"
description: "Stáhněte si templatefor virtuálního počítače můžete k automatizaci nasazení v modelu nasazení Resource Manager"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 51ef4f51-0942-4249-afea-4a3f87ce1ff8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 9e4c0c3cf0e233447369a24b1d5fe27495abd1cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="download-the-template-for-a-vm"></a><span data-ttu-id="d2e48-103">Stažení šablony pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="d2e48-103">Download the template for a VM</span></span>
<span data-ttu-id="d2e48-104">Při vytváření virtuálního počítače v Azure pomocí portálu nebo prostředí PowerShell, je pro vás automaticky vytvoří šablonu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d2e48-104">When you create a VM in Azure using the portal or PowerShell, a Resource Manager template is automatically created for you.</span></span> <span data-ttu-id="d2e48-105">Tuto šablonu můžete použít k rychlé duplicitní nasazení.</span><span class="sxs-lookup"><span data-stu-id="d2e48-105">You can use this template to quickly duplicate a deployment.</span></span> <span data-ttu-id="d2e48-106">Šablona obsahuje informace o všechny prostředky ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="d2e48-106">The template contains information about all of the resources in a resource group.</span></span> <span data-ttu-id="d2e48-107">Pro virtuální počítač to znamená, že šablona obsahuje všechno, která je vytvořena na podporu virtuálních počítačů v příslušné skupině prostředků, včetně síťových prostředků.</span><span class="sxs-lookup"><span data-stu-id="d2e48-107">For a virtual machine, this means the template contains everything that is created in support of the VM in that resource group, including the networking resources.</span></span>

## <a name="download-the-template-using-the-portal"></a><span data-ttu-id="d2e48-108">Stažení šablony pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="d2e48-108">Download the template using the portal</span></span>
1. <span data-ttu-id="d2e48-109">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d2e48-109">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="d2e48-110">Jeden nabídce centra vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="d2e48-110">One the hub menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="d2e48-111">Ze seznamu vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d2e48-111">Select the virtual machine from the list.</span></span>
4. <span data-ttu-id="d2e48-112">Vyberte **skriptu pro automatizaci**.</span><span class="sxs-lookup"><span data-stu-id="d2e48-112">Select **Automation script**.</span></span>
5. <span data-ttu-id="d2e48-113">Vyberte **Stáhnout** a uložte soubor .zip do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="d2e48-113">Select **Download** and save the .zip file to your local computer.</span></span>
6. <span data-ttu-id="d2e48-114">Otevřete soubor .zip a rozbalte soubory do složky.</span><span class="sxs-lookup"><span data-stu-id="d2e48-114">Open the .zip file and extract the files to a folder.</span></span> <span data-ttu-id="d2e48-115">Bude obsahovat soubor .zip:</span><span class="sxs-lookup"><span data-stu-id="d2e48-115">The .zip file will contain:</span></span>
   
   * <span data-ttu-id="d2e48-116">Deploy.ps1</span><span class="sxs-lookup"><span data-stu-id="d2e48-116">deploy.ps1</span></span>
   * <span data-ttu-id="d2e48-117">Deploy.SH</span><span class="sxs-lookup"><span data-stu-id="d2e48-117">deploy.sh</span></span> 
   * <span data-ttu-id="d2e48-118">deployer.RB</span><span class="sxs-lookup"><span data-stu-id="d2e48-118">deployer.rb</span></span>
   * <span data-ttu-id="d2e48-119">DeploymentHelper.cs</span><span class="sxs-lookup"><span data-stu-id="d2e48-119">DeploymentHelper.cs</span></span>
   * <span data-ttu-id="d2e48-120">Parameters.JSON tímto kódem</span><span class="sxs-lookup"><span data-stu-id="d2e48-120">parameters.json</span></span>
   * <span data-ttu-id="d2e48-121">Template.JSON</span><span class="sxs-lookup"><span data-stu-id="d2e48-121">template.json</span></span>

<span data-ttu-id="d2e48-122">Soubor template.json je šablona.</span><span class="sxs-lookup"><span data-stu-id="d2e48-122">The template.json file is the template.</span></span>

## <a name="download-the-template-using-powershell"></a><span data-ttu-id="d2e48-123">Stažení šablony pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d2e48-123">Download the template using PowerShell</span></span>
<span data-ttu-id="d2e48-124">Můžete také stáhnout pomocí soubor .json [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="d2e48-124">You can also download the .json template file using the [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) cmdlet.</span></span> <span data-ttu-id="d2e48-125">Můžete použít `-path` parametru zadat název a cesta k souboru .json.</span><span class="sxs-lookup"><span data-stu-id="d2e48-125">You can use the `-path` parameter to provide the filename and path for the .json file.</span></span> <span data-ttu-id="d2e48-126">Tento příklad ukazuje, jak stáhnout šablonu pro skupinu prostředků s názvem **myResourceGroup** k **C:\users\public\downloads** složky v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="d2e48-126">This example shows how to download the template for the resource group named **myResourceGroup** to the **C:\users\public\downloads** folder on your local computer.</span></span>

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a><span data-ttu-id="d2e48-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d2e48-127">Next steps</span></span>
<span data-ttu-id="d2e48-128">Další informace o nasazení prostředků pomocí šablony najdete v tématu [názorný Průvodce šablonou Resource Manageru](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="d2e48-128">To learn more about deploying resources using templates, see [Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

