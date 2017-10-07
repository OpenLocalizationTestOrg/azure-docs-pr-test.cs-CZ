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
# <a name="managing-azure-sql-databases-using-azure-automation"></a>Správa databází Azure SQL pomocí Azure Automation.
Tento průvodce vás seznámí toohello služba Azure Automation a jak může být použité toosimplify správu databází Azure SQL.

## <a name="what-is-azure-automation"></a>Co je Azure Automation?
[Služby Azure Automation](https://azure.microsoft.com/services/automation/) je služba Azure pro zjednodušenou správu cloudu pomocí Automatizace procesu. Pomocí Azure Automation, může být časově náročné, ruční, problematických a často se opakujících úloh, automatizované tooincrease spolehlivost, efektivitu a toovalue čas pro vaši organizaci.

Azure Automation nabízí modulu provádění vysoce spolehlivé a vysoce dostupné pracovního postupu, který přizpůsobí vašim potřebám toomeet podle růstu vaší organizace. Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.

Nižší provozní režie a uvolněte IT / DevOps zaměstnanci toofocus na práci, kterou přidá obchodní hodnotu přesunutím vaší toobe úlohy správy cloudu automaticky spouštěné v Azure Automation.

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Jak Azure Automation pomoci spravovat databáze Azure SQL?
Azure SQL Database lze spravovat ve službě Azure Automation pomocí hello [rutiny prostředí PowerShell databáze SQL Azure](https://docs.microsoft.com/powershell/servicemanagement/azure.sqldatabase/v1.6.1/azure.sqldatabase/) které jsou k dispozici v hello [nástroje Azure PowerShell](/powershell/azure/overview). Služby Azure Automation má tyto rutiny prostředí PowerShell databáze SQL Azure k dispozici předinstalované hello, takže můžete provádět všechny úlohy správy vaší databázi SQL v rámci služby hello. Tyto rutiny ve službě Azure Automation pomocí hello rutin pro ostatní služby Azure, tooautomate složité úlohy může také párovat napříč službami Azure a systémech 3. stran.

Automatizace Azure má také možnost toocommunicate hello se SQL servery přímo, vydáním příkazů SQL pomocí prostředí PowerShell.

Hello [Galerie runbooků automatizace Azure](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) obsahuje celou řadu produktu team a Komunita sady runbook tooget začít s automatizací správu databáze SQL Azure, ostatní služby Azure a systémech 3. stran. Galerie runbooků patří:

* [Spouštění dotazů SQL na databázi systému SQL Server](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
* [Svisle škálování (nahoru nebo dolů) Azure SQL Database podle plánu](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
* [Zkrácení tabulky SQL, pokud jeho databáze blíží maximální velikosti](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
* [Index tabulky v databázi SQL Azure, pokud jsou vysoce fragmentován](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello Azure Automation a jak může být použité toomanage databází Azure SQL, použijte tyto odkazy toolearn Další informace o Azure Automation.

* [Přehled služby Azure Automation](../automation/automation-intro.md)
* [Můj první runbook](../automation/automation-first-runbook-graphical.md)
* [Mapy kurzů Azure Automation.](https://azure.microsoft.com/documentation/learning-paths/automation/)
* [Azure Automation: Vaše SQL Agent hello cloudu](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 

