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
# <a name="download-hello-template-for-a-vm"></a>Stáhnout hello šablonu pro virtuální počítač
Při vytváření virtuálního počítače v Azure pomocí portálu hello nebo prostředí PowerShell, správce prostředků šablony se automaticky vytvoří za vás. Pomocí této šablony tooquickly duplicitní nasazení. Šablona Hello obsahuje informace o všech hello prostředků ve skupině prostředků. Pro virtuální počítač to znamená, že šablona hello obsahuje vše, co je vytvořena na podporu hello virtuálního počítače v příslušné skupině prostředků, včetně hello síťových prostředků.

## <a name="download-hello-template-using-hello-portal"></a>Stažení šablony hello pomocí portálu hello
1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Jeden hello nabídce centra vyberte **virtuální počítače**.
3. Vyberte virtuální počítač hello hello seznamu.
4. Vyberte **skriptu pro automatizaci**.
5. Vyberte **Stáhnout** a uložte hello .zip souboru tooyour místního počítače.
6. Otevřete soubor .zip hello a extrakci hello soubory tooa složku. bude obsahovat soubor .zip Hello:
   
   * Deploy.ps1
   * Deploy.SH 
   * deployer.RB
   * DeploymentHelper.cs
   * Parameters.JSON tímto kódem
   * Template.JSON

soubor template.json Hello je hello šablony.

## <a name="download-hello-template-using-powershell"></a>Stažení šablony hello pomocí prostředí PowerShell
Můžete také stáhnout soubor šablony .json hello pomocí hello [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) rutiny. Můžete použít hello `-path` parametr tooprovide hello název souboru a cesta k souboru .json hello. Tento příklad ukazuje, jak toodownload hello šablony pro hello skupinu prostředků s názvem **myResourceGroup** toohello **C:\users\public\downloads** složky v místním počítači.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Další kroky
toolearn Další informace o nasazení prostředků pomocí šablony, najdete v části [názorný Průvodce šablonou Resource Manageru](../../azure-resource-manager/resource-manager-template-walkthrough.md).

