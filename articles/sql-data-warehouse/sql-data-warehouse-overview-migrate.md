---
title: "Migrace vašeho řešení do SQL Data Warehouse | Microsoft Docs"
description: "Migrace pokyny k uvedení řešení pro platformu Azure SQL Data Warehouse."
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
ms.openlocfilehash: 771b9456e66b8a1e41f72340b695b19e2adaf793
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-your-solution-to-azure-sql-data-warehouse"></a>Migrace vašeho řešení do Azure SQL Data Warehouse
Najdete v části Postup při migraci do stávajícího řešení pro databázi Azure SQL Data Warehouse. 

## <a name="profile-your-workload"></a>Profil velikosti pracovní zátěže
Před migrací, mají být, že určité SQL Data Warehouse je to správné řešení pro úlohy. Datový sklad SQL je navržený tak, aby provádět analýzy na velkých objemů dat distribuovaného systému.  Migrace do SQL Data Warehouse vyžaduje některé změny návrhu, které nejsou příliš pevného, abyste pochopili, ale může trvat nějakou dobu implementace. Jestli podnik potřebuje podnikové třídy datového skladu, výhody to stojí. Pokud nepotřebujete power služby SQL Data Warehouse, je však cenově výhodnější používat SQL Server nebo Azure SQL Database.

Zvažte použití SQL Data Warehouse při můžete:
- Mít jeden nebo více terabajtů dat
- Chcete spustit analýzu na velkých objemů dat.
- Potřebují možnost škálovat výpočetní kapacity a úložiště 
- Chcete uložit náklady pozastavení výpočetní prostředky, když je nepotřebujete.

Nepoužívejte SQL Data Warehouse pro provozní úlohy (OLTP), které mají:
- Vysoká frekvence čte a zapisuje
- Vybere velkého počtu singleton
- Velký objem jednoho řádku vložení
- Řádek po řádku zpracování potřeb
- Nekompatibilních formátech (JSON, XML)


## <a name="plan-the-migration"></a>Plánování migrace

Až se rozhodnete k migraci existujícího řešení do SQL Data Warehouse, je důležité plánovat migraci před Začínáme. 

Plánování jeden cílem je zajistit, že vaše data, vaše schémata tabulek a kódu jsou kompatibilní s SQL Data Warehouse. Existují určité rozdíly kompatibility obejít mezi aktuálním systému a SQL Data Warehouse. Plus migrace velké objemy dat do Azure trvá určitou dobu. Pečlivé plánování urychluje získávání dat do Azure. 

Jiné cílem plánování je provádět úpravy návrhu Ujistěte se, že vaše řešení využívá výhod vysokého výkonu dotazu, který SQL Data Warehouse je určená k poskytnutí. Návrh datových skladů pro škálování představuje různé návrhu vzory a proto tradiční přístupy nejsou vždy nejvhodnější. I když můžete provést některé úpravy návrhu po migraci, provedení změn dříve v procesu uloží později.

K provedení úspěšné migrace, musíte migrovat vaší schémata tabulek, kód a data. Pokyny v těchto tématech migrace najdete v tématu:

-  [Migrace vaší schémata](sql-data-warehouse-migrate-schema.md)
-  [Migrace vašeho kódu](sql-data-warehouse-migrate-code.md)
-  [Migrace dat](sql-data-warehouse-migrate-data.md). 

<!--
## Perform the migration


## Deploy the solution


## Validate the migration

-->

## <a name="next-steps"></a>Další kroky
CAT (poradní tým) má také některé skvělé SQL Data Warehouse pokyny, které publikují prostřednictvím blogy.  Podívejte se na jejich článku [migrace dat do Azure SQL Data Warehouse v praxi] [ Migrating data to Azure SQL Data Warehouse in practice] o další pokyny k migraci.

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
[Migrating data to Azure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
