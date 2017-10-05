---
title: "Ukázka odebrat Azure Service Fabric rozhraní příkazového řádku skriptu"
description: "Odebrání aplikace z clusteru služby Azure Service Fabric pomocí rozhraní příkazového řádku Azure Service Fabric"
services: service-fabric
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: d86f195d2c37a71e476c5ba4eec040dd46931d23
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Odebrání aplikace z clusteru Service Fabric

Tento ukázkový skript odstraní spuštěné instance aplikace Service Fabric, zrušení registrace typ a verze aplikace z clusteru.  Odstranění instance aplikace na také odstraní všechny spuštěné služby instance přidružené s touto aplikací. Soubory aplikace v dalším kroku se odstraní z úložiště bitových kopií. 

V případě potřeby nainstalujte [Service Fabric rozhraní příkazového řádku](../service-fabric-cli.md).

## <a name="sample-script"></a>Ukázkový skript

[!code-sh[hlavní](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "odebrání aplikace z clusteru")]

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Service Fabric rozhraní příkazového řádku dokumentaci](../service-fabric-cli.md).

Další ukázky pro Service Fabric rozhraní příkazového řádku pro Azure Service Fabric najdete v [Service Fabric rozhraní příkazového řádku ukázky](../samples-cli.md).
