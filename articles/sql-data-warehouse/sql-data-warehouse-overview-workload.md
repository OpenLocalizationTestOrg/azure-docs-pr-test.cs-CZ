---
title: "aaaLearn o operacích Azure SQL Data Warehouse | Microsoft Docs"
description: "Pružnost služby SQL Data Warehouse vám umožňuje zvýšit, snížit nebo pozastavit výpočetní výkon pomocí posuvné stupnice jednotek datového skladu (DWU). Tento článek vysvětluje metriky datového skladu hello a jejich vzájemných tooDWUs. "
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
ms.openlocfilehash: 8be5ff6b14ab907e2b0a7eb55e0e2f4139aca8b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-warehouse-workload"></a>Úlohy datového skladu
Úlohy datového skladu odkazuje tooall hello operací, které transpire proti datového skladu. Hello úlohy datového skladu zahrnují celý proces hello načítání dat do skladu hello, provádění analýzy a zprávy o hello datového skladu, správy dat v hello datového skladu a exportu dat z datového skladu hello. Hello hloubka a šířka těchto složek je často úměrná úrovni vyspělosti hello hello datového skladu.

## <a name="new-toodata-warehousing"></a>Nové skladování toodata?
Datový sklad je kolekce dat, který je načten z jednoho nebo více dat zdroje a je použité tooperform úloh obchodních informací, jako je například analýzy dat a generování sestav.

Datové sklady jsou charakteristické dotazy, které prohledávají větší počty řádků nebo velké oblasti dat a můžou vrátit relativně velké výsledky pro účely hello analýzy a vytváření sestav. Datové sklady se taky vyznačují načítáním relativně velkých dat, které kontrastuje s malým vkládáním/aktualizací/odstraňováním na úrovni transakcí.

* Datový sklad poskytuje nejlepší výkon, pokud hello data uložená způsobem, který optimalizuje dotazy vyžadující tooscan velkého počtu řádků nebo velkých oblastí dat.. Tento typ prohledávání funguje nejlíp, když hello data ukládají a prohledávají po sloupcích, nikoli po řádcích.

> [!NOTE]
> index columnstore v paměti Hello, který používá sloupcové ukládání, poskytuje too10x větší zvýšení komprese a 100 x větší zvýšení výkonu dotazů oproti tradičním binárním stromům pro dotazy analýz a generování sestav. Považujeme za indexy columnstore jako hello standard pro ukládání a prohledávání velkých objemů dat v datovém skladu.
> 
> 

* Datový sklad má jiné požadavky než systém optimalizovaný pro online zpracování transakcí (OLTP). Hello systém OLTP obsahuje hodně vložit, aktualizovat a odstranit operace. Tyto operace hledají toospecific řádky v tabulce hello. Tabulka bude hledat nejlépe provést v případě, že je hello data uložená způsobem řádek po řádku. Hello data dají řadit a rychle prohledat Rozděl a dobytí přístup názvem binárního stromu vyhledávání.

## <a name="data-loading"></a>Načítání dat
Načítání dat je velká část hello úlohy datového skladu. Podniky mají obvykle zaneprázdněný systém OLTP, který sleduje změny v den hello když zákazníci generují obchodní transakce. Často v noci během časového období údržby, pravidelně hello transakce jsou přesunout ani zkopírovat toohello datového skladu. Jakmile hello data v hello datového skladu, analytici můžete provádět analýzy a rozhodování o datech hello.

* Hello proces načítání se tradičně, nazývá ETL pro extrakce, transformace a načítání. Data je obvykle nutné transformovat tak, aby byl konzistentní s ostatními daty v datovém skladu hello toobe. Podniky dřív používaly vyhrazené ETL servery tooperform hello transformace. Nyní s takové rychlého masivně paralelní zpracování můžete nejdřív načíst data do SQL Data Warehouse a pak provést transformace hello. Tento proces se nazývá extrakce, načítání a transformace ELT () a stává se novým standardem pro úlohy datového skladu hello.

> [!NOTE]
> Pomocí SQL Serveru 2016 teď můžete provádět analýzy tabulky OLTP v reálném čase. Toto není nahradit potřebu hello toostore datového skladu a analyzovat data, ale je poskytnout způsob tooperform analýzu v reálném čase.
> 
> 

### <a name="reporting-and-analysis-queries"></a>Dotazy analýz a generování sestav
Dotazy analýz a generování sestav se podle různých kritérií často dělí na malé, střední a velké, obvykle podle doby. Ve většině datových skladů jsou smíšené úlohy, které obsahují rychle běžící i dlouho běžící dotazy. V každém případě je důležité toodetermine tato kombinace a toodetermine její interval (hodinový, denní, Konec měsíce, konec, čtvrtletí a tak dále). Je důležité toounderstand, který hello úlohy se smíšenými dotazy společně se souběžností realizace tooproper plánování kapacity pro datový sklad.

* Plánování kapacity může být složité úlohy pro úlohy se smíšenými dotazy, zejména v případě, že potřebujete rozsáhlé čas tooadd kapacity toohello datový sklad. SQL Data Warehouse odebere hello naléhavost plánování kapacity, protože můžete zvýšit nebo snížit výpočetní kapacitu, kdykoli a od úložiště a výpočetní kapacitu nezávisle mají velikost.

### <a name="data-management"></a>Správa dat
Správa dat je důležitá, hlavně když víte, že může dojít volné místo na disku v blízké budoucnosti hello. Datové sklady obvykle rozdělují hello data na smysluplné oblasti, které se ukládají jako oddíly v tabulce. Všechny produkty založené na systému SQL Server umožňují přesouvat oddíly z / hello tabulky. Tato výměna oddílů umožňuje přesunout starší data tooless levnější úložiště a zachovat hello k dispozici nejnovější data na online úložiště.

* Indexy columnstore podporují dělené tabulky. V případě indexů columnstore se dělené tabulky používají k správě dat a archivaci. V případě tabulek uložených řádek po řádku hrají oddíly větší roli ve výkonu dotazů.  
* Při správě dat hraje důležitou roli technologie PolyBase. Pomocí PolyBase máte možnost hello tooarchive starší data tooHadoop nebo úložiště objektů blob Azure.  To nabízí spoustu možností, protože hello data jsou dál online.  Může trvat déle tooretrieve dat Hadoop, ale hello delší dobu načtení vyváží nižší náklady na úložiště hello.

### <a name="exporting-data"></a>Export dat
Jedním ze způsobů toomake data k dispozici pro sestavy a analýzy se toosend data z hello datového skladu tooservers vyhrazené pro analýzy a spouštění sestav. Těmto serverům se říká datová tržiště. Můžete například předběžně zpracovat data sestav a exportovat je z servery toomany kolem hello, world toomake hello datového skladu je široce dostupné zákazníkům a analytikům.

* Generování sestav, každou noc můžete naplnit servery sestav jen pro čtení se snímkem denních dat hello. To dává větší šířku pásma pro zákazníky při snížení požadavků na výpočetní prostředky hello na hello datového skladu. Z hlediska zabezpečení datová Tržiště umožňují tooreduce hello počet uživatelů, kteří mají přístup toohello datového skladu.
* Pro analýzu můžete buď vytvořit analytickou datovou krychli na hello datového skladu a spustit analýzu na hello datového skladu, nebo předběžně zpracovat data a exportovat ho toohello serveru pro analýzu k další analýze.

## <a name="next-steps"></a>Další kroky
Teď, když víte o něco o SQL Data Warehouse, zjistěte, jak tooquickly [vytvořit SQL Data Warehouse] [ create a SQL Data Warehouse] a [načíst ukázková data][load sample data].

<!--Image references-->

<!--Article references-->
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other web references-->
