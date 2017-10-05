---
title: "Řešení problémů s Azure registrace | Microsoft Docs"
description: "Popisuje postupy řešení některé běžné Azure přihlašovací až problémy."
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
ms.openlocfilehash: 647509ea36e487aca5db661adb3268e845988f78
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-sign-up-issues-for-azure"></a>Řešení problémů registrace pro Azure
Pokud nelze registrace ke službě Azure, použijte typy v tomto článku řešení běžných problémů. Pokud máte potíže s platební karty během registrace, viz [debetní karty nebo platební karty je odmítnuta na Azure registrace](billing-credit-card-fails-during-azure-sign-up.md). Pokud máte účet Azure, ale nemůžete se přihlásit, přečtěte si téma [nemůžete se přihlásit ke správě předplatného Azure](billing-cannot-login-subscription.md).

## <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>Indikátor průběhu přestane reagovat v části "Ověření Identity pomocí karty"

K dokončení ověření identity karty, musí být soubory cookie třetích stran povoleno pro prohlížeč.

![Snímek obrazovky ověření Identity karty oddílu hanging během registrace](./media/billing-troubleshoot-azure-sign-up-issues/identity-verification-hangs.PNG)

Použijte následující kroky k aktualizaci nastavení souborů cookie v prohlížeči.

1. Pokud používáte Chrome, přejděte na **nastavení** > **zobrazit upřesňující nastavení** > **o ochraně osobních údajů** > **obsahu nastavení**. Zrušte zaškrtnutí políčka **blokovat soubory cookie třetích stran a data lokality**.
2. Pokud používáte Edge, přejděte na **nastavení** > **zobrazení upřesňující nastavení** > **soubory cookie**. Vyberte **nedošlo k blokování souborů cookie**.
3. Aktualizovat Azure stránku pro přihlášení a zkontrolujte, jestli je problém vyřešen.
4. Pokud k aktualizaci nebyla tento problém vyřešit, ukončete a restartujte prohlížeč a akci opakujte.

## <a name="credit-card-form-doesnt-support-my-billing-address"></a>Formulář platební karty nepodporuje fakturační adresa
Fakturační adresu musí být v zemi, který jste vybrali v **o vás** části. Ujistěte se, zda že jste vybrali správnou zemi.

## <a name="no-text-messages-or-calls-during-sign-up-account-verification"></a>Žádné textové zprávy nebo volání během ověření registrace účtu
I když je obvykle mnohem rychlejší, může trvat až čtyři minut ověřovací kód, který bude doručen. Telefonní číslo, které zadáte pro ověření není uložena jako kontaktní číslo pro účet.

Zde jsou některé další tipy:
* Telefonní číslo VOIP nelze použít pro proces ověření telefonu.
* Překontrolujte telefonní číslo zadáte, včetně kód země, které jste vybrali v rozevírací nabídce.
* Pokud váš telefon není přijímat textové zprávy (SMS), zkuste **zavolat mi** možnost.
* Zajistěte, že váš telefon můžete volání nebo SMS zprávy z USA na základě čísla.

Když získáte textové zprávy nebo telefonní hovor, zadejte do textového pole kód, který se zobrazí.

## <a name="credit-card-declined-or-not-accepted"></a>Platební karty odmítl nebo nebylo přijato.
Virtuální nebo předplacené karty kreditní nebo debetní nejsou přijata jako platbu předplatných Azure. Co může způsobit, že karty odmítnutá najdete v tématu [debetní karty nebo platební karty je odmítnuta na Azure registrace](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="free-trial-is-not-available"></a>"Bezplatnou zkušební verzi není k dispozici.
Použili jste předplatné Azure v minulosti? Smlouvu Azure podmínky použití omezení bezplatná zkušební verze aktivace pouze pro uživatele, který je nové do Azure. Pokud jste předtím jiný typ předplatného Azure, nelze aktivovat bezplatnou zkušební verzi. Zvažte využití [předplatné s průběžnými platbami](https://azure.microsoft.com/offers/ms-azr-0003p/).

## <a name="i-saw-a-charge-on-my-free-trial-account"></a>Byl uveden jako náklady na můj účet bezplatné zkušební verze
Může se zobrazit malá ověření uložení na vašem účtu platební karty po přihlášení, které je odebrána do 3 až 5 dní. Pokud jste obavy týkající se správy náklady, další informace o [brání neočekávané náklady](https://docs.microsoft.com/azure/billing/billing-getting-started).

## <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>Nelze aktivovat Azure benefit plán jako MSDN, BizSpark, BizSparkPlus nebo MPN
Ujistěte se, že používáte pravém přihlašovací údaje. Zkontrolujte program zvýhodnění a ujistěte se, že jste vhodné. 

* MSDN
  * Ověřte stav vaší podmínky v vaše [stránku účtu MSDN](https://msdn.microsoft.com/subscriptions/manage/default.aspx).
  * Pokud nemůžete ověřit stav vaší, obraťte se [MSDN odběry podpory pro zákazníky](https://msdn.microsoft.com/subscriptions/contactus.aspx)
* BizSpark
  * Přihlaste se k [BizSpark portál](https://www.microsoft.com/bizspark/default.aspx#start-two) a ověřte stav vaší podmínky pro BizSpark a BizSpark Plus.
  * Pokud nemůžete ověřit váš stav, můžete [získání nápovědy na fórech BizSpark](http://aka.ms/bzforums).
* MPN
  * Přihlaste se k [MPN portál](https://mspartner.microsoft.com/en/us/Pages/Locale.aspx) a ověřte stav vaší podmínky. Pokud máte příslušné [cloudové platformy možnosti](https://mspartner.microsoft.com/en/us/pages/membership/cloud-platform-competency.aspx), může být vhodné pro další výhody.
  * Pokud nemůžete ověřit stav vaší, obraťte se na [MPN podporu](https://mspartner.microsoft.com/en/us/Pages/Support/Premium/contact-support.aspx).

## <a name="cant-activate-new-azure-in-open-subscription"></a>Nelze aktivovat nové předplatné Azure v otevřené
Chcete-li vytvořit předplatné služby Azure v otevřené, musí mít platný klíč Online služby Aktivace (OSA) s alespoň jeden Azure v otevřené token přidružené k němu. Pokud nemáte klíčem OSA, obraťte se na jednu z Microsoft Partners uvedené [Microsoft Pinpoint](http://pinpoint.microsoft.com/).

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.
