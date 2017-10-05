---
title: "Kontejner Azure registru úložiště | Microsoft Docs"
description: "Jak používat Azure registru kontejner úložiště pro imagí Dockeru"
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
ms.openlocfilehash: dd4feff057269ed7106990bb63eed7fcffa2dbec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-container-registry-repositories"></a>Kontejner Azure registru úložiště

Azure registrech kontejneru jsou kompatibilní s velkého množství služeb a orchestrators. Aby bylo snazší sledují zdroj služeb a agentů, ze kterých se používá ACR, jsme spustili pomocí pole hlavičky Docker v souboru Docker.config.



## <a name="viewing-repositories-in-the-portal"></a>Zobrazení úložiště na portálu

Hlavičky ACR použijte formát:
```
X-Meta-Source-Client: <cloud>/<service>/<optionalservicename>
```

* Cloudu: Azure, Azure zásobník, nebo jiných vládních nebo Azure cloudy jednotlivé země. I když Azure zásobníku a government cloudy nejsou aktuálně podporovány, tento parametr umožňuje podpory v budoucnu.
* Služba: název služby.
* Optionalservicename: volitelný parametr pro služby s subservices nebo k zadání SKU (např: odpovídají webové aplikace s Azure nebo-aplikace služby App service/web).

Služby partnera a orchestrators doporučujeme použít hodnoty konkrétní hlavičky usnadní naše telemetrie. Uživatele můžete také upravit hodnota předaná hlavičku svého rozhodnutí.

Hodnoty chceme ACR partnery sloužící k vyplnění pole "X-Meta-zdroje-Client" jsou následující:

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
[Další informace o registrech a podporované služby a orchestrators](container-registry-intro.md)
