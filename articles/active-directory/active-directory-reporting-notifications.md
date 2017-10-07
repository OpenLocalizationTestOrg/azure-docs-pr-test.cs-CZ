---
title: "aaaAzure oznámení vytváření sestav Active Directory"
description: "Jak toouse hello oznámení vytváření sestav Azure Active Directory pro podezřelé přihlášení in."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ae6d4b0e-5931-4cb3-98bf-9be91b381c92
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.custom: oldportal
ms.reviewer: dhanyahk
ms.openlocfilehash: 3843c45eaf9d68e671943bfdbc7ab68933f38fbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-notifications"></a>Oznámení vytváření sestav Azure Active Directory
## <a name="what-reports-generate-email-notifications"></a>Jaké sestavy generovat e-mailových oznámení
V tomto okamžiku pouze hello nestandardní přihlašovací v aktivační události sestavy aktivity e-mailová oznámení.

## <a name="what-is-an-irregular-sign-in"></a>Co je "nestandardní přihlašovací"?
Nestandardní přihlášení jsou ty, které byly identifikovány naše algoritmů strojového učení, na základě hello podmínku "neuskutečnitelná cesta" v kombinaci s neobvyklá přihlášení, umístění a zařízení. To může znamenat, že se hacker byla při toosign pomocí tohoto účtu.

## <a name="who-receives-hello-email-notifications"></a>Kdo obdrží hello e-mailová oznámení?
Hello e-mail je odeslán tooall globální správci, kteří mají přiřazený licenci služby Active Directory Premium. tooensure, které se doručí, odešleme ho také toohello admins alternativní e-mailovou adresu. Správci by měla obsahovat aad-alerts-noreply@mail.windowsazure.com v jejich seznamu bezpečných odesílatelů, nemusíte promeškají hello e-mailu.

## <a name="how-often-are-these-emails-sent"></a>Jak často jsou tyto e-maily odesílané?
Hello e-mail je odeslán, pokud dojít 10 nové nestandardní přihlašovací aktivity v hello posledních 30 dnů nebo od poslední e-mailu hello odeslání, podle toho, která je menší.

## <a name="how-do-i-access-hello-report-mentioned-in-hello-email"></a>Přístupu hello sestavy uvedených v e-mailu hello
Když kliknete na odkaz hello, bude stránka přesměrovaného toohello sestavy v rámci hello portál Azure classic. V pořadí tooaccess hello sestavy, musíte toobe obou:

* Na správce nebo spolusprávce předplatného Azure
* Globální správce adresáře hello a přiřadit licenci služby Active Directory Premium. Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).

## <a name="can-i-turn-off-these-emails"></a>Můžete vypnout těchto e-mailů?
Ano, tooturn zakázat upozornění související s tooanomalous přihlášení v rámci hello portál Azure classic, klikněte na tlačítko **konfigurace**a potom vyberte **zakázané** pod hello **oznámení**části.

## <a name="whats-next"></a>Kam dál
* Chcete zjistit, jaké sestavy zabezpečení, auditování a aktivity jsou k dispozici? Podívejte se na [zabezpečení Azure AD, auditování a protokolování aktivit](active-directory-view-access-usage-reports.md)
* [Začínáme se službou Azure Active Directory Premium](active-directory-get-started-premium.md)
* [Přidání firemního brandingu tooyour stránky přihlásit a přístupového panelu](active-directory-add-company-branding.md)

