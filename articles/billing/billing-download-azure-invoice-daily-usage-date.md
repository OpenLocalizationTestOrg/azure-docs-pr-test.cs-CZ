---
title: "aaaDownload Azure fakturace faktury a denní data o využití | Microsoft Docs"
description: "Popisuje, jak toodownload nebo zobrazení vaší Azure fakturace faktury a dat o denním využití."
keywords: "fakturace faktury, stažení faktury, faktury azure, azure využití"
services: 
documentationcenter: 
author: genlin
manager: tonguyen
editor: 
tags: billing
ms.assetid: 6d568d1d-3bd6-4348-97d0-1098b5fe0661
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2826df10f39914fcaeb9985271dadde550c68dfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>Stažení nebo zobrazení Azure fakturace faktury a dat o denním využití
Vaše faktura si můžete stáhnout z hello [portál Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) nebo jej odeslat e-mailem. toodownload denní využití, přejděte toohello [centra účtů Azure](https://account.windowsazure.com). Pouze některé role mají oprávnění tooget fakturace faktury a využití informace, jako je hello správce účtu. toolearn Další informace o získání informací toobilling přístup, najdete v části [spravovat přístup tooAzure fakturace použití rolí](billing-manage-access.md).

## <a name="get-your-invoice-in-email-pdf"></a>Získat faktury v e-mailu (PDF)
Můžete vyjádřit výslovný souhlas a nakonfigurovat další příjemce tooreceive vaše Azure fakturovat e-mailem. Tato funkce nemusí být dostupné pro určité odběry například nabízí podporu, smlouvy Enterprise nebo Azure v otevřené.

1. Vyberte své předplatné z hello [odběry okno](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Výslovný souhlas pro každé předplatné, které vlastníte. Klikněte na tlačítko **faktury** pak **e-mailu fakturou**. 

    ![Snímek obrazovky, který zobrazuje hello výslovný souhlas tok](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)
    
2. Klikněte na tlačítko **vyjádřit výslovný souhlas** a přijměte podmínky hello.

    ![Snímek obrazovky, který zobrazuje hello výslovný souhlas tok](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. Jakmile jste přijatá hello smlouvy, můžete nakonfigurovat další příjemce.

    ![Snímek obrazovky, který zobrazuje hello výslovný souhlas tok](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)
    
Pokud neobdržíte e-mail po hello postupem, ujistěte se, e-mailová adresa je správný v hello [předvolby komunikace na váš profil](https://account.windowsazure.com/profile).

## <a name="download-invoice-from-azure-portal-pdf"></a>Stahovat faktury z portálu Azure (PDF)

1. Vyberte své předplatné z hello [odběry okno](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) na portálu Azure jako [uživatelé s přístup tooinvoices](billing-manage-access.md).

2. Vyberte **faktury**. 

    ![Snímek obrazovky, který ukazuje možnost využití & fakturace hello](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

3. Klikněte na tlačítko **stažení faktury** tooview kopii faktury PDF. Pokud se říká **není k dispozici**, najdete v části [Proč nevidím faktury pro hello poslední fakturační období?](#noinvoice)

    ![Snímek obrazovky, který zobrazuje fakturační období, možnost stažení hello a celkové náklady pro každé fakturační období](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Denní využití můžete také zobrazit kliknutím hello fakturačního období. 

Další informace o vaší faktuře najdete v tématu [porozumět vaší faktuře pro Microsoft Azure](billing-understand-your-bill.md). Správa nákladů pomoc najdete v tématu [zabránit neočekávané náklady s Azure fakturace a náklady na správu](billing-getting-started.md).

## <a name="download-usage-from-hello-account-center-csv"></a>Stáhnout využití z hello centra účtů (CSV)

1. Přihlaste se k hello [centra účtů Azure](https://account.windowsazure.com/subscriptions) jako hello správce účtu.

2. Vyberte hello předplatné, pro které chcete informace o hello faktury a využití.

3. Vyberte **HISTORIE FAKTURACE**. 

    ![Snímek obrazovky, který ukazuje možnost historie fakturace](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. Můžete zobrazit vaše příkazy pro hello poslední šesti fakturační období a hello aktuální nefakturovaný období. 

    ![Snímek obrazovky, který zobrazuje fakturační období, možnosti toodownload faktury a denní využití a celkové náklady pro každé fakturační období](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. Vyberte **zobrazení aktuální příkaz** toosee odhad vaše poplatky v hello čas hello odhad byl vygenerován. Tyto informace se pouze denně aktualizují a nemusí obsahovat všechny využití. Měsíční faktury může lišit od tento odhad.

    ![Snímek obrazovky, který ukazuje možnost zobrazení aktuální příkaz hello](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Snímek obrazovky zobrazující hello odhad aktuální poplatky](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. Vyberte **stáhnout využití** toodownload hello dat o denním využití do souboru CSV. Pokud se zobrazují dvě verze, které jsou k dispozici, stáhněte si verze 2.

    ![Snímek obrazovky, který ukazuje možnost stáhnout využití hello](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

Hello pouze správce účtu mají přístup k centru účtů Azure hello. Jiní správci fakturace, jako je vlastníkem, můžete získat informace o využití pomocí hello [fakturace rozhraní API](billing-usage-rate-card-overview.md).

Další informace o denní využití najdete v tématu [porozumět vaší faktuře pro Microsoft Azure](billing-understand-your-bill.md). Správa nákladů pomoc najdete v tématu [zabránit neočekávané náklady s Azure fakturace a náklady na správu](billing-getting-started.md).

## <a name="noinvoice"></a>Proč nevidím faktury pro hello poslední fakturační období?

Je možné, že nevidíte faktury z několika důvodů:

- Máte ve vašem předplatném, které nebylo delší než měsíční částku Dal nebo máte bezplatnou zkušební verzi. Faktury se vygeneruje pouze tehdy, když dlužíte peníze.

- Je menší než 30 dní od odebíráte tooAzure den hello.

- Hello faktury ještě nevygenerovala. Počkejte, až hello konce fakturačního období hello.

- Pokud si nejste hello účet správce, nemusí být starší faktury avaialbe tooyou.

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud máte další otázky, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém.

