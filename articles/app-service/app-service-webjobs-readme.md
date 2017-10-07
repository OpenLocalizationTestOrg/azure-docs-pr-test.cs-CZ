---
title: "aaaWebJobs ve službě Azure App Service"
description: "Zjistěte, jak toobuild pozadí toorun webové testy, komunikují s služby, jako je úložiště a Service Bus a vytvoření naplánované úlohy."
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
ms.openlocfilehash: 25c24bfe71a64036cd48e58f471995b4a06e3b33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-webjobs-in-azure-app-service"></a>Používání WebJobs v Azure App Service
V tomto článku odkazy toodocumentation prostředky o toouse Azure WebJobs a hello Azure WebJobs SDK. Azure WebJobs zadejte snadný způsob toorun skriptů nebo programů jako procesy na pozadí na [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Můžete nahrát a spustit spustitelný soubor, například jako cmd, bat, exe (.NET), ps1, TV, php, py, js a jar. Tyto programy spustit jako webové úlohy podle plánu (cron) nebo nepřetržitě.

Hello WebJobs SDK umožňuje snazší toouse Azure Storage. Hello WebJobs SDK má vazba a systém aktivační události, který funguje s Microsoft Azure Storage Blobs, fronty a tabulky, jakož i fronty služby Service Bus.

Vytváření, nasazování a správě webové úlohy je bezproblémové s integrované nástrojů v sadě Visual Studio. Můžete vytvářet webové úlohy ze šablon, publikovat a spravovat (spuštění nebo zastavení nebo monitorování/debug) je.

řídicím panelu WebJobs Hello v hello portál Azure poskytuje výkonné funkce, které poskytují plnou kontrolu nad hello spuštění webové úlohy, včetně hello možnost tooinvoke jednotlivých funkcí v rámci webové úlohy. řídicí panel Hello také zobrazuje funkce moduly runtime a výstup protokolování.

[!INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]

