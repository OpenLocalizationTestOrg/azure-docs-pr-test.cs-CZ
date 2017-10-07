---
title: "aaaManage webové aplikace Azure pomocí Azure Automation | Microsoft Docs"
description: "Informace o tom, jak hello služby Azure Automation lze použít toomanage webové aplikace Azure."
services: app-service\web, automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: c960a44f-f941-401d-afba-a4bc0f0394eb
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: magoedte
ms.openlocfilehash: 6d80351a2927f26753cfbaead6e1c0c3c94e86e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-web-app-using-azure-automation"></a><span data-ttu-id="402aa-103">Správa webové aplikace Azure pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="402aa-103">Managing Azure Web App using Azure Automation</span></span>
<span data-ttu-id="402aa-104">Tento průvodce vás seznámí toohello služba Azure Automation a jak může být použité toosimplify Správa webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="402aa-104">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of Azure Web App.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="402aa-105">Co je Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="402aa-105">What is Azure Automation?</span></span>
<span data-ttu-id="402aa-106">[Služby Azure Automation](../automation/automation-intro.md) je služba Azure pro zjednodušenou správu cloudu pomocí Automatizace procesu.</span><span class="sxs-lookup"><span data-stu-id="402aa-106">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="402aa-107">Pomocí Azure Automation, může být ruční, opakované, dlouhotrvajících a náchylné úlohy automatizované tooincrease spolehlivost, efektivitu a toovalue čas pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="402aa-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="402aa-108">Azure Automation nabízí modulu provádění vysoce spolehlivou a vysokou dostupností pracovního postupu, která je škálovatelná toomeet vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="402aa-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales toomeet your needs.</span></span> <span data-ttu-id="402aa-109">Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="402aa-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="402aa-110">Snížit provozní režie a uvolněte IT a spustit automaticky DevOps zaměstnanci toofocus na práci, kterou přidá obchodní hodnotu přesunutím vaší toobe úlohy správy cloudu Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="402aa-110">Reduce operational overhead and free up IT and DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-web-app"></a><span data-ttu-id="402aa-111">Jak Azure Automation pomoci spravovat webové aplikace Azure?</span><span class="sxs-lookup"><span data-stu-id="402aa-111">How can Azure Automation help manage Azure Web App?</span></span>
<span data-ttu-id="402aa-112">Webové aplikace lze spravovat ve službě Azure Automation pomocí rutin prostředí PowerShell text hello, které jsou k dispozici v hello [modulů prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="402aa-112">Web App can be managed in Azure Automation by using hello PowerShell cmdlets that are available in hello [Azure PowerShell modules](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="402aa-113">Můžete [nainstalovat tyto rutiny prostředí PowerShell webové aplikace ve službě Azure Automation](https://azure.microsoft.com/blog/announcing-azure-resource-manager-support-azure-automation-runbooks/), aby mohli provést všechny úkoly správy webové aplikace v rámci služby hello.</span><span class="sxs-lookup"><span data-stu-id="402aa-113">You can [install these Web App PowerShell cmdlets in Azure Automation](https://azure.microsoft.com/blog/announcing-azure-resource-manager-support-azure-automation-runbooks/), so that you can perform all of your Web App management tasks within hello service.</span></span> <span data-ttu-id="402aa-114">Tyto rutiny ve službě Azure Automation pomocí hello rutin pro ostatní služby Azure tooautomate složité úlohy může také párovat napříč službami Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="402aa-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="402aa-115">Tady jsou některé příklady App Services s automatizací správy:</span><span class="sxs-lookup"><span data-stu-id="402aa-115">Here are some examples of managing App Services with Automation:</span></span>

* [<span data-ttu-id="402aa-116">Skripty pro správu webových aplikací</span><span class="sxs-lookup"><span data-stu-id="402aa-116">Scripts for managing Web Apps</span></span>](https://azure.microsoft.com/documentation/scripts/)

## <a name="next-steps"></a><span data-ttu-id="402aa-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="402aa-117">Next steps</span></span>
<span data-ttu-id="402aa-118">Teď, když jste se naučili základy hello Azure Automation a jak může být použité toomanage webové aplikace Azure, použijte tyto odkazy toolearn Další informace o Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="402aa-118">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure Web App, follow these links toolearn more about Azure Automation.</span></span>

* <span data-ttu-id="402aa-119">V tématu hello Azure Automation [kurzu Začínáme](../automation/automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="402aa-119">See hello Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md)</span></span>

