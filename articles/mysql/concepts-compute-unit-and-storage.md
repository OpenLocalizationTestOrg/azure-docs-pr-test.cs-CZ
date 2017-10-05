---
title: "Vysvětlení výpočetní jednotky v Azure databáze pro databázi MySQL | Microsoft Docs"
description: "Azure DB pro databázi MySQL: Tento článek vysvětluje koncepty výpočetní jednotky a co se stane, když vaše úlohy dosáhne maximální výpočetní jednotky."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/23/2017
ms.openlocfilehash: a82c283df688d36cd284973312e276f30ed893c0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="explaining-compute-units-in-azure-database-for-mysql"></a>Vysvětlení výpočetní jednotky v Azure databáze pro databázi MySQL
Tento článek vysvětluje Princip výpočetní jednotky a co se stane, když vaše úlohy dosáhne maximální výpočetní jednotky.

## <a name="what-are-compute-units"></a>Co jsou výpočetní jednotky?
Výpočetní že jednotky jsou měřítkem propustnost zpracování procesoru, který zaručeně být k dispozici pro jednu databázi Azure pro server databáze MySQL. Výpočetní jednotka je kombinaci měření prostředků procesoru a paměti. 50 výpočetní jednotky obecně rovnat polovinu jádro. 100 výpočetní jednotky rovnat jednoho jádra. Výpočetní jednotky 2000 rovnat dvacet jader propustnosti garantované zpracování k dispozici pro váš server.

Množství paměti na výpočetní jednotku je optimalizovaná pro Basic a Standard cenové úrovně. Zvýší výpočetní jednotky zvýšením úrovně výkonu rovná zvýší sadu prostředků, které jsou k dispozici pro jednu databázi Azure pro databázi MySQL.

Například poskytuje standardní účet 800 výpočetní jednotky propustnosti 8 x další procesoru a paměti, než je konfigurace standardní 100 výpočetní jednotky. Zatímco standardní 100 výpočetní jednotky poskytují stejné ve srovnání s základní 100 výpočetní jednotky propustnosti procesoru, je množství paměti, který je předem nakonfigurované v standardní cenovou úroveň však double objem paměti konfigurované pro základní cenovou úroveň. Proto cenová úroveň Standard poskytuje lepší výkon pracovního vytížení a nižší latenci transakcí než základní cenové úrovně se stejným výpočetní jednotky vybraná.

## <a name="how-can-i-determine-the-number-of-compute-units-needed-for-my-workload"></a>Jak můžete určit počet výpočetní jednotky potřebné pro moje zatížení?
Pokud hledáte k migraci stávajícího serveru MySQL běžících místně nebo na virtuálním počítači, můžete určit počet jednotek výpočetní podle odhad, kolik jader propustnosti zpracování musí vaše úlohy. 

Pokud vaše stávající místní nebo virtuální počítač server je aktuálně využívá 4 jádra (bez počítání hyperthread procesoru), spusťte nakonfigurováním 400 výpočetní jednotky pro vaši databázi Azure pro server databáze MySQL. Výpočetní jednotky můžete dynamicky škálovat nahoru nebo dolů podle potřeb pracovního vytížení s téměř žádné výpadky aplikací. 

Příkazy - monitorování metriky graf na portálu Azure nebo zápisu rozhraní příkazového řádku Azure k měření výpočetní jednotky. Relevantní metriky pro monitorování jsou procento výpočetní jednotky a výpočetní jednotky limit.

>[!IMPORTANT]
> Pokud zjistíte, úložiště, které nejsou IOPS plně využívat na maximum, zvažte, monitorování jednotky využití výpočetních také. Vyvolání výpočetní jednotky může povolit pro vyšší propustnost vstupně-výstupní operace lessening kritická místa výkonu kvůli omezené procesoru nebo paměti.

## <a name="what-happens-when-i-hit-my-maximum-compute-units"></a>Co se stane, když dosáhnu Moje maximální výpočetní jednotky?
Úrovně výkonu jsou kalibrován a řídí k poskytnutí prostředků ke spuštění úlohy databáze až do maximální limity pro vybrané cenové úrovně a výkonu. 

Pokud vaše úlohy dosáhne maximální limit v buď výpočetní jednotky nebo zřízené IOPS omezení, můžete nadále využívat prostředky na maximální povolenou úroveň, ale vaše dotazy nejsou pravděpodobně zobrazí vyšší latence. Tyto limity výsledkem není žádné chyby, ale spíš zpomalení zatížení, pokud zpomalení stane závažné, který dotazuje vypršení časového limitu. 

Pokud vaše úlohy dosáhne maximální limit počtu připojení, je vyvolána explicitní chyby. Další informace o omezení prostředků najdete v tématu [omezení v Azure Database pro databázi MySQL](concepts-limits.md).

## <a name="next-steps"></a>Další kroky
Další informace o cenových úrovní najdete v tématu [Azure Database pro databázi MySQL cenové úrovně](./concepts-service-tiers.md).
