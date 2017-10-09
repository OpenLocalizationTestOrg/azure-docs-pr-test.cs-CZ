---
title: "aaaPrevent neočekávané náklady, spravovat fakturace - Azure | Microsoft Docs"
description: "Zjistěte, jak tooavoid neočekávané poplatky na faktury Azure. Pomocí funkce sledování nákladů a správy pro předplatné Microsoft Azure."
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/10/2017
ms.author: tonguyen
experimental_id: a2b2579c-cd2e-41
ms.openlocfilehash: 4827c65a55fe953c329ab26cc4e882266073c60a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prevent-unexpected-costs-with-azure-billing-and-cost-management"></a>Zabránit neočekávané náklady s Azure fakturace a náklady na správu

Při registraci v Azure je několik věcí, které můžete provést tooget lepší představu o vaší výdaji. V hello [portál Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), když vyberete hello předplatného, můžete zobrazit vaše aktuální rozdělení nákladů a vypálíte rychlost. Můžete také [stáhnout po faktury a soubory využití podrobností](billing-download-azure-invoice-daily-usage-date.md). Pokud chcete toogroup náklady pro prostředky používané pro různé projekty nebo týmy, podívejte se na [označování prostředků](../azure-resource-manager/resource-group-using-tags.md). Pokud má vaše organizace systém hlášení toouse dáváte přednost, podívejte se na hello [fakturace rozhraní API](billing-usage-rate-card-overview.md). 

Další informace o denní využití najdete v tématu [porozumět vaší faktuře pro Microsoft Azure](billing-understand-your-bill.md).

Pokud je vaše předplatné prostřednictvím Enterprise Agreement (EA), Cloud Solution Provider (CSP) nebo Azure Sponsorship, pak řadu funkcí v tomto článku se nevztahují tooyou. Místo toho máme jinou sadu nástrojů, které můžete použít pro náklady na správu. V tématu [další prostředky pro EA, CSP a sponzorství](#other-offers).

Pokud je vaše předplatné bezplatnou zkušební verzi, [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)Azure v otevřené (AIO) nebo BizSpark, pak další informace o [limitů útraty](#spending-limit) tooavoid s předplatného unexpectantly zakázána. 

## <a name="day-0-before-you-add-azure-services"></a>Den 0: Před přidáte služby Azure

### <a name="estimate-cost-online-using-hello-pricing-calculator"></a>Online odhad nákladů pomocí cenové kalkulačky hello

Podívejte se na hello [cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/) a [celkové náklady na vlastnictví kalkulačky](https://aka.ms/azure-tco-calculator) tooget odhad hello měsíční náklady na služby hello vás zajímá. Virtuální počítač A1 systému Windows (VM) je třeba odhadované toocost 66.96 USD za měsíc v výpočetní hodiny, pokud je necháte hello celou dobu spuštění:

![Snímek obrazovky zobrazující, že je virtuální počítač s Windows A1 odhadované toocost 66.96 USD měsíčně cenové kalkulačky hello](./media/billing-getting-started/pricing-calcVM.png)

Další informace najdete v tématu [ceny – nejčastější dotazy](https://azure.microsoft.com/pricing/faq/). Nebo pokud chcete, aby osoba tooa tootalk, volejte 1-800-867-1389.

### <a name="check-your-subscription-and-access"></a>Zkontrolujte předplatné a přístup

Náklady na zobrazení vyžadují [přístup na úrovni odběry toobilling informace](billing-manage-access.md), ale pouze správce účtu hello přístup hello [centra účtů](https://account.windowsazure.com/Home/Index), změňte fakturační informace a spravovat odběry. Dobrý den, správce účtu je hello osobě, která se prostřednictvím procesu registrace hello. Další informace najdete v tématu [přidání nebo změna role Správce služby Azure, které spravují předplatné hello nebo služby](billing-add-change-azure-subscription-administrator.md).

toosee, pokud jste hello účet správce, přejděte toohello [okno předplatná v portálu Azure hello](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) a prohlédněte si seznam hello odběry, které máte přístup. Podívejte se do části **Moje Role**. Pokud se říká *správce účtu*, pak jste ok. Pokud se říká něco jiného jako *vlastníka*, pak nemáte s úplnými právy.

![Snímek obrazovky vaše role při hello zobrazení odběry v hello portálu Azure](./media/billing-getting-started/sub-blade-view.PNG)

Pokud nejsou hello účet správce, pak někdo pravděpodobně vám Dal částečný přístup prostřednictvím [řízení přístupu na základě Role v Azure Active Directory](../active-directory/role-based-access-control-configure.md) (RBAC). toomanage odběry a změňte fakturační údaje, [najít Dobrý den, správce účtu](billing-subscription-transfer.md#whoisaa) a požádejte je tooperform hello úlohy nebo [přenosu hello předplatné tooyou](billing-subscription-transfer.md).

Pokud váš správce účtu je už ve vaší organizaci a potřebujete toomanage fakturace, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). 

### <a name="spending-limit"></a>Zkontrolujte, pokud máte limitu útraty a automaticky

Pokud máte předplatné, které používá kredity, pak hello útrat je pro vás ve výchozím nastavení zapnuta. Tímto způsobem, když tráví vaše kredity platební karty není získat účtovat. V tématu hello [úplný seznam Azure nabízí a dostupnost hello útrat](https://azure.microsoft.com/support/legal/offer-details/).

Pokud jste dosáhl svého limitu útraty, získat zakázána vašim službám. To znamená, že virtuální počítače jsou navrácena. výpadek služby tooavoid, musíte vypnout hello útrat. Získá všechny Nadlimitní účtovat na platební karty u souboru. 

toosee, pokud jste získali výdaje omezení na, přejděte toohello [odběry zobrazit v centru účtů hello](https://account.windowsazure.com/Subscriptions). Pokud limit útraty na se zobrazí nápis informující o:

![Snímek obrazovky, který zobrazí upozornění o výdaje limit používán hello centra účtů](./media/billing-getting-started/spending-limit-banner.PNG)

Klikněte na hlavičku hello a postupujte podle pokynů tooremove hello útrat. Pokud se informace o kreditní kartě nezadali jste při registraci, musíte ho zadat hello tooremove útrat. Další informace najdete v tématu [Azure výdaje omezit – jak funguje a jak tooenable nebo ji odeberte](https://azure.microsoft.com/pricing/spending-limits/).

### <a name="set-up-billing-alerts"></a>Nastavení upozornění fakturace

Nastavení fakturace výstrahy e-mailů tooget při náklady na použití překročit částku, kterou zadáte. Pokud máte měsíční kredity, nastavení oznámení pro při použití až po zadanou dobu. Další informace najdete v tématu [nastavit výstrahy pro vaše předplatné Microsoft Azure billing](billing-set-up-alerts.md).

![Snímek obrazovky fakturace výstrahy e-mailu](./media/billing-getting-started/billing-alert.png)

> [!NOTE]
> Tato funkce je stále ve verzi preview, takže byste měli zkontrolovat, pravidelně vašeho využití.

Můžete chtít toouse hello náklady odhad z hello cenová Kalkulačka funkcí jako vodítko pro první výstraha.

### <a name="understand-limits-and-quotas-for-your-subscription"></a>Pochopení omezení a kvóty pro vaše předplatné

Neexistují výchozí omezení tooeach předplatné pro takové věci, jako hello počet jader procesoru a IP adresy. Mějte na paměti tyto omezení. Další informace najdete v tématu [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md). Můžete požádat zvýšení limitu tooyour nebo kvóta podle [obrátíte na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="day-1-as-you-add-services"></a>Den 1: Jak přidat služby

### <a name="review-hello-estimated-cost-in-hello-portal"></a>Zkontrolujte hello odhadované náklady na portálu hello

Obvykle Pokud přidáte službu v hello portálu Azure, je zobrazení, které ukazuje, podobně jako odhadované náklady za měsíc. Například když zvolíte velikost virtuálního počítače Windows hello uvidíte, že hello odhadované měsíční náklady na výpočetní hodiny hello:

![Příklad: virtuální počítač A1 Windows je odhadované toocost 66.96 USD za měsíc](./media/billing-getting-started/vm-size-cost.PNG)

### <a name="tags"></a>Přidání značek tooyour prostředky toogroup fakturační údaje

Značky toogroup fakturační údaje můžete použít pro podporované služby. Například pokud spustíte několik virtuálních počítačů pro různé týmy, pak můžete značky toocategorize náklady nákladové středisko (HR, marketing, finance) nebo prostředí (test předprodukční, produkčním prostředí). 

![Snímek obrazovky, který ukazuje nastavení značky hello portálu](./media/billing-getting-started/tags.PNG)

značky Hello zobrazí v rámci různých náklady na vytváření sestav zobrazení. Například jsou viditelné v vaše [náklady analysis zobrazení](#costs) hned a [podrobností využití .csv](#invoice-and-usage) po vaší první fakturačního období.

Další informace najdete v tématu [pomocí značky tooorganize vašich prostředků Azure](../azure-resource-manager/resource-group-using-tags.md).

### <a name="consider-enabling-cost-cutting-features-like-auto-shutdown-for-vms"></a>Zvažte povolení náklady vyjímání funkcí, jako je automatické vypnutí pro virtuální počítače

V závislosti na scénáři může nakonfigurovat automatické vypnutí pro virtuální počítače v hello portálu Azure. Další informace najdete v tématu [automaticky vypnutí pro virtuální počítače pomocí Azure Resource Manager](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Snímek obrazovky možnost Automatické vypnutí hello portálu](./media/billing-getting-started/auto-shutdown.PNG)

Vypnutí automatického není hello stejné jako při vypínání v rámci hello virtuálních počítačů s možnosti napájení. Vypnutí automatického zastaví a zruší přidělení vaše virtuální počítače toostop náklady. Další informace najdete v tématu Nejčastější dotazy týkající se ceny [virtuální počítače s Linuxem](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) a [virtuálních počítačů Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) o stavech virtuálních počítačů.

Pro další náklady vyjímání funkce pro vývojové a testovací prostředí, podívejte se na [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

## <a name="cost-reporting"></a>Den 2 +: Po použití služeb zobrazení využití

### <a name="costs"></a>Pravidelně zkontrolujte hello portál pro rozdělení nákladů a vypálíte rychlost

Po získání vaší služby spuštěné pravidelně kontrolovat, kolik je jste nákladů. Zobrazí aktuální hello tráví a zápis míry v portálu Azure. 

1. Navštivte hello [okno předplatná na portálu Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

2. Vyberte předplatné chcete toosee. Může mít pouze jeden tooselect.

3. By měl zobrazit rozpis nákladů hello a zápis rychlost v místním okně hello. Nemusí být podporována pro vaši nabídku (upozornění se zobrazí v horní hello). Počkejte, než 24 hodin, po přidání služby pro hello data toopopulate.
    
    ![Snímek obrazovky s pracovní tempo a rozpis v hello portálu Azure](./media/billing-getting-started/burn-rate.PNG)

4. Můžete chtít toopin hello zobrazení tooyour řídicího panelu.

    ![Snímek obrazovky Připnutí na řídicí panel toohello zobrazení](./media/billing-getting-started/pin.PNG)

5. Klikněte na tlačítko **analýza nákladů** v hello seznamu toohello levém toosee hello náklady rozpis podle prostředků.

    ![Snímek obrazovky zobrazení analysis hello náklady na portálu Azure](./media/billing-getting-started/cost-analysis.PNG)

6. Můžete filtrovat podle různých vlastnosti, například [značky](#tags), skupinu prostředků a časový interval. Klikněte na tlačítko **použít** tooconfirm hello filtry a **Stáhnout** tooexport hello zobrazení tooa Comma-Separated souboru CSV (hodnoty).

7. Klikněte na prostředek toosee tráví historie a kolik ho byl výpočet nákladů můžete každý den.

    ![Snímek obrazovky hello tráví zobrazení historie na portálu Azure](./media/billing-getting-started/costhistory.PNG)

Doporučujeme zkontrolovat hello náklady, které vidíte hello odhady, které jste viděli, pokud jste vybrali hello služby. Pokud hello náklady velkým lišit od odhady, Překontrolujte hello cenový plán (vs A1 virtuální počítač A0, např.), který jste vybrali pro vaše prostředky. 

#### <a name="view-costs-for-all-your-subscriptions-in-hello-billing-blade"></a>Náklady na zobrazení pro všechna předplatná v okně fakturace hello

Pokud spravujete více předplatných jako hello účet správce, můžete zobrazit hello agregační fakturovaná částka a rozpis pro všechna předplatná v hello [fakturace okno](https://portal.azure.com/#blade/Microsoft_Azure_Billing/BillingBlade). 

<!-- Add screenshots of multiple subs each with billed usage -->

### <a name="turn-on-and-check-out-azure-advisor-recommendations"></a>Zapnout a podívejte se na doporučení služby Advisor Azure

[Azure Advisor](../advisor/advisor-overview.md) je funkce preview, která vám pomůže snížit náklady tím, že identifikujete zdroje s nízkou využití. Zapněte v hello portálu Azure:

![Snímek obrazovky Azure Advisor tlačítka na portálu Azure](./media/billing-getting-started/advisor-button.PNG)

Potom můžete získat řešitelné doporučení v hello **náklady** kartě hello Advisor řídicím panelu:

![Snímek obrazovky Advisor náklady na doporučení příklad](./media/billing-getting-started/advisor-action.PNG)

Další informace najdete v tématu [doporučení služby Advisor náklady](../advisor/advisor-cost-recommendations.md).

### <a name="invoice-and-usage"></a>Získá vaše faktura a podrobnosti o využití za vaše první fakturační období

Po vaší první fakturační období můžete si stáhnout faktura (PDF) Portable Document Format a podrobnosti o použití Comma-Separated hodnot (CSV). Můžete také zvolit v toohave faktury tooyou e-mailem. Tyto soubory pomoci toounderstand, co je nakonec fakturovaná tooyou po daně, slevy a kredity. Pokud nebyly platebních metoda připojené tooyour předplatné, může být pro vás k dispozici tyto soubory. Další informace najdete v tématu [jak tooget vaše Azure fakturace fakturace a dat o denním využití](billing-download-azure-invoice-daily-usage-date.md) a [porozumět vaší faktuře pro Microsoft Azure](billing-understand-your-bill.md).

![Snímek obrazovky .pdf faktury](./media/billing-getting-started/invoice.png)

Hello značky, které jste nastavili dříve zobrazí v soubory .csv využití hello podrobností:

![Snímek obrazovky zobrazující značky v hello využití .csv](./media/billing-getting-started/csv.png)

### <a name="billing-api"></a>Rozhraní API pro fakturaci

Použijte naše fakturace rozhraní API tooprogrammatically get data o využití. Pomocí vašeho fakturovaná využití hello RateCard rozhraní API a společně tooget hello využití rozhraní API. Další informace najdete v tématu [proniknout do vaší spotřeby prostředků Microsoft Azure](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a>Další prostředky pro EA a sponzorství CSP

Kontaktovat tooyour account manažera nebo partnera Azure tooget spuštěna.

| Nabídka | Zdroje |
|-------------------------------|-----------------------------------------------------------------------------------|
| Smlouva Enterprise Agreement (EA) | [Portál EA](https://ea.azure.com/), [pomoci dokumentace](https://ea.azure.com/helpdocs), a [sestavy Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Cloud Solution Provider (CSP) | Komunikovat tooyour zprostředkovatele |
| Sponzorství Azure | [Sponzorství portálu](https://www.microsoftazuresponsorships.com/) |

Pokud spravujete IT ve velkých organizacích doporučujeme čtení [vygenerované uživatelské rozhraní Azure enterprise](../azure-resource-manager/resource-manager-subscription-governance.md) a hello [enterprise IT dokumentu white paper](http://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (.pdf ke stažení, pouze v angličtině).

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Kontaktování podpory

Pokud potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém.
