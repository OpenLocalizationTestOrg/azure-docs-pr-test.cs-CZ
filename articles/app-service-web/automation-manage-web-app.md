---
title: "Správa webové aplikace Azure pomocí Azure Automation | Microsoft Docs"
description: "Další informace o používání služby Azure Automation ke správě webové aplikace Azure."
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
ms.openlocfilehash: a96332346747114620ee6ebd2a2d0c4417d4a213
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="managing-azure-web-app-using-azure-automation"></a><span data-ttu-id="058d0-103">Správa webové aplikace Azure pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="058d0-103">Managing Azure Web App using Azure Automation</span></span>
<span data-ttu-id="058d0-104">Tento průvodce vás seznámí s služba Azure Automation a jak může sloužit ke zjednodušení správy webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="058d0-104">This guide will introduce you to the Azure Automation service, and how it can be used to simplify management of Azure Web App.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="058d0-105">Co je Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="058d0-105">What is Azure Automation?</span></span>
<span data-ttu-id="058d0-106">[Služby Azure Automation](../automation/automation-intro.md) je služba Azure pro zjednodušenou správu cloudu pomocí Automatizace procesu.</span><span class="sxs-lookup"><span data-stu-id="058d0-106">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="058d0-107">Pomocí Azure Automation, ruční, opakované, dlouhotrvajících a k chybám úloh je možné automatizovat zvýšit spolehlivost, efektivitu a času na hodnotu pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="058d0-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated to increase reliability, efficiency, and time to value for your organization.</span></span>

<span data-ttu-id="058d0-108">Azure Automation nabízí modul provádění vysoce spolehlivou a vysokou dostupností pracovního postupu, který rozšiřuje podle vašich potřeb.</span><span class="sxs-lookup"><span data-stu-id="058d0-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales to meet your needs.</span></span> <span data-ttu-id="058d0-109">Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="058d0-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="058d0-110">Snížit provozní režie a uvolněte IT a zaměstnanci DevOps a zaměřit se na práci, kterou přidá obchodní hodnotu přesunutím vašeho cloudu spuštění úloh správy se automaticky automatizace Azure.</span><span class="sxs-lookup"><span data-stu-id="058d0-110">Reduce operational overhead and free up IT and DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-web-app"></a><span data-ttu-id="058d0-111">Jak Azure Automation pomoci spravovat webové aplikace Azure?</span><span class="sxs-lookup"><span data-stu-id="058d0-111">How can Azure Automation help manage Azure Web App?</span></span>
<span data-ttu-id="058d0-112">Webové aplikace lze spravovat ve službě Azure Automation pomocí rutin prostředí PowerShell, které jsou k dispozici v [modulů prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="058d0-112">Web App can be managed in Azure Automation by using the PowerShell cmdlets that are available in the [Azure PowerShell modules](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="058d0-113">Můžete [nainstalovat tyto rutiny prostředí PowerShell webové aplikace ve službě Azure Automation](https://azure.microsoft.com/blog/announcing-azure-resource-manager-support-azure-automation-runbooks/), aby mohli provést všechny úkoly správy webové aplikace v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="058d0-113">You can [install these Web App PowerShell cmdlets in Azure Automation](https://azure.microsoft.com/blog/announcing-azure-resource-manager-support-azure-automation-runbooks/), so that you can perform all of your Web App management tasks within the service.</span></span> <span data-ttu-id="058d0-114">Může také párovat tyto rutiny ve službě Azure Automation pomocí rutin pro jinými službami Azure k automatizují komplexní úlohy v služeb Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="058d0-114">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services to automate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="058d0-115">Tady jsou některé příklady App Services s automatizací správy:</span><span class="sxs-lookup"><span data-stu-id="058d0-115">Here are some examples of managing App Services with Automation:</span></span>

* [<span data-ttu-id="058d0-116">Skripty pro správu webových aplikací</span><span class="sxs-lookup"><span data-stu-id="058d0-116">Scripts for managing Web Apps</span></span>](https://azure.microsoft.com/documentation/scripts/)

## <a name="next-steps"></a><span data-ttu-id="058d0-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="058d0-117">Next steps</span></span>
<span data-ttu-id="058d0-118">Teď, když jste se naučili základy Azure Automation a jak může sloužit ke správě webové aplikace Azure, postupujte podle následujících odkazech na další informace o Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="058d0-118">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure Web App, follow these links to learn more about Azure Automation.</span></span>

* <span data-ttu-id="058d0-119">Najdete v části služby Azure Automation [kurz Začínáme](../automation/automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="058d0-119">See the Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md)</span></span>

