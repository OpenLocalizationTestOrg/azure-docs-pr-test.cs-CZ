---
title: "Správa databází Azure SQL pomocí Azure Automation | Microsoft Docs"
description: "Další informace o používání služby Azure Automation pro správu databáze Azure SQL ve velkém měřítku."
services: sql-database, automation
documentationcenter: 
author: jodoglevy
manager: jhubbard
editor: monicar
ms.assetid: 77c262a1-9b93-456d-b3c7-b2f23bdfcd61
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: jhubbard
ms.openlocfilehash: 7f45b8b654691063823c13bee61e9bb874a6a13a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-sql-databases-using-azure-automation"></a><span data-ttu-id="5da12-103">Správa databází Azure SQL pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5da12-103">Managing Azure SQL Databases using Azure Automation</span></span>
<span data-ttu-id="5da12-104">Tento průvodce vás seznámí s služba Azure Automation a jak může sloužit ke zjednodušení správy vašich databází Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="5da12-104">This guide will introduce you to the Azure Automation service, and how it can be used to simplify management of your Azure SQL databases.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="5da12-105">Co je Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="5da12-105">What is Azure Automation?</span></span>
<span data-ttu-id="5da12-106">[Služby Azure Automation](https://azure.microsoft.com/services/automation/) je služba Azure pro zjednodušenou správu cloudu pomocí Automatizace procesu.</span><span class="sxs-lookup"><span data-stu-id="5da12-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="5da12-107">Pomocí Azure Automation, dlouhotrvajících, ruční, problematických a často se opakujících úloh je možné automatizovat zvýšit spolehlivost, efektivitu a času na hodnotu pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="5da12-107">Using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated to increase reliability, efficiency, and time to value for your organization.</span></span>

<span data-ttu-id="5da12-108">Azure Automation nabízí modul provádění vysoce spolehlivé a vysoce dostupné pracovního postupu, který rozšiřuje podle vašich potřeb podle růstu vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="5da12-108">Azure Automation provides a highly-reliable and highly-available workflow execution engine that scales to meet your needs as your organization grows.</span></span> <span data-ttu-id="5da12-109">Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="5da12-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="5da12-110">Nižší provozní režie a uvolněte IT / DevOps zaměstnanci a zaměřit se na práci, kterou přidá obchodní value přesunutím vašeho cloudu spuštění úloh správy se automaticky automatizace Azure.</span><span class="sxs-lookup"><span data-stu-id="5da12-110">Lower operational overhead and free up IT / DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a><span data-ttu-id="5da12-111">Jak Azure Automation pomoci spravovat databáze Azure SQL?</span><span class="sxs-lookup"><span data-stu-id="5da12-111">How can Azure Automation help manage Azure SQL databases?</span></span>
<span data-ttu-id="5da12-112">Azure SQL Database lze spravovat ve službě Azure Automation pomocí [rutiny prostředí PowerShell databáze SQL Azure](https://docs.microsoft.com/powershell/servicemanagement/azure.sqldatabase/v1.6.1/azure.sqldatabase/) které jsou k dispozici v [nástroje Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5da12-112">Azure SQL Database can be managed in Azure Automation by using the [Azure SQL Database PowerShell cmdlets](https://docs.microsoft.com/powershell/servicemanagement/azure.sqldatabase/v1.6.1/azure.sqldatabase/) that are available in the [Azure PowerShell tools](/powershell/azure/overview).</span></span> <span data-ttu-id="5da12-113">Automatizace Azure má tyto rutiny prostředí PowerShell databáze SQL Azure k dispozici předinstalované, aby mohli provést všechny úkoly správy SQL DB v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="5da12-113">Azure Automation has these Azure SQL Database PowerShell cmdlets available out of the box, so that you can perform all of your SQL DB management tasks within the service.</span></span> <span data-ttu-id="5da12-114">Může také párovat tyto rutiny ve službě Azure Automation pomocí rutin pro jinými službami Azure, na automatizují komplexní úlohy v služeb Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="5da12-114">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="5da12-115">Automatizace Azure má také možnost ke komunikaci se servery SQL přímo, vydáním příkazů SQL pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5da12-115">Azure Automation also has the ability to communicate with SQL servers directly, by issuing SQL commands using PowerShell.</span></span>

<span data-ttu-id="5da12-116">[Galerie runbooků automatizace Azure](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) obsahuje celou řadu produktu team a komunita runbooky tak, aby okamžitě začít s automatizací správu databáze SQL Azure, ostatní služby Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="5da12-116">The [Azure Automation runbook gallery](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) contains a variety of product team and community runbooks to get started automating management of Azure SQL Databases, other Azure services, and 3rd party systems.</span></span> <span data-ttu-id="5da12-117">Galerie runbooků patří:</span><span class="sxs-lookup"><span data-stu-id="5da12-117">Gallery runbooks include:</span></span>

* [<span data-ttu-id="5da12-118">Spouštění dotazů SQL na databázi systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="5da12-118">Run SQL queries against a SQL Server database</span></span>](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
* [<span data-ttu-id="5da12-119">Svisle škálování (nahoru nebo dolů) Azure SQL Database podle plánu</span><span class="sxs-lookup"><span data-stu-id="5da12-119">Vertically scale (up or down) an Azure SQL Database on a schedule</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
* [<span data-ttu-id="5da12-120">Zkrácení tabulky SQL, pokud jeho databáze blíží maximální velikosti</span><span class="sxs-lookup"><span data-stu-id="5da12-120">Truncate a SQL table if its database approaches its maximum size</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
* [<span data-ttu-id="5da12-121">Index tabulky v databázi SQL Azure, pokud jsou vysoce fragmentován</span><span class="sxs-lookup"><span data-stu-id="5da12-121">Index tables in an Azure SQL Database if they are highly fragmented</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a><span data-ttu-id="5da12-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5da12-122">Next steps</span></span>
<span data-ttu-id="5da12-123">Teď, když jste se naučili základy Azure Automation a jak může sloužit ke správě databází Azure SQL, postupujte podle následujících odkazech na další informace o Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5da12-123">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure SQL databases, follow these links to learn more about Azure Automation.</span></span>

* [<span data-ttu-id="5da12-124">Přehled služby Azure Automation</span><span class="sxs-lookup"><span data-stu-id="5da12-124">Azure Automation Overview</span></span>](../automation/automation-intro.md)
* [<span data-ttu-id="5da12-125">Můj první runbook</span><span class="sxs-lookup"><span data-stu-id="5da12-125">My first runbook</span></span>](../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="5da12-126">Mapy kurzů Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5da12-126">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)
* [<span data-ttu-id="5da12-127">Azure Automation: Agenta SQL v cloudu</span><span class="sxs-lookup"><span data-stu-id="5da12-127">Azure Automation: Your SQL Agent in the Cloud</span></span>](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 

