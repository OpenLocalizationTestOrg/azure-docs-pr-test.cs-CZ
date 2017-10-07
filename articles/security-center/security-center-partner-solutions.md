---
title: "aaaManaging partnerských řešení v Azure Security Center | Microsoft Docs"
description: "Tento dokument vás provede jak Azure Security Center umožňuje monitorování v první pohled hello stav vašich partnerských řešení integrovaných ve vašem předplatném Azure."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: terrylan
ms.openlocfilehash: fc97aedf709b9044bfd3d4ecae0b58d5fa716bbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-partner-solutions-with-azure-security-center"></a>Sledování partnerských řešení pomocí Azure Security Center
Tento dokument vás provede jak toomonitor hello stav vašich partnerských řešení v Azure Security Center.

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení. Tento dokument není to podrobný průvodce.
>
>

## <a name="monitoring-partner-solutions"></a>Sledování partnerských řešení
Hello **Partner solutions** na hello dlaždici **Security Center** okno umožňuje sledovat na první pohled hello stav vašich partnerských řešení, které jsou integrované s předplatným Azure.

![Dlaždice Partner solutions (Partnerská řešení)][1]

Hello **Partner solutions** dlaždice zobrazí hello počet partnerských řešení integrovaných ve vašem předplatném. Pokud je integrovaná žádná řešení, dlaždice hello zobrazí hello hodnotu nula.

tooview hello stav vašich partnerských řešení:

1. Vyberte hello **Partner solutions** dlaždici. Hello **Partner solutions** tooSecurity Center připojené otevře se okno se seznamem partnerských řešení.

   ![Partnerská řešení][3]

   Hello stav partnerských řešení může být:

   * Chráněné (zelená) – Stav je zcela v pořádku.
   * Není v pořádku (červená) – Existuje problém stavu, které si žádá okamžitou pozornost.
   * Ukončeno generování sestav (oranžová) – řešení hello byla zastavena hlásit svůj stav.
   * Neznámý stav ochrany (oranžová) – hello stav řešení hello Neznámý v tuto chvíli kvůli tooa se nezdařil proces přidávání nového prostředku toohello existujícího řešení.
   * Neuveden (šedá) – řešení hello neohlásil nic ještě nemusí být na řešení stav uvedený, pokud se nedávno připojil a ještě probíhá jeho nasazení.

2. Vyberte jedno partnerské řešení. V tomto příkladu umožní vyberte hello **Qualys** řešení.  Otevře se okno ukazuje, že stav hello hello partnerského řešení a řešení hello přidružené prostředky. Vyberte **řešení konzoly** tooopen hello partnera správu prostředí pro toto řešení.

   ![Podrobné zobrazení partnerského řešení][4]
3. Přejděte zpět toohello **Qualys** a vyberte **odkaz VM**. Hello **odkaz aplikace** otevře se okno. Sem se můžete připojit prostředky toohello partnerských řešení.

   ![Odkaz prostředky toopartner řešení][5]

## <a name="next-steps"></a>Další kroky
V tomto dokumentu jste přináší toohello **partnerských řešení** dlaždici v Centru zabezpečení. toolearn Další informace o Security Center, najdete v části hello následující články:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Další informace jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – Další informace jak toomanage a reakce toosecurity výstrahy.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure hello a informace.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
