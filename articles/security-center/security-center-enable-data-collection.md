---
title: "shromažďování dat aaaEnable v Azure Security Center | Microsoft Docs"
description: " Zjistěte, jak tooenable shromažďování dat v Azure Security Center. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 78bbf9a3d852095e2a1387c1606ff4bbb778a0dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-data-collection-in-azure-security-center"></a>Povolení shromažďování dat v Azure Security Center

> [!NOTE]
> Počínaje časná 2017 června, Security Center použije hello agenta Microsoft Monitoring Agent toocollect a ukládat data. Další, najdete v části toolearn [Azure Security Center platformy migrace](security-center-platform-migration.md). Hello informace v tomto článku představuje funkce Security Center po přechodu toohello agenta Microsoft Monitoring Agent.
>
>

Security Center shromažďuje data z vaší virtuální počítače (VM) tooassess jejich stavu zabezpečení, zadejte doporučení zabezpečení a výstrah toothreats. Pokud nejprve přístup k Security Center, máte hello možnost tooenable shromažďování dat pro všechny virtuální počítače ve vašem předplatném. Pokud se shromažďování dat není povolené, Security Center doporučí, zapnout shromažďování dat v zásadách zabezpečení hello pro toto předplatné.

Při shromažďování dat je povolené, Security Center zřizuje hello agenta Microsoft Monitoring Agent na všechny stávající podporovaných virtuálních počítačích Azure a všechny nové, které jsou vytvořené. Hello agenta Microsoft Monitoring Agent hledá různé konfigurace související se zabezpečením. Kromě toho hello operačního systému vyvolá událost protokolu událostí. Mezi příklady těchto údajů patří: typ a verze operačního systému, protokoly operačního systému (protokoly událostí systému Windows), spuštěné procesy, název počítače, IP adresy, přihlášený uživatel a ID klienta. Hello agenta Microsoft Monitoring Agent čte položky protokolu událostí a konfigurací a zkopíruje prostoru tooyour hello data pro analýzu. Hello agenta Microsoft Monitoring Agent také zkopíruje prostoru tooyour soubory výpisu havárií.

Pokud používáte úroveň Free hello služby Security Center, můžete zakázat shromažďování dat z virtuálních počítačů tak, že zakážete shromažďování dat v zásadách zabezpečení hello. Zakázání shromažďování dat omezením posouzení zabezpečení pro virtuální počítače. Další, najdete v části toolearn [zakázání shromažďování dat](#disabling-data-collection). Jsou povoleny snímků disku virtuálního počítače a kolekce artefaktů i v případě, že shromažďování dat je zakázané. Shromažďování dat je vyžadován pro odběry ve standardní vrstvě hello služby Security Center.

> [!NOTE]
> Další informace o volné a Standard Security Center [cenové úrovně](security-center-pricing.md).
>
>

## <a name="implement-hello-recommendation"></a>Implementace doporučení hello

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení. Tento dokument není to podrobný průvodce.
>
>

1. V hello **doporučení** vyberte **povolení shromažďování dat pro odběry**.  Tím se otevře hello **shromažďování dat zapnout** okno.
   ![Okno doporučení][2]
2. Na hello **shromažďování dat zapnout** okně vyberte své předplatné. Hello **zásady zabezpečení** otevře se okno pro toto předplatné.
3. Na hello **zásady zabezpečení** vyberte **na** pod **shromažďování dat** tooautomatically shromažďovat protokoly. V odběru hello zapnete dat kolekce zřizuje hello rozšíření monitorování na všech aktuálních a nových podporované virtuální počítače.
4. Vyberte **Uložit**.
5. Vyberte **OK**.

## <a name="disabling-data-collection"></a>Zakázání shromažďování dat
Pokud používáte úroveň Free hello služby Security Center, můžete zakázat shromažďování dat z virtuálních počítačů v každém okamžiku tak, že zakážete shromažďování dat v zásadách zabezpečení hello. Shromažďování dat je vyžadován pro odběry ve standardní vrstvě hello služby Security Center.

1. Vrátí toohello **Security Center** okno a vyberte hello **zásad** dlaždici. Tím se otevře hello **zabezpečení zásady definovat zásady, za předplatné** okno.
   ![Vyberte dlaždici zásad hello][5]
2. Na hello **zabezpečení zásady definovat zásady, za předplatné** okně, vyberte hello předplatné chcete shromažďování dat toodisable.
3. Hello **zásady zabezpečení** otevře se okno pro toto předplatné.  Vyberte **vypnout** pod shromažďování dat.
4. Vyberte **Uložit** hello nejvyšší pásu karet.

## <a name="next-steps"></a>Další kroky
Tento článek ukázal, jak tooimplement hello Security Center doporučení "povolení shromažďování dat." toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md)– zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md)– zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
- [Zabezpečení dat v Azure Security Center](security-center-data-security.md) -další způsob správy a zabezpečení ve službě Security Center.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md)– přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/)– získejte nejnovější informace zabezpečení Azure hello a informace.

<!--Image references-->
[2]: ./media/security-center-enable-data-collection/recommendations.png
[3]: ./media/security-center-enable-data-collection/data-collection.png
[4]: ./media/security-center-enable-data-collection/storage-account.png
[5]: ./media/security-center-enable-data-collection/policy.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
