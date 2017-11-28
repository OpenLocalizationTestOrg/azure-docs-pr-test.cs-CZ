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
# <a name="using-webjobs-in-azure-app-service"></a><span data-ttu-id="4351a-103">Používání WebJobs v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4351a-103">Using WebJobs in Azure App Service</span></span>
<span data-ttu-id="4351a-104">V tomto článku odkazy toodocumentation prostředky o toouse Azure WebJobs a hello Azure WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="4351a-104">This article links toodocumentation resources about how toouse Azure WebJobs and hello Azure WebJobs SDK.</span></span> <span data-ttu-id="4351a-105">Azure WebJobs zadejte snadný způsob toorun skriptů nebo programů jako procesy na pozadí na [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="4351a-105">Azure WebJobs provide an easy way toorun scripts or programs as background processes on [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="4351a-106">Můžete nahrát a spustit spustitelný soubor, například jako cmd, bat, exe (.NET), ps1, TV, php, py, js a jar.</span><span class="sxs-lookup"><span data-stu-id="4351a-106">You can upload and run an executable file such as as cmd, bat, exe (.NET), ps1, sh, php, py, js and jar.</span></span> <span data-ttu-id="4351a-107">Tyto programy spustit jako webové úlohy podle plánu (cron) nebo nepřetržitě.</span><span class="sxs-lookup"><span data-stu-id="4351a-107">These programs run as WebJobs on a schedule (cron) or continuously.</span></span>

<span data-ttu-id="4351a-108">Hello WebJobs SDK umožňuje snazší toouse Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="4351a-108">hello WebJobs SDK makes it easier toouse Azure Storage.</span></span> <span data-ttu-id="4351a-109">Hello WebJobs SDK má vazba a systém aktivační události, který funguje s Microsoft Azure Storage Blobs, fronty a tabulky, jakož i fronty služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="4351a-109">hello WebJobs SDK has a binding and trigger system which works with Microsoft Azure Storage Blobs, Queues and Tables as well as Service Bus Queues.</span></span>

<span data-ttu-id="4351a-110">Vytváření, nasazování a správě webové úlohy je bezproblémové s integrované nástrojů v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4351a-110">Creating, deploying, and managing WebJobs is seamless with integrated tooling in Visual Studio.</span></span> <span data-ttu-id="4351a-111">Můžete vytvářet webové úlohy ze šablon, publikovat a spravovat (spuštění nebo zastavení nebo monitorování/debug) je.</span><span class="sxs-lookup"><span data-stu-id="4351a-111">You can create WebJobs from templates, publish, and manage (run/stop/monitor/debug) them.</span></span>

<span data-ttu-id="4351a-112">řídicím panelu WebJobs Hello v hello portál Azure poskytuje výkonné funkce, které poskytují plnou kontrolu nad hello spuštění webové úlohy, včetně hello možnost tooinvoke jednotlivých funkcí v rámci webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="4351a-112">hello WebJobs dashboard in hello Azure portal provides powerful management capabilities that give you full control over hello execution of WebJobs, including hello ability tooinvoke individual functions within WebJobs.</span></span> <span data-ttu-id="4351a-113">řídicí panel Hello také zobrazuje funkce moduly runtime a výstup protokolování.</span><span class="sxs-lookup"><span data-stu-id="4351a-113">hello dashboard also displays function runtimes and logging output.</span></span>

[!INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]

