---
title: "aaaHandling výstrah zabezpečení v Azure Security Center | Microsoft Docs"
description: "Tento dokument vám pomůže bezpečnostní incidenty v možnosti toohandle toouse Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: e8feb669-8f30-49eb-ba38-046edf3f9656
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.openlocfilehash: edb911c298a2ce93cd0ea5b22ce002005040090f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="handling-security-incidents-in-azure-security-center"></a>Řešení bezpečnostních incidentů v Azure Security Center
Triaging a prošetřování výstrah zabezpečení může být časově náročná i hello nejvíce zkušený analytikům zabezpečení a pro mnoho je pevný tooeven věděli, kde toobegin. Pomocí [analytics](security-center-detection-capabilities.md) tooconnect hello informací mezi odlišné [výstrahy zabezpečení](security-center-managing-and-responding-alerts.md), Security Center můžete poskytnout jediné zobrazení kampaně útoku a všechny hello související výstrahy – můžete rychle pochopit, jaké akce hello útočník trvalo a jaké prostředky měla vliv na.

Tento dokument popisuje, jak toouse zabezpečení funkci tooassist Security Center vás upozornit zpracování incidentů zabezpečení.

## <a name="what-is-a-security-incident"></a>Co je bezpečnostní incident?
Ve službě Security Center představuje bezpečnostní incident souhrn všech výstrah pro určitý prostředek, které odpovídají schématům [modelu kill chain](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/). Incident se zobrazí v hello [výstrahy zabezpečení](security-center-managing-and-responding-alerts.md) dlaždice a okno. Další informace o každém výskytu bude hello seznamu související výstrahy, což umožňuje vám tooobtain odhalit incidentu.

## <a name="managing-security-incidents"></a>Správa incidentů zabezpečení
Vaše aktuální incidenty zabezpečení můžete zkontrolovat prohlížením hello dlaždice výstrahy zabezpečení. Přístup k hello portálu Azure a postupujte podle kroků hello toosee další podrobnosti o každém incidentu zabezpečení:

1. Na řídicím panelu Security Center hello, uvidíte hello **výstrahy zabezpečení** dlaždici.

    ![Dlaždice Výstrahy zabezpečení ve službě Security Center](./media/security-center-incident/security-center-incident-fig1.png)

2. Klikněte na tuto dlaždici tooexpand, a pokud je zjištěn bezpečnostního incidentu, se zobrazí v grafu výstrahy zabezpečení hello jak je uvedeno níže:

    ![Incident zabezpečení](./media/security-center-incident/security-center-incident-fig2.png)

3. Všimněte si, že popis incidentu hello zabezpečení má vlastní ikonu porovná tooother výstrahy. Klikněte na jeho tooview více podrobností o tohoto incidentu.

    ![Incident zabezpečení](./media/security-center-incident/security-center-incident-fig3.png)

4. Na hello **incident** okno se zobrazí další podrobnosti o této bezpečnostního incidentu, což zahrnuje jeho úplný popis jeho závažnosti (což je v tomto případě vysoká), jeho aktuální stav (v tomto případě je stále *aktivní*, což naznačuje hello uživatele nepřijme akci tooit – to můžete provést kliknutím pravým tlačítkem na incident hello v hello **výstrahy zabezpečení** okno), hello napadení prostředků (v tomto případě *VM1*), hello nápravy kroky pro hello incident, a v dolním podokně hello máte hello výstrahy, které byly součástí tohoto incidentu. Pokud chcete další informace o každé výstraze tooobtain, jednoduše klikněte na jeho a další okno otevře, jak je uvedeno níže:

    ![Incident zabezpečení](./media/security-center-incident/security-center-incident-fig4.png)

Hello informace v tomto okně se bude lišit podle výstrahy toohello. Čtení [správy a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) pro další informace o toomanage tyto výstrahy. Některé důležité informace týkající se této funkce:

* Nový filtr umožňuje toocustomize, které vaše tooIncident zobrazení pouze výstrahy pouze nebo obojí.
* Hello stejná výstraha může existovat v rámci incidentu (pokud existuje), stejně jako toobe viditelné jako samostatnou výstrahu.

## <a name="see-also"></a>Viz také
V tomto dokumentu jste zjistili, jak toouse hello incidentu funkce zabezpečení ve službě Security Center. toolearn Další informace o Security Center, najdete v části hello následující:

* [Správa a zpracování výstrah toosecurity v Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Funkce detekce ve službě Azure Security Center](security-center-detection-capabilities.md)
* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md)
* [Správa a zpracování výstrah toosecurity v Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md)– přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.
