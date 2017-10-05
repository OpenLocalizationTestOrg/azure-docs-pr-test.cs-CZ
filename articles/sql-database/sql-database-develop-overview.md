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
# <a name="sql-database-application-development-overview"></a>Přehled vývoje aplikace SQL Database
V tomto článku se seznámíte se základními předpoklady, které by měl mít vývojář na zřeteli při zapisování kódu pro připojení ke službě Azure SQL Database.

> [!TIP]
> Pokud potřebujete kurz, ve kterém se naučíte vytvořit server, vytvořit bránu firewall založenou na serveru, zobrazit vlastnosti serveru, připojit se pomocí nástroje SQL Server Management Studio, vytvářet dotazy v hlavní databázi, vytvořit vzorovou databázi a prázdnou databázi nebo se připojit pomocí nástroje SQL Server Management Studio a vytvářet dotazy ve vzorové databázi, přejděte na stránku [kurzu Začínáme](sql-database-get-started-portal.md).
>

## <a name="language-and-platform"></a>Jazyk a platforma
K dispozici jsou ukázky kódu pro různé programovací jazyky a platformy. Odkazy na ukázky kódu najdete tady: 

* Další informace: [Připojení knihoven pro službu SQL Database a SQL Server](sql-database-libraries.md)

## <a name="tools"></a>Nástroje 
Můžete využít opensourcové nástroje, jako je [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli) nebo [VS Code](https://code.visualstudio.com/). Kromě toho Azure SQL Database pracuje s nástroji Microsoftu jako [Visual Studio](https://www.visualstudio.com/downloads/) a [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).  Můžete taky použít Portál pro správu Azure, PowerShell a rozhraní REST API, které vám pomůžou získat další produktivitu.

## <a name="resource-limitations"></a>Omezení prostředků
Azure SQL Database spravuje prostředky dostupné pro databázi pomocí dvou různých mechanismů: řízení prostředků a vynucení limitů.

* Další informace: [Limity prostředků Azure SQL Database](sql-database-resource-limits.md)

## <a name="security"></a>Zabezpečení
Azure SQL Database poskytuje prostředky pro omezení přístupu, ochranu dat a sledování aktivit služby SQL Database.

* Další informace: [Zabezpečení služby SQL Database](sql-database-security-overview.md)

## <a name="authentication"></a>Ověřování
* Azure SQL Database podporuje uživatele a přihlašování systému SQL Server i uživatele a přihlašování ověřování [Azure Active Directory](sql-database-aad-authentication.md).
* Musíte zadat konkrétní databázi místo nastavení na výchozí *hlavní* databázi.
* Nemůžete použít příkaz Transact-SQL **USE myDatabaseName;** ve službě SQL Database pro přechod na jinou databázi.
* Další informace: [Zabezpečení služby SQL Database: správa přístupu k databázi a zabezpečení přihlašování](sql-database-manage-logins.md)

## <a name="resiliency"></a>Odolnost
V případě přechodné chyby při připojování ke službě SQL Database by měl váš kód volání zopakovat.  Doporučujeme, aby logika dalších pokusů používala logiku opakování, aby služba SQL Database nebyla zaplavená opakovanými pokusy několika klientů současně.

* Ukázky kódu: U ukázek kódu, které ilustrují logiku opakovaných pokusů, se podívejte na ukázky jazyka podle vašeho výběru v části [Připojení knihoven pro službu SQL Database a systém SQL Server](sql-database-libraries.md)
* Další informace: [Chybové zprávy pro klientské programy služby SQL Database](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Správa připojení
* V logice připojování klienta přepište výchozí časový limit na 30 sekund.  Výchozí hodnota 15 sekund je příliš krátká pro připojení, která jsou závislá na internetu.
* Pokud používáte [fond připojení](http://msdn.microsoft.com/library/8xx3tyca.aspx), ukončete připojení v okamžiku, kdy ho program aktivně nepoužívá a není připravený na opakované použití.

## <a name="network-considerations"></a>Důležité informace o síti
* Na počítači, který hostuje klientský program, zajistěte, aby brána firewall umožňovala odchozí komunikaci TCP na portu 1433.  Další informace: [Konfigurace brány firewall služby Azure SQL Database](sql-database-configure-firewall-settings.md)
* Pokud váš klientský program připojí k databázi SQL průběhu vašeho klienta na virtuální počítač Azure (VM), musíte otevřít určité rozsahy portů ve virtuálním počítači. Další informace: [porty nad rámec 1433 pro technologii ADO.NET 4.5 a databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md)
* Připojení klienta ke službě Azure SQL Database někdy obcházení proxy serveru a komunikovat přímo s databází. Na významu nabývají jiné porty než 1433. Další informace najdete [architektura připojení k databázi SQL Azure](sql-database-connectivity-architecture.md) a [porty nad rámec 1433 pro technologii ADO.NET 4.5 a SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="data-sharding-with-elastic-scale"></a>Horizontálního dělení dat s elastickým Škálováním
Elastické škálování zjednodušuje proces škálování oběma směry. 

* [Vzory návrhu pro aplikace SaaS s více tenanty využívající Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Směrování závislé na datech](sql-database-elastic-scale-data-dependent-routing.md)
* [Začínáme s Azure SQL Database Elastic Scale Preview](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Další kroky
Prozkoumejte všechny [schopnosti služby SQL Database](sql-database-technical-overview.md).
