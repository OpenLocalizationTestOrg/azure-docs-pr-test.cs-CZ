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
# <a name="move-your-long-term-storage-from-tape-toohello-azure-cloud"></a><span data-ttu-id="b05db-103">Přesun dlouhodobého úložiště z pásky toohello cloudu Azure</span><span class="sxs-lookup"><span data-stu-id="b05db-103">Move your long-term storage from tape toohello Azure cloud</span></span>
<span data-ttu-id="b05db-104">Může Azure zákazníky zálohování a System Center Data Protection Manager:</span><span class="sxs-lookup"><span data-stu-id="b05db-104">Azure Backup and System Center Data Protection Manager customers can:</span></span>

* <span data-ttu-id="b05db-105">Zálohování dat v plány, které co nejlépe vyhovovaly potřebám organizace hello.</span><span class="sxs-lookup"><span data-stu-id="b05db-105">Back up data in schedules which best suit hello organizational needs.</span></span>
* <span data-ttu-id="b05db-106">Zachovat zálohovaná data hello delší</span><span class="sxs-lookup"><span data-stu-id="b05db-106">Retain hello backup data for longer periods</span></span>
* <span data-ttu-id="b05db-107">Ujistěte se, Azure, které jsou součástí jejich dlouhodobé uchovávání musí (namísto pásku).</span><span class="sxs-lookup"><span data-stu-id="b05db-107">Make Azure a part of their long-term retention needs (instead of tape).</span></span>

<span data-ttu-id="b05db-108">Tento článek vysvětluje, jak zákazníci povolit zásady zálohování a uchovávání.</span><span class="sxs-lookup"><span data-stu-id="b05db-108">This article explains how customers can enable backup and retention policies.</span></span> <span data-ttu-id="b05db-109">Zákazníci, kteří používají pásky tooaddress jejich dlouhodobý-dlouhodobé uchovávání musí Teď máte výkonný a přijatelná alternativa s dostupností hello této funkce.</span><span class="sxs-lookup"><span data-stu-id="b05db-109">Customers who use tapes tooaddress their long-term-retention needs now have a powerful and viable alternative with hello availability of this feature.</span></span> <span data-ttu-id="b05db-110">Hello je povolena funkce v hello nejnovější verzi hello Azure Backup (která je k dispozici [sem](http://aka.ms/azurebackup_agent)).</span><span class="sxs-lookup"><span data-stu-id="b05db-110">hello feature is enabled in hello latest release of hello Azure Backup (which is available [here](http://aka.ms/azurebackup_agent)).</span></span> <span data-ttu-id="b05db-111">System Center DPM zákazníci musí aktualizovat na, minimálně, hello DPM 2012 R2 UR5 před použitím aplikace DPM pomocí služby zálohování Azure.</span><span class="sxs-lookup"><span data-stu-id="b05db-111">System Center DPM customers must update to, at least, DPM 2012 R2 UR5 before using DPM with hello Azure Backup service.</span></span>

## <a name="what-is-hello-backup-schedule"></a><span data-ttu-id="b05db-112">Co je hello plán zálohování?</span><span class="sxs-lookup"><span data-stu-id="b05db-112">What is hello Backup Schedule?</span></span>
<span data-ttu-id="b05db-113">plán zálohování Hello označuje hello četnost zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="b05db-113">hello backup schedule indicates hello frequency of hello backup operation.</span></span> <span data-ttu-id="b05db-114">Například hello nastavení v hello následující obrazovka udávají, že jsou provedeny zálohy denně v 18: 00 a půlnoci.</span><span class="sxs-lookup"><span data-stu-id="b05db-114">For example, hello settings in hello following screen indicate that backups are taken daily at 6pm and at midnight.</span></span>

![Denní plán](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

<span data-ttu-id="b05db-116">Zákazníci můžete také naplánovat týdenní zálohování.</span><span class="sxs-lookup"><span data-stu-id="b05db-116">Customers can also schedule a weekly backup.</span></span> <span data-ttu-id="b05db-117">Například hello nastavení v hello následující obrazovka udávají, že jsou provedeny zálohy každých alternativní neděle & středa v 9:30:00 a 1:00:00.</span><span class="sxs-lookup"><span data-stu-id="b05db-117">For example, hello settings in hello following screen indicate that backups are taken every alternate Sunday & Wednesday at 9:30AM and 1:00AM.</span></span>

![Týdenní plán](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-hello-retention-policy"></a><span data-ttu-id="b05db-119">Co je hello zásady uchovávání informací?</span><span class="sxs-lookup"><span data-stu-id="b05db-119">What is hello Retention Policy?</span></span>
<span data-ttu-id="b05db-120">Hello zásady uchovávání informací Určuje dobu hello, pro které musí být uložen hello zálohování.</span><span class="sxs-lookup"><span data-stu-id="b05db-120">hello retention policy specifies hello duration for which hello backup must be stored.</span></span> <span data-ttu-id="b05db-121">Zákazníci místo zadání "ploché zásady" pro všechny body zálohy, můžete zadat různé zásady uchovávání informací na základě při pořízení zálohy hello.</span><span class="sxs-lookup"><span data-stu-id="b05db-121">Rather than just specifying a “flat policy” for all backup points, customers can specify different retention policies based on when hello backup is taken.</span></span> <span data-ttu-id="b05db-122">Hello bodu zálohy pořizovat denně, který slouží jako bod provozní obnovení, je třeba zachovaná 90 dní.</span><span class="sxs-lookup"><span data-stu-id="b05db-122">For example, hello backup point taken daily, which serves as an operational recovery point, is preserved for 90 days.</span></span> <span data-ttu-id="b05db-123">bod zálohy Hello prováděné na konci hello každé čtvrtletí pro účely auditu je zachovaná delší dobu.</span><span class="sxs-lookup"><span data-stu-id="b05db-123">hello backup point taken at hello end of each quarter for audit purposes is preserved for a longer duration.</span></span>

![Zásady uchovávání informací](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

<span data-ttu-id="b05db-125">Hello celkový počet "body uchovávání informací" uvedenými v této zásadě je 90 (denní body) + 40 (jeden pro každé čtvrtletí 10 let) = 130.</span><span class="sxs-lookup"><span data-stu-id="b05db-125">hello total number of “retention points” specified in this policy is 90 (daily points) + 40 (one each quarter for 10 years) = 130.</span></span>

## <a name="example--putting-both-together"></a><span data-ttu-id="b05db-126">Příklad – obě připravuje umístění</span><span class="sxs-lookup"><span data-stu-id="b05db-126">Example – Putting both together</span></span>
![Ukázka obrazovky](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. <span data-ttu-id="b05db-128">**Denní zásady uchovávání informací**: zálohy pořizovat denně jsou uložené sedm dní.</span><span class="sxs-lookup"><span data-stu-id="b05db-128">**Daily retention policy**: Backups taken daily are stored for seven days.</span></span>
2. <span data-ttu-id="b05db-129">**Týdenní zásady uchovávání informací**: zálohy pořízené každý den o půlnoci a 18: 00 Sobota se zachovají pro čtyři týdny</span><span class="sxs-lookup"><span data-stu-id="b05db-129">**Weekly retention policy**: Backups taken every day at midnight and 6PM Saturday are preserved for four weeks</span></span>
3. <span data-ttu-id="b05db-130">**Měsíční zásady uchovávání informací**: zálohy pořízené o půlnoci a 18: 00 na hello Poslední sobota každý měsíc zachovány po dobu 12 měsíců</span><span class="sxs-lookup"><span data-stu-id="b05db-130">**Monthly retention policy**: Backups taken at midnight and 6pm on hello last Saturday of each month are preserved for 12 months</span></span>
4. <span data-ttu-id="b05db-131">**Roční zásady uchovávání informací**: zálohy pořízené půlnoci na hello Poslední sobota každých března zachovány 10 let.</span><span class="sxs-lookup"><span data-stu-id="b05db-131">**Yearly retention policy**: Backups taken at midnight on hello last Saturday of every March are preserved for 10 years</span></span>

<span data-ttu-id="b05db-132">Celkový počet "body uchovávání informací" Hello (body, ze kterých zákazníka, můžete obnovit data) v hello předchozím obrázku je vypočítán takto:</span><span class="sxs-lookup"><span data-stu-id="b05db-132">hello total number of “retention points” (points from which a customer can restore data) in hello preceding diagram is computed as follows:</span></span>

* <span data-ttu-id="b05db-133">dva body na každý den pro sedm dní = 14 bodů obnovení</span><span class="sxs-lookup"><span data-stu-id="b05db-133">two points per day for seven days = 14 recovery points</span></span>
* <span data-ttu-id="b05db-134">dva body za týden pro čtyři týdny = 8 bodů obnovení</span><span class="sxs-lookup"><span data-stu-id="b05db-134">two points per week for four weeks = 8 recovery points</span></span>
* <span data-ttu-id="b05db-135">dva body USD měsíčně po 12 měsíců = 24 bodů obnovení</span><span class="sxs-lookup"><span data-stu-id="b05db-135">two points per month for 12 months = 24 recovery points</span></span>
* <span data-ttu-id="b05db-136">jeden bod za rok a na 10 let = 10 obnovení body</span><span class="sxs-lookup"><span data-stu-id="b05db-136">one point per year per 10 years = 10 recovery points</span></span>

<span data-ttu-id="b05db-137">Celkový počet bodů obnovení Hello je 56.</span><span class="sxs-lookup"><span data-stu-id="b05db-137">hello total number of recovery points is 56.</span></span>

> [!NOTE]
> <span data-ttu-id="b05db-138">Zálohování Azure nemá omezení počtu bodů obnovení.</span><span class="sxs-lookup"><span data-stu-id="b05db-138">Azure backup doesn't have a restriction on number of recovery points.</span></span>
>
>

## <a name="advanced-configuration"></a><span data-ttu-id="b05db-139">Pokročilá konfigurace</span><span class="sxs-lookup"><span data-stu-id="b05db-139">Advanced configuration</span></span>
<span data-ttu-id="b05db-140">Kliknutím na **upravit** v hello předcházející obrazovky, zákazníci mají další flexibilitu při zadání plány uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="b05db-140">By clicking **Modify** in hello preceding screen, customers have further flexibility in specifying retention schedules.</span></span>

![Úpravy](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a><span data-ttu-id="b05db-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b05db-142">Next Steps</span></span>
<span data-ttu-id="b05db-143">Další informace o zálohování Azure najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="b05db-143">For more information about Azure Backup, see:</span></span>

* [<span data-ttu-id="b05db-144">Úvod tooAzure zálohování</span><span class="sxs-lookup"><span data-stu-id="b05db-144">Introduction tooAzure Backup</span></span>](backup-introduction-to-azure-backup.md)
* [<span data-ttu-id="b05db-145">Vyzkoušejte zálohování Azure</span><span class="sxs-lookup"><span data-stu-id="b05db-145">Try Azure Backup</span></span>](backup-try-azure-backup-in-10-mins.md)
