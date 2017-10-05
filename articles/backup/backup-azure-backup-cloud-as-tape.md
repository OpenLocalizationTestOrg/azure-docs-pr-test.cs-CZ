---
title: "Použít Azure Backup k nahrazení infrastruktury pásky | Microsoft Docs"
description: "Zjistěte, jak Azure Backup poskytuje sémantiku podobné pásku, která umožňuje zálohování a obnovení dat v Azure"
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
ms.openlocfilehash: f0f3152daf5f91f7c9e540797bf09b21969d2d33
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-your-long-term-storage-from-tape-to-the-azure-cloud"></a><span data-ttu-id="1d46f-103">Přesunout do cloudu Azure a dlouhodobého úložiště z pásky</span><span class="sxs-lookup"><span data-stu-id="1d46f-103">Move your long-term storage from tape to the Azure cloud</span></span>
<span data-ttu-id="1d46f-104">Může Azure zákazníky zálohování a System Center Data Protection Manager:</span><span class="sxs-lookup"><span data-stu-id="1d46f-104">Azure Backup and System Center Data Protection Manager customers can:</span></span>

* <span data-ttu-id="1d46f-105">Zálohování dat v plány, které co nejlépe vyhovovaly potřebám organizace.</span><span class="sxs-lookup"><span data-stu-id="1d46f-105">Back up data in schedules which best suit the organizational needs.</span></span>
* <span data-ttu-id="1d46f-106">Zachovat zálohovaná data delší</span><span class="sxs-lookup"><span data-stu-id="1d46f-106">Retain the backup data for longer periods</span></span>
* <span data-ttu-id="1d46f-107">Ujistěte se, Azure, které jsou součástí jejich dlouhodobé uchovávání musí (namísto pásku).</span><span class="sxs-lookup"><span data-stu-id="1d46f-107">Make Azure a part of their long-term retention needs (instead of tape).</span></span>

<span data-ttu-id="1d46f-108">Tento článek vysvětluje, jak zákazníci povolit zásady zálohování a uchovávání.</span><span class="sxs-lookup"><span data-stu-id="1d46f-108">This article explains how customers can enable backup and retention policies.</span></span> <span data-ttu-id="1d46f-109">Zákazníci, kteří používají pásky k vyřešení jejich dlouhodobý-dlouhodobé uchovávání musí mít teď výkonný a přijatelná alternativa k dispozici tato funkce.</span><span class="sxs-lookup"><span data-stu-id="1d46f-109">Customers who use tapes to address their long-term-retention needs now have a powerful and viable alternative with the availability of this feature.</span></span> <span data-ttu-id="1d46f-110">Tato funkce je povolena v nejnovější verzi služby Azure Backup (která je k dispozici [sem](http://aka.ms/azurebackup_agent)).</span><span class="sxs-lookup"><span data-stu-id="1d46f-110">The feature is enabled in the latest release of the Azure Backup (which is available [here](http://aka.ms/azurebackup_agent)).</span></span> <span data-ttu-id="1d46f-111">System Center DPM zákazníci musí aktualizovat, minimálně, DPM 2012 R2 UR5 před použitím aplikace DPM pomocí služby Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="1d46f-111">System Center DPM customers must update to, at least, DPM 2012 R2 UR5 before using DPM with the Azure Backup service.</span></span>

## <a name="what-is-the-backup-schedule"></a><span data-ttu-id="1d46f-112">Co je plán zálohování?</span><span class="sxs-lookup"><span data-stu-id="1d46f-112">What is the Backup Schedule?</span></span>
<span data-ttu-id="1d46f-113">Plán zálohování určuje četnost zálohování.</span><span class="sxs-lookup"><span data-stu-id="1d46f-113">The backup schedule indicates the frequency of the backup operation.</span></span> <span data-ttu-id="1d46f-114">Nastavení na následující obrazovce například znamenat, že jsou provedeny zálohy denně v 18: 00 a půlnoci.</span><span class="sxs-lookup"><span data-stu-id="1d46f-114">For example, the settings in the following screen indicate that backups are taken daily at 6pm and at midnight.</span></span>

![Denní plán](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

<span data-ttu-id="1d46f-116">Zákazníci můžete také naplánovat týdenní zálohování.</span><span class="sxs-lookup"><span data-stu-id="1d46f-116">Customers can also schedule a weekly backup.</span></span> <span data-ttu-id="1d46f-117">Nastavení na následující obrazovce například znamenat, že zálohy se pořídí každých alternativní neděle & středa na 9:30:00 a 1:00:00.</span><span class="sxs-lookup"><span data-stu-id="1d46f-117">For example, the settings in the following screen indicate that backups are taken every alternate Sunday & Wednesday at 9:30AM and 1:00AM.</span></span>

![Týdenní plán](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-the-retention-policy"></a><span data-ttu-id="1d46f-119">Co je zásady uchovávání informací?</span><span class="sxs-lookup"><span data-stu-id="1d46f-119">What is the Retention Policy?</span></span>
<span data-ttu-id="1d46f-120">Zásady uchovávání informací Určuje dobu, pro které musí být uložen zálohování.</span><span class="sxs-lookup"><span data-stu-id="1d46f-120">The retention policy specifies the duration for which the backup must be stored.</span></span> <span data-ttu-id="1d46f-121">Místo zadání "ploché zásady" pro všechny body zálohy, zákazníky, můžete zadat různé zásady uchovávání informací podle toho, kdy dochází k zálohování.</span><span class="sxs-lookup"><span data-stu-id="1d46f-121">Rather than just specifying a “flat policy” for all backup points, customers can specify different retention policies based on when the backup is taken.</span></span> <span data-ttu-id="1d46f-122">Zálohování bod pořizovat denně, který slouží jako bod provozní obnovení, je třeba zachovaná 90 dní.</span><span class="sxs-lookup"><span data-stu-id="1d46f-122">For example, the backup point taken daily, which serves as an operational recovery point, is preserved for 90 days.</span></span> <span data-ttu-id="1d46f-123">Bod zálohy prováděné na konci každé čtvrtletí pro účely auditu je zachovaná delší dobu.</span><span class="sxs-lookup"><span data-stu-id="1d46f-123">The backup point taken at the end of each quarter for audit purposes is preserved for a longer duration.</span></span>

![Zásady uchovávání informací](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

<span data-ttu-id="1d46f-125">Celkový počet "body uchovávání informací" uvedenými v této zásadě je 90 (denní body) + 40 (jeden pro každé čtvrtletí 10 let) = 130.</span><span class="sxs-lookup"><span data-stu-id="1d46f-125">The total number of “retention points” specified in this policy is 90 (daily points) + 40 (one each quarter for 10 years) = 130.</span></span>

## <a name="example--putting-both-together"></a><span data-ttu-id="1d46f-126">Příklad – obě připravuje umístění</span><span class="sxs-lookup"><span data-stu-id="1d46f-126">Example – Putting both together</span></span>
![Ukázka obrazovky](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. <span data-ttu-id="1d46f-128">**Denní zásady uchovávání informací**: zálohy pořizovat denně jsou uložené sedm dní.</span><span class="sxs-lookup"><span data-stu-id="1d46f-128">**Daily retention policy**: Backups taken daily are stored for seven days.</span></span>
2. <span data-ttu-id="1d46f-129">**Týdenní zásady uchovávání informací**: zálohy pořízené každý den o půlnoci a 18: 00 Sobota se zachovají pro čtyři týdny</span><span class="sxs-lookup"><span data-stu-id="1d46f-129">**Weekly retention policy**: Backups taken every day at midnight and 6PM Saturday are preserved for four weeks</span></span>
3. <span data-ttu-id="1d46f-130">**Měsíční zásady uchovávání informací**: zálohy pořízené o půlnoci a 18: 00 na poslední sobotu každý měsíc zachovány po dobu 12 měsíců</span><span class="sxs-lookup"><span data-stu-id="1d46f-130">**Monthly retention policy**: Backups taken at midnight and 6pm on the last Saturday of each month are preserved for 12 months</span></span>
4. <span data-ttu-id="1d46f-131">**Roční zásady uchovávání informací**: zálohy pořízené o půlnoci poslední sobotu každých března zachovány 10 let.</span><span class="sxs-lookup"><span data-stu-id="1d46f-131">**Yearly retention policy**: Backups taken at midnight on the last Saturday of every March are preserved for 10 years</span></span>

<span data-ttu-id="1d46f-132">Celkový počet "body uchovávání informací" (body, ze kterých může zákazník obnovení dat) na předchozím obrázku je vypočítán takto:</span><span class="sxs-lookup"><span data-stu-id="1d46f-132">The total number of “retention points” (points from which a customer can restore data) in the preceding diagram is computed as follows:</span></span>

* <span data-ttu-id="1d46f-133">dva body na každý den pro sedm dní = 14 bodů obnovení</span><span class="sxs-lookup"><span data-stu-id="1d46f-133">two points per day for seven days = 14 recovery points</span></span>
* <span data-ttu-id="1d46f-134">dva body za týden pro čtyři týdny = 8 bodů obnovení</span><span class="sxs-lookup"><span data-stu-id="1d46f-134">two points per week for four weeks = 8 recovery points</span></span>
* <span data-ttu-id="1d46f-135">dva body USD měsíčně po 12 měsíců = 24 bodů obnovení</span><span class="sxs-lookup"><span data-stu-id="1d46f-135">two points per month for 12 months = 24 recovery points</span></span>
* <span data-ttu-id="1d46f-136">jeden bod za rok a na 10 let = 10 obnovení body</span><span class="sxs-lookup"><span data-stu-id="1d46f-136">one point per year per 10 years = 10 recovery points</span></span>

<span data-ttu-id="1d46f-137">Celkový počet bodů obnovení je 56.</span><span class="sxs-lookup"><span data-stu-id="1d46f-137">The total number of recovery points is 56.</span></span>

> [!NOTE]
> <span data-ttu-id="1d46f-138">Zálohování Azure nemá omezení počtu bodů obnovení.</span><span class="sxs-lookup"><span data-stu-id="1d46f-138">Azure backup doesn't have a restriction on number of recovery points.</span></span>
>
>

## <a name="advanced-configuration"></a><span data-ttu-id="1d46f-139">Pokročilá konfigurace</span><span class="sxs-lookup"><span data-stu-id="1d46f-139">Advanced configuration</span></span>
<span data-ttu-id="1d46f-140">Kliknutím na **upravit** na předchozí obrazovce zákazníci mají další flexibilitu při zadání plány uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="1d46f-140">By clicking **Modify** in the preceding screen, customers have further flexibility in specifying retention schedules.</span></span>

![Úpravy](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a><span data-ttu-id="1d46f-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d46f-142">Next Steps</span></span>
<span data-ttu-id="1d46f-143">Další informace o zálohování Azure najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="1d46f-143">For more information about Azure Backup, see:</span></span>

* [<span data-ttu-id="1d46f-144">Seznámení s Azure Backup</span><span class="sxs-lookup"><span data-stu-id="1d46f-144">Introduction to Azure Backup</span></span>](backup-introduction-to-azure-backup.md)
* [<span data-ttu-id="1d46f-145">Vyzkoušejte zálohování Azure</span><span class="sxs-lookup"><span data-stu-id="1d46f-145">Try Azure Backup</span></span>](backup-try-azure-backup-in-10-mins.md)
