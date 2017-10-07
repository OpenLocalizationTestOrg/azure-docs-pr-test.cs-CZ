---
title: "Azure Backup tooreplace aaaUse infrastruktury pásky | Microsoft Docs"
description: "Zjistěte, jak Azure Backup poskytuje sémantiku podobné pásku, což vám umožní toobackup a obnovení dat v Azure"
services: backup
documentationcenter: 
author: trinadhk
manager: vijayts
editor: 
ms.assetid: 2e1bb67d-986c-4437-8056-3a63169b4214
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 1/10/2017
ms.author: saurse;trinadhk;markgal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4c5b095d95d39267c54b1eed9427bda09658bb94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-your-long-term-storage-from-tape-toohello-azure-cloud"></a>Přesun dlouhodobého úložiště z pásky toohello cloudu Azure
Může Azure zákazníky zálohování a System Center Data Protection Manager:

* Zálohování dat v plány, které co nejlépe vyhovovaly potřebám organizace hello.
* Zachovat zálohovaná data hello delší
* Ujistěte se, Azure, které jsou součástí jejich dlouhodobé uchovávání musí (namísto pásku).

Tento článek vysvětluje, jak zákazníci povolit zásady zálohování a uchovávání. Zákazníci, kteří používají pásky tooaddress jejich dlouhodobý-dlouhodobé uchovávání musí Teď máte výkonný a přijatelná alternativa s dostupností hello této funkce. Hello je povolena funkce v hello nejnovější verzi hello Azure Backup (která je k dispozici [sem](http://aka.ms/azurebackup_agent)). System Center DPM zákazníci musí aktualizovat na, minimálně, hello DPM 2012 R2 UR5 před použitím aplikace DPM pomocí služby zálohování Azure.

## <a name="what-is-hello-backup-schedule"></a>Co je hello plán zálohování?
plán zálohování Hello označuje hello četnost zálohování hello. Například hello nastavení v hello následující obrazovka udávají, že jsou provedeny zálohy denně v 18: 00 a půlnoci.

![Denní plán](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

Zákazníci můžete také naplánovat týdenní zálohování. Například hello nastavení v hello následující obrazovka udávají, že jsou provedeny zálohy každých alternativní neděle & středa v 9:30:00 a 1:00:00.

![Týdenní plán](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-hello-retention-policy"></a>Co je hello zásady uchovávání informací?
Hello zásady uchovávání informací Určuje dobu hello, pro které musí být uložen hello zálohování. Zákazníci místo zadání "ploché zásady" pro všechny body zálohy, můžete zadat různé zásady uchovávání informací na základě při pořízení zálohy hello. Hello bodu zálohy pořizovat denně, který slouží jako bod provozní obnovení, je třeba zachovaná 90 dní. bod zálohy Hello prováděné na konci hello každé čtvrtletí pro účely auditu je zachovaná delší dobu.

![Zásady uchovávání informací](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

Hello celkový počet "body uchovávání informací" uvedenými v této zásadě je 90 (denní body) + 40 (jeden pro každé čtvrtletí 10 let) = 130.

## <a name="example--putting-both-together"></a>Příklad – obě připravuje umístění
![Ukázka obrazovky](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. **Denní zásady uchovávání informací**: zálohy pořizovat denně jsou uložené sedm dní.
2. **Týdenní zásady uchovávání informací**: zálohy pořízené každý den o půlnoci a 18: 00 Sobota se zachovají pro čtyři týdny
3. **Měsíční zásady uchovávání informací**: zálohy pořízené o půlnoci a 18: 00 na hello Poslední sobota každý měsíc zachovány po dobu 12 měsíců
4. **Roční zásady uchovávání informací**: zálohy pořízené půlnoci na hello Poslední sobota každých března zachovány 10 let.

Celkový počet "body uchovávání informací" Hello (body, ze kterých zákazníka, můžete obnovit data) v hello předchozím obrázku je vypočítán takto:

* dva body na každý den pro sedm dní = 14 bodů obnovení
* dva body za týden pro čtyři týdny = 8 bodů obnovení
* dva body USD měsíčně po 12 měsíců = 24 bodů obnovení
* jeden bod za rok a na 10 let = 10 obnovení body

Celkový počet bodů obnovení Hello je 56.

> [!NOTE]
> Zálohování Azure nemá omezení počtu bodů obnovení.
>
>

## <a name="advanced-configuration"></a>Pokročilá konfigurace
Kliknutím na **upravit** v hello předcházející obrazovky, zákazníci mají další flexibilitu při zadání plány uchovávání informací.

![Úpravy](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a>Další kroky
Další informace o zálohování Azure najdete v tématu:

* [Úvod tooAzure zálohování](backup-introduction-to-azure-backup.md)
* [Vyzkoušejte zálohování Azure](backup-try-azure-backup-in-10-mins.md)
