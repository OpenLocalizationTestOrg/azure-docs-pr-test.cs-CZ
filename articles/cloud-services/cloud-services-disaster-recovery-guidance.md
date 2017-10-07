---
title: "aaaWhat toodo v události hello narušení služby Azure, který má dopad na Azure Cloud Services | Microsoft Docs"
description: "Zjistěte, jaké toodo v události hello narušení služby Azure, který má dopad na Azure Cloud Services."
services: cloud-services
documentationcenter: 
author: mmccrory
manager: timlt
editor: 
ms.assetid: e52634ab-003d-4f1e-85fa-794f6cd12ce4
ms.service: cloud-services
ms.workload: cloud-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: mmccrory
ms.openlocfilehash: bb1f1835fc91a23772f81801b67d5786c190ba19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-toodo-in-hello-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Jaké toodo v události hello narušení služby Azure, který má dopad na Azure Cloud Services
Společnost Microsoft se jsme fungovat pevný toomake se, že našich služeb jsou vždy k dispozici tooyou až je budete potřebovat. Vynutí nad rámec naše řízení někdy vliv nám způsoby, které způsobit přerušení poskytování služeb neplánované.

Společnost Microsoft poskytuje smlouvy úroveň služeb (SLA) pro jeho služby jako závazek provozu a připojení. Hello SLA pro jednotlivé služby Azure naleznete na adrese [smlouvy o úrovni služeb Azure](https://azure.microsoft.com/support/legal/sla/).

Azure již obsahuje mnoho funkcí integrovanou platformu, které podporují aplikace s vysokou dostupností. Další informace o těchto službách, přečtěte si [zotavení po havárii a vysoká dostupnost pro aplikace Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Tento článek se zabývá true zotavení po havárii, když dojde k výpadku kvůli toomajor přírodní katastrofě nebo přerušení rozšířeným služby celou oblast. Toto jsou výjimečných výskytů, ale je nutné připravit pro možnost hello se k výpadku celou oblast. Pokud celou oblast dojde k přerušení služby, hello místně redundantní kopie dat by být dočasně k dispozici. Pokud jste povolili geografická replikace, tři další kopie objektů BLOB služby Azure Storage a tabulek jsou uložené v jiné oblasti. V případě hello dokončení regionální výpadku nebo havárii, ve které hello primární oblast není použitelná pro obnovení znovu mapuje Azure, všechny hello DNS položky toohello geograficky replikované oblasti.

> [!NOTE]
> Uvědomte si, že nemáte žádné kontrolu nad tento proces a se vztahuje pouze na přerušení datacenter úrovni služeb. Z tohoto důvodu musíte také spoléhat na jiné strategii zálohování konkrétní aplikace tooachieve hello nejvyšší úroveň dostupnosti. Další informace najdete v tématu [zotavení po havárii a vysoká dostupnost pro aplikace založené na Microsoft Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md). Pokud chcete mít tooaffect toobe vlastní převzetí služeb při selhání, můžete chtít použít hello tooconsider [geograficky redundantní úložiště s přístupem pro čtení (RA-GRS)](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage), která vytvoří kopii dat jen pro čtení v jiné oblasti.
>
>


## <a name="option-1-use-a-backup-deployment-through-azure-traffic-manager"></a>Možnost 1: Použití zálohování nasazení prostřednictvím Azure Traffic Manager
Hello nejvíce robustní řešení pro zotavení po havárii zahrnuje zachování více nasazení vaší aplikace v různých oblastech a potom pomocí [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) toodirect provoz mezi nimi. Azure Traffic Manager poskytuje více [metody směrování](../traffic-manager/traffic-manager-routing-methods.md), takže si můžete vybrat, zda toomanage vaše nasazení pomocí modelu primární nebo zálohování nebo toosplit provoz mezi je.

![Vyrovnávání Azure Cloud Services v oblastech Azure Traffic Manager](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

Hello nejrychlejší odpovědi toohello dojít ke ztrátě oblasti, je důležité, který konfigurujete Traffic Manager [monitorování koncového bodu](../traffic-manager/traffic-manager-monitoring.md).

## <a name="option-2-deploy-your-application-tooa-new-region"></a>Možnost 2: Nasazení vaší aplikace tooa nové oblasti
Udržování víc aktivní nasazení, jak je popsáno v předchozí možnost hello způsobuje další průběžných nákladů. Pokud je dostatečně flexibilní cíli času obnovení (RTO) a máte hello původní kód nebo kompilované balíček cloudové služby, můžete vytvořit novou instanci aplikace v jiné oblasti a aktualizovat DNS záznamy toopoint toohello nové nasazení.

Další podrobnosti o toocreate a nasadit aplikace cloudové služby, najdete v části [jak toocreate a nasazení cloudové služby](cloud-services-how-to-create-deploy-portal.md).

V závislosti na vašich zdrojů dat aplikace může být nutné postupy toocheck hello obnovení pro zdroj dat aplikace.

* U zdrojů dat úložiště Azure, najdete v části [replikace Azure Storage](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage) toocheck na hello možnosti, které jsou k dispozici podle hello zvolili modelu replikace pro vaši aplikaci.
* Databáze SQL zdroje, najdete v tématu [přehled: obchodní kontinuitu a databáze zotavení po havárii s databází SQL v cloudu](../sql-database/sql-database-business-continuity.md) toocheck na hello možnosti, které jsou k dispozici na základě hello vybrali replikace modelu pro vaši aplikaci.


## <a name="option-3-wait-for-recovery"></a>Možnost 3: Počkejte obnovení
V takovém případě není třeba žádné akce z vaší strany, ale služby nebude k dispozici, dokud je obnoven hello oblast. Zobrazí aktuální stav služby hello na hello [řídicím panelu stavu služeb Azure](https://azure.microsoft.com/status/).

## <a name="next-steps"></a>Další kroky
Další informace o tom, jak tooimplement zotavení po havárii a strategie vysoké dostupnosti, najdete v části toolearn [zotavení po havárii a vysoká dostupnost pro aplikace Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

toodevelop podrobné technické vysvětlení funkcí Cloudová platforma, najdete v části [technické pokyny Azure odolnosti](../resiliency/resiliency-technical-guidance.md).
