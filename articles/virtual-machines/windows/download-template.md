---
title: "Šablona hello aaaDownload pro virtuální počítač Azure | Microsoft Docs"
description: "Stáhnout hello templatefor toohelp virtuálních počítačů k automatizaci nasazení v modelu nasazení Resource Manager hello"
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
ms.openlocfilehash: 86fd05f67409019b5e5c9023881745047860eee1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="download-hello-template-for-a-vm"></a><span data-ttu-id="9ffc0-103">Stáhnout hello šablonu pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="9ffc0-103">Download hello template for a VM</span></span>
<span data-ttu-id="9ffc0-104">Při vytváření virtuálního počítače v Azure pomocí portálu hello nebo prostředí PowerShell, správce prostředků šablony se automaticky vytvoří za vás.</span><span class="sxs-lookup"><span data-stu-id="9ffc0-104">When you create a VM in Azure using hello portal or PowerShell, a Resource Manager template is automatically created for you.</span></span> <span data-ttu-id="9ffc0-105">Pomocí této šablony tooquickly duplicitní nasazení.</span><span class="sxs-lookup"><span data-stu-id="9ffc0-105">You can use this template tooquickly duplicate a deployment.</span></span> <span data-ttu-id="9ffc0-106">Šablona Hello obsahuje informace o všech hello prostředků ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="9ffc0-106">hello template contains information about all of hello resources in a resource group.</span></span> <span data-ttu-id="9ffc0-107">Pro virtuální počítač to znamená, že šablona hello obsahuje vše, co je vytvořena na podporu hello virtuálního počítače v příslušné skupině prostředků, včetně hello síťových prostředků.</span><span class="sxs-lookup"><span data-stu-id="9ffc0-107">For a virtual machine, this means hello template contains everything that is created in support of hello VM in that resource group, including hello networking resources.</span></span>

## <a name="download-hello-template-using-hello-portal"></a><span data-ttu-id="9ffc0-108">Stažení šablony hello pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="9ffc0-108">Download hello template using hello portal</span></span>
1. <span data-ttu-id="9ffc0-109">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="9ffc0-109">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="9ffc0-110">Jeden hello nabídce centra vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="9ffc0-110">One hello hub menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="9ffc0-111">Vyberte virtuální počítač hello hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="9ffc0-111">Select hello virtual machine from hello list.</span></span>
4. <span data-ttu-id="9ffc0-112">Vyberte **skriptu pro automatizaci**.</span><span class="sxs-lookup"><span data-stu-id="9ffc0-112">Select **Automation script**.</span></span>
5. <span data-ttu-id="9ffc0-113">Vyberte **Stáhnout** a uložte hello .zip souboru tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="9ffc0-113">Select **Download** and save hello .zip file tooyour local computer.</span></span>
6. <span data-ttu-id="9ffc0-114">Otevřete soubor .zip hello a extrakci hello soubory tooa složku.</span><span class="sxs-lookup"><span data-stu-id="9ffc0-114">Open hello .zip file and extract hello files tooa folder.</span></span> <span data-ttu-id="9ffc0-115">bude obsahovat soubor .zip Hello:</span><span class="sxs-lookup"><span data-stu-id="9ffc0-115">hello .zip file will contain:</span></span>
   
   * <span data-ttu-id="9ffc0-116">Deploy.ps1</span><span class="sxs-lookup"><span data-stu-id="9ffc0-116">deploy.ps1</span></span>
   * <span data-ttu-id="9ffc0-117">Deploy.SH</span><span class="sxs-lookup"><span data-stu-id="9ffc0-117">deploy.sh</span></span> 
   * <span data-ttu-id="9ffc0-118">deployer.RB</span><span class="sxs-lookup"><span data-stu-id="9ffc0-118">deployer.rb</span></span>
   * <span data-ttu-id="9ffc0-119">DeploymentHelper.cs</span><span class="sxs-lookup"><span data-stu-id="9ffc0-119">DeploymentHelper.cs</span></span>
   * <span data-ttu-id="9ffc0-120">Parameters.JSON tímto kódem</span><span class="sxs-lookup"><span data-stu-id="9ffc0-120">parameters.json</span></span>
   * <span data-ttu-id="9ffc0-121">Template.JSON</span><span class="sxs-lookup"><span data-stu-id="9ffc0-121">template.json</span></span>

<span data-ttu-id="9ffc0-122">soubor template.json Hello je hello šablony.</span><span class="sxs-lookup"><span data-stu-id="9ffc0-122">hello template.json file is hello template.</span></span>

## <a name="download-hello-template-using-powershell"></a><span data-ttu-id="9ffc0-123">Stažení šablony hello pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9ffc0-123">Download hello template using PowerShell</span></span>
<span data-ttu-id="9ffc0-124">Můžete také stáhnout soubor šablony .json hello pomocí hello [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="9ffc0-124">You can also download hello .json template file using hello [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) cmdlet.</span></span> <span data-ttu-id="9ffc0-125">Můžete použít hello `-path` parametr tooprovide hello název souboru a cesta k souboru .json hello.</span><span class="sxs-lookup"><span data-stu-id="9ffc0-125">You can use hello `-path` parameter tooprovide hello filename and path for hello .json file.</span></span> <span data-ttu-id="9ffc0-126">Tento příklad ukazuje, jak toodownload hello šablony pro hello skupinu prostředků s názvem **myResourceGroup** toohello **C:\users\public\downloads** složky v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="9ffc0-126">This example shows how toodownload hello template for hello resource group named **myResourceGroup** toohello **C:\users\public\downloads** folder on your local computer.</span></span>

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a><span data-ttu-id="9ffc0-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ffc0-127">Next steps</span></span>
<span data-ttu-id="9ffc0-128">toolearn Další informace o nasazení prostředků pomocí šablony, najdete v části [názorný Průvodce šablonou Resource Manageru](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="9ffc0-128">toolearn more about deploying resources using templates, see [Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

