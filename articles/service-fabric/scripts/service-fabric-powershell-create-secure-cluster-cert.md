---
title: "Azure skript prostředí PowerShell ukázkový – vytvořit cluster Service Fabric | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvořit cluster Service Fabric."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/19/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: b0d33b714092a826012677c95124d74bf2c72999
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/20/2018
---
# <a name="create-a-service-fabric-cluster"></a>Vytvořit cluster Service Fabric

Tento ukázkový skript vytvoří cluster Service Fabric pěti uzly zabezpečená certifikátem X.509.  Příkaz vytvoří certifikát podepsaný svým držitelem a nahraje ho do nového trezoru klíčů. Certifikát se také zkopíruje do místního adresáře.  Nastavením parametru *-OS* zvolte verzi Windows nebo Linuxu, která se používá na uzlech clusteru.  Podle potřeby upravte parametry.

V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview) a spusťte `Login-AzureRmAccount` vytvořit připojení s Azure. 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-secure-cluster/create-secure-cluster.ps1 "Create a Service Fabric cluster")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků clusteru a všechny související prostředky.

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | Vytvoří nový cluster Service Fabric. |

## <a name="next-steps"></a>Další postup

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky pro Azure Service Fabric Azure Powershell lze nalézt v [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).
