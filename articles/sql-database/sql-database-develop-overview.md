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
# <a name="sql-database-application-development-overview"></a>Přehled vývoje aplikace SQL Database
Tento článek vás provede hello základní aspekty, které vývojáři měli vzít v potaz při psaní kódu tooconnect tooAzure databáze SQL.

> [!TIP]
> Kurz zobrazující můžete jak toocreate na server, vytvoření brány firewall na serveru, zobrazení vlastností serveru připojit pomocí SQL Server Management Studio, dotaz hello hlavní databázi, vytvoření ukázkové databáze a prázdnou databázi, dotaz na vlastnosti databáze připojení pomocí SQL Server Management Studio a dotaz hello ukázkové databáze, najdete v tématu [získat kurz](sql-database-get-started-portal.md).
>

## <a name="language-and-platform"></a>Jazyk a platforma
K dispozici jsou ukázky kódu pro různé programovací jazyky a platformy. Můžete najít ukázky kódu toohello odkazy na: 

* Další informace: [Připojení knihoven pro službu SQL Database a SQL Server](sql-database-libraries.md)

## <a name="tools"></a>Nástroje 
Můžete využít opensourcové nástroje, jako je [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli) nebo [VS Code](https://code.visualstudio.com/). Kromě toho Azure SQL Database pracuje s nástroji Microsoftu jako [Visual Studio](https://www.visualstudio.com/downloads/) a [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).  Můžete taky hello portálu pro správu Azure, prostředí PowerShell a rozhraní REST API, které vám pomůžou získat další produktivitu.

## <a name="resource-limitations"></a>Omezení prostředků
Azure SQL Database spravuje hello prostředky k dispozici tooa databáze pomocí dvou různých mechanismů: prostředky zásad správného řízení a vynucení omezení.

* Další informace: [Limity prostředků Azure SQL Database](sql-database-resource-limits.md)

## <a name="security"></a>Zabezpečení
Azure SQL Database poskytuje prostředky pro omezení přístupu, ochranu dat a sledování aktivit služby SQL Database.

* Další informace: [Zabezpečení služby SQL Database](sql-database-security-overview.md)

## <a name="authentication"></a>Ověřování
* Azure SQL Database podporuje uživatele a přihlašování systému SQL Server i uživatele a přihlašování ověřování [Azure Active Directory](sql-database-aad-authentication.md).
* Je třeba toospecify konkrétní databázi, namísto výchozí toohello *hlavní* databáze.
* Nemůžete použít hello Transact-SQL **použití myDatabaseName;** příkaz na databázi SQL tooswitch tooanother databáze.
* Další informace: [Zabezpečení služby SQL Database: správa přístupu k databázi a zabezpečení přihlašování](sql-database-manage-logins.md)

## <a name="resiliency"></a>Odolnost
V případě přechodná chyba při připojování tooSQL databáze by měl váš kód opakujte volání hello.  Doporučujeme logika opakovaných pokusů použít logiku omezení rychlosti tak, aby není zahlcovat hello databázi SQL s více klienty najednou opakování.

* Ukázky kódu jsou: ukázky kódu, které ilustrují logika opakovaných pokusů, najdete v části Ukázky pro jazyk hello zvoleného v: [knihovny připojení k databázi SQL a SQL Server](sql-database-libraries.md)
* Další informace: [Chybové zprávy pro klientské programy služby SQL Database](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Správa připojení
* Logiky připojení klienta přepište hello výchozí časový limit toobe 30 sekund.  Výchozí hodnota Hello 15 sekund je příliš krátká pro připojení, které jsou závislé na hello Internetu.
* Pokud používáte [fondu připojení](http://msdn.microsoft.com/library/8xx3tyca.aspx), že tooclose hello připojení hello rychlých aktivně nepoužívá, je váš program a není Příprava tooreuse ho.

## <a name="network-considerations"></a>Důležité informace o síti
* Na počítači hello, který hostuje váš klientský program zajistěte, aby hello brána firewall umožňuje odchozí komunikaci TCP na portu 1433.  Další informace: [Konfigurace brány firewall služby Azure SQL Database](sql-database-configure-firewall-settings.md)
* Pokud váš klientský program připojí tooSQL databáze, zatímco vašeho klienta běží na virtuální počítač Azure (VM), musíte otevřít určité rozsahy portů na hello virtuálních počítačů. Další informace: [porty nad rámec 1433 pro technologii ADO.NET 4.5 a databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md)
* Klient připojení tooAzure SQL Database někdy Nepoužívat proxy server hello a komunikovat přímo s databází hello. Na významu nabývají jiné porty než 1433. Další informace najdete [architektura připojení k databázi SQL Azure](sql-database-connectivity-architecture.md) a [porty nad rámec 1433 pro technologii ADO.NET 4.5 a SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="data-sharding-with-elastic-scale"></a>Horizontálního dělení dat s elastickým Škálováním
Elastické škálování zjednodušuje proces hello škálování out (a v). 

* [Vzory návrhu pro aplikace SaaS s více tenanty využívající Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Směrování závislé na datech](sql-database-elastic-scale-data-dependent-routing.md)
* [Začínáme s Azure SQL Database Elastic Scale Preview](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Další kroky
Prozkoumejte všechny hello [možnosti databáze SQL](sql-database-technical-overview.md)
