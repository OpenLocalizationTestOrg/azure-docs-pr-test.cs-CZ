---
title: "Azure skript prostředí PowerShell ukázkový – přidání certifikátu aplikací do clusteru s podporou | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – přidání certifikát aplikací do clusteru Service Fabric."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 9ccd6bb0458bc03e52103fa70cad26bd6bf98bd5
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Přidat certifikát aplikací do clusteru Service Fabric

Tento ukázkový skript vytvoří certifikát podepsaný svým držitelem v zadané Azure key vault a nainstaluje na všech uzlech tohoto clusteru Service Fabric. Certifikát se také stáhne do místní složky. Název stažený certifikát je stejný jako název certifikátu v trezoru klíčů. Podle potřeby upravte požadované parametry.

V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview) a spusťte `Login-AzureRmAccount` vytvořit připojení s Azure. 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[hlavní](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "přidat certifikát aplikací do clusteru")]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy: každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Přidat AzureRmServiceFabricApplicationCertificate](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | Přidáte nový certifikát aplikace do sady škálování virtuálního počítače, který vytváří cluster.  |

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky pro Azure Service Fabric Azure Powershell lze nalézt v [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).
