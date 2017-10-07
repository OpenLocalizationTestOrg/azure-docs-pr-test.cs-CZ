---
title: "cílení na aaaSolution v OMS | Microsoft Docs"
description: "Cílení na řešení je funkce v Operations Management Suite (OMS), který vám umožní toolimit správu řešení tooa konkrétní sadu agentů.  Tento článek popisuje, jak toocreate konfigurace oboru a použijte ho tooa řešení."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1f054a4e-6243-4a66-a62a-0031adb750d8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: 6f8c8109e0d9e282e18724bf8b673b10de8e498a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-solution-targeting-in-operations-management-suite-oms-tooscope-management-solutions-toospecific-agents-preview"></a>Použít řešení cílení v Operations Management Suite (OMS) tooscope agenti pro toospecific řešení pro správu (Preview)
Když přidáte tooOMS řešení, je automaticky nasadit ve výchozí tooall Windows a Linux agenty připojené tooyour pracovní prostor analýzy protokolů.  Řešení můžete chtít toomanage vaše náklady a limit hello množství dat shromažďovaných omezením tooa konkrétní sadu agenty.  Tento článek popisuje, jak toouse **cílení na řešení** což je funkce OMS, která vám umožní tooapply obor tooyour řešení.

## <a name="how-tootarget-a-solution"></a>Jak tootarget řešení
Existují tři kroky tootargeting řešení jak je popsáno v následující části hello.  Všimněte si, že budete potřebovat portálu OMS hello i hello portál Azure pro jiný postup.


### <a name="1-create-a-computer-group"></a>1. Vytvořit skupinu počítačů
Zadejte hello počítače chcete tooinclude v oboru tak, že vytvoříte [skupinu počítačů](../log-analytics/log-analytics-computer-groups.md) v analýzy protokolů.  Skupina počítačů Hello lze podle protokolu hledání nebo importovat z jiných zdrojů, například služby Active Directory nebo skupiny služby WSUS. Jako [popsané níže](#solutions-and-agents-that-cant-be-targeted), pouze počítače, které jsou přímo připojené tooLog analýzy budou zahrnuty v oboru hello.

Jakmile máte hello skupiny počítačů v pracovním prostoru, budete jej zahrnout do konfigurace oboru, který může být použité tooone nebo další řešení.
 
 
 ### <a name="2-create-a-scope-configuration"></a>2. Vytvoření konfigurace oboru
 A **konfigurace oboru** obsahuje jeden nebo více skupin počítačů a může být použité tooone nebo další řešení. 
 
 Vytvořte obor konfiguraci pomocí následující proces hello.  

 1. V hello portálu Azure, přejděte příliš**analýzy protokolů** a vyberte pracovní prostor.
 2. Ve vlastnostech hello pro hello prostoru v **zdroje dat pracovního prostoru** vyberte **konfigurace oboru**.
 3. Klikněte na tlačítko **přidat** toocreate novou konfiguraci oboru.
 4. Zadejte **název** pro konfiguraci rozsahu hello.
 5. Klikněte na tlačítko **vyberte skupiny počítačů**.
 6. Vyberte skupiny počítačů hello, kterou jste vytvořili a volitelně ostatní skupiny tooadd toohello konfigurace.  Klikněte na **Vybrat**.  
 6. Klikněte na tlačítko **OK** konfigurace oboru toocreate hello. 


 ### <a name="3-apply-hello-scope-configuration-tooa-solution"></a>3. Použijte hello oboru konfigurace tooa řešení.
Jakmile je konfigurace oboru, pak můžete použít ho tooone nebo další řešení.  Všimněte si, že při konfiguraci jeden obor lze použít s více řešení, každé řešení můžete použít pouze jednu konfiguraci oboru.

Použití konfigurace oboru pomocí postupu hello.  

 1. V hello portálu Azure, přejděte příliš**analýzy protokolů** a vyberte pracovní prostor.
 2. Ve vlastnostech hello pro hello prostoru vyberte **řešení**.
 3. Klikněte na řešení hello chcete tooscope.
 4. Ve vlastnostech hello hello řešení v části **zdroje dat pracovního prostoru** vyberte **cílení na řešení**.  Pokud není k dispozici možnost hello pak [toto řešení nemůžou být cílem](#solutions-and-agents-that-cant-be-targeted).
 5. Klikněte na tlačítko **konfigurace oboru přidat**.  Pokud již máte řešení toothis použitá konfigurace tato možnost nebude k dispozici.  Je nutné odebrat existující konfiguraci hello před přidáním jiný.
 6. Klikněte na konfigurace hello oboru, který jste vytvořili.
 7. Kukátko hello **stav** z tooensure konfigurace hello, že se zobrazí **úspěšné**.  Pokud stav hello označuje chybu, pak klikněte na hello elipsy toohello vpravo od hello konfigurace a vyberte **konfiguraci úpravy rozsahu** toomake změny.

## <a name="solutions-and-agents-that-cant-be-targeted"></a>Řešení a agenty, kteří nemůžou být cílem
Následují hello kritéria pro agenty a řešení, které nelze použít s cílem řešení.

- Cílení na řešení, vztahuje se pouze toosolutions, který nasadit tooagents.
- Cílení na řešení, vztahuje se pouze toosolutions od společnosti Microsoft.  Nevztahuje se toosolutions [vytvořené sami nebo partnery](operations-management-suite-solutions-creating.md).
- Můžete filtrovat pouze na agenty, které se připojují přímo tooLog Analytics.  Řešení automaticky nasadí tooany agentů, které jsou součástí připojené skupiny pro správu nástroje Operations Manager, jestli jsou zahrnuté v konfiguraci oboru.

### <a name="exceptions"></a>Výjimky
Cílení na řešení nelze použít s hello následující řešení, i když se vešly hello uvedená kritéria.

- Vyhodnocení stavu agenta

## <a name="next-steps"></a>Další kroky
- Další informace o řešení pro správu včetně hello řešení, které jsou k dispozici tooinstall ve vašem prostředí v [pracovní prostor analýzy protokolů Azure přidat management řešení tooyour](../log-analytics/log-analytics-add-solutions.md).
- Další informace o vytváření skupin počítačů v [skupiny počítačů v analýzy protokolů protokolu hledání](../log-analytics/log-analytics-computer-groups.md).