---
title: "aaaAdd brána firewall příští generace v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello doporučení služby Azure Security Center ** přidat další generace brány Firewall ** a ** trasy traffice prostřednictvím NGFW pouze **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 48b99015-4db8-4ce8-85e4-b544c0fa203e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 9a80f12571ba08eadf3361728c6321388c863235
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Přidat Brána Firewall příští generace v Azure Security Center
Azure Security Center může doporučujeme přidat brána firewall příští generace (NGFW) z tooincrease partnera Microsoft vaše ochranu zabezpečení. Tento dokument vás příklad provede toodo to.

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení.  Není to podrobný průvodce.
>
>

## <a name="implement-hello-recommendation"></a>Implementace doporučení hello
1. V hello **doporučení** vyberte **přidat Brána Firewall příští generace**.
   ![Přidání brány firewall příští generace][1]
2. V hello **přidat Brána Firewall příští generace** okně vyberte koncový bod.
   ![Vyberte koncový bod][2]
3. Druhý **přidat Brána Firewall příští generace** otevře se okno. Můžete zvolit toouse do stávajícího řešení Pokud je k dispozici nebo můžete vytvořit nový. V tomto příkladu nejsou žádná existující řešení k dispozici, vytvoříme NGFW.
   ![Vytvoření Brána Firewall příští generace][3]
4. toocreate NGFW, vybrat řešení z hello seznam integrovaných partnerů. V tomto příkladu jsme vyberte **kontrolní bod**.
   ![Vybrat řešení Brána Firewall příští generace][4]
5. Hello **kontrolní bod** otevře se okno poskytování informací o hello partnerských řešení. Vyberte **vytvořit** v okně informace hello.
   ![Okno informace o brány firewall][5]
6. Hello **vytvořit virtuální počítač** otevře se okno. Zde můžete zadat informace požadované toospin virtuálního počítače (VM), který spouští hello NGFW. Postupujte podle kroků hello a zadejte požadované informace NGFW hello. Vyberte OK tooapply.
   ![Vytvoření virtuálního počítače toorun NGFW][6]

## <a name="route-traffic-through-ngfw-only"></a>Směrovat přenosy jenom přes firewall nové generace
Vrátí toohello **doporučení** okno. Nový záznam vygenerovalo po přidání NGFW prostřednictvím Security Center, nazývá **směrování provozu prostřednictvím NGFW pouze**. Toto doporučení je vytvořen jen v případě, že jste nainstalovali vaší NGFW prostřednictvím Security Center. Pokud máte internetových koncových bodů, Security Center doporučí konfiguraci pravidel skupin zabezpečení sítě, které vynutit tooyour příchozí přenosy virtuálních počítačů prostřednictvím vaší NGFW.

1. V hello **doporučení okno**, vyberte **směrování provozu prostřednictvím NGFW pouze**.
   ![Směrování provozu jenom přes NGFW][7]
2. Otevře se okno hello **směrování provozu prostřednictvím NGFW pouze**, který obsahuje seznam virtuálních počítačů, které je možné směrovat provoz. Vyberte virtuální počítač ze seznamu hello.
   ![Vyberte virtuální počítač][8]
3. Okno pro hello vybraný virtuální počítač se otevře, zobrazení související příchozích pravidel. Popis vám poskytne další informace o možných další kroky. Vyberte **upravit příchozí pravidla** tooproceed s úpravy příchozího pravidla. Hello předpokládají, že **zdroj** není nastaven příliš**žádné** pro koncové body internetového hello propojené s hello NGFW. toolearn Další informace o vlastnosti hello hello příchozí pravidlo, najdete v části [pravidla NSG](../virtual-network/virtual-networks-nsg.md#nsg-rules).
   ![Konfigurace pravidel přístupu toolimit][9]
   ![příchozí pravidlo úpravy][10]

## <a name="see-also"></a>Viz také
Tento dokument ukázal, jak tooimplement hello Security Center doporučení "Přidat Brána Firewall příští generace". toolearn informace o NGFWs a hello kontrolní bod partnerských řešení, najdete v části hello následující:

* [Brána Firewall příští generace](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
* [Kontrolní bod vSEC](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
