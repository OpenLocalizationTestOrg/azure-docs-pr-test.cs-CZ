---
title: "aaaMigrate tooSQL vaše řešení datového skladu | Microsoft Docs"
description: "Migrace pokyny k uvedení platforma SQL Data Warehouse tooAzure vaše řešení."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 198365eb-7451-4222-b99c-d1d9ef687f1b
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/27/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: 27b51f15247603f054070f360ede7f24541c0288
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-solution-tooazure-sql-data-warehouse"></a>Migrace vašeho řešení tooAzure SQL Data Warehouse
Najdete v části Postup při migraci stávající tooAzure řešení databáze SQL Data Warehouse. 

## <a name="profile-your-workload"></a>Profil velikosti pracovní zátěže
Před migrací, budete chtít toobe určité že datový sklad SQL je hello správným řešením pro úlohy. Datový sklad SQL je tooperform analýzy velkých objemů dat distribuovaný systém určený.  Migrace tooSQL datového skladu vyžaduje některé změny návrhu, které nejsou příliš pevný toounderstand, ale může trvat některé tooimplement čas. Jestli podnik potřebuje podnikové třídy datového skladu, hello výhody jsou vhodné hello úsilí. Pokud nepotřebujete hello power služby SQL Data Warehouse, je však další nákladově efektivní toouse systému SQL Server nebo Azure SQL Database.

Zvažte použití SQL Data Warehouse při můžete:
- Mít jeden nebo více terabajtů dat
- Plán toorun analýzy velkých objemů dat.
- Třeba hello možnost tooscale výpočetního prostředí a úložiště 
- Chtít toosave náklady ponecháte-výpočetní prostředky, když je nepotřebujete.

Nepoužívejte SQL Data Warehouse pro provozní úlohy (OLTP), které mají:
- Vysoká frekvence čte a zapisuje
- Vybere velkého počtu singleton
- Velký objem jednoho řádku vložení
- Řádek po řádku zpracování potřeb
- Nekompatibilních formátech (JSON, XML)


## <a name="plan-hello-migration"></a>Plánování migrace hello

Jakmile jste se rozhodli toomigrate existující řešení tooSQL datového skladu, je důležité tooplan hello migrace před Začínáme. 

Jeden cíl plánování tooensure data, vaše schémata tabulek a kódu jsou kompatibilní s SQL Data Warehouse. Existují toowork některé kompatibility rozdíly kolem mezi aktuálním systému a SQL Data Warehouse. Plus migraci velkých objemů dat tooAzure trvá určitou dobu. Pečlivé plánování urychluje získávání tooAzure vaše data. 

Jiné cílem plánování je toomake návrhu tooensure úpravy, které řešení využívá výhod hello vysokého výkonu dotazu, který datový sklad SQL je určený tooprovide. Návrh datových skladů pro škálování zavádí vzory návrhu různých a proto tradiční přístupy nejsou vždy hello nejlépe. I když můžete provést některé úpravy návrhu po migraci, uloží dříve provádění změn v procesu hello později.

tooperform úspěšné migrace, je nutné toomigrate vaše schémata tabulek, kód a data. Pokyny v těchto tématech migrace najdete v tématu:

-  [Migrace vaší schémata](sql-data-warehouse-migrate-schema.md)
-  [Migrace vašeho kódu](sql-data-warehouse-migrate-code.md)
-  [Migrace dat](sql-data-warehouse-migrate-data.md). 

<!--
## Perform hello migration


## Deploy hello solution


## Validate hello migration

-->

## <a name="next-steps"></a>Další kroky
Hello CAT (poradní tým) má také některé skvělé SQL Data Warehouse pokyny, které publikují prostřednictvím blogy.  Podívejte se na jejich článku [migrace dat tooAzure SQL Data Warehouse v praxi] [ Migrating data tooAzure SQL Data Warehouse in practice] o další pokyny k migraci.

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
[Migrating data tooAzure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
