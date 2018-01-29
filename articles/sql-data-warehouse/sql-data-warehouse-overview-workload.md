---
title: "Další informace o operacích Azure SQL Data Warehouse | Microsoft Docs"
description: "Pružnost služby SQL Data Warehouse vám umožňuje zvýšit, snížit nebo pozastavit výpočetní výkon pomocí posuvné stupnice jednotek datového skladu (DWU). Tento článek vysvětluje metriky datového skladu a jejich vztah k jednotkám DWU. "
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: cadffa9c-589d-4db7-888a-1f202a753bc5
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 629ce22bf669a760d041bbd006b836d2da5d237b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="data-warehouse-workload"></a>Úlohy datového skladu
Úlohami datového skladu se rozumí všechny operace, které na datovém skladu probíhají. Úlohy datového skladu zahrnují celý proces načtení dat do skladu, provedení analýzy a vytvoření sestav na datovém skladu, správy dat v datovém skladu a exportu dat z datového skladu. Hloubka a šířka těchto složek je často úměrná úrovni vyspělosti datového skladu.

## <a name="new-to-data-warehousing"></a>Jsou pro vás datové sklady nové?
Datový sklad je kolekce dat načtených z jednoho nebo více zdrojů dat a slouží k provádění úloh obchodních informací, jako je například vytváření sestav nebo analýza dat.

Pro datové sklady jsou charakteristické dotazy, které prohledávají větší počty řádků nebo velké oblasti dat a můžou vrátit relativně velké výsledky pro účely analýzy a vytváření sestav. Datové sklady se taky vyznačují načítáním relativně velkých dat, které kontrastuje s malým vkládáním/aktualizací/odstraňováním na úrovni transakcí.

* Datový sklad poskytuje nejlepší výkon, pokud jsou data uložená způsobem, který optimalizuje dotazy vyžadující prohledávání velkého počtu řádků nebo velkých oblastí dat. Tento typ prohledávání funguje nejlíp, když se data ukládají a prohledávají po sloupcích, nikoli po řádcích.

> [!NOTE]
> Index columnstore v paměti, který používá sloupcové ukládání, poskytuje až 10x větší zvýšení komprese a 100x větší zvýšení výkonu dotazů oproti tradičním binárním stromům pro dotazy analýz a generování sestav. Indexy columnstore považujeme za standard pro ukládání a prohledávání velkých objemů dat v datovém skladu.
> 
> 

* Datový sklad má jiné požadavky než systém optimalizovaný pro online zpracování transakcí (OLTP). Systém OLTP obsahuje hodně operací vložení, aktualizace a odstranění. Tyto operace hledají konkrétní řádky v tabulce. Hledání v tabulce je nejvýkonnější, když jsou data uložená způsobem řádek po řádku. Taková data se dají řadit a rychle prohledat přístupem typu „rozděl a panuj“, který se nazývá hledání pomocí binárního stromu.

## <a name="data-loading"></a>Načítání dat
Načítání dat představuje významnou část úloh datového skladu. Podniky mají obvykle zaneprázdněný systém OLTP, který během dne sleduje změny, když zákazníci generují obchodní transakce. Tyto transakce se pravidelně (často v noci během doby údržby) přesouvají nebo kopírují do datového skladu. Jakmile jsou data v datovém skladu, analytici můžou provádět analýzy a přijímat na základě dat podnikatelská rozhodnutí.

* Proces načítání se tradičně nazývá ETL (extrakce, transformace a načítání). Data je obvykle nutné transformovat tak, aby byla konzistentní s ostatními daty v datovém skladu. K provádění těchto transformací podniky dřív používaly vyhrazené servery ETL. Dnešní masivně paralelní zpracování je ale tak rychlé, že můžete data nejdřív načíst do služby SQL Data Warehouse a pak provést transformace. Tento proces se nazývá ELT (extrakce, načítání a transformace) a stává se novým standardem pro úlohy datového skladu.

> [!NOTE]
> Pomocí SQL Serveru 2016 teď můžete provádět analýzy tabulky OLTP v reálném čase. Toto nenahrazuje nutnost ukládat a analyzovat data v datovém skladu, ale nabízí to možnost provádět analýzu v reálném čase.
> 
> 

### <a name="reporting-and-analysis-queries"></a>Dotazy analýz a generování sestav
Dotazy analýz a generování sestav se podle různých kritérií často dělí na malé, střední a velké, obvykle podle doby. Ve většině datových skladů jsou smíšené úlohy, které obsahují rychle běžící i dlouho běžící dotazy. V každém případě je důležité určit tuto směsici a určit její interval (hodinový, denní, konec měsíce, konec čtvrtletí a tak dále). Je důležité si uvědomit, že tyto úlohy se smíšenými dotazy společně se souběžností vedou k správnému plánování kapacity datového skladu.

* V případě úloh se smíšenými dotazy může být plánování kapacity složitým úkolem, hlavně pokud rozšíření kapacity datového skladu dlouho trvá. Se službou SQL Data Warehouse není nutné plánovat kapacitu, protože výpočetní kapacitu jde kdykoli zvýšit nebo snížit a protože velikost kapacity úložiště a výpočetní kapacity jde určit nezávisle.

### <a name="data-management"></a>Správa dat
Správa dat je důležitá, hlavně když víte, že vám brzy může dojít volné místo na disku. Datové sklady obvykle rozdělují data na smysluplné oblasti, které se ukládají jako oddíly v tabulce. Všechny produkty založené na systému SQL Server umožňují přesouvat oddíly z/do tabulky. Tato výměna oddílů umožňuje přesunout starší data do levnějšího úložiště a nejnovější data ponechat dostupná v online úložišti.

* Indexy columnstore podporují dělené tabulky. V případě indexů columnstore se dělené tabulky používají k správě dat a archivaci. V případě tabulek uložených řádek po řádku hrají oddíly větší roli ve výkonu dotazů.  
* Při správě dat hraje důležitou roli technologie PolyBase. Pomocí PolyBase máte možnost archivovat starší data v Hadoopu nebo v úložišti objektů blob Azure.  To nabízí spoustu možností, protože data jsou dál online.  Načtení dat z Hadoopu může trvat déle, ale delší dobu načtení vyváží nižší náklady na úložiště.

### <a name="exporting-data"></a>Export dat
Jednou z možností, jak zpřístupnit data pro sestavy a analýzy, je odesílání dat z datového skladu na servery vyhrazené pro analýzy a spouštění sestav. Těmto serverům se říká datová tržiště. Můžete například předběžně zpracovat data sestav a potom je z datového skladu exportovat do mnoha serverů po celém světě, abyste je co nejvíce zpřístupnili zákazníkům a analytikům.

* Pokud jde o generování sestav, každou noc můžete snímkem denních dat naplnit servery sestav jen pro čtení. Tato možnost dává zákazníkům větší šířku a současně snižuje nároky na výpočetní prostředky datového skladu. Z hlediska zabezpečení vám datová tržiště umožňují snížit počet uživatelů, kteří mají přístup k datovému skladu.
* Pokud jde o analýzy, můžete buď vytvořit analytickou datovou krychli na datovém skladu a spustit analýzu na datovém skladu, nebo můžete předběžně zpracovat data a exportovat je do serveru pro analýzu k další analýze.

## <a name="next-steps"></a>Další kroky
Teď, když jste se s SQL Data Warehouse seznámili, můžete zjistit, jak rychle [vytvořit datový sklad SQL Data Warehouse][create a SQL Data Warehouse] a [načíst ukázková data][load sample data].

<!--Image references-->

<!--Article references-->
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other web references-->
