---
title: aaaUse Power BI s SQL Data Warehouse | Microsoft Docs
description: "Tipy pro používání Power BI s datovým skladem SQL Azure pro vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: b12bee87-2268-40c2-81bf-ab27588b32e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: a3a347493d07af6824a561567f05894cfe3c0471
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-power-bi-with-sql-data-warehouse"></a>SQL Data Warehouse pomocí Power BI
Stejně jako u databáze SQL Azure SQL Data Warehouse přímé připojení umožňuje uživatele tooleverage výkonné logické přenos směrem dolů vedle hello analytické možnosti Power BI.  S přímé připojení dotazy jsou odesílány zpět tooyour Azure SQL Data Warehouse v reálném čase jako zkoumat hello data.  To, společně s měřítkem hello služby SQL Data Warehouse, umožňuje uživatelům toocreate dynamické sestavy minut před terabajtů dat.  Kromě toho hello zavedení hello otevřete v tlačítko Power BI umožňuje uživatelům toodirectly připojit Power BI tootheir SQL Data Warehouse, aniž by shromažďovat informace z dalších částí Azure.

Při používání přímé připojení prosím Poznámka:

* Zadejte název plně kvalifikovaný server hello při připojování (podrobnosti viz níže)
* Ujistěte se, pravidla brány firewall pro hello databáze jsou nakonfigurované příliš "Povolit přístup tooAzure služby".
* Všechny akce, jako výběrem sloupce nebo přidáním filtru se bude dotazovat přímo hello datového skladu
* Dlaždice se aktualizuje přibližně každých 15 minut (aktualizace nevyžaduje toobe naplánované)
* Modul otázky A odpovědi není k dispozici pro přímé připojení datové sady
* Změny schématu nejsou automaticky převzata
* Všechny dotazy přímé připojení bude časový limit po dvou minutách

Při dalším tooimprove hello prostředí může změnit těchto omezení a poznámky. tooconnect Hello kroky jsou podrobně popsány níže.  

## <a name="using-hello-open-in-power-bi-button"></a>Pomocí tlačítka hello otevřít v Power BI
Hello nejjednodušší způsob, jak toomove mezi SQL Data Warehouse a Power BI je s hello otevřete v tlačítko Power BI. Toto tlačítko, které vám umožní tooseamlessly začněte vytvářet nové řídicí panely v Power BI.  

1. tooget spuštění přejděte instanci SQL Data Warehouse tooyour v hello portálu Azure Classic.
2. Klikněte na tlačítko Otevřít v Power BI hello.
3. Pokud jsme nejsou možné toosign v přímo, nebo pokud nemáte účet Power BI, budete potřebovat toosign v.  
4. Budete přesměrováni, že předem vyplnit toohello stránka pro připojení k SQL Data Warehouse, hello informace z SQL Data Warehouse.
5. Po zadání přihlašovacích údajů bude plně připojené tooyour SQL Data Warehouse.

## <a name="connecting-through-hello-power-bi-portal"></a>Připojení prostřednictvím portálu hello Power BI
V přidání toousing hello otevřete v Power BI tlačítko mohou uživatelé také připojit tootheir SQL Data Warehouse prostřednictvím hello portál Power BI.

1. Klikněte na tlačítko 'načíst Data, na hello dolní části navigačního podokna hello.
2. Vyberte "Databáze".
3. Jednou na stránce databáze hello, vyberte, Azure SQL Data Warehouse a klepněte na tlačítko "Připojit".
4. Zadejte hello potřebné informace o připojení.  Název serveru a název databáze najdete v hello portálu Azure.
5. Budete přesměrováni zpět, hlavní stránky toohello Power BI a po připojení se nový záznam v části "datové sady, se zobrazí s názvem hello vaší instance.  
6. Kliknutím na nová datová sada tooexplore hello všechny hello tabulek a zobrazení v databázi. Výběrem sloupce odešle zdroj back toohello dotazu dynamicky vytváření vizuál. Tyto vizuály lze uložit do nové sestavy a připnuté zpět tooyour řídicího panelu.

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
