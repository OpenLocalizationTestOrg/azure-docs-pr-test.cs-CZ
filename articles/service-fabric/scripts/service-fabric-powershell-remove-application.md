---
title: "aaaAzure ukázka skriptu prostředí PowerShell - aplikaci odebrat z clusteru s podporou | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - odebrat aplikaci z clusteru Service Fabric."
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
ms.openlocfilehash: 3fe2082c2fbeffbff1622f206021d4d907197d19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Odebrání aplikace z clusteru Service Fabric

Tento ukázkový skript odstraní spuštěné instance aplikace Service Fabric, zrušení registrace typ a verze aplikace z hello clusteru a balíček aplikace hello z úložiště bitových kopií clusteru hello.  Odstraňování instance aplikace hello také odstraní všechny hello spuštění instance služby, které jsou přidružené s touto aplikací. Parametry hello přizpůsobte podle potřeby. 

V případě potřeby nainstalujte modul Service Fabric prostředí PowerShell hello s hello [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "Remove an application from a cluster")]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Odebrat ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) | Odebere z clusteru hello spuštěné instance aplikace Service Fabric.  |
| [Zrušit registraci ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Zrušení registrace typu aplikace Service Fabric a verze z clusteru hello. |
| [Odebrat ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | Balíček aplikace Service Fabric se odebere z úložiště bitových kopií hello.|

## <a name="next-steps"></a>Další kroky

Další informace o modulu hello Service Fabric prostředí PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/service-fabric/?view=azureservicefabricps).

Další ukázky pro Azure Service Fabric Powershell naleznete v hello [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).
