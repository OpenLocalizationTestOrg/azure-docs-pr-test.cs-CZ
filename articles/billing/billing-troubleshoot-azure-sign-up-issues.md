---
title: "aaaTroubleshoot Azure registrace problémy | Microsoft Docs"
description: "Popisuje, jak tootroubleshoot některé běžné Azure zaregistrovat problémy."
services: 
documentationcenter: 
author: JiangChen79
manager: adpick
editor: 
tags: billing,top-support-issue
ms.assetid: a0907da1-cb2d-41d1-a97f-43841fabe355
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ee649bfc3be7ba1fe2dd863fac09e1c2311d835b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-sign-up-issues-for-azure"></a>Řešení problémů registrace pro Azure
Pokud nelze registrace ke službě Azure, použijte hello tipy v tento článek tootroubleshoot běžné problémy. Pokud máte potíže s platební karty během registrace, viz [debetní karty nebo platební karty je odmítnuta na Azure registrace](billing-credit-card-fails-during-azure-sign-up.md). Pokud máte účet Azure, ale nemůžete se přihlásit, přečtěte si téma [nelze se přihlásit toomanage předplatného Azure](billing-cannot-login-subscription.md).

## <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>Indikátor průběhu přestane reagovat v části "Ověření Identity pomocí karty"

ověření identity toocomplete hello pomocí karty, musí být soubory cookie třetích stran povoleno pro prohlížeč.

![Snímek obrazovky hello ověření Identity karty oddílu hanging během registrace](./media/billing-troubleshoot-azure-sign-up-issues/identity-verification-hangs.PNG)

Pomocí následujících kroků tooupdate hello nastavení souborů cookie v prohlížeči.

1. Pokud používáte Chrome, přejděte příliš**nastavení** > **zobrazit upřesňující nastavení** > **o ochraně osobních údajů** > **obsahu nastavení**. Zrušte zaškrtnutí políčka **blokovat soubory cookie třetích stran a data lokality**.
2. Pokud používáte Edge, přejděte příliš**nastavení** > **zobrazení upřesňující nastavení** > **soubory cookie**. Vyberte **nedošlo k blokování souborů cookie**.
3. Aktualizovat hello Azure stránku pro přihlášení a zkontrolujte, zda text hello problém vyřešen.
4. Pokud aktualizace hello nebyla hello problém vyřešit, ukončete a restartujte prohlížeč a akci opakujte.

## <a name="credit-card-form-doesnt-support-my-billing-address"></a>Formulář platební karty nepodporuje fakturační adresa
Fakturační adresu musí toobe v hello země, který jste vybrali v hello **o vás** části. Ujistěte se, zda že jste vybrali správnou zemi hello.

## <a name="no-text-messages-or-calls-during-sign-up-account-verification"></a>Žádné textové zprávy nebo volání během ověření registrace účtu
I když je obvykle mnohem rychlejší, může trvat až minut toofour pro ověření kódu toobe doručit. Hello telefonní číslo, které zadáte pro ověření není uložena jako kontaktní číslo pro účet hello.

Zde jsou některé další tipy:
* Telefonní číslo VOIP nelze použít pro proces ověření phone hello.
* Překontrolujte hello telefonní číslo, které zadáte, včetně hello kód země, které jste vybrali v rozevírací nabídce hello.
* Pokud váš telefon není přijímat textové zprávy (SMS), zkuste hello **zavolat mi** možnost.
* Zajistěte, že váš telefon můžete volání nebo SMS zprávy z USA na základě čísla.

Když získáte hello textové zprávy nebo telefonní hovor, zadejte do textového pole pro hello kód, který se zobrazí.

## <a name="credit-card-declined-or-not-accepted"></a>Platební karty odmítl nebo nebylo přijato.
Virtuální nebo předplacené karty kreditní nebo debetní nejsou přijata jako platbu předplatných Azure. toosee co může způsobit, že vaše karty toobe odmítnutá, najdete v části [debetní karty nebo platební karty je odmítnuta na Azure registrace](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="free-trial-is-not-available"></a>"Bezplatnou zkušební verzi není k dispozici.
Používali jste v minulosti hello předplatné Azure? Hello smlouvy Azure podmínky použití omezení bezplatná zkušební verze aktivace pouze pro uživatele, který je nové tooAzure. Pokud jste předtím jiný typ předplatného Azure, nelze aktivovat bezplatnou zkušební verzi. Zvažte využití [předplatné s průběžnými platbami](https://azure.microsoft.com/offers/ms-azr-0003p/).

## <a name="i-saw-a-charge-on-my-free-trial-account"></a>Byl uveden jako náklady na můj účet bezplatné zkušební verze
Může se zobrazit malá ověření uložení na vašem účtu platební karty po přihlášení, které je v období 3 dní too5 odebrána. Pokud jste obavy týkající se správy náklady, další informace o [brání neočekávané náklady](https://docs.microsoft.com/azure/billing/billing-getting-started).

## <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>Nelze aktivovat Azure benefit plán jako MSDN, BizSpark, BizSparkPlus nebo MPN
Ujistěte se, že používáte hello správné přihlášení přihlašovací údaje. Zkontrolujte toomake program zvýhodnění hello se, že jste vhodné. 

* MSDN
  * Ověřte stav vaší podmínky v vaše [stránku účtu MSDN](https://msdn.microsoft.com/subscriptions/manage/default.aspx).
  * Pokud nemůžete ověřit stav vaší, obraťte se na hello [MSDN odběry podpory pro zákazníky](https://msdn.microsoft.com/subscriptions/contactus.aspx)
* BizSpark
  * Přihlaste se toohello [BizSpark portál](https://www.microsoft.com/bizspark/default.aspx#start-two) a ověřte stav vaší podmínky pro BizSpark a BizSpark Plus.
  * Pokud nemůžete ověřit váš stav, můžete [získání nápovědy na fórech BizSpark hello](http://aka.ms/bzforums).
* MPN
  * Přihlaste se toohello [MPN portál](https://mspartner.microsoft.com/en/us/Pages/Locale.aspx) a ověřte stav vaší podmínky. Pokud máte hello odpovídající [cloudové platformy možnosti](https://mspartner.microsoft.com/en/us/pages/membership/cloud-platform-competency.aspx), může být vhodné pro další výhody.
  * Pokud nemůžete ověřit stav vaší, obraťte se na [MPN podporu](https://mspartner.microsoft.com/en/us/Pages/Support/Premium/contact-support.aspx).

## <a name="cant-activate-new-azure-in-open-subscription"></a>Nelze aktivovat nové předplatné Azure v otevřené
toocreate předplatné služby Azure v otevřené, musí mít platný klíč Online služby Aktivace (OSA) s alespoň jeden Azure v otevřené tokenu přidružený tooit. Pokud nemáte klíčem OSA, obraťte se na jednu z Microsoft Partners uvedené [Microsoft Pinpoint](http://pinpoint.microsoft.com/).

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém.
