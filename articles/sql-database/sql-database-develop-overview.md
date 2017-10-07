---
title: "Přehled vývoje aplikace databáze aaaSQL | Microsoft Docs"
description: "Další informace o dostupné připojení knihovny a osvědčené postupy pro připojování tooSQL databáze aplikace."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: genemi
ms.assetid: 67c02204-d1bd-4622-acce-92115a7cde03
ms.service: sql-database
ms.custom: develop apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: sstein
ms.openlocfilehash: 17f04db600828f90c42c750c9abdb92cfa4ca817
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-database-application-development-overview"></a><span data-ttu-id="25fa8-103">Přehled vývoje aplikace SQL Database</span><span class="sxs-lookup"><span data-stu-id="25fa8-103">SQL Database application development overview</span></span>
<span data-ttu-id="25fa8-104">Tento článek vás provede hello základní aspekty, které vývojáři měli vzít v potaz při psaní kódu tooconnect tooAzure databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="25fa8-104">This article walks through hello basic considerations that a developer should be aware of when writing code tooconnect tooAzure SQL Database.</span></span>

> [!TIP]
> <span data-ttu-id="25fa8-105">Kurz zobrazující můžete jak toocreate na server, vytvoření brány firewall na serveru, zobrazení vlastností serveru připojit pomocí SQL Server Management Studio, dotaz hello hlavní databázi, vytvoření ukázkové databáze a prázdnou databázi, dotaz na vlastnosti databáze připojení pomocí SQL Server Management Studio a dotaz hello ukázkové databáze, najdete v tématu [získat kurz](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="25fa8-105">For a tutorial showing you how toocreate a server, create a server-based firewall, view server properties, connect using SQL Server Management Studio, query hello master database, create a sample database and a blank database, query database properties, connect using SQL Server Management Studio, and query hello sample database, see [Get Started Tutorial](sql-database-get-started-portal.md).</span></span>
>

## <a name="language-and-platform"></a><span data-ttu-id="25fa8-106">Jazyk a platforma</span><span class="sxs-lookup"><span data-stu-id="25fa8-106">Language and platform</span></span>
<span data-ttu-id="25fa8-107">K dispozici jsou ukázky kódu pro různé programovací jazyky a platformy.</span><span class="sxs-lookup"><span data-stu-id="25fa8-107">There are code samples available for various programming languages and platforms.</span></span> <span data-ttu-id="25fa8-108">Můžete najít ukázky kódu toohello odkazy na:</span><span class="sxs-lookup"><span data-stu-id="25fa8-108">You can find links toohello code samples at:</span></span> 

* <span data-ttu-id="25fa8-109">Další informace: [Připojení knihoven pro službu SQL Database a SQL Server](sql-database-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="25fa8-109">More Information: [Connection libraries for SQL Database and SQL Server](sql-database-libraries.md)</span></span>

## <a name="tools"></a><span data-ttu-id="25fa8-110">Nástroje</span><span class="sxs-lookup"><span data-stu-id="25fa8-110">Tools</span></span> 
<span data-ttu-id="25fa8-111">Můžete využít opensourcové nástroje, jako je [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli) nebo [VS Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="25fa8-111">You can leverage open source tools like [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli), [VS Code](https://code.visualstudio.com/).</span></span> <span data-ttu-id="25fa8-112">Kromě toho Azure SQL Database pracuje s nástroji Microsoftu jako [Visual Studio](https://www.visualstudio.com/downloads/) a [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).</span><span class="sxs-lookup"><span data-stu-id="25fa8-112">Additionally, Azure SQL Database works with Microsoft tools like [Visual Studio](https://www.visualstudio.com/downloads/) and  [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).</span></span>  <span data-ttu-id="25fa8-113">Můžete taky hello portálu pro správu Azure, prostředí PowerShell a rozhraní REST API, které vám pomůžou získat další produktivitu.</span><span class="sxs-lookup"><span data-stu-id="25fa8-113">You can also use hello Azure Management Portal, PowerShell, and REST APIs help you gain additional productivity.</span></span>

## <a name="resource-limitations"></a><span data-ttu-id="25fa8-114">Omezení prostředků</span><span class="sxs-lookup"><span data-stu-id="25fa8-114">Resource limitations</span></span>
<span data-ttu-id="25fa8-115">Azure SQL Database spravuje hello prostředky k dispozici tooa databáze pomocí dvou různých mechanismů: prostředky zásad správného řízení a vynucení omezení.</span><span class="sxs-lookup"><span data-stu-id="25fa8-115">Azure SQL Database manages hello resources available tooa database using two different mechanisms: Resources Governance and Enforcement of Limits.</span></span>

* <span data-ttu-id="25fa8-116">Další informace: [Limity prostředků Azure SQL Database](sql-database-resource-limits.md)</span><span class="sxs-lookup"><span data-stu-id="25fa8-116">More Information: [Azure SQL Database resource limits](sql-database-resource-limits.md)</span></span>

## <a name="security"></a><span data-ttu-id="25fa8-117">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="25fa8-117">Security</span></span>
<span data-ttu-id="25fa8-118">Azure SQL Database poskytuje prostředky pro omezení přístupu, ochranu dat a sledování aktivit služby SQL Database.</span><span class="sxs-lookup"><span data-stu-id="25fa8-118">Azure SQL Database provides resources for limiting access, protecting data, and monitoring activities on a SQL Database.</span></span>

* <span data-ttu-id="25fa8-119">Další informace: [Zabezpečení služby SQL Database](sql-database-security-overview.md)</span><span class="sxs-lookup"><span data-stu-id="25fa8-119">More Information: [Securing your SQL Database](sql-database-security-overview.md)</span></span>

## <a name="authentication"></a><span data-ttu-id="25fa8-120">Ověřování</span><span class="sxs-lookup"><span data-stu-id="25fa8-120">Authentication</span></span>
* <span data-ttu-id="25fa8-121">Azure SQL Database podporuje uživatele a přihlašování systému SQL Server i uživatele a přihlašování ověřování [Azure Active Directory](sql-database-aad-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="25fa8-121">Azure SQL Database supports both SQL Server authentication users and logins, as well as [Azure Active Directory authentication](sql-database-aad-authentication.md) users and logins.</span></span>
* <span data-ttu-id="25fa8-122">Je třeba toospecify konkrétní databázi, namísto výchozí toohello *hlavní* databáze.</span><span class="sxs-lookup"><span data-stu-id="25fa8-122">You need toospecify a particular database, instead of defaulting toohello *master* database.</span></span>
* <span data-ttu-id="25fa8-123">Nemůžete použít hello Transact-SQL **použití myDatabaseName;** příkaz na databázi SQL tooswitch tooanother databáze.</span><span class="sxs-lookup"><span data-stu-id="25fa8-123">You cannot use hello Transact-SQL **USE myDatabaseName;** statement on SQL Database tooswitch tooanother database.</span></span>
* <span data-ttu-id="25fa8-124">Další informace: [Zabezpečení služby SQL Database: správa přístupu k databázi a zabezpečení přihlašování](sql-database-manage-logins.md)</span><span class="sxs-lookup"><span data-stu-id="25fa8-124">More information: [SQL Database security: Manage database access and login security](sql-database-manage-logins.md)</span></span>

## <a name="resiliency"></a><span data-ttu-id="25fa8-125">Odolnost</span><span class="sxs-lookup"><span data-stu-id="25fa8-125">Resiliency</span></span>
<span data-ttu-id="25fa8-126">V případě přechodná chyba při připojování tooSQL databáze by měl váš kód opakujte volání hello.</span><span class="sxs-lookup"><span data-stu-id="25fa8-126">When a transient error occurs while connecting tooSQL Database, your code should retry hello call.</span></span>  <span data-ttu-id="25fa8-127">Doporučujeme logika opakovaných pokusů použít logiku omezení rychlosti tak, aby není zahlcovat hello databázi SQL s více klienty najednou opakování.</span><span class="sxs-lookup"><span data-stu-id="25fa8-127">We recommend that retry logic use backoff logic, so that it does not overwhelm hello SQL Database with multiple clients retrying simultaneously.</span></span>

* <span data-ttu-id="25fa8-128">Ukázky kódu jsou: ukázky kódu, které ilustrují logika opakovaných pokusů, najdete v části Ukázky pro jazyk hello zvoleného v: [knihovny připojení k databázi SQL a SQL Server](sql-database-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="25fa8-128">Code samples:  For code samples that illustrate retry logic, see samples for hello language of your choice at: [Connection libraries for SQL Database and SQL Server](sql-database-libraries.md)</span></span>
* <span data-ttu-id="25fa8-129">Další informace: [Chybové zprávy pro klientské programy služby SQL Database](sql-database-develop-error-messages.md)</span><span class="sxs-lookup"><span data-stu-id="25fa8-129">More information: [Error messages for SQL Database client programs](sql-database-develop-error-messages.md)</span></span>

## <a name="managing-connections"></a><span data-ttu-id="25fa8-130">Správa připojení</span><span class="sxs-lookup"><span data-stu-id="25fa8-130">Managing connections</span></span>
* <span data-ttu-id="25fa8-131">Logiky připojení klienta přepište hello výchozí časový limit toobe 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="25fa8-131">In your client connection logic, override hello default timeout toobe 30 seconds.</span></span>  <span data-ttu-id="25fa8-132">Výchozí hodnota Hello 15 sekund je příliš krátká pro připojení, které jsou závislé na hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="25fa8-132">hello default of 15 seconds is too short for connections that depend on hello internet.</span></span>
* <span data-ttu-id="25fa8-133">Pokud používáte [fondu připojení](http://msdn.microsoft.com/library/8xx3tyca.aspx), že tooclose hello připojení hello rychlých aktivně nepoužívá, je váš program a není Příprava tooreuse ho.</span><span class="sxs-lookup"><span data-stu-id="25fa8-133">If you are using a [connection pool](http://msdn.microsoft.com/library/8xx3tyca.aspx), be sure tooclose hello connection hello instant your program is not actively using it, and is not preparing tooreuse it.</span></span>

## <a name="network-considerations"></a><span data-ttu-id="25fa8-134">Důležité informace o síti</span><span class="sxs-lookup"><span data-stu-id="25fa8-134">Network considerations</span></span>
* <span data-ttu-id="25fa8-135">Na počítači hello, který hostuje váš klientský program zajistěte, aby hello brána firewall umožňuje odchozí komunikaci TCP na portu 1433.</span><span class="sxs-lookup"><span data-stu-id="25fa8-135">On hello computer that hosts your client program, ensure hello firewall allows outgoing TCP communication on port 1433.</span></span>  <span data-ttu-id="25fa8-136">Další informace: [Konfigurace brány firewall služby Azure SQL Database](sql-database-configure-firewall-settings.md)</span><span class="sxs-lookup"><span data-stu-id="25fa8-136">More information: [Configure an Azure SQL Database firewall](sql-database-configure-firewall-settings.md)</span></span>
* <span data-ttu-id="25fa8-137">Pokud váš klientský program připojí tooSQL databáze, zatímco vašeho klienta běží na virtuální počítač Azure (VM), musíte otevřít určité rozsahy portů na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="25fa8-137">If your client program connects tooSQL Database while your client runs on an Azure virtual machine (VM), you must open certain port ranges on hello VM.</span></span> <span data-ttu-id="25fa8-138">Další informace: [porty nad rámec 1433 pro technologii ADO.NET 4.5 a databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md)</span><span class="sxs-lookup"><span data-stu-id="25fa8-138">More information: [Ports beyond 1433 for ADO.NET 4.5 and SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md)</span></span>
* <span data-ttu-id="25fa8-139">Klient připojení tooAzure SQL Database někdy Nepoužívat proxy server hello a komunikovat přímo s databází hello.</span><span class="sxs-lookup"><span data-stu-id="25fa8-139">Client connections tooAzure SQL Database sometimes bypass hello proxy and interact directly with hello database.</span></span> <span data-ttu-id="25fa8-140">Na významu nabývají jiné porty než 1433.</span><span class="sxs-lookup"><span data-stu-id="25fa8-140">Ports other than 1433 become important.</span></span> <span data-ttu-id="25fa8-141">Další informace najdete [architektura připojení k databázi SQL Azure](sql-database-connectivity-architecture.md) a [porty nad rámec 1433 pro technologii ADO.NET 4.5 a SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).</span><span class="sxs-lookup"><span data-stu-id="25fa8-141">For more information, [Azure SQL Database connectivity architecture](sql-database-connectivity-architecture.md) and [Ports beyond 1433 for ADO.NET 4.5 and SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).</span></span>

## <a name="data-sharding-with-elastic-scale"></a><span data-ttu-id="25fa8-142">Horizontálního dělení dat s elastickým Škálováním</span><span class="sxs-lookup"><span data-stu-id="25fa8-142">Data sharding with elastic scale</span></span>
<span data-ttu-id="25fa8-143">Elastické škálování zjednodušuje proces hello škálování out (a v).</span><span class="sxs-lookup"><span data-stu-id="25fa8-143">Elastic Scale simplifies hello process of scaling out (and in).</span></span> 

* [<span data-ttu-id="25fa8-144">Vzory návrhu pro aplikace SaaS s více tenanty využívající Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="25fa8-144">Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database</span></span>](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [<span data-ttu-id="25fa8-145">Směrování závislé na datech</span><span class="sxs-lookup"><span data-stu-id="25fa8-145">Data dependent routing</span></span>](sql-database-elastic-scale-data-dependent-routing.md)
* [<span data-ttu-id="25fa8-146">Začínáme s Azure SQL Database Elastic Scale Preview</span><span class="sxs-lookup"><span data-stu-id="25fa8-146">Get Started with Azure SQL Database Elastic Scale Preview</span></span>](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a><span data-ttu-id="25fa8-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25fa8-147">Next steps</span></span>
<span data-ttu-id="25fa8-148">Prozkoumejte všechny hello [možnosti databáze SQL](sql-database-technical-overview.md)</span><span class="sxs-lookup"><span data-stu-id="25fa8-148">Explore all hello [capabilities of SQL Database](sql-database-technical-overview.md)</span></span>
