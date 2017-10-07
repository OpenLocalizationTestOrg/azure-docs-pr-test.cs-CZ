---
title: "stav výstrahy aaaResolve endpoint protection v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello Azure Security Center doporučení ** výstrahy stavu řešení Endpoint Protection **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4050f453-98fc-4314-8438-d476469757fb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
ms.openlocfilehash: 9631d15aa1dfa9003d56332363ae7911061ed0b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Vyřešení výstrahy stavu aplikace endpoint protection v Azure Security Center
Azure Security Center doporučí, vyřešení výstrahy stavu zjištěné endpoint protection.  Security Center umožňuje toosee virtuálních počítačů (VM) mají selhání endpoint protection a kolik selhání.

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení. Není to podrobný průvodce.
> 
> 

## <a name="implement-hello-recommendation"></a>Implementace doporučení hello
1. V hello **doporučení okno**, vyberte **výstrahy stavu řešení Endpoint Protection**.
   ![Vyřešení výstrah stavu služby Endpoint Protection][1]
2. Otevře se okno hello **Endpoint Protection selhání** který obsahuje seznam virtuálních počítačů s chybami a hello počet selhání pro každý virtuální počítač. Vyberte virtuální počítač ze seznamu hello.
   ![Selhání ochrany koncového bodu][2]
3. A **selhání seznamu** otevře se okno pro hello vybrané virtuální počítač, zobrazení seznamu chyb. Vyberte další toolearn seznamu hello selhání. Otevře se okno s informacemi o selhání hello vybrané.
   ![Seznam selhání][3]
   ![událostí selhání][4]

## <a name="see-also"></a>Viz také
toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md)– zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – Zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md)– zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md)– zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md)– přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/)– získejte nejnovější informace zabezpečení Azure hello a informace.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
