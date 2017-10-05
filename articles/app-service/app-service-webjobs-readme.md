---
title: "Webové úlohy v Azure App Service"
description: "Naučte se vytvářet webové testy pozadí, pracovat s služby, jako je úložiště a Service Bus a vytvoření naplánované úlohy."
services: app-service
documentationcenter: 
author: christopheranderson
manager: erikre
editor: mollybos
ms.assetid: 85975432-04c9-4b83-b937-b30c082d52a1
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2015
ms.author: chrande
ms.openlocfilehash: 1ca6d2eabe9781a8bb09fc5948ed306e3e8b013c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-webjobs-in-azure-app-service"></a>Používání WebJobs v Azure App Service
Tento článek obsahuje odkazy na zdroje informací k dokumentaci o tom, jak používat Azure WebJobs a Azure WebJobs SDK. Azure WebJobs poskytují snadný způsob, jak spustit skripty nebo programy jako procesy na pozadí [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Můžete nahrát a spustit spustitelný soubor, například jako cmd, bat, exe (.NET), ps1, TV, php, py, js a jar. Tyto programy spustit jako webové úlohy podle plánu (cron) nebo nepřetržitě.

Sada WebJobs SDK usnadňuje používání Azure Storage. Sada WebJobs SDK má vazba a systém aktivační události, který funguje s Microsoft Azure Storage Blobs, fronty a tabulky, jakož i fronty služby Service Bus.

Vytváření, nasazování a správě webové úlohy je bezproblémové s integrované nástrojů v sadě Visual Studio. Můžete vytvářet webové úlohy ze šablon, publikovat a spravovat (spuštění nebo zastavení nebo monitorování/debug) je.

Na řídicím panelu WebJobs na portálu Azure poskytuje výkonné funkce, které poskytují plnou kontrolu nad spuštění webové úlohy, včetně možnosti vyvolání jednotlivých funkcí v rámci webové úlohy. Na řídicím panelu zobrazí také funkce moduly runtime a výstup protokolování.

[!INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]

