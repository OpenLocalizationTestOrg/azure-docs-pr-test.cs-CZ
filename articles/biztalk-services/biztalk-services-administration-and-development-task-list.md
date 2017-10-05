---
title: "Vývoj a správu úloh seznamu ve službě BizTalk Services | Microsoft Docs"
description: "Plánování a úlohy podpory pro nasazení služby Azure BizTalk Services."
services: biztalk-services
documentationcenter: 
author: msftman
manager: erikre
editor: 
ms.assetid: 0ab70b5b-1a88-4ba5-b329-ec51b785010e
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.openlocfilehash: 7d4532daf5e4b8f45de94bbec230633978814a6e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="administration-and-development-task-list-in-biztalk-services"></a><span data-ttu-id="6f062-103">Správu a vývoj seznamu úloh ve službě BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="6f062-103">Administration and Development Task List in BizTalk Services</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

## <a name="getting-started"></a><span data-ttu-id="6f062-104">Začínáme</span><span class="sxs-lookup"><span data-stu-id="6f062-104">Getting Started</span></span>
<span data-ttu-id="6f062-105">Při práci se službou Microsoft Azure BizTalk Services, je několik místní a cloudové součásti vzít v úvahu.</span><span class="sxs-lookup"><span data-stu-id="6f062-105">When working with Microsoft Azure BizTalk Services, there are several on-premises and cloud-based components to consider.</span></span> <span data-ttu-id="6f062-106">Abyste mohli začít, zvažte následující postup proces:</span><span class="sxs-lookup"><span data-stu-id="6f062-106">To get started, consider the following process flow:</span></span>  

| <span data-ttu-id="6f062-107">Krok</span><span class="sxs-lookup"><span data-stu-id="6f062-107">Step</span></span> | <span data-ttu-id="6f062-108">Kdo odpovídá</span><span class="sxs-lookup"><span data-stu-id="6f062-108">Who's responsible</span></span> | <span data-ttu-id="6f062-109">Úkol</span><span class="sxs-lookup"><span data-stu-id="6f062-109">Task</span></span> | <span data-ttu-id="6f062-110">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="6f062-110">Related Links</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6f062-111">1.</span><span class="sxs-lookup"><span data-stu-id="6f062-111">1.</span></span> |<span data-ttu-id="6f062-112">Správce</span><span class="sxs-lookup"><span data-stu-id="6f062-112">Administrator</span></span> |<span data-ttu-id="6f062-113">Vytvořit odběr služby Microsoft Azure pomocí účtu Microsoft nebo účtu organizace</span><span class="sxs-lookup"><span data-stu-id="6f062-113">Create the Microsoft Azure Subscription using a Microsoft account or an Organizational account</span></span> |[<span data-ttu-id="6f062-114">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="6f062-114">Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=213885) |
| <span data-ttu-id="6f062-115">2.</span><span class="sxs-lookup"><span data-stu-id="6f062-115">2.</span></span> |<span data-ttu-id="6f062-116">Správce</span><span class="sxs-lookup"><span data-stu-id="6f062-116">Administrator</span></span> |<span data-ttu-id="6f062-117">Vytvořit nebo zřizování služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="6f062-117">Create or provision a BizTalk Service.</span></span> |[<span data-ttu-id="6f062-118">Vytvoření služby BizTalk pomocí portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="6f062-118">Create a BizTalk Service using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280) |
| <span data-ttu-id="6f062-119">3.</span><span class="sxs-lookup"><span data-stu-id="6f062-119">3.</span></span> |<span data-ttu-id="6f062-120">Správce</span><span class="sxs-lookup"><span data-stu-id="6f062-120">Administrator</span></span> |<span data-ttu-id="6f062-121">Registrace vám nebo vaší společnosti nasazení služby BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="6f062-121">Register you or your company’s BizTalk Services deployment</span></span> |[<span data-ttu-id="6f062-122">Registrace a aktualizace nasazení služby BizTalk na portálu služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="6f062-122">Registering and Updating a BizTalk Service Deployment on the BizTalk Services Portal</span></span>](https://msdn.microsoft.com/library/azure/hh689837.aspx) |
| <span data-ttu-id="6f062-123">4.</span><span class="sxs-lookup"><span data-stu-id="6f062-123">4.</span></span> |<span data-ttu-id="6f062-124">Správce</span><span class="sxs-lookup"><span data-stu-id="6f062-124">Administrator</span></span> |<span data-ttu-id="6f062-125">Platí, pokud aplikace používá služba BizTalk Adapter Service se připojit k-obchodní (LOB) v místním systému nebo používá fronta nebo téma cílový.</span><span class="sxs-lookup"><span data-stu-id="6f062-125">Applies if the application uses BizTalk Adapter Service to connect to an on-premises Line-of-Business (LOB) system or uses a Queue or Topic Destination.</span></span>  <span data-ttu-id="6f062-126">Vytvořte Namespace sběrnice služby Azure.</span><span class="sxs-lookup"><span data-stu-id="6f062-126">Create the Azure Service Bus Namespace.</span></span> <span data-ttu-id="6f062-127">Tento obor názvů, název vystavitele sběrnice služby a klíč vystavitele sběrnice služby hodnoty předáte vývojářům.</span><span class="sxs-lookup"><span data-stu-id="6f062-127">Give this namespace, Service Bus Issuer Name, and Service Bus Issuer Key values to the developer.</span></span> |<span data-ttu-id="6f062-128">[Postupy: vytvoření nebo úprava Namespace služby Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) a [hodnoty získat název vystavitele a klíč vystavitele](biztalk-issuer-name-issuer-key.md)</span><span class="sxs-lookup"><span data-stu-id="6f062-128">[How to: Create or Modify a Service Bus Service Namespace](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) and [Get Issuer Name and Issuer Key values](biztalk-issuer-name-issuer-key.md)</span></span> |
| <span data-ttu-id="6f062-129">5.</span><span class="sxs-lookup"><span data-stu-id="6f062-129">5.</span></span> |<span data-ttu-id="6f062-130">Developer</span><span class="sxs-lookup"><span data-stu-id="6f062-130">Developer</span></span> |<span data-ttu-id="6f062-131">Instalace sady SDK a vytvoření projektu služby BizTalk v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6f062-131">Install the SDK and create the BizTalk Service project in Visual Studio.</span></span> |<span data-ttu-id="6f062-132">[Instalace služby Azure BizTalk Services SDK](https://msdn.microsoft.com/library/azure/hh689760.aspx) a [vytvoření bohaté zasílání zpráv koncových bodů v Azure](https://msdn.microsoft.com/library/azure/hh689766.aspx)</span><span class="sxs-lookup"><span data-stu-id="6f062-132">[Install Azure BizTalk Services SDK](https://msdn.microsoft.com/library/azure/hh689760.aspx) and [Create Rich Messaging Endpoints on Azure](https://msdn.microsoft.com/library/azure/hh689766.aspx)</span></span> |
| <span data-ttu-id="6f062-133">6.</span><span class="sxs-lookup"><span data-stu-id="6f062-133">6.</span></span> |<span data-ttu-id="6f062-134">Developer</span><span class="sxs-lookup"><span data-stu-id="6f062-134">Developer</span></span> |<span data-ttu-id="6f062-135">Nasazení služby BizTalk projektu pro svoji službu BizTalk hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="6f062-135">Deploy your BizTalk Service project to your BizTalk Service hosted on Azure.</span></span> |[<span data-ttu-id="6f062-136">Nasazení a aktualizaci projektu služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="6f062-136">Deploying and Refreshing the BizTalk Services Project</span></span>](https://msdn.microsoft.com/library/azure/hh689881.aspx) |
| <span data-ttu-id="6f062-137">7.</span><span class="sxs-lookup"><span data-stu-id="6f062-137">7.</span></span> |<span data-ttu-id="6f062-138">Správce</span><span class="sxs-lookup"><span data-stu-id="6f062-138">Administrator</span></span> |<span data-ttu-id="6f062-139">Pokud používáte EDI se vztahuje.</span><span class="sxs-lookup"><span data-stu-id="6f062-139">Applies if you are using EDI.</span></span>  <span data-ttu-id="6f062-140">Můžete přidat partnery a vytvořte smlouvy na portálu Microsoft Azure BizTalk Services.</span><span class="sxs-lookup"><span data-stu-id="6f062-140">You can add Partners and create Agreements on the Microsoft Azure BizTalk Services Portal.</span></span> <span data-ttu-id="6f062-141">Když vytvoříte smlouvu, můžete přidat most nebo transformací vytvořené vývojáři nastavení smlouvy.</span><span class="sxs-lookup"><span data-stu-id="6f062-141">When you create an Agreement, you can add the bridge and/or Transforms created by the developer to the Agreement settings.</span></span> |[<span data-ttu-id="6f062-142">Konfigurace EDI a AS2, EDIFACT na portálu služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="6f062-142">Configuring EDI, AS2, and EDIFACT on BizTalk Services Portal</span></span>](https://msdn.microsoft.com/library/azure/hh689853.aspx) |
| <span data-ttu-id="6f062-143">8.</span><span class="sxs-lookup"><span data-stu-id="6f062-143">8.</span></span> |<span data-ttu-id="6f062-144">Správce</span><span class="sxs-lookup"><span data-stu-id="6f062-144">Administrator</span></span> |<span data-ttu-id="6f062-145">Pomocí portálu Azure classic, monitorování stavu služby BizTalk, včetně metrik výkonu.</span><span class="sxs-lookup"><span data-stu-id="6f062-145">Using the Azure classic Portal, monitor the health of your BizTalk Service, including performance metrics.</span></span> |[<span data-ttu-id="6f062-146">BizTalk Services: Karty Řídicí panel, Sledování a Škálování</span><span class="sxs-lookup"><span data-stu-id="6f062-146">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281) |
| <span data-ttu-id="6f062-147">9.</span><span class="sxs-lookup"><span data-stu-id="6f062-147">9.</span></span> |<span data-ttu-id="6f062-148">Správce</span><span class="sxs-lookup"><span data-stu-id="6f062-148">Administrator</span></span> |<span data-ttu-id="6f062-149">Pomocí portálu Microsoft Azure BizTalk Services, spravujte artefakty používané služby BizTalk Services a sledování zpráv, jako jsou zpracovány pomocí most soubory.</span><span class="sxs-lookup"><span data-stu-id="6f062-149">Using the Microsoft Azure BizTalk Services Portal, manage the artifacts used by BizTalk Services, and track messages as they are processed by the bridge files.</span></span> |[<span data-ttu-id="6f062-150">Pomocí portálu služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="6f062-150">Using the BizTalk Services Portal</span></span>](https://msdn.microsoft.com/library/azure/dn874043.aspx) |
| <span data-ttu-id="6f062-151">10.</span><span class="sxs-lookup"><span data-stu-id="6f062-151">10.</span></span> |<span data-ttu-id="6f062-152">Správce</span><span class="sxs-lookup"><span data-stu-id="6f062-152">Administrator</span></span> |<span data-ttu-id="6f062-153">Vytvoření plánu zálohování k zálohování službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="6f062-153">Create a backup plan to back up the BizTalk Service.</span></span> |[<span data-ttu-id="6f062-154">Kontinuita podnikových procesů a zotavení po havárii ve službě BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="6f062-154">Business Continuity and Disaster Recovery in BizTalk Services</span></span>](https://msdn.microsoft.com/library/azure/dn509557.aspx) |

## <a name="next-steps"></a><span data-ttu-id="6f062-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f062-155">Next Steps</span></span>
[<span data-ttu-id="6f062-156">Výukové programy a ukázky</span><span class="sxs-lookup"><span data-stu-id="6f062-156">Tutorials and Samples</span></span>](https://msdn.microsoft.com/library/azure/hh689895.aspx)

[<span data-ttu-id="6f062-157">Vytvoření projektu v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f062-157">Create the project in Visual Studio</span></span>](https://msdn.microsoft.com/library/azure/hh689811.aspx)

[<span data-ttu-id="6f062-158">Instalace služby Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="6f062-158">Install Azure BizTalk Services SDK</span></span>](https://msdn.microsoft.com/library/azure/hh689760.aspx)

## <a name="concepts"></a><span data-ttu-id="6f062-159">Koncepty</span><span class="sxs-lookup"><span data-stu-id="6f062-159">Concepts</span></span>
[<span data-ttu-id="6f062-160">Vytvoření projektu v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f062-160">Create the project in Visual Studio</span></span>](https://msdn.microsoft.com/library/azure/hh689811.aspx)  
[<span data-ttu-id="6f062-161">EDI, AS2 a zasílání zpráv EDIFACT (Business-Business)</span><span class="sxs-lookup"><span data-stu-id="6f062-161">EDI, AS2, and EDIFACT Messaging (Business to Business)</span></span>](https://msdn.microsoft.com/library/azure/hh689898.aspx)  

## <a name="other-resources"></a><span data-ttu-id="6f062-162">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="6f062-162">Other Resources</span></span>
[<span data-ttu-id="6f062-163">Přidání zdroje, cílové a most koncové body pro zasílání zpráv</span><span class="sxs-lookup"><span data-stu-id="6f062-163">Add Source, Destination, and Bridge Messaging Endpoints</span></span>](https://msdn.microsoft.com/library/azure/hh689877.aspx)  
[<span data-ttu-id="6f062-164">Další informace a vytvoření mapy zpráv a transformace</span><span class="sxs-lookup"><span data-stu-id="6f062-164">Learn and create Message Maps and Transforms</span></span>](https://msdn.microsoft.com/library/azure/hh689905.aspx)  
[<span data-ttu-id="6f062-165">Pomocí služby BizTalk Adapter Service (BAS)</span><span class="sxs-lookup"><span data-stu-id="6f062-165">Using the BizTalk Adapter Service (BAS)</span></span>](https://msdn.microsoft.com/library/azure/hh689889.aspx)  
[<span data-ttu-id="6f062-166">Služby Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="6f062-166">Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303664)
