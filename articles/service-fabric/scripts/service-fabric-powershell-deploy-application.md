---
title: "aaaAzure ukázkový skript prostředí PowerShell – nasazení aplikace tooa clusteru | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – cluster Service Fabric tooa aplikaci nasadit."
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
ms.openlocfilehash: b417c9908c72f016e930c43ff2d13e0cc5451f46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-tooa-service-fabric-cluster"></a>Nasazení clusteru Service Fabric tooa aplikace

Tento ukázkový skript zkopíruje úložišti aplikace clusteru tooa balíček bitové kopie, zaregistruje typ aplikace hello v clusteru hello a vytvoří instanci aplikace z typu aplikace hello.  Pokud žádné výchozí služby byly definovány v manifestu aplikace hello typu hello cílové aplikace, vytvoří se v tuto chvíli těchto služeb. Parametry hello přizpůsobte podle potřeby. 

V případě potřeby nainstalujte modul Service Fabric prostředí PowerShell hello s hello [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application tooa cluster")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Po spuštění ukázka skriptu hello hello skript v [odebrání aplikace](service-fabric-powershell-remove-application.md) může být instanci aplikace hello použité tooremove, zrušení registrace typu aplikace hello a odstranit balíček aplikace hello z úložiště bitových kopií hello.

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Kopírování ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Zkopírujte úložišti aplikace clusteru toohello balíček bitové kopie.  |
|[Registrace ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| Zaregistruje typ a verze aplikace v clusteru hello. |
|[Nové ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| Vytvoří aplikace z typu zaregistrovanou aplikaci. |

## <a name="next-steps"></a>Další kroky

Další informace o modulu hello Service Fabric prostředí PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/service-fabric/?view=azureservicefabricps).

Další ukázky pro Azure Service Fabric Powershell naleznete v hello [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).
