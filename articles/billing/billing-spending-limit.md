---
title: "Pochopení Azure útrat | Microsoft Docs"
description: "Popisuje, jak funguje Azure útrat a jak ho odebrat"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: genli
ms.openlocfilehash: a2743ef34bde0faabb3afd2ace27acddd59d3d70
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="understand-azure-spending-limit-and-how-to-remove-it"></a>Pochopení omezení a jak ho odebrat útraty u Azure

Limit útraty u Azure je omezena na kolik můžete tráví vaše předplatné Azure. Všechny nové zákazníci, kteří si zaregistrovat zkušební nabídku nebo nabídky, které zahrnuje kredity přes několik měsíců mají limit útraty ve výchozím nastavení zapnuto. Limit útraty je $0. Nelze změnit. Tento limit útraty není dostupný pro typy předplatného, jako jsou předplatná s průběžnými platbami a plány závazků. Najdete v článku [úplný seznam Azure nabízí a dostupnost limit útraty](https://azure.microsoft.com/support/legal/offer-details/).

## <a name="what-happens-when-i-reach-the-spending-limit"></a>Co se stane, když se dosáhne limitu útraty?

Při použití výsledky v poplatky, které vyčerpat měsíční množství zahrnutý do vaší nabídky, jsou zakázané služby, které jste nasadili pro zbytek tohoto měsíce fakturace deaktivujeme. Například se přestane provozovat služba Cloud Services, kterou jste nasadili, a virtuální počítače Azure se zastaví a zruší se jejich přidělení. Pokud chcete zabránit případné deaktivaci služeb, můžete se rozhodnout limit útraty odebrat. Pokud vaše služby jsou zakázané, data v účtech úložiště a databáze jsou k dispozici způsobem jen pro čtení pro správce. Na začátku dalšího měsíce fakturace, pokud vaši nabídku zahrnuje kredity přes několik měsíců, předplatné bude znovu zapnout. Pak můžete znovu nasadit cloudové služby a mají plný přístup ke své účty úložiště a databáze.

Po skončení zkušební verze dosáhne limitu útraty, můžete znovu povolit odběr a má automaticky [upgradovat na naše standardní nabídka průběžné platby](billing-upgrade-azure-subscription.md) do 90 dnů.

Oznámení se zobrazí při dosáhl limitu útraty pro vaši nabídku. Přihlaste se k [centra účtů Azure](https://account.windowsazure.com), vyberte **účet**a potom vyberte **odběry**. Zobrazí oznámení o odběry, které bylo dosaženo limitu útraty.

## <a name="things-you-are-charged-for-even-if-you-have-a-spending-limit-enabled"></a>Věcí, které vám budou účtovat i v případě, že máte limitu útraty a automaticky povoleno

Některé služby Azure a [Marketplace zakoupí](https://azure.microsoft.com/marketplace/) může platit poplatky v části způsob platby (kopie) i v případě, že je nastaven limit útraty. Příklady jsou licence Visual studio, Azure Active Directory premium, plánům podpory a většina třetích stran značky služby za účelem prodeje přes Marketplace.


## <a name="when-not-to-use-the-spending-limit"></a>Kdy limit útraty nepoužívat

Limit útraty vám může bránit v nasazení nebo použití některých služeb webu Marketplace a společnosti Microsoft. Tady jsou scénáře, kdy byste měli limit útraty pro svoje předplatné odebrat.

- Chcete nasadit image Microsoftu, třeba Oracle, a služby, jako je Visual Studio Team Services. V tomto scénáři způsobí, že je delší než limit útraty téměř okamžitě a způsobí, že vaše předplatné se zakáže.

- Používáte ale služby, které nejde přerušit.

- Nechcete ztratit nastavení služeb a prostředků, jako jsou virtuální IP adresy. Tato nastavení jsou ztraceny, když jsou navrácena službám a prostředkům.


## <a name="remove-the-spending-limit"></a>Odebrání limitu útraty

Limit útraty můžete odebrat kdykoli za předpokladu, že je k vašemu předplatnému přidružen platný způsob platby. U nabídek s kreditem na několik měsíců můžete limit útraty také znovu aktivovat na začátku dalšího fakturačního cyklu.

Pokud chcete odebrat limit útraty, postupujte takto:

1. Přihlaste se na [centra účtů Azure](https://account.windowsazure.com).

2. Vyberte předplatné.

3. Pokud je předplatné vyčerpané, klikněte na toto oznámení: U vašeho předplatného jste dosáhli limitu útraty a automaticky se deaktivovalo, aby vám nezačaly nabíhat poplatky. Jinak, klikněte na tlačítko **odebrat útrat** v **stav PŘEDPLATNÉHO** oblasti.

4. Vyberte vhodnou možnost.

|Možnost|Efekt|
|-------|-----|
|Odebrat limit útraty neomezeně|Odebere limit útraty. Na začátku dalšího fakturačního období se automaticky neaktivuje.|
|Odebrat limit útraty pro aktuální fakturační období|Odebere limit útraty. Na začátku dalšího fakturačního období se automaticky znovu aktivuje.|

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.
