---
title: "aaaManage databáze SQL Azure pomocí Azure Automation | Microsoft Docs"
description: "Informace o tom, jak hello služby Azure Automation může být použité toomanage databází Azure SQL ve velkém měřítku."
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
ms.openlocfilehash: d613cca32ba86e27b9c1b952c4e723ea7f07beb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-sql-databases-using-azure-automation"></a><span data-ttu-id="efcb2-103">Správa databází Azure SQL pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="efcb2-103">Managing Azure SQL Databases using Azure Automation</span></span>
<span data-ttu-id="efcb2-104">Tento průvodce vás seznámí toohello služba Azure Automation a jak může být použité toosimplify správu databází Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="efcb2-104">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of your Azure SQL databases.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="efcb2-105">Co je Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="efcb2-105">What is Azure Automation?</span></span>
<span data-ttu-id="efcb2-106">[Služby Azure Automation](https://azure.microsoft.com/services/automation/) je služba Azure pro zjednodušenou správu cloudu pomocí Automatizace procesu.</span><span class="sxs-lookup"><span data-stu-id="efcb2-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="efcb2-107">Pomocí Azure Automation, může být časově náročné, ruční, problematických a často se opakujících úloh, automatizované tooincrease spolehlivost, efektivitu a toovalue čas pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="efcb2-107">Using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="efcb2-108">Azure Automation nabízí modulu provádění vysoce spolehlivé a vysoce dostupné pracovního postupu, který přizpůsobí vašim potřebám toomeet podle růstu vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="efcb2-108">Azure Automation provides a highly-reliable and highly-available workflow execution engine that scales toomeet your needs as your organization grows.</span></span> <span data-ttu-id="efcb2-109">Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="efcb2-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="efcb2-110">Nižší provozní režie a uvolněte IT / DevOps zaměstnanci toofocus na práci, kterou přidá obchodní hodnotu přesunutím vaší toobe úlohy správy cloudu automaticky spouštěné v Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="efcb2-110">Lower operational overhead and free up IT / DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a><span data-ttu-id="efcb2-111">Jak Azure Automation pomoci spravovat databáze Azure SQL?</span><span class="sxs-lookup"><span data-stu-id="efcb2-111">How can Azure Automation help manage Azure SQL databases?</span></span>
<span data-ttu-id="efcb2-112">Azure SQL Database lze spravovat ve službě Azure Automation pomocí hello [rutiny prostředí PowerShell databáze SQL Azure](https://docs.microsoft.com/powershell/servicemanagement/azure.sqldatabase/v1.6.1/azure.sqldatabase/) které jsou k dispozici v hello [nástroje Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="efcb2-112">Azure SQL Database can be managed in Azure Automation by using hello [Azure SQL Database PowerShell cmdlets](https://docs.microsoft.com/powershell/servicemanagement/azure.sqldatabase/v1.6.1/azure.sqldatabase/) that are available in hello [Azure PowerShell tools](/powershell/azure/overview).</span></span> <span data-ttu-id="efcb2-113">Služby Azure Automation má tyto rutiny prostředí PowerShell databáze SQL Azure k dispozici předinstalované hello, takže můžete provádět všechny úlohy správy vaší databázi SQL v rámci služby hello.</span><span class="sxs-lookup"><span data-stu-id="efcb2-113">Azure Automation has these Azure SQL Database PowerShell cmdlets available out of hello box, so that you can perform all of your SQL DB management tasks within hello service.</span></span> <span data-ttu-id="efcb2-114">Tyto rutiny ve službě Azure Automation pomocí hello rutin pro ostatní služby Azure, tooautomate složité úlohy může také párovat napříč službami Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="efcb2-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="efcb2-115">Automatizace Azure má také možnost toocommunicate hello se SQL servery přímo, vydáním příkazů SQL pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="efcb2-115">Azure Automation also has hello ability toocommunicate with SQL servers directly, by issuing SQL commands using PowerShell.</span></span>

<span data-ttu-id="efcb2-116">Hello [Galerie runbooků automatizace Azure](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) obsahuje celou řadu produktu team a Komunita sady runbook tooget začít s automatizací správu databáze SQL Azure, ostatní služby Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="efcb2-116">hello [Azure Automation runbook gallery](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) contains a variety of product team and community runbooks tooget started automating management of Azure SQL Databases, other Azure services, and 3rd party systems.</span></span> <span data-ttu-id="efcb2-117">Galerie runbooků patří:</span><span class="sxs-lookup"><span data-stu-id="efcb2-117">Gallery runbooks include:</span></span>

* [<span data-ttu-id="efcb2-118">Spouštění dotazů SQL na databázi systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="efcb2-118">Run SQL queries against a SQL Server database</span></span>](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
* [<span data-ttu-id="efcb2-119">Svisle škálování (nahoru nebo dolů) Azure SQL Database podle plánu</span><span class="sxs-lookup"><span data-stu-id="efcb2-119">Vertically scale (up or down) an Azure SQL Database on a schedule</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
* [<span data-ttu-id="efcb2-120">Zkrácení tabulky SQL, pokud jeho databáze blíží maximální velikosti</span><span class="sxs-lookup"><span data-stu-id="efcb2-120">Truncate a SQL table if its database approaches its maximum size</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
* [<span data-ttu-id="efcb2-121">Index tabulky v databázi SQL Azure, pokud jsou vysoce fragmentován</span><span class="sxs-lookup"><span data-stu-id="efcb2-121">Index tables in an Azure SQL Database if they are highly fragmented</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a><span data-ttu-id="efcb2-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="efcb2-122">Next steps</span></span>
<span data-ttu-id="efcb2-123">Teď, když jste se naučili základy hello Azure Automation a jak může být použité toomanage databází Azure SQL, použijte tyto odkazy toolearn Další informace o Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="efcb2-123">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure SQL databases, follow these links toolearn more about Azure Automation.</span></span>

* [<span data-ttu-id="efcb2-124">Přehled služby Azure Automation</span><span class="sxs-lookup"><span data-stu-id="efcb2-124">Azure Automation Overview</span></span>](../automation/automation-intro.md)
* [<span data-ttu-id="efcb2-125">Můj první runbook</span><span class="sxs-lookup"><span data-stu-id="efcb2-125">My first runbook</span></span>](../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="efcb2-126">Mapy kurzů Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="efcb2-126">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)
* [<span data-ttu-id="efcb2-127">Azure Automation: Vaše SQL Agent hello cloudu</span><span class="sxs-lookup"><span data-stu-id="efcb2-127">Azure Automation: Your SQL Agent in hello Cloud</span></span>](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 

