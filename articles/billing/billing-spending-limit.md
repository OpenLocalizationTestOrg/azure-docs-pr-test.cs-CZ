---
title: "omezit výdaje Azure aaaUnderstand | Microsoft Docs"
description: "Popisuje, jak Azure výdaje omezit funguje a jak tooremove ho"
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
ms.openlocfilehash: ed01401a07c3d0e7edebe42fb1482b7b60b1df51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-spending-limit-and-how-tooremove-it"></a>Pochopení limit útraty u Azure a jak tooremove ho

Limit útraty u Azure je omezena na kolik můžete tráví vaše předplatné Azure. Všechny nové zákazníky, kteří si zaregistrovat zkušební nabídku hello nebo nabídky, které zahrnuje kredity přes několik měsíců mít hello útrat ve výchozím nastavení zapnuto. Hello útrat je $0. Nelze změnit. Hello útrat není k dispozici pro typy předplatného například průběžné platby odběry a plány závazků. V tématu hello [úplný seznam Azure nabízí a dostupnost hello hello útrat](https://azure.microsoft.com/support/legal/offer-details/).

## <a name="what-happens-when-i-reach-hello-spending-limit"></a>Co se stane, pokud mám dosáhnou hello útrat?

Pokud vaše využití výsledkem poplatky, které vyčerpat hello měsíční množství zahrnutý do vaší nabídky, jsou zakázaná hello služby, které jste nasadili pro hello zbytek tohoto měsíce fakturace deaktivujeme. Například se přestane provozovat služba Cloud Services, kterou jste nasadili, a virtuální počítače Azure se zastaví a zruší se jejich přidělení. tooprevent vašimi službami z zakázaná, můžete zvolit tooremove svého limitu útraty. Pokud vaše služby jsou zakázané, hello data ve své účty úložiště a databáze jsou k dispozici způsobem jen pro čtení pro správce. Na hello začátku hello dalšího měsíce fakturace, pokud vaši nabídku zahrnuje kredity přes několik měsíců, předplatné bude znovu zapnout. Pak můžete znovu nasadit cloudové služby a účty úložiště tooyour plný přístup a databází.

Po bezplatné předplatné zkušební verze hello dosáhne hello útrat, můžete znovu povolit hello předplatného a má automaticky [standardní nabídku průběžných plateb upgradu tooour](billing-upgrade-azure-subscription.md) do 90 dnů.

Oznámení se zobrazí při průchodu hello útrat pro vaši nabídku. Přihlaste se toohello [centra účtů Azure](https://account.windowsazure.com), vyberte **účet**a potom vyberte **odběry**. Zobrazí oznámení o odběry, které bylo dosaženo omezení výdajů hello.

## <a name="things-you-are-charged-for-even-if-you-have-a-spending-limit-enabled"></a>Věcí, které vám budou účtovat i v případě, že máte limitu útraty a automaticky povoleno

Některé služby Azure a [Marketplace zakoupí](https://azure.microsoft.com/marketplace/) může platit poplatky za hello platby (kopie) i v případě, že je nastaven limit útraty. Příklady jsou licence Visual studio, Azure Active Directory premium, plánům podpory a většina značky služby za účelem prodeje prostřednictvím hello Marketplace třetích stran.


## <a name="when-not-toouse-hello-spending-limit"></a>Pokud není toouse hello limitu útraty

Hello útrat může zabránit vám v nasazení nebo pomocí určitých marketplace a služby společnosti Microsoft. Tady jsou hello scénáře, kde byste měli odebrat hello útrat vaše předplatné.

- Máte v plánu toodeploy první strany Image, jako je Oracle a službám, jako je Visual Studio Team Services. Tento scénář vám způsobí tooexceed výdajů omezit téměř okamžitě a způsobí, že vaše předplatné toobe zakázána.

- Používáte ale služby, které nejde přerušit.

- Máte služby a prostředky s nastavením jako virtuální IP adresy, které nechcete, aby toolose. Tato nastavení jsou ztraceny, když jsou navrácena hello službám a prostředkům.


## <a name="remove-hello-spending-limit"></a>Odebrat hello útrat

Můžete odebrat hello útrat kdykoli, dokud není platný platby spojené s vaším předplatným. Nabídky, které mají platební přes několik měsíců můžete také znovu povolit hello útrat na začátku hello v příštím účtovacím období.

tooremove výdajů omezit, postupujte takto:

1. Přihlaste se toohello [centra účtů Azure](https://account.windowsazure.com).

2. Vyberte předplatné.

3. Pokud hello předplatné je zakázané kvůli toohello dosažení limitu útraty, klikněte na toto oznámení: "Předplatné dosaženo hello limitu útraty a bylo zakázáno tooprevent poplatky." Jinak, klikněte na tlačítko **odebrat útrat** v hello **stav PŘEDPLATNÉHO** oblasti.

4. Vyberte vhodnou možnost.

|Možnost|Efekt|
|-------|-----|
|Odebrat limit útraty neomezeně|Odebere hello útrat bez zapnutí automaticky při spuštění hello hello další fakturačního období.|
|Odeberte limit útraty pro aktuální fakturační období hello|Odebere hello útrat tak, aby se změní zpět automaticky při spuštění hello hello další fakturačního období.|

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém.
