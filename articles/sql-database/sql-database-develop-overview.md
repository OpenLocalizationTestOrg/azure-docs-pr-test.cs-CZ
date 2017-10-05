---
title: "Přehled vývoje databázových aplikací služby SQL Database | Dokumentace Microsoftu"
description: "Další informace o dostupných knihovnách připojení a osvědčených postupech pro aplikace, které se připojují ke službě SQL Database"
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
ms.openlocfilehash: 94257b68b3a0f62f4ade727277a904ceec082c05
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="sql-database-application-development-overview"></a><span data-ttu-id="f7b05-103">Přehled vývoje aplikace SQL Database</span><span class="sxs-lookup"><span data-stu-id="f7b05-103">SQL Database application development overview</span></span>
<span data-ttu-id="f7b05-104">V tomto článku se seznámíte se základními předpoklady, které by měl mít vývojář na zřeteli při zapisování kódu pro připojení ke službě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="f7b05-104">This article walks through the basic considerations that a developer should be aware of when writing code to connect to Azure SQL Database.</span></span>

> [!TIP]
> <span data-ttu-id="f7b05-105">Pokud potřebujete kurz, ve kterém se naučíte vytvořit server, vytvořit bránu firewall založenou na serveru, zobrazit vlastnosti serveru, připojit se pomocí nástroje SQL Server Management Studio, vytvářet dotazy v hlavní databázi, vytvořit vzorovou databázi a prázdnou databázi nebo se připojit pomocí nástroje SQL Server Management Studio a vytvářet dotazy ve vzorové databázi, přejděte na stránku [kurzu Začínáme](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f7b05-105">For a tutorial showing you how to create a server, create a server-based firewall, view server properties, connect using SQL Server Management Studio, query the master database, create a sample database and a blank database, query database properties, connect using SQL Server Management Studio, and query the sample database, see [Get Started Tutorial](sql-database-get-started-portal.md).</span></span>
>

## <a name="language-and-platform"></a><span data-ttu-id="f7b05-106">Jazyk a platforma</span><span class="sxs-lookup"><span data-stu-id="f7b05-106">Language and platform</span></span>
<span data-ttu-id="f7b05-107">K dispozici jsou ukázky kódu pro různé programovací jazyky a platformy.</span><span class="sxs-lookup"><span data-stu-id="f7b05-107">There are code samples available for various programming languages and platforms.</span></span> <span data-ttu-id="f7b05-108">Odkazy na ukázky kódu najdete tady:</span><span class="sxs-lookup"><span data-stu-id="f7b05-108">You can find links to the code samples at:</span></span> 

* <span data-ttu-id="f7b05-109">Další informace: [Připojení knihoven pro službu SQL Database a SQL Server](sql-database-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="f7b05-109">More Information: [Connection libraries for SQL Database and SQL Server](sql-database-libraries.md)</span></span>

## <a name="tools"></a><span data-ttu-id="f7b05-110">Nástroje</span><span class="sxs-lookup"><span data-stu-id="f7b05-110">Tools</span></span> 
<span data-ttu-id="f7b05-111">Můžete využít opensourcové nástroje, jako je [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli) nebo [VS Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="f7b05-111">You can leverage open source tools like [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli), [VS Code](https://code.visualstudio.com/).</span></span> <span data-ttu-id="f7b05-112">Kromě toho Azure SQL Database pracuje s nástroji Microsoftu jako [Visual Studio](https://www.visualstudio.com/downloads/) a [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7b05-112">Additionally, Azure SQL Database works with Microsoft tools like [Visual Studio](https://www.visualstudio.com/downloads/) and  [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).</span></span>  <span data-ttu-id="f7b05-113">Můžete taky použít Portál pro správu Azure, PowerShell a rozhraní REST API, které vám pomůžou získat další produktivitu.</span><span class="sxs-lookup"><span data-stu-id="f7b05-113">You can also use the Azure Management Portal, PowerShell, and REST APIs help you gain additional productivity.</span></span>

## <a name="resource-limitations"></a><span data-ttu-id="f7b05-114">Omezení prostředků</span><span class="sxs-lookup"><span data-stu-id="f7b05-114">Resource limitations</span></span>
<span data-ttu-id="f7b05-115">Azure SQL Database spravuje prostředky dostupné pro databázi pomocí dvou různých mechanismů: řízení prostředků a vynucení limitů.</span><span class="sxs-lookup"><span data-stu-id="f7b05-115">Azure SQL Database manages the resources available to a database using two different mechanisms: Resources Governance and Enforcement of Limits.</span></span>

* <span data-ttu-id="f7b05-116">Další informace: [Limity prostředků Azure SQL Database](sql-database-resource-limits.md)</span><span class="sxs-lookup"><span data-stu-id="f7b05-116">More Information: [Azure SQL Database resource limits](sql-database-resource-limits.md)</span></span>

## <a name="security"></a><span data-ttu-id="f7b05-117">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="f7b05-117">Security</span></span>
<span data-ttu-id="f7b05-118">Azure SQL Database poskytuje prostředky pro omezení přístupu, ochranu dat a sledování aktivit služby SQL Database.</span><span class="sxs-lookup"><span data-stu-id="f7b05-118">Azure SQL Database provides resources for limiting access, protecting data, and monitoring activities on a SQL Database.</span></span>

* <span data-ttu-id="f7b05-119">Další informace: [Zabezpečení služby SQL Database](sql-database-security-overview.md)</span><span class="sxs-lookup"><span data-stu-id="f7b05-119">More Information: [Securing your SQL Database](sql-database-security-overview.md)</span></span>

## <a name="authentication"></a><span data-ttu-id="f7b05-120">Ověřování</span><span class="sxs-lookup"><span data-stu-id="f7b05-120">Authentication</span></span>
* <span data-ttu-id="f7b05-121">Azure SQL Database podporuje uživatele a přihlašování systému SQL Server i uživatele a přihlašování ověřování [Azure Active Directory](sql-database-aad-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="f7b05-121">Azure SQL Database supports both SQL Server authentication users and logins, as well as [Azure Active Directory authentication](sql-database-aad-authentication.md) users and logins.</span></span>
* <span data-ttu-id="f7b05-122">Musíte zadat konkrétní databázi místo nastavení na výchozí *hlavní* databázi.</span><span class="sxs-lookup"><span data-stu-id="f7b05-122">You need to specify a particular database, instead of defaulting to the *master* database.</span></span>
* <span data-ttu-id="f7b05-123">Nemůžete použít příkaz Transact-SQL **USE myDatabaseName;** ve službě SQL Database pro přechod na jinou databázi.</span><span class="sxs-lookup"><span data-stu-id="f7b05-123">You cannot use the Transact-SQL **USE myDatabaseName;** statement on SQL Database to switch to another database.</span></span>
* <span data-ttu-id="f7b05-124">Další informace: [Zabezpečení služby SQL Database: správa přístupu k databázi a zabezpečení přihlašování](sql-database-manage-logins.md)</span><span class="sxs-lookup"><span data-stu-id="f7b05-124">More information: [SQL Database security: Manage database access and login security](sql-database-manage-logins.md)</span></span>

## <a name="resiliency"></a><span data-ttu-id="f7b05-125">Odolnost</span><span class="sxs-lookup"><span data-stu-id="f7b05-125">Resiliency</span></span>
<span data-ttu-id="f7b05-126">V případě přechodné chyby při připojování ke službě SQL Database by měl váš kód volání zopakovat.</span><span class="sxs-lookup"><span data-stu-id="f7b05-126">When a transient error occurs while connecting to SQL Database, your code should retry the call.</span></span>  <span data-ttu-id="f7b05-127">Doporučujeme, aby logika dalších pokusů používala logiku opakování, aby služba SQL Database nebyla zaplavená opakovanými pokusy několika klientů současně.</span><span class="sxs-lookup"><span data-stu-id="f7b05-127">We recommend that retry logic use backoff logic, so that it does not overwhelm the SQL Database with multiple clients retrying simultaneously.</span></span>

* <span data-ttu-id="f7b05-128">Ukázky kódu: U ukázek kódu, které ilustrují logiku opakovaných pokusů, se podívejte na ukázky jazyka podle vašeho výběru v části [Připojení knihoven pro službu SQL Database a systém SQL Server](sql-database-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="f7b05-128">Code samples:  For code samples that illustrate retry logic, see samples for the language of your choice at: [Connection libraries for SQL Database and SQL Server](sql-database-libraries.md)</span></span>
* <span data-ttu-id="f7b05-129">Další informace: [Chybové zprávy pro klientské programy služby SQL Database](sql-database-develop-error-messages.md)</span><span class="sxs-lookup"><span data-stu-id="f7b05-129">More information: [Error messages for SQL Database client programs](sql-database-develop-error-messages.md)</span></span>

## <a name="managing-connections"></a><span data-ttu-id="f7b05-130">Správa připojení</span><span class="sxs-lookup"><span data-stu-id="f7b05-130">Managing connections</span></span>
* <span data-ttu-id="f7b05-131">V logice připojování klienta přepište výchozí časový limit na 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="f7b05-131">In your client connection logic, override the default timeout to be 30 seconds.</span></span>  <span data-ttu-id="f7b05-132">Výchozí hodnota 15 sekund je příliš krátká pro připojení, která jsou závislá na internetu.</span><span class="sxs-lookup"><span data-stu-id="f7b05-132">The default of 15 seconds is too short for connections that depend on the internet.</span></span>
* <span data-ttu-id="f7b05-133">Pokud používáte [fond připojení](http://msdn.microsoft.com/library/8xx3tyca.aspx), ukončete připojení v okamžiku, kdy ho program aktivně nepoužívá a není připravený na opakované použití.</span><span class="sxs-lookup"><span data-stu-id="f7b05-133">If you are using a [connection pool](http://msdn.microsoft.com/library/8xx3tyca.aspx), be sure to close the connection the instant your program is not actively using it, and is not preparing to reuse it.</span></span>

## <a name="network-considerations"></a><span data-ttu-id="f7b05-134">Důležité informace o síti</span><span class="sxs-lookup"><span data-stu-id="f7b05-134">Network considerations</span></span>
* <span data-ttu-id="f7b05-135">Na počítači, který hostuje klientský program, zajistěte, aby brána firewall umožňovala odchozí komunikaci TCP na portu 1433.</span><span class="sxs-lookup"><span data-stu-id="f7b05-135">On the computer that hosts your client program, ensure the firewall allows outgoing TCP communication on port 1433.</span></span>  <span data-ttu-id="f7b05-136">Další informace: [Konfigurace brány firewall služby Azure SQL Database](sql-database-configure-firewall-settings.md)</span><span class="sxs-lookup"><span data-stu-id="f7b05-136">More information: [Configure an Azure SQL Database firewall](sql-database-configure-firewall-settings.md)</span></span>
* <span data-ttu-id="f7b05-137">Pokud váš klientský program připojí k databázi SQL průběhu vašeho klienta na virtuální počítač Azure (VM), musíte otevřít určité rozsahy portů ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="f7b05-137">If your client program connects to SQL Database while your client runs on an Azure virtual machine (VM), you must open certain port ranges on the VM.</span></span> <span data-ttu-id="f7b05-138">Další informace: [porty nad rámec 1433 pro technologii ADO.NET 4.5 a databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md)</span><span class="sxs-lookup"><span data-stu-id="f7b05-138">More information: [Ports beyond 1433 for ADO.NET 4.5 and SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md)</span></span>
* <span data-ttu-id="f7b05-139">Připojení klienta ke službě Azure SQL Database někdy obcházení proxy serveru a komunikovat přímo s databází.</span><span class="sxs-lookup"><span data-stu-id="f7b05-139">Client connections to Azure SQL Database sometimes bypass the proxy and interact directly with the database.</span></span> <span data-ttu-id="f7b05-140">Na významu nabývají jiné porty než 1433.</span><span class="sxs-lookup"><span data-stu-id="f7b05-140">Ports other than 1433 become important.</span></span> <span data-ttu-id="f7b05-141">Další informace najdete [architektura připojení k databázi SQL Azure](sql-database-connectivity-architecture.md) a [porty nad rámec 1433 pro technologii ADO.NET 4.5 a SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).</span><span class="sxs-lookup"><span data-stu-id="f7b05-141">For more information, [Azure SQL Database connectivity architecture](sql-database-connectivity-architecture.md) and [Ports beyond 1433 for ADO.NET 4.5 and SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).</span></span>

## <a name="data-sharding-with-elastic-scale"></a><span data-ttu-id="f7b05-142">Horizontálního dělení dat s elastickým Škálováním</span><span class="sxs-lookup"><span data-stu-id="f7b05-142">Data sharding with elastic scale</span></span>
<span data-ttu-id="f7b05-143">Elastické škálování zjednodušuje proces škálování oběma směry.</span><span class="sxs-lookup"><span data-stu-id="f7b05-143">Elastic Scale simplifies the process of scaling out (and in).</span></span> 

* [<span data-ttu-id="f7b05-144">Vzory návrhu pro aplikace SaaS s více tenanty využívající Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="f7b05-144">Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database</span></span>](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [<span data-ttu-id="f7b05-145">Směrování závislé na datech</span><span class="sxs-lookup"><span data-stu-id="f7b05-145">Data dependent routing</span></span>](sql-database-elastic-scale-data-dependent-routing.md)
* [<span data-ttu-id="f7b05-146">Začínáme s Azure SQL Database Elastic Scale Preview</span><span class="sxs-lookup"><span data-stu-id="f7b05-146">Get Started with Azure SQL Database Elastic Scale Preview</span></span>](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a><span data-ttu-id="f7b05-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f7b05-147">Next steps</span></span>
<span data-ttu-id="f7b05-148">Prozkoumejte všechny [schopnosti služby SQL Database](sql-database-technical-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f7b05-148">Explore all the [capabilities of SQL Database](sql-database-technical-overview.md)</span></span>
