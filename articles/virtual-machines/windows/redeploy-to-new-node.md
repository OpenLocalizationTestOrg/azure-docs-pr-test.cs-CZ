---
title: "Znovu nasadit virtuální počítače s Windows v Azure | Microsoft Docs"
description: "Jak znovu nasadit virtuální počítače s Windows v Azure a zmírnit problémy připojení RDP."
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
ms.openlocfilehash: a607d0747f64ee6b224d300113905f54e003aef0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="redeploy-windows-virtual-machine-to-new-azure-node"></a><span data-ttu-id="5b96d-103">Znovu nasaďte Windows virtuálního počítače do nového uzlu Azure</span><span class="sxs-lookup"><span data-stu-id="5b96d-103">Redeploy Windows virtual machine to new Azure node</span></span>
<span data-ttu-id="5b96d-104">Pokud jste byla směrem problémy je řešení potíží s Remote Desktop (RDP) připojení nebo aplikaci přístup k systému Windows Azure virtuálnímu počítači (VM), opětovného nasazení virtuálního počítače může pomoct.</span><span class="sxs-lookup"><span data-stu-id="5b96d-104">If you have been facing difficulties troubleshooting Remote Desktop (RDP) connection or application access to Windows-based Azure virtual machine (VM), redeploying the VM may help.</span></span> <span data-ttu-id="5b96d-105">Při opětovném nasazování virtuálního počítače, virtuální počítač přesune do nového uzlu v rámci infrastruktury Azure a potom zapne ji zpět, zachování všech možností konfigurace a přidružených prostředků.</span><span class="sxs-lookup"><span data-stu-id="5b96d-105">When you redeploy a VM, it moves the VM to a new node within the Azure infrastructure and then powers it back on, retaining all your configuration options and associated resources.</span></span> <span data-ttu-id="5b96d-106">Tento článek ukazuje, jak znovu nasadit virtuální počítač pomocí prostředí Azure PowerShell nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5b96d-106">This article shows you how to redeploy a VM using Azure PowerShell or the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="5b96d-107">Po opětovném virtuálního počítače, dojde ke ztrátě dočasným diskovým a dynamické IP adresy přidružené k rozhraní virtuální sítě se aktualizují.</span><span class="sxs-lookup"><span data-stu-id="5b96d-107">After you redeploy a VM, the temporary disk is lost and dynamic IP addresses associated with virtual network interface are updated.</span></span> 


## <a name="using-azure-powershell"></a><span data-ttu-id="5b96d-108">Použití Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="5b96d-108">Using Azure PowerShell</span></span>
<span data-ttu-id="5b96d-109">Ujistěte se, že máte nejnovější prostředí Azure PowerShell 1.x nainstalovaný na počítači.</span><span class="sxs-lookup"><span data-stu-id="5b96d-109">Make sure you have the latest Azure PowerShell 1.x installed on your machine.</span></span> <span data-ttu-id="5b96d-110">Další informace najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5b96d-110">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="5b96d-111">Následující příklad nasadí virtuální počítač s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="5b96d-111">The following example deploys the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

```powershell
Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
```


[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a><span data-ttu-id="5b96d-112">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5b96d-112">Next steps</span></span>
<span data-ttu-id="5b96d-113">Pokud máte problémy s připojením k virtuálnímu počítači, můžete najít konkrétní pomoc na [řešení potíží s připojeními RDP](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [podrobné RDP při řešení potíží](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5b96d-113">If you are having issues connecting to your VM, you can find specific help on [troubleshooting RDP connections](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) or [detailed RDP troubleshooting steps](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="5b96d-114">Pokud nelze získat přístup k aplikaci běžící na vašem virtuálním počítači, můžete si také přečíst [řešení potíží s aplikací](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5b96d-114">If you cannot access an application running on your VM, you can also read [application troubleshooting issues](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

