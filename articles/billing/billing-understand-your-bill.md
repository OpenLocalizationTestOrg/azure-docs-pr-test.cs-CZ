---
title: "aaaUnderstand vaše fakturovaná částka u Azure | Microsoft Docs"
description: "Zjistěte, jak tooread a porozumět využívání a fakturovaná částka u předplatného Azure"
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/14/2017
ms.author: tonguyen
ms.openlocfilehash: a3195eb129b1576e8cb665aa6f88a1a2647edd78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-your-bill-for-microsoft-azure"></a>Vysvětlení vašeho vyúčtování služeb Microsoft Azure
toounderstand účtovat pošle váš Azure, porovnat faktury s hello soubor podrobné denní využití a hello náklady sestavy správy v hello portálu Azure.

tooobtain PDF části faktury a kopii podrobné denní využití souboru CSV stahování, najdete v části [získat vaše Azure fakturace faktury a denní data o využití](billing-download-azure-invoice-daily-usage-date.md). 

Podrobné podmínky a popisy faktury a podrobný soubor denní využití najdete v tématu [pochopit podmínky v Microsoft Azure faktury](billing-understand-your-invoice.md) a [Rady pro pochopení podmínky v Microsoft Azure podrobné využití](billing-understand-your-usage.md). 

Podrobnosti na hello náklady sestavy správy najdete v tématu [Správa nákladů na portálu Azure](https://docs.microsoft.com/en-us/azure/billing/billing-getting-started).


## <a name="charges"></a>Jak se ujistěte se, zda jsou informace správné hello poplatky v fakturou?
Pokud je na vaší faktuře, které chcete podrobnosti na zpoplatněny, existuje několik možností.

### <a name="option-1-review-your-invoice-and-compare-hello-usage-and-costs-with-hello-detailed-usage-csv-file"></a>Možnost 1: Zkontrolujte faktury a porovnání s hello podrobné hello využití a náklady na použití souboru CSV

Hello podrobné informace o použití souboru CSV zobrazuje vaše poplatky podle denní využití a fakturační období. tooget vaší podrobné informace o použití souboru CSV, najdete v části [získat vaše Azure fakturace faktury a denní data o využití](https://docs.microsoft.com/en-us/azure/billing/billing-download-azure-invoice-daily-usage-date).

Vaše poplatky za používání se zobrazí na úrovni měření hello. Hello následující termíny znamenají hello samé v obou hello faktury a hello podrobné informace o použití souboru. Například hello fakturační cyklus na faktuře hello je ekvivalentní toohello uvedené v souboru podrobné informace o použití hello fakturačního období.

 | Faktura (PDF) | Podrobné informace o použití (CSV)|
 | --- | --- |
|Fakturační cyklus | Fakturační období |
 |Name (Název) |Kategorie měření |
 |Typ |Měřicí dílčí kategorie |
 |Prostředek |Název měření |
 |Oblast |Oblast měření |
 |Spotřebované |Spotřebované množství |
 |Zahrnuje |Zahrnuté množství |
 |Fakturovatelné |Překročené množství |

Hello **poplatky za používání** části faktury má hello celkové hodnoty pro každé monitorování, která se spotřebovala během fakturačního období. Například hello následující snímek obrazovky ukazuje použití zdarma pro hello služby Azure Scheduler.

![Poplatky za používání faktury](./media/billing-understand-your-bill/1.png)

Hello **příkaz** část vaší podrobné využití CSV ukazuje hello stejné poplatků. Obě hello *Potřebováno* velikost a *hodnotu* shodu hello faktury.

![Poplatky za používání sdíleného svazku clusteru](./media/billing-understand-your-bill/2.png)

toosee rozpis tento poplatek za každý den, přejděte toohello **denní využití** části hello sdíleného svazku clusteru. Filtrovat "Scheduler" v části *měření kategorie* a zobrazí se použila, které měření hello dnů a kolik se spotřebovala. Hello *prostředků* a *skupiny prostředků* informace jsou také uvedeny pro porovnání. Hello *Potřebováno* hodnoty měli přidat až toowhat je zobrazena na faktuře hello.

![Denní využití kapitoly hello sdíleného svazku clusteru](./media/billing-understand-your-bill/3.png)

tooget hello náklady za den, vynásobte hello *Potřebováno* objemy s hello *míra* hodnotu z hello **příkaz** části.

toolearn Další informace o hello faktury, najdete v části [pochopit Azure faktury](billing-understand-your-invoice.md).

toolearn o jednotlivých sloupců hello v hello sdíleného svazku clusteru, najdete v části [porozumět Azure podrobné používání](billing-understand-your-invoice.md).

### <a name="option-2-review-your-invoice-and-compare-with-hello-usage-and-costs-in-hello-azure-portal"></a>Možnost 2: Přečtěte si faktury a porovnání s hello využití a náklady v hello portálu Azure

Hello portál Azure také můžete ověřit vaše poplatky. Hello portál Azure poskytuje náklady na správu grafy pro rychlý přehled o vaše využití a hello poplatky na faktury.

toocontinue s hello výše uvedeného příkladu, navštivte hello [předplatné](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), vyberte předplatné a potom zvolte **analýza nákladů**. Odtud můžete zadat období, hello a najdete v části poplatků využití služby Azure Scheduler hello.

![Zobrazení analýza nákladů na portálu Azure](./media/billing-understand-your-bill/4.png)

toosee hello denní náklady na rozpis v **náklady historie**, klikněte na řádek hello.

![Zobrazení historie náklady na portálu Azure](./media/billing-understand-your-bill/5.png)

Další, najdete v části toolearn [zabránit neočekávané náklady s Azure fakturace a náklady na správu](billing-getting-started.md#costs).

## <a name="external"></a>Co externí poplatky za služby?
Externích služeb (také označované jako Azure Marketplace objednávky) jsou k dispozici nezávislé služby dodavatelé a se účtují samostatně. poplatky za Hello nezobrazovat Azure faktury. Další, najdete v části toolearn [pochopit Azure externí poplatky za](billing-understand-your-azure-marketplace-charges.md).

## <a name="payment"></a>Jak lze vytvořit platba?

Pokud jste nastavili kreditní nebo debetní karty jako váš způsob platby, platebních hello je účtován automaticky do 10 dnů po skončení hello fakturačního období. Na příkazu platební karty, by vyslovení položky na řádku hello **MSFT Azure**.

Pokud jste [platíte podle fakturace](billing-how-to-pay-by-invoice.md), odeslání vaši polohu toohello platebních uvedený na hello spodní části faktury. Další pomoc [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="how-do-i-check-hello-status-of-a-payment-made-by-credit-card"></a>Jak zkontrolovat stav hello platba platební karty?

[Vytvořit lístek podpory](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooask hello stav platby. 

## <a name="tips-for-cost-management"></a>Tipy pro náklady na správu
- Odhad nákladů pomocí hello [cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/) a [celkové náklady na vlastnictví kalkulačky](https://aka.ms/azure-tco-calculator)a získat hello [podrobné informace o cenách pro každou službu](https://azure.microsoft.com/en-us/pricing/).
- [Nastavit výstrahy fakturace](billing-set-up-alerts.md).
- [Zkontrolujte využití a náklady na pravidelně v hello portál Azure](billing-getting-started.md#costs).

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.

Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém.
