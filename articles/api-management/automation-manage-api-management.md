---
title: "Správa Azure API Management pomocí Azure Automation."
description: "Další informace o používání služby Azure Automation pro správu Azure API Management."
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
ms.openlocfilehash: 73a1135482b88ea7c228bc8b228d47c57b2e70a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-api-management-using-azure-automation"></a><span data-ttu-id="bebe4-103">Správa Azure API Management pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="bebe4-103">Managing Azure API Management using Azure Automation</span></span>
<span data-ttu-id="bebe4-104">Tento průvodce vás seznámí s služba Azure Automation a jak může sloužit ke zjednodušení správy služby Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="bebe4-104">This guide will introduce you to the Azure Automation service, and how it can be used to simplify management of Azure API Management.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="bebe4-105">Co je Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="bebe4-105">What is Azure Automation?</span></span>
<span data-ttu-id="bebe4-106">[Služby Azure Automation](https://azure.microsoft.com/services/automation/) je služba Azure pro zjednodušenou správu cloudu pomocí Automatizace procesu.</span><span class="sxs-lookup"><span data-stu-id="bebe4-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="bebe4-107">Pomocí Azure Automation, ruční, opakované, dlouhotrvajících a k chybám úloh je možné automatizovat zvýšit spolehlivost, efektivitu a času na hodnotu pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="bebe4-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated to increase reliability, efficiency, and time to value for your organization.</span></span>

<span data-ttu-id="bebe4-108">Azure Automation nabízí modul provádění vysoce spolehlivou a vysokou dostupností pracovního postupu, který rozšiřuje podle vašich potřeb.</span><span class="sxs-lookup"><span data-stu-id="bebe4-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales to meet your needs.</span></span> <span data-ttu-id="bebe4-109">Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="bebe4-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="bebe4-110">Snížit provozní režie a uvolněte IT a zaměstnanci DevOps a zaměřit se na práci, kterou přidá obchodní hodnotu přesunutím vašeho cloudu spuštění úloh správy se automaticky automatizace Azure.</span><span class="sxs-lookup"><span data-stu-id="bebe4-110">Reduce operational overhead and free up IT and DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-api-management"></a><span data-ttu-id="bebe4-111">Jak Azure Automation pomoci spravovat Azure API Management?</span><span class="sxs-lookup"><span data-stu-id="bebe4-111">How can Azure Automation help manage Azure API Management?</span></span>
<span data-ttu-id="bebe4-112">API Management můžete spravovat ve službě Azure Automation pomocí [rutiny prostředí Windows PowerShell pro Azure API Management API](https://azure.microsoft.com/updates/full-set-of-windows-powershell-cmdlets-for-azure-api-management-api/).</span><span class="sxs-lookup"><span data-stu-id="bebe4-112">API Management can be managed in Azure Automation by using the [Windows PowerShell cmdlets for Azure API Management API](https://azure.microsoft.com/updates/full-set-of-windows-powershell-cmdlets-for-azure-api-management-api/).</span></span> <span data-ttu-id="bebe4-113">V rámci Azure Automation můžete napsat skripty prostředí PowerShell pracovní postup provádět spoustu úlohami správy rozhraní API pomocí rutin.</span><span class="sxs-lookup"><span data-stu-id="bebe4-113">Within Azure Automation you can write PowerShell workflow scripts to perform many of your API Management tasks using the cmdlets.</span></span> <span data-ttu-id="bebe4-114">Může také párovat tyto rutiny ve službě Azure Automation pomocí rutin pro jinými službami Azure, na automatizují komplexní úlohy v služeb Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="bebe4-114">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="bebe4-115">Tady jsou některé příklady použití služby API Management s automatizace:</span><span class="sxs-lookup"><span data-stu-id="bebe4-115">Here are some examples of using API Management with Automation:</span></span>

* [<span data-ttu-id="bebe4-116">Azure API Management – pomocí prostředí PowerShell pro zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="bebe4-116">Azure API Management – Using PowerShell for backup and restore</span></span>](https://blogs.msdn.microsoft.com/katriend/2015/10/02/azure-api-management-using-powershell-for-backup-and-restore/)

## <a name="next-steps"></a><span data-ttu-id="bebe4-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bebe4-117">Next Steps</span></span>
<span data-ttu-id="bebe4-118">Teď, když jste se naučili základy Azure Automation a jak může sloužit ke správě Azure API Management, postupujte podle následujících odkazech na další informace.</span><span class="sxs-lookup"><span data-stu-id="bebe4-118">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure API Management, follow these links to learn more.</span></span>

* <span data-ttu-id="bebe4-119">V tématu Azure Automation [kurz Začínáme](../automation/automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="bebe4-119">See the Azure Automation [getting started tutorial](../automation/automation-first-runbook-graphical.md).</span></span>

