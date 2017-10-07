---
title: "aaaAzure Service Fabric rozhraní příkazového řádku skriptu nasazení ukázkové"
description: "Nasazení clusteru Azure Service Fabric tooan aplikace pomocí příkazového řádku Azure Service Fabric hello"
services: service-fabric
documentationcenter: 
author: Thraka
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
ms.openlocfilehash: aaec7042a4fd7ed32ad706cde70361f23d18fb48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-tooa-service-fabric-cluster"></a>Nasazení clusteru Service Fabric tooa aplikace

Tento ukázkový skript zkopíruje úložišti aplikace clusteru tooa balíček bitové kopie, zaregistruje typ aplikace hello v clusteru hello a vytvoří instanci aplikace z typu aplikace hello. V tuto chvíli se také vytvořit žádné výchozí služby.

V případě potřeby nainstalujte hello [Service Fabric rozhraní příkazového řádku](../service-fabric-cli.md).

## <a name="sample-script"></a>Ukázkový skript

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application tooa cluster")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Až budete hotoví, hello [odebrat](cli-remove-application.md) skriptu lze použít tooremove hello aplikace. skript odebrat Hello odstraní instanci aplikace hello, zrušení registrace typu aplikace hello a balíček aplikace hello z úložiště bitových kopií.

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu hello [Service Fabric rozhraní příkazového řádku dokumentaci](../service-fabric-cli.md).

Další ukázky pro Service Fabric rozhraní příkazového řádku pro Azure Service Fabric lze nalézt v hello [Service Fabric rozhraní příkazového řádku ukázky](../samples-cli.md).
