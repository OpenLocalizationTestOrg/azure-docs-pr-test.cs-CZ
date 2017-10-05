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
# <a name="using-webjobs-in-azure-app-service"></a><span data-ttu-id="334c3-103">Používání WebJobs v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="334c3-103">Using WebJobs in Azure App Service</span></span>
<span data-ttu-id="334c3-104">Tento článek obsahuje odkazy na zdroje informací k dokumentaci o tom, jak používat Azure WebJobs a Azure WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="334c3-104">This article links to documentation resources about how to use Azure WebJobs and the Azure WebJobs SDK.</span></span> <span data-ttu-id="334c3-105">Azure WebJobs poskytují snadný způsob, jak spustit skripty nebo programy jako procesy na pozadí [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="334c3-105">Azure WebJobs provide an easy way to run scripts or programs as background processes on [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="334c3-106">Můžete nahrát a spustit spustitelný soubor, například jako cmd, bat, exe (.NET), ps1, TV, php, py, js a jar.</span><span class="sxs-lookup"><span data-stu-id="334c3-106">You can upload and run an executable file such as as cmd, bat, exe (.NET), ps1, sh, php, py, js and jar.</span></span> <span data-ttu-id="334c3-107">Tyto programy spustit jako webové úlohy podle plánu (cron) nebo nepřetržitě.</span><span class="sxs-lookup"><span data-stu-id="334c3-107">These programs run as WebJobs on a schedule (cron) or continuously.</span></span>

<span data-ttu-id="334c3-108">Sada WebJobs SDK usnadňuje používání Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="334c3-108">The WebJobs SDK makes it easier to use Azure Storage.</span></span> <span data-ttu-id="334c3-109">Sada WebJobs SDK má vazba a systém aktivační události, který funguje s Microsoft Azure Storage Blobs, fronty a tabulky, jakož i fronty služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="334c3-109">The WebJobs SDK has a binding and trigger system which works with Microsoft Azure Storage Blobs, Queues and Tables as well as Service Bus Queues.</span></span>

<span data-ttu-id="334c3-110">Vytváření, nasazování a správě webové úlohy je bezproblémové s integrované nástrojů v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="334c3-110">Creating, deploying, and managing WebJobs is seamless with integrated tooling in Visual Studio.</span></span> <span data-ttu-id="334c3-111">Můžete vytvářet webové úlohy ze šablon, publikovat a spravovat (spuštění nebo zastavení nebo monitorování/debug) je.</span><span class="sxs-lookup"><span data-stu-id="334c3-111">You can create WebJobs from templates, publish, and manage (run/stop/monitor/debug) them.</span></span>

<span data-ttu-id="334c3-112">Na řídicím panelu WebJobs na portálu Azure poskytuje výkonné funkce, které poskytují plnou kontrolu nad spuštění webové úlohy, včetně možnosti vyvolání jednotlivých funkcí v rámci webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="334c3-112">The WebJobs dashboard in the Azure portal provides powerful management capabilities that give you full control over the execution of WebJobs, including the ability to invoke individual functions within WebJobs.</span></span> <span data-ttu-id="334c3-113">Na řídicím panelu zobrazí také funkce moduly runtime a výstup protokolování.</span><span class="sxs-lookup"><span data-stu-id="334c3-113">The dashboard also displays function runtimes and logging output.</span></span>

[!INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]

