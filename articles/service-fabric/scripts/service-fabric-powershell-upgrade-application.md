---
title: "Upgrade aplikace Service Fabric aaaAzure ukázkový skript prostředí PowerShell - | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - upgradu aplikace Service Fabric."
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
ms.date: 08/23/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 4f4777607bd6b35a76029e09ddb441006565d4cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-service-fabric-application"></a>Upgrade aplikace Service Fabric

Tento ukázkový skript provede upgrade spuštěné tooversion instance Service Fabric aplikace 1.3.0. skript Hello zkopíruje hello nové aplikace balíčku toohello clusteru úložiště image store, zaregistruje typ aplikace hello, spustí monitorovaných upgradu a neustále kontroluje stav upgradu hello až do dokončení upgradu hello nebo vrátí zpět. Parametry hello přizpůsobte podle potřeby. 

V případě potřeby nainstalujte modul Service Fabric prostředí PowerShell hello s hello [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Upgrade an application")]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | Získá všechny aplikace hello v clusteru Service Fabric hello nebo na konkrétní aplikaci.  |
| [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | Získá stav hello upgradu aplikace Service Fabric. |
| [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | Získá typy aplikací Service Fabric hello zaregistrovat u clusteru Service Fabric hello. |
| [Zrušit registraci ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Zrušení registrace typu aplikace Service Fabric.  |
| [Kopírování ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Kopie úložiště bitové kopie toohello balíčku aplikace Service Fabric.  |
| [Registrace ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | Zaregistruje typ aplikace Service Fabric. |
| [Počáteční ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | Upgraduje typ verzi aplikace Service Fabric aplikace toohello zadané aplikace. |


## <a name="next-steps"></a>Další kroky

Další informace o modulu hello Service Fabric prostředí PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/service-fabric/?view=azureservicefabricps).

Další ukázky pro Azure Service Fabric Powershell naleznete v hello [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).
