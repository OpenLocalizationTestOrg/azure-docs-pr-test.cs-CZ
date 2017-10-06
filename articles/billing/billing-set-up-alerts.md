---
title: "aaaSet až fakturace nebo platební výstrahy pro předplatná Azure | Microsoft Docs"
description: "Popisuje, jak můžete na můžete nastavit výstrahy faktury Azure, se můžete vyhnout fakturace výskyt nečekaných událostí."
keywords: "platební výstraze, fakturace výstrahu"
services: 
documentationcenter: 
author: vikdesai
manager: tonguyen
editor: 
tags: billing
ms.assetid: 9b7b3eeb-cd9d-4690-86a3-51b1e2a8974f
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: vikdesai
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 711b9c72c59874792b0e229cdc5ec0fa517c24c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-billing-or-credit-alerts-for-your-microsoft-azure-subscriptions"></a>Nastavení výstrah fakturace nebo platební pro vaše předplatné Microsoft Azure
Pokud jste hello správce účtů pro předplatné Azure, můžete použít hello fakturace výstrah služby Azure toocreate přizpůsobit fakturace výstrahy, které pomáhají sledovat a spravovat fakturace aktivity pro vaše účty Azure.

Tato služba je ve verzi preview, proto musíte tooenable ho na stránce funkce verze Preview hello nejdřív.

## <a name="set-hello-alert-threshold-and-email-recipients"></a>Nastavit výstrahy prahovou hodnotu a e-mailu příjemce hello
1. Navštivte [stránku hello funkce Preview](https://account.windowsazure.com/PreviewFeatures) a povolte **fakturace výstrah služby**.

1. Po obdržení hello e-mailu potvrzení, že je zapnutý hello fakturace služby pro vaše předplatné, navštivte [stránku hello odběry](https://account.windowsazure.com/Subscriptions) hello portálu účtů. Klikněte na předplatné hello toomonitor a pak klikněte na **výstrahy**.

    ![Snímek obrazovky zobrazení odběry hello centra účtů Azure, se zvýrazněnou výstrahy][Image1]

2. Klikněte na tlačítko **přidat výstrahy** toocreate vaše první z nich. Můžete nastavit celkem pět fakturace výstrahy na předplatné s různé prahové hodnoty a až tootwo příjemců e-mailu pro každou výstrahu.

    ![Snímek obrazovky hello zobrazení výstrahy, kde můžete přidat výstrahy][Image2]

3. Přidáte-li výstrahu, můžete zadat jedinečný název, zvolte útraty prahovou hodnotu a vyberte hello e-mailové adresy, které jsou odesílány výstrahy. Při nastavování hello prahovou hodnotu, můžete buď **fakturace celkový** nebo **peněžního kreditu, který** z hello **výstrahy pro** seznamu. Fakturace celkem zobrazí se výstraha Pokud předplatné výdaje překročí prahovou hodnotu hello. Pro peněžního kreditu zobrazí se výstraha při peněžní kredity neklesla pod hello limit. Peněžní kredity obvykle platí odběry tooFree zkušební verze a Visual Studio.

    ![Snímek obrazovky zobrazení výstrah přidání hello, kde můžete konfigurovat příjemce][Image3]

Azure podporuje všechny e-mailovou adresu, ale není ověřte, že funguje hello e-mailovou adresu, proto tuto možnost pro překlepům.

## <a name="check-on-your-alerts"></a>Zkontrolujte na upozornění
Po nastavení výstrahy hello centra účtů je uvádí a ukazuje, kolik více můžete nastavit. Pro každou výstrahu zobrazí hello datum a čas, který byl odeslán, jestli je výstraha fakturace celkový nebo peněžního kreditu, který a hello limit, kterou vytvoříte. Datum hello je formát rrrr mm-dd Hello formát data a času je 24 hodin koordinaci světový čas (UTC). Klikněte na tlačítko hello plus přihlášení pro výstrahu v seznamu tooedit hello nebo klikněte na hello odpadkového koše toodelete ho.

## <a name="billing-alerts-for-enterprise-agreement-ea-customers"></a>Výstrahy fakturace pro zákazníky Enterprise Agreement (EA)
Zákazníci EA můžete získat výstrahy pro každé oddělení v zápisu nastavení výdaje kvóty. V tématu [oddělení výdaje kvóty](https://ea.azure.com/helpdocs/departmentSpendingQuotas) v hello EA portálu tooget spuštěna.

## <a name="learn-more-about-azure-cost-management"></a>Další informace o Azure náklady na správu
- Odhad nákladů pomocí hello [cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/), [celkové náklady na vlastnictví kalkulačky](https://aka.ms/azure-tco-calculator), a když přidáte službu.
- [Zkontrolujte využití a náklady na portálu Azure pravidelně](billing-getting-started.md#costs).
- Zapnout [Azure Advisor náklady doporučení](../advisor/advisor-cost-recommendations.md).

Další, najdete v části toolearn [Azure náklady správy pokyny](billing-getting-started.md).

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png 
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png 
