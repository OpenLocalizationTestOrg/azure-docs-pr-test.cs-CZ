---
title: "aaaDisaster scénáře obnovení pro virtuální počítače Azure | Microsoft Docs"
description: "Zjistěte, jaké toodo hello události, přerušení služby Azure má to dopad na virtuálních počítačích Azure."
services: virtual-machines
documentationcenter: 
author: kmouss
manager: timlt
editor: 
ms.assetid: 65272148-ff06-4bce-91f1-851d706d4d40
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: kmouss;aglick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 839c6f9c51a7f35b48ef3636bc346eef332bb06d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-toodo-in-hello-event-that-an-azure-service-disruption-impacts-azure-vms"></a>Jaké toodo hello události, přerušení služby Azure má to dopad na virtuálních počítačích Azure
Společnost Microsoft se jsme fungovat pevný toomake se, že našich služeb jsou vždy k dispozici tooyou až je budete potřebovat. Vynutí nad rámec naše řízení někdy vliv nám způsoby, které způsobit přerušení poskytování služeb neplánované.

Společnost Microsoft poskytuje smlouvy úroveň služeb (SLA) pro jeho služby jako závazek provozu a připojení. Hello SLA pro jednotlivé služby Azure naleznete na adrese [smlouvy o úrovni služeb Azure](https://azure.microsoft.com/support/legal/sla/).

Azure již obsahuje mnoho funkcí integrovanou platformu, které podporují aplikace s vysokou dostupností. Další informace o těchto službách, přečtěte si [zotavení po havárii a vysoká dostupnost pro aplikace Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Tento článek se zabývá true zotavení po havárii, když dojde k výpadku kvůli toomajor přírodní katastrofě nebo přerušení rozšířeným služby celou oblast. Toto jsou výjimečných výskytů, ale je nutné připravit pro možnost hello se k výpadku celou oblast. Pokud celou oblast dojde k přerušení služby, hello místně redundantní kopie dat by být dočasně k dispozici. Pokud jste povolili geografická replikace, tři další kopie objektů BLOB služby Azure Storage a tabulek jsou uložené v jiné oblasti. V případě hello dokončení regionální výpadku nebo havárii, ve které hello primární oblast není použitelná pro obnovení znovu mapuje Azure, všechny hello DNS položky toohello geograficky replikované oblasti.

toohelp zpracování těchto výjimečných výskytů, poskytujeme hello pokynů pro virtuální počítače Azure v případě hello přerušení služby hello celé oblasti, kde je virtuální počítač Azure aplikaci nasadit.

## <a name="option-1-initiate-a-failover-by-using-azure-site-recovery"></a>Možnost 1: Spuštění převzetí služeb při selhání pomocí Azure Site Recovery
Azure Site Recovery pro virtuální počítače můžete nakonfigurovat tak, aby vaše aplikace s jedním kliknutím v několika minut můžete obnovit. Můžete provádět replikaci tooAzure oblast podle svého výběru a není omezená toopaired oblastí. Abyste mohli začít podle [replikaci virtuálních počítačů](https://aka.ms/a2a-getting-started). Můžete [vytvoření plánu obnovení](../site-recovery/site-recovery-create-recovery-plans.md) tak, aby můžete automatizovat proces hello celý převzetí služeb při selhání pro vaši aplikaci. Můžete [testování vaší převzetí služeb při selhání](../site-recovery/site-recovery-test-failover-to-azure.md) předem, bez dopadu na produkční aplikaci nebo hello probíhající replikace. V případě hello narušení primární oblasti, jste právě [zahájení převzetí služeb při selhání](../site-recovery/site-recovery-failover.md) a převeďte aplikace v cílové oblasti.


## <a name="option-2-wait-for-recovery"></a>Možnost 2: Čekání pro obnovení
V takovém případě není třeba žádné akce z vaší strany. Vědět, že pracujeme usilovně toorestore dostupnost služeb. Zobrazí aktuální stav služby hello na našem [řídicím panelu stavu služeb Azure](https://azure.microsoft.com/status/).

Toto je nejlepší možnost hello, pokud jste nenastavili Azure Site Recovery, geograficky redundantní úložiště s přístupem pro čtení nebo přerušení předchozí toohello geograficky redundantní úložiště. Pokud jste nastavili geograficky redundantní úložiště nebo geograficky redundantní úložiště s přístupem pro čtení pro účet úložiště hello kde jsou uloženy vaše virtuální počítač virtuální pevné disky (VHD), můžete hledat toorecover hello základní image virtuálního pevného disku a opakujte tooprovision nový virtuální počítač z něj. Toto není upřednostňovanou možnost, protože nejsou k dispozici žádné záruky synchronizace dat. Tato možnost v důsledku toho není zaručena toowork.


> [!NOTE]
> Uvědomte si, že nemáte žádné kontrolu nad tento proces a se vztahuje pouze na přerušení služeb celou oblast. Z tohoto důvodu musíte také spoléhat na jiné strategii zálohování konkrétní aplikace tooachieve hello nejvyšší úroveň dostupnosti. Další informace najdete v tématu část hello na [Data strategie zotavení po havárii](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications#data-strategies-for-disaster-recovery).
>
>

## <a name="next-steps"></a>Další kroky

- Spustit [chrání vaše aplikace spuštěné na virtuálních počítačích Azure](https://aka.ms/a2a-getting-started) pomocí Azure Site Recovery

- Další informace o tom, jak tooimplement zotavení po havárii a strategie vysoké dostupnosti, najdete v části toolearn [zotavení po havárii a vysoká dostupnost pro aplikace Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

- toodevelop podrobné technické vysvětlení funkcí Cloudová platforma, najdete v části [technické pokyny Azure odolnosti](../resiliency/resiliency-technical-guidance.md).


- Pokud nejsou jasné pokyny hello nebo pokud chcete Microsoft toodo hello operations vaším jménem, obraťte se na [zákaznickou podporu](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
