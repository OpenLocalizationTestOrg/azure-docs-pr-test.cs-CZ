---
title: "aaaAzure kontejneru registru úložiště | Microsoft Docs"
description: "Jak toouse registru Azure kontejner úložiště pro Docker bitové kopie"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: cristyg
ms.openlocfilehash: 06172a63465838a78a607f268da116d8158789ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-registry-repositories"></a>Kontejner Azure registru úložiště

Azure registrech kontejneru jsou kompatibilní s velkého množství služeb a orchestrators. toomake je snazší tootrack hello zdroj služeb a agentů, ze kterých se používá ACR, jsme spustili pomocí pole hlavičky hello Docker v souboru Docker.config hello.



## <a name="viewing-repositories-in-hello-portal"></a>Zobrazení úložiště v hello portálu

Hello ACR záhlaví podle hello formátu:
```
X-Meta-Source-Client: <cloud>/<service>/<optionalservicename>
```

* Cloudu: Azure, Azure zásobník, nebo jiných vládních nebo Azure cloudy jednotlivé země. I když Azure zásobníku a government cloudy nejsou aktuálně podporovány, tento parametr umožňuje podpory v budoucnu.
* Služby: název služby hello.
* Optionalservicename: volitelný parametr pro služby s subservices nebo toospecify SKU (např: odpovídají webové aplikace s Azure nebo-aplikace služby App service/web).

Služby partnera a orchestrators jsou podporovali toouse konkrétní hlavičky hodnoty toohelp s naše telemetrií. Uživatele můžete také upravit hello předaná hodnota hlavičky toohello svého rozhodnutí.

Hello hodnoty chceme ACR partnery toouse toopopulate hello "X-Meta-zdroje-Client" pole jsou níže:

| Název služby              | Záhlaví                                |
| ------------------------- | ------------------------------------- |
| Azure Container Service   | Azure nebo výpočetní/azure kontejneru service |
| Aplikační služby - webové aplikace    | Azure nebo-aplikace služby App service/web            |
| Aplikační služby - Logic Apps  | Azure nebo-aplikace služby App service nebo logiku          |
| Batch                     | Azure nebo výpočetní/batch                   |
| Cloud Console             | Azure nebo cloudu konzoly                   |
| Funkce                 | Azure nebo výpočetní nebo funkce               |
| Internet věcí - Hub  | Azure nebo iot nebo Centrum                         |
| HDInsight                 | Azure/data/hdinsight                  |
| Jenkins                   | Azure nebo volaných                         |
| Machine Learning          | Azure/data/machile-learning           |
| Service Fabric            | Azure nebo výpočetní/service-prostředků infrastruktury          |
| SLUŽBY VSTS                      | Azure nebo služby vsts                            |


## <a name="next-steps"></a>Další kroky
[Další informace o registrech a hello podporované služby a orchestrators](container-registry-intro.md)
