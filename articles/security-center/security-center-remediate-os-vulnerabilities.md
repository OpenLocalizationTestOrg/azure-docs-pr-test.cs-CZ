---
title: "Slabá místa zabezpečení aaaRemediate operačního systému v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** ohrožení zabezpečení operačního systému opravit **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 704103f7fb15835943d74b665d2bd56cb5e0a36d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Oprava chyb zabezpečení operačního systému v Azure Security Center
Azure Security Center analyzuje denně virtuální počítač (VM) operačního systému (OS) pro konfigurace, které by mohly zvýšit hello virtuálních počítačů zranitelnější konfigurace tooattack a doporučuje změny tooaddress tyto chyby zabezpečení. Security Center doporučí, vyřešte chyby zabezpečení, pokud konfigurace operačního systému Virtuálního počítače neodpovídá hello doporučená pravidla konfigurace.

> [!NOTE]
> Další informace o hello konkrétní konfigurace se sledují, najdete v části hello [seznam doporučenou konfiguraci pravidel](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
>
>

## <a name="implement-hello-recommendation"></a>Implementace doporučení hello

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení.  Tento dokument není to podrobný průvodce.
>
>

1. V hello **doporučení** vyberte **ohrožení zabezpečení operačního systému opravit**.
   ![Náprava ohrožení zabezpečení operačního systému][1]

    Hello **ohrožení zabezpečení operačního systému opravit** okno k otevření a virtuální počítače s konfigurací operačního systému, které neodpovídají hello doporučená konfigurační pravidla.  Pro každý virtuální počítač identifikuje hello okno:

   * **PRAVIDLA se nezdařilo** – hello počet pravidel, které hello konfigurace operačního systému Virtuálního počítače se nezdařilo.
   * **ČAS poslední kontroly** – hello datum a čas, Security Center naposledy hledala konfigurace operačního systému hello Virtuálního počítače.
   * **Stav** – hello aktuální stav hello ohrožení zabezpečení:

     * Otevřete: ohrožení zabezpečení hello dosud nebylo řešeno.
     * V průběhu: Ohrožení zabezpečení hello se aktuálně, není třeba žádné akce
     * Přeložit: ohrožení zabezpečení hello byl již řešit (když hello problém vyřešen, hello položka je zobrazena šedě)
   * **ZÁVAŽNOST** – všechna ohrožení zabezpečení. jsou nastaveny tooa závažnost nízká, což znamená, ohrožení zabezpečení, mělo by se řešit, ale nevyžaduje okamžitou pozornost.

2. Vyberte virtuální počítač. Okno pro tento virtuální počítač se zobrazí hello pravidla, které selhaly.
   ![Pravidla konfigurace, které selhaly][2]

3. Vyberte pravidlo. V tomto příkladu umožní vybrat **heslo musí splňovat požadavky na složitost**. Otevře se okno popisující dopad pravidlo a hello hello se nezdařilo. Zkontrolujte podrobnosti hello a zvažte, jak se používají konfigurace operačního systému.
  ![Popis pravidla pro neúspěšné hello][3]

  Security Center používá společné konfigurace – výčet (CCE) tooassign jedinečné identifikátory pro konfigurační pravidla. v tomto okně je k dispozici Hello následující informace:

  - NÁZEV – Název pravidla
  - ZÁVAŽNOST – Hodnota závažnosti CCE kritická, důležité nebo upozornění
  - CCIED – CCE jedinečný identifikátor pro pravidlo hello
  - Popis – Popis pravidla
  - Ohrožení zabezpečení – Vysvětlení ohrožení zabezpečení nebo riziko, pokud není použita pravidla
  - DOPAD – Dopad na chod firmy při použití pravidla
  - OČEKÁVANÁ hodnota – Hodnota očekávané při Security Center analyzuje konfiguraci operačního systému virtuálního počítače podle pravidla hello
  - – PRAVIDLO pravidlo operace použije pomocí služby Security Center při analýze konfigurace operačního systému virtuálního počítače podle pravidla hello
  - Skutečná hodnota – Hodnota vrácena po dokončení analýzy konfigurace operačního systému virtuálního počítače podle pravidla hello
  - Výsledek vyhodnocení –-výsledek analýzy: předání služeb při selhání

## <a name="see-also"></a>Viz také
Tento článek ukázal, jak tooimplement hello Security Center doporučení "napravit OS ohrožení zabezpečení." Můžete zkontrolovat hello sadu pravidel konfigurace [zde](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Security Center používá CCE (Common Configuration výčtu) tooassign jedinečné identifikátory pro konfigurační pravidla. Navštivte hello [CCE](https://nvd.nist.gov/cce/index.cfm) lokality pro další informace.

toolearn Další informace o Security Center, najdete v části hello následující prostředky:

* [Podporované platformy v Azure Security Center](security-center-os-coverage.md) -obsahuje seznam podporovaných Windows a virtuální počítače s Linuxem.
* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
