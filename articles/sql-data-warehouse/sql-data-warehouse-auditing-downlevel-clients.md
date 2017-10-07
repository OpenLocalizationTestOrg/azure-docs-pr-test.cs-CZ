---
title: "Podpora klientů nižší úrovně aaaSQL datového skladu dat auditování | Microsoft Docs"
description: "Další informace o podpoře klientů nižší úrovně SQL Data Warehouse pro auditování dat"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: dfe29ff3-dfeb-4309-83c0-c1a300f4f44e
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 377488680eb297c3e9b1dc754c003c5b19b47996
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse----downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>SQL Data Warehouse – podpora klientů nižší úrovně pro auditování a dynamické maskování dat
[Auditování](sql-data-warehouse-auditing-overview.md) funguje s klienty SQL, které podporují přesměrování TDS.

Jakýkoli klient, který implementuje TDS 7.4 také podporuje přesměrování. Výjimky toothis zahrnuty JDBC 4.0 kterou hello funkci přesměrování není plně podporována a Tedious pro Node.JS, ve kterém nebyla implementována přesměrování.

Pro "Klienty nižší úrovně", tj. který podporují TDS verze 7.3 a níže - hello plně kvalifikovaný název domény serveru v připojovacím řetězci hello by měl být upraven:

Původní server plně kvalifikovaný název domény v připojovacím řetězci hello: <*název serveru*>. database.windows.net

Upravené plně kvalifikovaný název domény serveru v připojovacím řetězci hello: <*název serveru*> .database. **zabezpečené**. windows.net

Částečný seznam "Klienty nižší úrovně" zahrnuje:

* Rozhraní .NET 4.0 a níže,
* ODBC 10.0 a níže.
* JDBC (při JDBC podporuje TDS 7.4, hello TDS přesměrování funkce není plně podporována)
* Zdlouhavé (pro platformu Node.JS)

**Poznámka:** hello výše serveru úpravy plně kvalifikovaného názvu domény může být užitečné také pro použití zásad auditování na úrovni SQL serveru bez potřebu konfiguraci krok v každé databázi (dočasné zmírnění).     

