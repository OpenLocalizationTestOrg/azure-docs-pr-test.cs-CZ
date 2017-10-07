---
title: "Vysvětlení výpočetní jednotky v Azure databáze pro databázi MySQL | Microsoft Docs"
description: "Azure DB pro databázi MySQL: Tento článek vysvětluje koncepty hello výpočetní jednotky a co se stane, když vaše úlohy dosáhne hello maximální výpočetní jednotky."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/23/2017
ms.openlocfilehash: 751b4fff2760e55565e2bc80d49db17a57397779
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="explaining-compute-units-in-azure-database-for-mysql"></a>Vysvětlení výpočetní jednotky v Azure databáze pro databázi MySQL
Tento článek vysvětluje hello konceptu výpočetní jednotky a co se stane, když vaše úlohy dosáhnou hello maximální výpočetní jednotky.

## <a name="what-are-compute-units"></a>Co jsou výpočetní jednotky?
Výpočetní jednotky jsou měřítkem propustnost zpracování procesoru, který zaručeně tooa toobe k dispozici jednotné Azure databáze pro server databáze MySQL. Výpočetní jednotka je kombinaci měření prostředků procesoru a paměti. Obecně platí 50 výpočetní jednotky označení rovnosti toohalf jádro. 100 výpočetní jednotky označení rovnosti tooone jádra. Výpočetní jednotky 2000 označení rovnosti tootwenty jádra serveru k dispozici tooyour propustnost garantované zpracování.

Hello množství paměti na výpočetní jednotku je optimalizovaná pro hello Basic a Standard cenové úrovně. Zvýší hello výpočetní jednotky zvýšením úrovně výkonu hello znamená zároveň toodoubling hello sadu prostředků dostupných toothat jednotné Azure Database pro databázi MySQL.

Například poskytuje standardní účet 800 výpočetní jednotky propustnosti 8 x další procesoru a paměti, než je konfigurace standardní 100 výpočetní jednotky. Však stejné hello při standardní 100 výpočetní jednotky propustnosti procesoru porovnání tooBasic 100 výpočetní jednotky, hello množství paměti, který je předem nakonfigurovaný v cenová úroveň Standard je double hello objem paměti konfigurované pro základní cenová úroveň. Proto cenová úroveň Standard poskytuje lepší výkon pracovního vytížení a nižší latenci transakcí než základní cenovou úroveň, s hello zvolené stejné výpočetní jednotky.

## <a name="how-can-i-determine-hello-number-of-compute-units-needed-for-my-workload"></a>Jak můžete určit počet hello výpočetní jednotky potřebné pro moje zatížení?
Pokud hledáte toomigrate existující server MySQL běžících místně nebo na virtuálním počítači, můžete určit počet hello výpočetní jednotky pomocí odhad, kolik jader propustnosti zpracování úlohy musí. 

Pokud vaše stávající místní nebo virtuální počítač server je aktuálně využívá 4 jádra (bez počítání hyperthread procesoru), spusťte nakonfigurováním 400 výpočetní jednotky pro vaši databázi Azure pro server databáze MySQL. Výpočetní jednotky můžete dynamicky škálovat nahoru nebo dolů podle potřeb pracovního vytížení s téměř žádné výpadky aplikací. 

Monitorování hello metriky grafu v hello Azure portal nebo zápisu rozhraní příkazového řádku Azure příkazy - toomeasure výpočetní jednotky. Relevantní metriky toomonitor jsou hello výpočetní jednotky procento a výpočetní jednotky limit.

>[!IMPORTANT]
> Pokud zjistíte, úložiště, které nejsou IOPS plně využívat maximální toohello, vezměte v úvahu monitorování hello výpočetní jednotky využití také. Vyvolání hello výpočetní jednotky může povolit pro vyšší propustnost vstupně-výstupní operace lessening hello kritických bodů výkonu z důvodu toolimited procesoru nebo paměti.

## <a name="what-happens-when-i-hit-my-maximum-compute-units"></a>Co se stane, když dosáhnu Moje maximální výpočetní jednotky?
Úrovně výkonu jsou nastaveny a řídí tooprovide prostředky toorun vaší databáze úloh toohello maximální limity pro hello vybraná cenová úroveň a úroveň výkonu. 

Pokud vaše úlohy dosáhne hello maximální limit v buď hello výpočetní jednotky nebo zřízené IOPS omezení, můžete prostředky hello tooutilize maximální povolené úrovni hello, ale vaše dotazy nejsou pravděpodobně toosee vyšší latence. Tyto limity nezpůsobovalo neustálé všechny chyby, ale spíš zpomalení hello zatížení, pokud hello zpomalení stane závažné, který dotazuje vypršení časového limitu. 

Pokud vaše úlohy dosáhne hello maximální limit počtu připojení, je vyvolána explicitní chyby. Další informace o omezení prostředků najdete v tématu [omezení v Azure Database pro databázi MySQL](concepts-limits.md).

## <a name="next-steps"></a>Další kroky
Další informace o cenových úrovní najdete v tématu [Azure Database pro databázi MySQL cenové úrovně](./concepts-service-tiers.md).
