---
title: "Spravovat službu Azure RemoteApp pomocí Azure Automation | Microsoft Docs"
description: "Informace o tom, jak služba Azure Automation slouží k spravovat službu Azure RemoteApp."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 589f01ef-f9c1-4e7b-a040-1b46862d3544
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: magoedte;csand
ms.openlocfilehash: 59ac11f153c0bd74a1106400dbbf5790968b657c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-remoteapp-using-azure-automation"></a><span data-ttu-id="aa504-103">Správa Azure RemoteApp pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="aa504-103">Managing Azure RemoteApp using Azure Automation</span></span>
> [!IMPORTANT]
> <span data-ttu-id="aa504-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="aa504-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="aa504-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="aa504-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="aa504-106">Tento průvodce vás seznámí s služba Azure Automation a jak může sloužit ke zjednodušení správy služby Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="aa504-106">This guide will introduce you to the Azure Automation service, and how it can be used to simplify management of Azure RemoteApp.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="aa504-107">Co je Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="aa504-107">What is Azure Automation?</span></span>
<span data-ttu-id="aa504-108">[Služby Azure Automation](../automation/automation-intro.md) je služba Azure pro zjednodušenou správu cloudu pomocí Automatizace procesu.</span><span class="sxs-lookup"><span data-stu-id="aa504-108">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="aa504-109">Pomocí Azure Automation, ruční, často opakují, dlouhotrvajících a náchylné úlohy je možné automatizovat zvýšit spolehlivost, efektivitu a času na hodnotu pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="aa504-109">Using Azure Automation, manual, frequently-repeated, long-running, and error-prone tasks can be automated to increase reliability, efficiency, and time to value for your organization.</span></span>

<span data-ttu-id="aa504-110">Azure Automation nabízí modul provádění vysoce spolehlivou a vysokou dostupností pracovního postupu, který rozšiřuje podle vašich potřeb.</span><span class="sxs-lookup"><span data-stu-id="aa504-110">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales to meet your needs.</span></span> <span data-ttu-id="aa504-111">Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="aa504-111">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="aa504-112">Snížit provozní režie a uvolněte IT a zaměstnanci DevOps a zaměřit se na práci, kterou přidá obchodní hodnotu přesunutím vašeho cloudu spuštění úloh správy se automaticky automatizace Azure.</span><span class="sxs-lookup"><span data-stu-id="aa504-112">Reduce operational overhead and free up IT and DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-remoteapp"></a><span data-ttu-id="aa504-113">Jak Azure Automation pomoci spravovat službu Azure RemoteApp?</span><span class="sxs-lookup"><span data-stu-id="aa504-113">How can Azure Automation help manage Azure RemoteApp?</span></span>
<span data-ttu-id="aa504-114">Vzdálené aplikace RemoteApp lze spravovat ve službě Azure Automation pomocí rutin prostředí PowerShell, které jsou k dispozici v [nástroje Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="aa504-114">RemoteApp can be managed in Azure Automation by using the PowerShell cmdlets that are available in the [Azure PowerShell tools](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="aa504-115">Automatizace Azure má těchto rutin RemoteApp prostředí PowerShell k dispozici předinstalované, aby mohli provést všechny úkoly správy vzdálené aplikace RemoteApp v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="aa504-115">Azure Automation has these RemoteApp PowerShell cmdlets available out of the box, so that you can perform all of your RemoteApp management tasks within the service.</span></span> <span data-ttu-id="aa504-116">Může také párovat tyto rutiny ve službě Azure Automation pomocí rutin pro jinými službami Azure, na automatizují komplexní úlohy v služeb Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="aa504-116">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and 3rd party systems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa504-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aa504-117">Next steps</span></span>
<span data-ttu-id="aa504-118">Teď, když jste se naučili základy Azure Automation a jak může sloužit ke správě Azure RemoteApp, postupujte podle následujících odkazech na další informace o Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="aa504-118">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure RemoteApp, follow these links to learn more about Azure Automation.</span></span>

* <span data-ttu-id="aa504-119">Najdete v části služby Azure Automation [kurz Začínáme](../automation/automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="aa504-119">See the Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md)</span></span>

