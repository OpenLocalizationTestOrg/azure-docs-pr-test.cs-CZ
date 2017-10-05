---
title: "Zabránit neočekávané náklady, spravovat fakturace - Azure | Microsoft Docs"
description: "Zjistěte, jak neočekávané náklady na faktury Azure. Pomocí funkce sledování nákladů a správy pro předplatné Microsoft Azure."
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
ms.openlocfilehash: 5f9ae830da86a9d03f6d862d4adc7c99849d5014
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="prevent-unexpected-costs-with-azure-billing-and-cost-management"></a>Zabránit neočekávané náklady s Azure fakturace a náklady na správu

Při registraci v Azure existuje několik věcí, které vám pomohou získat lepší představu o vaší výdaji. V [portál Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), když vyberte předplatné, můžete zobrazit vaše aktuální rozdělení nákladů a vypálíte rychlost. Můžete také [stáhnout po faktury a soubory využití podrobností](billing-download-azure-invoice-daily-usage-date.md). Pokud chcete skupinu náklady pro prostředky používané pro různé projekty nebo týmy, podívejte se na [označování prostředků](../azure-resource-manager/resource-group-using-tags.md). Pokud má vaše organizace reporting systém, který byste radši chtěli použít, podívejte se [fakturace rozhraní API](billing-usage-rate-card-overview.md). 

Další informace o denní využití najdete v tématu [porozumět vaší faktuře pro Microsoft Azure](billing-understand-your-bill.md).

Pokud je vaše předplatné prostřednictvím Enterprise Agreement (EA), Cloud Solution Provider (CSP) nebo Azure Sponsorship, není na vás vztahovat řadu funkcí v tomto článku. Místo toho máme jinou sadu nástrojů, které můžete použít pro náklady na správu. V tématu [další prostředky pro EA, CSP a sponzorství](#other-offers).

Pokud je vaše předplatné bezplatnou zkušební verzi, [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)Azure v otevřené (AIO) nebo BizSpark, pak další informace o [limitů útraty](#spending-limit) -li se vyhnout předplatného unexpectantly zakázána. 

## <a name="day-0-before-you-add-azure-services"></a>Den 0: Před přidáte služby Azure

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Odhad náklady online pomocí cenové kalkulačky

Podívejte se [cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/) a [celkové náklady na vlastnictví kalkulačky](https://aka.ms/azure-tco-calculator) získat tak odhad měsíční náklady na služby, které vás zajímají. Například virtuální počítač A1 systému Windows (VM) by mělo náklady 66.96 USD za měsíc v výpočetní hodiny, pokud je necháte spuštěné po celou dobu:

![Snímek obrazovky zobrazující, že virtuální počítač A1 Windows by mělo náklady 66.96 USD měsíčně cenové kalkulačky](./media/billing-getting-started/pricing-calcVM.png)

Další informace najdete v tématu [ceny – nejčastější dotazy](https://azure.microsoft.com/pricing/faq/). Nebo pokud chcete, aby komunikoval s osoby, volejte 1-800-867-1389.

### <a name="check-your-subscription-and-access"></a>Zkontrolujte předplatné a přístup

Náklady na zobrazení vyžadují [přístup na úrovni odběry na fakturační informace](billing-manage-access.md), ale přístup pouze správce účtu [centra účtů](https://account.windowsazure.com/Home/Index), změňte fakturační informace a spravovat odběry. Správce účtu je osoba, která se prostřednictvím procesu registrace. Další informace najdete v tématu [přidání nebo změna role Správce služby Azure, které spravují předplatné nebo služby](billing-add-change-azure-subscription-administrator.md).

Pokud chcete zobrazit, pokud jste správce účtu, přejděte na [okno předplatná na portálu Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) a prohlédněte si seznam odběrů, máte přístup k. Podívejte se do části **Moje Role**. Pokud se říká *správce účtu*, pak jste ok. Pokud se říká něco jiného jako *vlastníka*, pak nemáte s úplnými právy.

![Snímek obrazovky vaše role při zobrazení odběry na portálu Azure](./media/billing-getting-started/sub-blade-view.PNG)

Pokud si nejste správce účtu, pak někdo pravděpodobně vám Dal částečný přístup prostřednictvím [řízení přístupu na základě Role v Azure Active Directory](../active-directory/role-based-access-control-configure.md) (RBAC). Správa předplatných a změnu fakturační údaje, [najít správce účtu](billing-subscription-transfer.md#whoisaa) a požádejte je o provádět úlohy nebo [převést předplatné vám](billing-subscription-transfer.md).

Pokud váš správce účtu je už ve vaší organizaci a potřebujete spravovat fakturace, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). 

### <a name="spending-limit"></a>Zkontrolujte, pokud máte limitu útraty a automaticky

Pokud máte předplatné, které používá kredity, pak limit útraty je pro vás ve výchozím nastavení zapnuta. Tímto způsobem, když tráví vaše kredity platební karty není získat účtovat. Najdete v článku [úplný seznam Azure nabízí a dostupnost útrat](https://azure.microsoft.com/support/legal/offer-details/).

Pokud jste dosáhl svého limitu útraty, získat zakázána vašim službám. To znamená, že virtuální počítače jsou navrácena. Výpadky služby, je nutné vypnout limitu útraty. Získá všechny Nadlimitní účtovat na platební karty u souboru. 

Pokud chcete zobrazit, pokud jste jste získali útrat na, přejděte na [odběry zobrazit v centru účtů](https://account.windowsazure.com/Subscriptions). Pokud limit útraty na se zobrazí nápis informující o:

![Snímek obrazovky, který zobrazí upozornění o výdaje limit používán centra účtů](./media/billing-getting-started/spending-limit-banner.PNG)

Klikněte na informační zprávě a postupujte podle výzev a odeberte limit útraty. Pokud se informace o kreditní kartě nezadali jste při registraci, musíte zadat ho odeberte limit útraty. Další informace najdete v tématu [Azure výdaje omezit – jak to funguje a jak povolit nebo ji odeberte](https://azure.microsoft.com/pricing/spending-limits/).

### <a name="set-up-billing-alerts"></a>Nastavení upozornění fakturace

Nastavení výstrah fakturace získat e-mailů, když vaše náklady na použití překročit částku, kterou zadáte. Pokud máte měsíční kredity, nastavení oznámení pro při použití až po zadanou dobu. Další informace najdete v tématu [nastavit výstrahy pro vaše předplatné Microsoft Azure billing](billing-set-up-alerts.md).

![Snímek obrazovky fakturace výstrahy e-mailu](./media/billing-getting-started/billing-alert.png)

> [!NOTE]
> Tato funkce je stále ve verzi preview, takže byste měli zkontrolovat, pravidelně vašeho využití.

Můžete chtít použít odhad náklady z cenové kalkulačky jako vodítko pro první výstraha.

### <a name="understand-limits-and-quotas-for-your-subscription"></a>Pochopení omezení a kvóty pro vaše předplatné

Existuje výchozí limity pro každé předplatné pro takové věci, jako počet jader procesoru a IP adresy. Mějte na paměti tyto omezení. Další informace najdete v tématu [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md). Může požádat o zvýšení limitu nebo kvóta podle [obrátíte na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="day-1-as-you-add-services"></a>Den 1: Jak přidat služby

### <a name="review-the-estimated-cost-in-the-portal"></a>Zkontrolujte odhadované náklady na portálu

Obvykle Pokud přidáte službu na portálu Azure, je zobrazení, které ukazuje, podobně jako odhadované náklady za měsíc. Například když zvolíte velikost virtuálního počítače Windows zobrazí odhadované měsíční náklady pro výpočetní hodiny:

![Příklad: virtuální počítač A1 Windows by mělo náklady 66.96 USD za měsíc](./media/billing-getting-started/vm-size-cost.PNG)

### <a name="tags"></a>Přidání značek k prostředkům na fakturační data seskupit

Značky, které skupiny fakturační údaje můžete použít pro podporované služby. Například pokud spustíte několik virtuálních počítačů pro různé týmy, pak můžete značek ke kategorizaci náklady nákladové středisko (HR, marketing, finance) nebo prostředí (test předprodukční, produkčním prostředí). 

![Snímek obrazovky, který ukazuje nastavení značky na portálu](./media/billing-getting-started/tags.PNG)

Značky zobrazí v rámci různých náklady na vytváření sestav zobrazení. Například jsou viditelné v vaše [náklady analysis zobrazení](#costs) hned a [podrobností využití .csv](#invoice-and-usage) po vaší první fakturačního období.

Další informace najdete v tématu [použití značek k uspořádání prostředků Azure](../azure-resource-manager/resource-group-using-tags.md).

### <a name="consider-enabling-cost-cutting-features-like-auto-shutdown-for-vms"></a>Zvažte povolení náklady vyjímání funkcí, jako je automatické vypnutí pro virtuální počítače

V závislosti na scénáři může nakonfigurovat automatické vypnutí pro virtuální počítače na portálu Azure. Další informace najdete v tématu [automaticky vypnutí pro virtuální počítače pomocí Azure Resource Manager](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Snímek obrazovky možnost Automatické vypnutí na portálu](./media/billing-getting-started/auto-shutdown.PNG)

Vypnutí automatického není stejný jako při vypnutí virtuálního počítače pomocí možnosti napájení. Vypnutí automatického zastaví a zruší přidělení virtuální počítače k zastavení náklady. Další informace najdete v tématu Nejčastější dotazy týkající se ceny [virtuální počítače s Linuxem](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) a [virtuálních počítačů Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) o stavech virtuálních počítačů.

Pro další náklady vyjímání funkce pro vývojové a testovací prostředí, podívejte se na [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

## <a name="cost-reporting"></a>Den 2 +: Po použití služeb zobrazení využití

### <a name="costs"></a>Pravidelně zkontrolujte na portálu a rozdělení nákladů a vypálíte rychlost

Po získání vaší služby spuštěné pravidelně kontrolovat, kolik je jste nákladů. Můžete zobrazit aktuální výdaji a zápis míry v portálu Azure. 

1. Přejděte [okno předplatná na portálu Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

2. Vyberte předplatné, které chcete zobrazit. Může mít pouze jednu vybrat.

3. By měl zobrazit rozpis nákladů a zápis rychlost v místním okně. Nemusí být podporována pro vaši nabídku (upozornění se zobrazí v horní). Počkejte, než 24 hodin, po přidání služby pro data k naplnění.
    
    ![Snímek obrazovky s pracovní tempo a rozdělení na portálu Azure](./media/billing-getting-started/burn-rate.PNG)

4. Můžete chtít připnout zobrazení řídicího panelu.

    ![Snímek obrazovky Připnutí na řídicí panel zobrazení](./media/billing-getting-started/pin.PNG)

5. Klikněte na tlačítko **analýza nákladů** v seznamu nalevo zobrazit rozpis nákladů prostředkem.

    ![Snímek obrazovky zobrazení analýza nákladů na portálu Azure](./media/billing-getting-started/cost-analysis.PNG)

6. Můžete filtrovat podle různých vlastnosti, například [značky](#tags), skupinu prostředků a časový interval. Klikněte na tlačítko **použít** potvrďte filtry a **Stáhnout** zobrazení exportovat do souboru Comma-Separated hodnot (CSV).

7. Klikněte na prostředek zobrazíte tráví historie a kolik ho byl výpočet nákladů můžete každý den.

    ![Snímek obrazovky zobrazení historie výdaji na portálu Azure](./media/billing-getting-started/costhistory.PNG)

Doporučujeme zkontrolovat náklady, které se zobrazí s odhad, které jste viděli, pokud jste vybrali služby. Pokud náklady na velkým liší od odhady, Překontrolujte cenový plán (vs A1 virtuální počítač A0, např.), který jste vybrali pro vaše prostředky. 

#### <a name="view-costs-for-all-your-subscriptions-in-the-billing-blade"></a>Náklady na zobrazení pro všechna předplatná v okně fakturace

Pokud spravujete více předplatných jako účet správce, můžete zobrazit agregační fakturovaná částka a rozpis pro všechna předplatná v [fakturace okno](https://portal.azure.com/#blade/Microsoft_Azure_Billing/BillingBlade). 

<!-- Add screenshots of multiple subs each with billed usage -->

### <a name="turn-on-and-check-out-azure-advisor-recommendations"></a>Zapnout a podívejte se na doporučení služby Advisor Azure

[Azure Advisor](../advisor/advisor-overview.md) je funkce preview, která vám pomůže snížit náklady tím, že identifikujete zdroje s nízkou využití. Aktivujte ji na portálu Azure:

![Snímek obrazovky Azure Advisor tlačítka na portálu Azure](./media/billing-getting-started/advisor-button.PNG)

Potom můžete získat řešitelné doporučení **náklady** kartě v řídicím panelu Advisor:

![Snímek obrazovky Advisor náklady na doporučení příklad](./media/billing-getting-started/advisor-action.PNG)

Další informace najdete v tématu [doporučení služby Advisor náklady](../advisor/advisor-cost-recommendations.md).

### <a name="invoice-and-usage"></a>Získá vaše faktura a podrobnosti o využití za vaše první fakturační období

Po vaší první fakturační období můžete si stáhnout faktura (PDF) Portable Document Format a podrobnosti o použití Comma-Separated hodnot (CSV). Můžete můžete také zvolit, zda má vaše faktura vám e-mailem. Tyto soubory pomoci zjistit, co se nakonec fakturuje vám po daně, slevy a kredity. Pokud nebyly způsob platby připojené k vašemu předplatnému, může být pro vás k dispozici tyto soubory. Další informace najdete v tématu [jak získat vaše Azure fakturace faktury a denní data o využití](billing-download-azure-invoice-daily-usage-date.md) a [porozumět vaší faktuře pro Microsoft Azure](billing-understand-your-bill.md).

![Snímek obrazovky .pdf faktury](./media/billing-getting-started/invoice.png)

Značky, které jste nastavili dříve zobrazí v souborech CSV podrobnosti o použití:

![Snímek obrazovky zobrazující značky v CSV využití](./media/billing-getting-started/csv.png)

### <a name="billing-api"></a>Rozhraní API pro fakturaci

Pomocí našich fakturace rozhraní API prostřednictvím kódu programu získat data o využití. Použijte RateCard rozhraní API a rozhraní API využití společně k získání vašeho fakturovaná využití. Další informace najdete v tématu [proniknout do vaší spotřeby prostředků Microsoft Azure](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a>Další prostředky pro EA a sponzorství CSP

Obraťte se na vašeho account manažera nebo partnera Azure, abyste mohli začít.

| Nabídka | Zdroje |
|-------------------------------|-----------------------------------------------------------------------------------|
| Smlouva Enterprise Agreement (EA) | [Portál EA](https://ea.azure.com/), [pomoci dokumentace](https://ea.azure.com/helpdocs), a [sestavy Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Cloud Solution Provider (CSP) | Obraťte se na svého poskytovatele |
| Sponzorství Azure | [Sponzorství portálu](https://www.microsoftazuresponsorships.com/) |

Pokud spravujete IT ve velkých organizacích doporučujeme čtení [vygenerované uživatelské rozhraní Azure enterprise](../azure-resource-manager/resource-manager-subscription-governance.md) a [enterprise IT dokumentu white paper](http://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (.pdf ke stažení, pouze v angličtině).

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Kontaktování podpory

Pokud potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.
