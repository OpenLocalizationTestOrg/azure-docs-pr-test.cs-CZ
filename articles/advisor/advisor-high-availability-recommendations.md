---
title: "doporučení pro vysokou dostupnost Advisor aaaAzure | Microsoft Docs"
description: "Pomocí Azure Advisor tooimprove vysokou dostupnost vašich Azure nasazení."
services: advisor
documentationcenter: NA
author: kumudd
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 3ac75ce401271f0212d198d7a7dc75ab702b6eda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-high-availability-recommendations"></a>Doporučení pro vysokou dostupnost služby Advisor

Azure Advisor pomáhá zajistit, a zlepšování kontinuity hello důležitými obchodními aplikacemi. Vysoká dostupnost doporučení Advisor můžete získat z hello **vysokou dostupnost** kartě hello Advisor řídicího panelu.

![Tlačítko vysoké dostupnosti na řídicím panelu Advisor hello](./media/advisor-high-availability-recommendations/advisor-high-availability-tab.png)


## <a name="ensure-virtual-machine-fault-tolerance"></a>Zajištění odolnosti proti chybám virtuálního počítače

Advisor identifikuje virtuálních počítačů, které nejsou součástí skupiny dostupnosti a doporučuje jejich přesunutí do skupiny dostupnosti. Zajistit aplikace tooyour redundanci, doporučujeme seskupit dva nebo více virtuálních počítačů v nastavení dostupnosti. Tato konfigurace zajistí, že během buď plánované i neplánované údržby, je k dispozici alespoň jeden virtuální počítač a splňuje hello virtuální počítač Azure SLA. Můžete buď toocreate sadu dostupnosti pro hello virtuálního počítače nebo tooadd hello virtuálního počítače tooan stávající sadu dostupnosti.

> [!NOTE]
> Pokud se rozhodnete toocreate dostupnosti nastavena, je nutné přidat alespoň jeden další virtuální počítač do ní. Doporučujeme seskupit dva nebo víc virtuálních počítačů ve skupině dostupnosti nastavena tooensure, který je k dispozici alespoň jeden počítač během výpadku.

![Advisor doporučení: redundanci virtuálního počítače, použijte nastavení dostupnosti](./media/advisor-high-availability-recommendations/advisor-high-availability-create-availability-set.png)

## <a name="ensure-availability-set-fault-tolerance"></a>Ujistěte se, že odolnost proti chybám sadu dostupnosti 

Advisor určí skupiny dostupnosti, které obsahují jeden virtuální počítač a doporučuje přidání tooit jeden nebo více virtuálních počítačů. Zajistit aplikace tooyour redundanci, doporučujeme seskupit dva nebo více virtuálních počítačů v nastavení dostupnosti. Tato konfigurace zajistí, že během buď plánované i neplánované údržby, je k dispozici alespoň jeden virtuální počítač a splňuje hello virtuální počítač Azure SLA. Můžete buď toocreate na virtuálním počítači nebo toouse existující virtuální počítač a tooadd ho toohello sady dostupnosti.  

![Advisor doporučení: přidejte jeden nebo více nastavení dostupnosti toothis virtuální počítače](./media/advisor-high-availability-recommendations/advisor-high-availability-add-vm-to-availability-set.png)


## <a name="ensure-application-gateway-fault-tolerance"></a>Ujistěte se, aplikace brány odolnost proti chybám
tooensure hello kontinuity podnikových procesů důležitých aplikací, které jsou zapnuté nástrojem application Gateway, Advisor určí instance brány aplikace, kteří nejsou nakonfigurovaní pro odolnost proti chybám, a navrhování nápravné akce, které můžete provést . Advisor určí střední a velké aplikace s jedinou instancí brány a doporučí přidat nejméně jedna další instance. Také identifikuje instance jednoho nebo více malé aplikačních bran a doporučuje migrace toomedium nebo velké SKU. Advisor doporučuje tyto akce tooensure, že vaše aplikace instance brány jsou nakonfigurované toosatisfy hello požadavků na aktuální SLA pro tyto prostředky.

![Advisor doporučení: nasazení dvou nebo více instancí brány střední a velké velikostí aplikace](./media/advisor-high-availability-recommendations/advisor-high-availability-application-gateway.png)

## <a name="improve-hello-performance-and-reliability-of-virtual-machine-disks"></a>Zvýšení výkonu hello a spolehlivost disky virtuálního počítače

Advisor určí virtuálních počítačů s standardní disky a doporučuje upgrade toopremium disky.
 
Azure Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disků pro virtuální počítače spustit I náročnými úlohy. Disky virtuálního počítače, které používají účty úložiště premium ukládat data do disků SSD (Solid-State Drive). Hello nejlepšího výkonu pro vaši aplikaci doporučujeme migraci všechny disky virtuálního počítače vyžadující vysokou úložiště toopremium IOPS. 

Pokud vaše disky nevyžadují vysokou IOPS, můžete omezit náklady na údržbu je v standardní úložiště. Standardní úložiště ukládá data disku virtuálního počítače na jednotkách pevných disků (HDD) namísto SSD. Můžete toomigrate disky toopremium disky virtuálního počítače. Pro prémiové disky jsou podporovány na virtuálním počítači většina SKU. Ale v některých případech, pokud chcete, aby toouse prémiové disky, můžete potřebovat tooupgrade virtuálního počítače SKU také.

![Advisor doporučení: upgrade toopremium disky vylepšit spolehlivost hello disky virtuálního počítače](./media/advisor-high-availability-recommendations/advisor-high-availability-upgrade-to-premium-disks.png)

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>Virtuální počítač data chránit před nechtěným odstraněním
Advisor identifikuje virtuální počítače, kde není povolené zálohování, a doporučí povolení zálohování. Nastavení zálohování virtuálních počítačů zajistí hello dostupnost důležitých podnikových dat a nabízí ochranu proti náhodnému odstranění nebo poškození.

![Advisor doporučení: Konfigurace virtuálního počítače zálohování tooprotect důležitá data](./media/advisor-high-availability-recommendations/advisor-high-availability-virtual-machine-backup.png)

## <a name="access-high-availability-recommendations-in-advisor"></a>Přístup k zajištění vysoké dostupnosti doporučení v Advisor

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).

2. V levém podokně hello, klikněte na **další služby**.

3. V hello služby nabídky podokně v části **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.  
 se zobrazí řídicí panel Advisor Hello.

4. Na řídicím panelu hello Advisor, klikněte na tlačítko hello **vysokou dostupnost** kartě a potom vyberte hello předplatné, pro které chcete tooreceive doporučení.

> [!NOTE]
> tooaccess doporučení služby Advisor, musíte nejdřív *zaregistrovat předplatné* službou Advisor. Předplatné je zaregistrován při *předplatné vlastníka* spustí hello Advisor řídicí panel a klikne na tlačítko hello **získat doporučení** tlačítko. Toto je *jednorázovou operaci*. Po registraci předplatného hello dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.

## <a name="next-steps"></a>Další kroky

Další informace o doporučení služby Advisor najdete v tématu:
* [Úvod tooAzure Advisor](advisor-overview.md)
* [Začínáme se službou Advisor](advisor-get-started.md)
* [Náklady na doporučení služby Advisor](advisor-performance-recommendations.md)
* [Poradce při hodnocení výkonu doporučení](advisor-performance-recommendations.md)
* [Doporučení zabezpečení Advisor](advisor-security-recommendations.md)

