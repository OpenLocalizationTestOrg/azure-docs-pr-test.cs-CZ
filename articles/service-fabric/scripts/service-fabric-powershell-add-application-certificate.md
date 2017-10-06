---
title: "aaaAzure ukázkový skript prostředí PowerShell – přidat aplikaci cert tooa cluster | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – Přidání clusteru Service Fabric tooa certifikát služby aplikací."
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
ms.openlocfilehash: aba62598e2e4775012f89b5070bef5e61aec64f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-an-application-certificate-tooa-service-fabric-cluster"></a>Přidejte cluster Service Fabric tooa certifikát služby aplikace

Tento ukázkový skript vytvoří certifikát podepsaný svým držitelem v hello zadaný Azure trezoru klíčů a ho nainstaluje tooall uzly clusteru Service Fabric hello. certifikát Hello také stáhne tooa místní složky. Název Hello hello stáhnout certifikát je hello stejný jako název hello hello certifikátu v trezoru klíčů hello. Parametry hello přizpůsobte podle potřeby.

V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview) a spusťte `Login-AzureRmAccount` toocreate připojení s Azure. 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate tooa cluster")]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy: každý příkaz v tabulce hello odkazy toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Přidat AzureRmServiceFabricApplicationCertificate](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | Přidáte že nové aplikace certifikát toohello škálovací sady virtuálních počítačů které tvoří hello clusteru.  |

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky pro Azure Service Fabric Azure Powershell lze nalézt v hello [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).
