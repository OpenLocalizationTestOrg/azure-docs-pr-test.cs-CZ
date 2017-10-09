---
title: "aaaManage Azure RemoteApp pomocí Azure Automation | Microsoft Docs"
description: "Informace o tom, jak hello služby Azure Automation může být použité toomanage Azure RemoteApp."
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
ms.openlocfilehash: a4cb23e292c797256e971acb3b363b025f140f16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-remoteapp-using-azure-automation"></a><span data-ttu-id="9b2d5-103">Správa Azure RemoteApp pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="9b2d5-103">Managing Azure RemoteApp using Azure Automation</span></span>
> [!IMPORTANT]
> <span data-ttu-id="9b2d5-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="9b2d5-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="9b2d5-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="9b2d5-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="9b2d5-106">Tento průvodce vás seznámí toohello služba Azure Automation a jak může být použité toosimplify správu služby Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="9b2d5-106">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of Azure RemoteApp.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="9b2d5-107">Co je Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="9b2d5-107">What is Azure Automation?</span></span>
<span data-ttu-id="9b2d5-108">[Služby Azure Automation](../automation/automation-intro.md) je služba Azure pro zjednodušenou správu cloudu pomocí Automatizace procesu.</span><span class="sxs-lookup"><span data-stu-id="9b2d5-108">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="9b2d5-109">Pomocí Azure Automation, může být ruční často opakují, dlouhotrvající úkoly a k chybám, automatizované tooincrease spolehlivost, efektivitu a toovalue čas pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="9b2d5-109">Using Azure Automation, manual, frequently-repeated, long-running, and error-prone tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="9b2d5-110">Azure Automation nabízí modulu provádění vysoce spolehlivou a vysokou dostupností pracovního postupu, která je škálovatelná toomeet vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="9b2d5-110">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales toomeet your needs.</span></span> <span data-ttu-id="9b2d5-111">Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="9b2d5-111">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="9b2d5-112">Snížit provozní režie a uvolněte IT a spustit automaticky DevOps zaměstnanci toofocus na práci, kterou přidá obchodní hodnotu přesunutím vaší toobe úlohy správy cloudu Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="9b2d5-112">Reduce operational overhead and free up IT and DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-remoteapp"></a><span data-ttu-id="9b2d5-113">Jak Azure Automation pomoci spravovat službu Azure RemoteApp?</span><span class="sxs-lookup"><span data-stu-id="9b2d5-113">How can Azure Automation help manage Azure RemoteApp?</span></span>
<span data-ttu-id="9b2d5-114">Vzdálené aplikace RemoteApp lze spravovat ve službě Azure Automation pomocí rutin prostředí PowerShell text hello, které jsou k dispozici v hello [nástroje Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="9b2d5-114">RemoteApp can be managed in Azure Automation by using hello PowerShell cmdlets that are available in hello [Azure PowerShell tools](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="9b2d5-115">Automatizace Azure má těchto rutin RemoteApp prostředí PowerShell k dispozici předinstalované hello, aby mohli provést všechny úkoly správy vzdálené aplikace RemoteApp v rámci služby hello.</span><span class="sxs-lookup"><span data-stu-id="9b2d5-115">Azure Automation has these RemoteApp PowerShell cmdlets available out of hello box, so that you can perform all of your RemoteApp management tasks within hello service.</span></span> <span data-ttu-id="9b2d5-116">Tyto rutiny ve službě Azure Automation pomocí hello rutin pro ostatní služby Azure, tooautomate složité úlohy může také párovat napříč službami Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="9b2d5-116">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9b2d5-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9b2d5-117">Next steps</span></span>
<span data-ttu-id="9b2d5-118">Teď, když jste se naučili základy hello Azure Automation a jak může být použité toomanage Azure RemoteApp, postupujte podle těchto odkazů toolearn Další informace o Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="9b2d5-118">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure RemoteApp, follow these links toolearn more about Azure Automation.</span></span>

* <span data-ttu-id="9b2d5-119">V tématu hello Azure Automation [kurzu Začínáme](../automation/automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="9b2d5-119">See hello Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md)</span></span>

