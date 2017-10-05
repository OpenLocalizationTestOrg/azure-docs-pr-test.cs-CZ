---
title: "Oprava chyb zabezpečení operačního systému v Azure Security Center | Microsoft Docs"
description: "Tento dokument se dozvíte, jak provést doporučení Azure Security Center ** ohrožení zabezpečení operačního systému opravit **."
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
ms.openlocfilehash: e6b251d5b97c57b3b6f79d14e53fbed5ca37ecb0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Oprava chyb zabezpečení operačního systému v Azure Security Center
Azure Security Center analyzuje denně virtuální počítač (VM) operačního systému (OS) pro konfigurace, které může zvýšit virtuálního počítače vůči útokům a doporučuje změny konfigurace, které tyto nedostatky. Security Center doporučí, řeší chyby zabezpečení, pokud konfigurace operačního systému Virtuálního počítače neodpovídá doporučenou konfiguraci pravidla.

> [!NOTE]
> Další informace o konkrétní konfigurace se sledují, najdete v článku [seznam doporučenou konfiguraci pravidel](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
>
>

## <a name="implement-the-recommendation"></a>Implementace doporučení

> [!NOTE]
> Tento dokument vám tuto službu představí formou ukázkového nasazení.  Tento dokument není to podrobný průvodce.
>
>

1. V **doporučení** vyberte **ohrožení zabezpečení operačního systému opravit**.
   ![Náprava ohrožení zabezpečení operačního systému][1]

    **Ohrožení zabezpečení operačního systému opravit** okno k otevření a virtuální počítače s konfigurací operačního systému, které neodpovídají doporučenou konfiguraci pravidla.  Pro každý virtuální počítač v okně identifikuje:

   * **PRAVIDLA se nezdařilo** – počet pravidel, která konfigurace operačního systému Virtuálního počítače se nezdařilo.
   * **ČAS poslední kontroly** – datum a čas, Security Center naposledy hledala konfigurace operačního systému Virtuálního počítače.
   * **Stav** --aktuální stav chyby zabezpečení:

     * Otevřete: Tuto chybu zabezpečení dosud nebylo řešeno.
     * V průběhu: Aktuálně se aplikuje ohrožení zabezpečení, není třeba žádné akce
     * Přeložit: Tuto chybu zabezpečení byl již řešit (Pokud problém vyřešen, položka je zobrazena šedě)
   * **ZÁVAŽNOST** – všechna ohrožení zabezpečení jsou nastaveny na závažnost nízká, což znamená, ohrožení zabezpečení, mělo by se řešit, ale nevyžaduje okamžitou pozornost.

2. Vyberte virtuální počítač. Okno pro tento virtuální počítač se zobrazí pravidla, které selhaly.
   ![Pravidla konfigurace, které selhaly][2]

3. Vyberte pravidlo. V tomto příkladu umožní vybrat **heslo musí splňovat požadavky na složitost**. Otevře se okno popisující neúspěšných pravidel a dopad. Zkontrolujte podrobnosti a zvažte, jak se používají konfigurace operačního systému.
  ![Popis pravidla se nezdařila][3]

  Security Center používá společné konfigurace – výčet (CCE) k přiřazení jedinečné identifikátory pro konfigurační pravidla. V tomto okně se poskytuje následující informace:

  - NÁZEV – Název pravidla
  - ZÁVAŽNOST – Hodnota závažnosti CCE kritická, důležité nebo upozornění
  - CCIED – CCE jedinečný identifikátor pro pravidlo
  - Popis – Popis pravidla
  - Ohrožení zabezpečení – Vysvětlení ohrožení zabezpečení nebo riziko, pokud není použita pravidla
  - DOPAD – Dopad na chod firmy při použití pravidla
  - OČEKÁVANÁ hodnota – Hodnota očekávané při Security Center analyzuje konfiguraci operačního systému virtuálního počítače podle pravidla
  - – PRAVIDLO pravidlo operace použije pomocí služby Security Center při analýze konfigurace operačního systému virtuálního počítače podle pravidla
  - Skutečná hodnota – Hodnota vrácena po dokončení analýzy konfigurace operačního systému virtuálního počítače podle pravidla
  - Výsledek vyhodnocení –-výsledek analýzy: předání služeb při selhání

## <a name="see-also"></a>Viz také
Tento článek vám ukázal, jak provést doporučení Security Center "Napravit OS ohrožení zabezpečení." Můžete zkontrolovat sadu pravidel, konfigurace [zde](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Security Center používá CCE (Common Configuration výčtu) k přiřazení jedinečné identifikátory pro konfigurační pravidla. Přejděte [CCE](https://nvd.nist.gov/cce/index.cfm) lokality pro další informace.

Další informace o službě Security Center, najdete v následujících zdrojích informací:

* [Podporované platformy v Azure Security Center](security-center-os-coverage.md) -obsahuje seznam podporovaných Windows a virtuální počítače s Linuxem.
* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak spravovat a reakce na výstrahy zabezpečení.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak sledovat stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití této služby.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
