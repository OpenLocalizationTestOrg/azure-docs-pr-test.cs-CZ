---
title: "virtuální počítače s Windows aaaRedeploy v Azure | Microsoft Docs"
description: "Jak problémy tooredeploy Windows virtuálních počítačů v Azure toomitigate připojení RDP."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: genlin
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: 0ee456ee-4595-4a14-8916-72c9110fc8bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 903d9d5bf241075931ee4b746690c553d808a58f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="redeploy-windows-virtual-machine-toonew-azure-node"></a><span data-ttu-id="d7407-103">Znovu nasaďte toonew virtuálního počítače Windows Azure uzlu</span><span class="sxs-lookup"><span data-stu-id="d7407-103">Redeploy Windows virtual machine toonew Azure node</span></span>
<span data-ttu-id="d7407-104">Pokud mají byla směrem potíží při řešení potíží s Remote Desktop (RDP) připojení nebo aplikace přístup na základě tooWindows virtuální počítač Azure (VM), opětovného nasazení hello, které mohou pomoci virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d7407-104">If you have been facing difficulties troubleshooting Remote Desktop (RDP) connection or application access tooWindows-based Azure virtual machine (VM), redeploying hello VM may help.</span></span> <span data-ttu-id="d7407-105">Při opětovném nasazování virtuálního počítače, přesune nový uzel tooa hello virtuálních počítačů v rámci hello infrastrukturu Azure a potom zapne ji zpět, zachování všech možností konfigurace a přidružených prostředků.</span><span class="sxs-lookup"><span data-stu-id="d7407-105">When you redeploy a VM, it moves hello VM tooa new node within hello Azure infrastructure and then powers it back on, retaining all your configuration options and associated resources.</span></span> <span data-ttu-id="d7407-106">Tento článek ukazuje, jak tooredeploy a virtuálních počítačů pomocí Azure PowerShell nebo hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d7407-106">This article shows you how tooredeploy a VM using Azure PowerShell or hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="d7407-107">Po opětovném virtuálního počítače, dojde ke ztrátě dočasným diskovým hello a dynamické IP adresy přidružené k rozhraní virtuální sítě se aktualizují.</span><span class="sxs-lookup"><span data-stu-id="d7407-107">After you redeploy a VM, hello temporary disk is lost and dynamic IP addresses associated with virtual network interface are updated.</span></span> 


## <a name="using-azure-powershell"></a><span data-ttu-id="d7407-108">Použití Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="d7407-108">Using Azure PowerShell</span></span>
<span data-ttu-id="d7407-109">Ujistěte se, zda se mají hello nejnovější prostředí Azure PowerShell 1.x nainstalovaný na počítači.</span><span class="sxs-lookup"><span data-stu-id="d7407-109">Make sure you have hello latest Azure PowerShell 1.x installed on your machine.</span></span> <span data-ttu-id="d7407-110">Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d7407-110">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="d7407-111">Hello následující příklad nasadí hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="d7407-111">hello following example deploys hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```powershell
Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
```


[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a><span data-ttu-id="d7407-112">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d7407-112">Next steps</span></span>
<span data-ttu-id="d7407-113">Pokud máte problémy s připojením tooyour virtuálních počítačů, najdete konkrétní pomoc na [řešení potíží s připojeními RDP](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [podrobné RDP při řešení potíží](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d7407-113">If you are having issues connecting tooyour VM, you can find specific help on [troubleshooting RDP connections](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) or [detailed RDP troubleshooting steps](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="d7407-114">Pokud nelze získat přístup k aplikaci běžící na vašem virtuálním počítači, můžete si také přečíst [řešení potíží s aplikací](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d7407-114">If you cannot access an application running on your VM, you can also read [application troubleshooting issues](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

