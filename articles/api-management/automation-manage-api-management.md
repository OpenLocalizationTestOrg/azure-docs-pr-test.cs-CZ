---
title: "aaaManage Azure API Management pomocí Azure Automation."
description: "Informace o tom, jak hello služby Azure Automation lze použít toomanage Azure API Management."
services: api-management, automation
documentationcenter: 
author: csand-msft
manager: eamono
editor: 
ms.assetid: 2e53c9af-f738-47f8-b1b6-593050a7c51b
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 05b8e3cad786fa701feb88f7b6d9629f16686d15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-api-management-using-azure-automation"></a><span data-ttu-id="0a8d1-103">Správa Azure API Management pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="0a8d1-103">Managing Azure API Management using Azure Automation</span></span>
<span data-ttu-id="0a8d1-104">Tento průvodce vás seznámí toohello služba Azure Automation a jak může být použité toosimplify správu služby Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="0a8d1-104">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of Azure API Management.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="0a8d1-105">Co je Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="0a8d1-105">What is Azure Automation?</span></span>
<span data-ttu-id="0a8d1-106">[Služby Azure Automation](https://azure.microsoft.com/services/automation/) je služba Azure pro zjednodušenou správu cloudu pomocí Automatizace procesu.</span><span class="sxs-lookup"><span data-stu-id="0a8d1-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="0a8d1-107">Pomocí Azure Automation, může být ruční, opakované, dlouhotrvajících a náchylné úlohy automatizované tooincrease spolehlivost, efektivitu a toovalue čas pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="0a8d1-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="0a8d1-108">Azure Automation nabízí modulu provádění vysoce spolehlivou a vysokou dostupností pracovního postupu, která je škálovatelná toomeet vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="0a8d1-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales toomeet your needs.</span></span> <span data-ttu-id="0a8d1-109">Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="0a8d1-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="0a8d1-110">Snížit provozní režie a uvolněte IT a spustit automaticky DevOps zaměstnanci toofocus na práci, kterou přidá obchodní hodnotu přesunutím vaší toobe úlohy správy cloudu Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="0a8d1-110">Reduce operational overhead and free up IT and DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-api-management"></a><span data-ttu-id="0a8d1-111">Jak Azure Automation pomoci spravovat Azure API Management?</span><span class="sxs-lookup"><span data-stu-id="0a8d1-111">How can Azure Automation help manage Azure API Management?</span></span>
<span data-ttu-id="0a8d1-112">API Management můžete spravovat ve službě Azure Automation pomocí hello [rutiny prostředí Windows PowerShell pro Azure API Management API](https://azure.microsoft.com/updates/full-set-of-windows-powershell-cmdlets-for-azure-api-management-api/).</span><span class="sxs-lookup"><span data-stu-id="0a8d1-112">API Management can be managed in Azure Automation by using hello [Windows PowerShell cmdlets for Azure API Management API](https://azure.microsoft.com/updates/full-set-of-windows-powershell-cmdlets-for-azure-api-management-api/).</span></span> <span data-ttu-id="0a8d1-113">V rámci Azure Automation můžete napsat tooperform skripty prostředí PowerShell workflow řadu úlohami správy rozhraní API pomocí rutin hello.</span><span class="sxs-lookup"><span data-stu-id="0a8d1-113">Within Azure Automation you can write PowerShell workflow scripts tooperform many of your API Management tasks using hello cmdlets.</span></span> <span data-ttu-id="0a8d1-114">Tyto rutiny ve službě Azure Automation pomocí hello rutin pro ostatní služby Azure, tooautomate složité úlohy může také párovat napříč službami Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="0a8d1-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="0a8d1-115">Tady jsou některé příklady použití služby API Management s automatizace:</span><span class="sxs-lookup"><span data-stu-id="0a8d1-115">Here are some examples of using API Management with Automation:</span></span>

* [<span data-ttu-id="0a8d1-116">Azure API Management – pomocí prostředí PowerShell pro zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="0a8d1-116">Azure API Management – Using PowerShell for backup and restore</span></span>](https://blogs.msdn.microsoft.com/katriend/2015/10/02/azure-api-management-using-powershell-for-backup-and-restore/)

## <a name="next-steps"></a><span data-ttu-id="0a8d1-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0a8d1-117">Next Steps</span></span>
<span data-ttu-id="0a8d1-118">Teď, když jste se naučili základy hello Azure Automation a jak může být použité toomanage Azure API Management, použijte tyto odkazy toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="0a8d1-118">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure API Management, follow these links toolearn more.</span></span>

* <span data-ttu-id="0a8d1-119">V tématu hello Azure Automation [kurz Začínáme](../automation/automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="0a8d1-119">See hello Azure Automation [getting started tutorial](../automation/automation-first-runbook-graphical.md).</span></span>

