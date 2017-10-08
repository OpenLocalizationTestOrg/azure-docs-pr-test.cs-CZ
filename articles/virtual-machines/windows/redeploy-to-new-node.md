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
# <a name="redeploy-windows-virtual-machine-toonew-azure-node"></a>Znovu nasaďte toonew virtuálního počítače Windows Azure uzlu
Pokud mají byla směrem potíží při řešení potíží s Remote Desktop (RDP) připojení nebo aplikace přístup na základě tooWindows virtuální počítač Azure (VM), opětovného nasazení hello, které mohou pomoci virtuálních počítačů. Při opětovném nasazování virtuálního počítače, přesune nový uzel tooa hello virtuálních počítačů v rámci hello infrastrukturu Azure a potom zapne ji zpět, zachování všech možností konfigurace a přidružených prostředků. Tento článek ukazuje, jak tooredeploy a virtuálních počítačů pomocí Azure PowerShell nebo hello portálu Azure.

> [!NOTE]
> Po opětovném virtuálního počítače, dojde ke ztrátě dočasným diskovým hello a dynamické IP adresy přidružené k rozhraní virtuální sítě se aktualizují. 


## <a name="using-azure-powershell"></a>Použití Azure Powershell
Ujistěte se, zda se mají hello nejnovější prostředí Azure PowerShell 1.x nainstalovaný na počítači. Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

Hello následující příklad nasadí hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:

```powershell
Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
```


[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Další kroky
Pokud máte problémy s připojením tooyour virtuálních počítačů, najdete konkrétní pomoc na [řešení potíží s připojeními RDP](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [podrobné RDP při řešení potíží](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Pokud nelze získat přístup k aplikaci běžící na vašem virtuálním počítači, můžete si také přečíst [řešení potíží s aplikací](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

