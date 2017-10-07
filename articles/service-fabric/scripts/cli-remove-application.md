---
title: "aaaAzure odebrat ukázka serveru Service Fabric rozhraní příkazového řádku skriptu"
description: "Odebrání aplikace z clusteru služby Azure Service Fabric pomocí hello příkazového řádku Azure Service Fabric"
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
ms.openlocfilehash: 3ccefd4a04c5b7af71a2f959e11da6e402f25881
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Odebrání aplikace z clusteru Service Fabric

Tento ukázkový skript odstraní spuštěné instance aplikace Service Fabric, zrušení registrace typ a verze aplikace z clusteru hello.  Odstraňování instance aplikace hello také odstraní všechny hello spuštění instance služby, které jsou přidružené s touto aplikací. V dalším kroku hello aplikace soubory se odstraní z úložiště bitových kopií hello. 

V případě potřeby nainstalujte hello [Service Fabric rozhraní příkazového řádku](../service-fabric-cli.md).

## <a name="sample-script"></a>Ukázkový skript

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu hello [Service Fabric rozhraní příkazového řádku dokumentaci](../service-fabric-cli.md).

Další ukázky pro Service Fabric rozhraní příkazového řádku pro Azure Service Fabric lze nalézt v hello [Service Fabric rozhraní příkazového řádku ukázky](../samples-cli.md).
