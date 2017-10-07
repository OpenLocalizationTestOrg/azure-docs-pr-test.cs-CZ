---
title: "Příklady skript prostředí PowerShell aaaAzure pro databázi SQL. | Microsoft Docs"
description: "Azure PowerShell skriptu příklady toohelp můžete vytvořit a spravovat servery Azure SQL Database, elastické fondy, databáze a brány firewall."
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: jhubbard
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: overview-samples
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 1130ffb0e1c2b94c676d564ad5c4eb3b86374dbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>Ukázek Azure PowerShell pro Azure SQL Database

Hello následující tabulka obsahuje odkazy toosample prostředí Azure PowerShell skripty pro databázi SQL Azure.

| |  |
|---|---|
|**Vytvoření jedné databáze a fondu elastické databáze**||
| [Vytvořte jednu databázi a nakonfigurujte pravidlo brány firewall](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript prostředí PowerShell vytvoří jednu databázi Azure SQL a nakonfiguruje pravidla brány firewall na úrovni serveru. |
| [Vytvoření elastických fondů a přesunutí databází ve fondu](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript prostředí PowerShell vytvoří fondů elastické databáze SQL Azure a přesune databáze ve fondu a změny úrovně výkonu.|
|**Konfigurace geografická replikace a převzetí služeb při selhání**||
| [Konfigurace a převzetí služeb při selhání jedné databáze používá aktivní geografickou replikaci](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript prostředí PowerShell nakonfiguruje aktivní geografickou replikací pro jednu databázi Azure SQL a převezme toohello sekundární repliky. |
| [Konfigurace a převzetí služeb při selhání ve fondu databáze používá aktivní geografickou replikaci](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript prostředí PowerShell nakonfiguruje aktivní geografickou replikací pro Azure SQL database v elastický fond SQL a převezme toohello sekundární repliky. |
| [Konfigurace a převzetí služeb při selhání a převzetí služeb při selhání skupiny pro jednu databázi (preview)](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript prostředí PowerShell nakonfiguruje skupinu převzetí služeb při selhání pro instanci serveru Azure SQL Database přidá skupinou databází toohello převzetí služeb při selhání a převezme toohello sekundární server |
|**Škálování izolovaných databází a fondu elastické databáze**||
| [Škálování jedné databáze](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript prostředí PowerShell monitoruje hello metriky výkonu databáze Azure SQL, se škáluje tooa vyšší úroveň výkonu a vytvoří pravidlo výstrahy na jednom hello metrik výkonu. |
| [Škálování fondu elastické databáze](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript prostředí PowerShell monitoruje hello metriky výkonu fondu elastické databáze Azure SQL Database, škáluje ho tooa vyšší úroveň výkonu a vytvoří pravidlo výstrahy na jednom hello metrik výkonu.  |
| **Auditování a zjišťování hrozeb** |
| [Konfigurace auditování a detekce hrozeb](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript prostředí PowerShell nakonfiguruje zásady detekce hrozeb a auditování pro databázi Azure SQL. |
| **Obnovit, kopírování a importování databáze.**||
| [Obnovení databáze](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript prostředí PowerShell obnoví databázi Azure SQL z geograficky redundantní zálohy a obnoví odstraněné Azure SQL toohello nejnovější zálohy databáze. |
| [Zkopírujte databázový server toonew](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript prostředí PowerShell vytvoří kopii existující databázi Azure SQL v nový server Azure SQL. |
| [Import ze souboru bacpac souboru databáze](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript Powershellu importuje databázový server Azure SQL tooan ze souboru bacpac souboru. |
| **Synchronizaci dat mezi databázemi**||
| [Synchronizaci dat mezi databází SQL](scripts/sql-database-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript prostředí PowerShell nakonfiguruje toosync synchronizaci dat mezi několika databází Azure SQL. |
| [Synchronizaci dat mezi SQL Database a SQL Server na místě](scripts/sql-database-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript prostředí PowerShell nakonfiguruje toosync synchronizaci dat mezi Azure SQL database a místní databázi systému SQL Server. |
|||
|||
