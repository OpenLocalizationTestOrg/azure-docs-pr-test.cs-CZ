---
title: "Vysvětlení výpočetní jednotky v Azure databáze pro PostgreSQL | Microsoft Docs"
description: "Azure DB pro PostgreSQL: Tento článek vysvětluje koncepty výpočetní jednotky a co se stane, když vaše úlohy dosáhne maximální výpočetní jednotky."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/23/2017
ms.openlocfilehash: 6077e52f60c7ef0afe7df9201ec05081ba81c179
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="explaining-compute-units-in-azure-database-for-postgresql"></a>Kde vysvětluje výpočetní jednotky v Azure databázi PostgreSQL
Tento článek vysvětluje Princip výpočetní jednotky a co se stane, když vaše úlohy dosáhne maximální výpočetní jednotky.

## <a name="what-are-compute-units"></a>Co jsou výpočetní jednotky?
Výpočetní že jednotky jsou měřítkem propustnost zpracování procesoru, který zaručeně být k dispozici pro jednu databázi Azure pro PostgreSQL server. Výpočetní jednotka je kombinaci měření prostředků procesoru a paměti. 50 výpočetní jednotky obecně rovnat polovinu jádro. 100 výpočetní jednotky rovnat jednoho jádra. Výpočetní jednotky 2000 rovnat dvacet jader propustnosti garantované zpracování k dispozici pro váš server.

Množství paměti na výpočetní jednotku je optimalizovaná pro Basic a Standard cenové úrovně. Zvýší výpočetní jednotky zvýšením úrovně výkonu rovná zvýší sadu prostředků, které jsou k dispozici pro jednu databázi Azure pro PostgreSQL.

Například poskytuje standardní účet 800 výpočetní jednotky propustnosti 8 x další procesoru a paměti, než je konfigurace standardní 100 výpočetní jednotky. Zatímco standardní 100 výpočetní jednotky poskytují stejné ve srovnání s základní 100 výpočetní jednotky propustnosti procesoru, je množství paměti, který je předem nakonfigurované v standardní cenovou úroveň však double objem paměti konfigurované pro základní cenovou úroveň. Proto cenová úroveň Standard poskytuje lepší výkon pracovního vytížení a nižší latenci transakcí než základní cenové úrovně se stejným výpočetní jednotky vybraná.

## <a name="how-can-i-determine-the-number-of-compute-units-needed-for-my-workload"></a>Jak můžete určit počet výpočetní jednotky potřebné pro moje zatížení?
Pokud hledáte k migraci stávajícího serveru PostgreSQL běžících místně nebo na virtuálním počítači, můžete určit počet jednotek výpočetní podle odhad, kolik jader propustnosti zpracování musí vaše úlohy. 

Pokud vaše stávající místní nebo virtuální počítač server je aktuálně využívá 4 jádra (bez počítání hyperthread procesoru), spusťte nakonfigurováním 400 výpočetní jednotky pro vaši databázi Azure pro PostgreSQL server. Výpočetní jednotky můžete dynamicky škálovat nahoru nebo dolů podle potřeb pracovního vytížení s téměř žádné výpadky aplikací. 

Příkazy - monitorování metriky graf na portálu Azure nebo zápisu rozhraní příkazového řádku Azure k měření výpočetní jednotky. Relevantní metriky pro monitorování jsou procento výpočetní jednotky a výpočetní jednotky limit.

>[!IMPORTANT]
> Pokud zjistíte, úložiště, které nejsou IOPS plně využívat na maximum, zvažte, monitorování jednotky využití výpočetních také. Vyvolání výpočetní jednotky může povolit pro vyšší propustnost vstupně-výstupní operace lessening kritická místa výkonu kvůli omezené procesoru nebo paměti.

## <a name="what-happens-when-i-hit-my-maximum-compute-units"></a>Co se stane, když dosáhnu Moje maximální výpočetní jednotky?
Úrovně výkonu jsou kalibrován a řídí k poskytnutí prostředků ke spuštění úlohy databáze až do maximální limity pro vybrané cenové úrovně a výkonu. 

Pokud vaše úlohy dosáhne maximální limit v buď výpočetní jednotky nebo zřízené IOPS omezení, můžete nadále využívat prostředky na maximální povolenou úroveň, ale vaše dotazy nejsou pravděpodobně zobrazí vyšší latence. Tyto limity výsledkem není žádné chyby, ale spíš zpomalení zatížení, pokud zpomalení stane závažné, který dotazuje vypršení časového limitu. 

Pokud vaše úlohy dosáhne maximální limit počtu připojení, je vyvolána explicitní chyby. Další informace o omezení prostředků najdete v tématu [omezení v Azure databázi PostgreSQL](concepts-limits.md).

## <a name="next-steps"></a>Další kroky
Další informace o cenových úrovní najdete v tématu [databáze Azure pro PostgreSQL cenové úrovně](./concepts-service-tiers.md).
