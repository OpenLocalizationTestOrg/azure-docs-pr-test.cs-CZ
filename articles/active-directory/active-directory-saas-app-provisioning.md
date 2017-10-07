---
title: "aaaAutomated SaaS aplikace zřizování uživatelů ve službě Azure AD | Microsoft Docs"
description: "Úvod toohow zřizovat tooautomatically Azure AD, můžete zrušit a průběžně aktualizovat uživatelské účty napříč různými aplikacemi SaaS jiných výrobců."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 58c5fa2d-bb33-4fba-8742-4441adf2cb62
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: curtand
ms.openlocfilehash: a1f3ecdd513e2b603f8ad9901e9f551b3b982b2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-user-provisioning-and-deprovisioning-toosaas-applications-with-azure-active-directory"></a>Automatizovat uživatele zajišťování a rušení zajištění tooSaaS aplikací s Azure Active Directory
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>Co je automatické zřizování uživatelů pro aplikace SaaS?
Azure Active Directory (Azure AD) umožňuje tooautomate hello vytváření, údržbu a odebírání uživatelských identit v cloudu ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) aplikace, jako je Dropbox, Salesforce, ServiceNow a další.

**Tady jsou některé příklady co tato funkce vám umožní toodo:**

* Automaticky vytvořte nové účty ve hello správné aplikace SaaS pro nové uživatele, když se váš tým.
* Když osoby nevyhnutelnou nechte hello team automaticky deaktivujte účty z aplikací SaaS.
* Zajišťují, aby hello identit v aplikace SaaS až toodate na základě změn v adresáři hello.
* Zřídit neuživatelských objekty, jako jsou skupiny, tooSaaS aplikace, které je podporují.

**Zřizování uživatelů automatizované také zahrnuje hello následující funkce:**

* Hello možnost toomatch existující identit mezi službou Azure AD a aplikace SaaS.
* Přizpůsobení možnosti toohelp Azure AD podle aktuální konfigurace hello hello SaaS aplikací, které vaše organizace aktuálně používá.
* Volitelné e-mailové výstrahy pro zřizování chyby.
* Vytváření sestav a aktivity toohelp protokoly s monitorování a řešení potíží.

## <a name="why-use-automated-provisioning"></a>Proč používat automatického zřizování?
Některé běžné motivace pro použití této funkce patří:

* tooavoid hello náklady, umožňuje zvýšit efektivitu a lidské chyby související s ručním procesy pro zřizování.
* toosecure organizaci tím, že okamžitě odebere identity uživatelů z klíče aplikace SaaS, když opustí organizaci hello.
* tooeasily importovat číslem hromadně uživatelů do určité aplikaci SaaS.
* Výhodou hello tooenjoy nutnosti řešení zřizování, spusťte z hello stejné zásady přístupu aplikace, které definované pro Azure AD jednotné přihlašování.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
**Jak často Azure AD zapisovat aplikaci SaaS toohello directory změny?**

Azure AD kontroluje změny každých pět minut tooten. Pokud aplikace SaaS hello vrací několik chyb (například v případě hello neplatný správce přihlašovacích údajů), pak v Azure AD postupně zpomalit jeho frekvence tooup tooonce za den, dokud se nevyřeší hello chyby.

**Jak dlouho trvá tooprovision Moji uživatelé?**

Přírůstkové změny dojít téměř okamžitě, ale pokud se pokoušíte tooprovision většina adresáře, pak závisí na počtu hello uživatelů a skupin, které máte. Malé adresáře jenom pár minut trvat, střední adresáře může trvat několik minut a velmi velkých adresářích může trvat několik hodin.

**Jak můžete sledovat průběh hello hello aktuální zřizování úlohy?**

Můžete zkontrolovat hello sestavu zřizování účtu části sestavy hello tohoto adresáře. Další možností je toovisit hello řídicí panel karta hello aplikace SaaS, která jsou zřízení až po a podívejte se do části hello v části "Integrace stav" hello dolní části stránky hello.

**Jak poznám, že pokud uživatelé nezdaří tooget zřízený správně?**

Na konci hello hello je zřizování Průvodce konfigurací možnost toosubscribe tooemail oznámení pro zřizování selhání. Můžete také zkontrolovat toosee hello sestavy chyb zřizování uživatelů, kteří se nezdařilo toobe zřízený a proč.

**Může Azure AD zapisovat změny z hello SaaS aplikace back toohello adresáře?**

Pro většinu aplikací SaaS zřizování je pouze odchozí, což znamená, že uživatelé se zapisují z aplikace toohello directory hello a změny z aplikace hello nelze zapsat zpět toohello adresáře. Pro [Workday](https://msdn.microsoft.com/library/azure/dn762434.aspx), ale zřizování je pouze příchozí, což znamená, že uživatelé jsou importovány do adresáře hello z Workday a podobně, změny v adresáři hello není získat zpět do pracovního dne.

**Jak mohu odeslat zpětnou vazbu toohello technický tým?**

Kontaktujte nás prostřednictvím hello [fóru pro zpětnou vazbu Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="how-does-automated-provisioning-work"></a>Jak funguje automatické zřizování?
Azure AD zřídí uživatelé tooSaaS aplikace připojením koncové body tooprovisioning poskytnutý dodavatelem každé aplikace. Povolit tyto koncové body Azure AD tooprogrammatically vytvářet, aktualizovat a odebírat uživatele. Níže je stručný přehled hello různé kroky, aby Azure AD trvá tooautomate zřizování.

1. Když povolíte zřizování pro aplikace pro hello poprvé, jsou prováděny hello následující akce:
   * Azure AD se pokusí toomatch všechny existující uživatele v aplikaci SaaS hello s jejich odpovídající identitami v adresáři hello. Pokud je nalezena shoda uživatele, jsou *není* automaticky povolené pro jednotné přihlašování. Pořadí pro aplikaci toohello uživatele toohave přístup musí být explicitně přiřazena toohello aplikace ve službě Azure AD, buď přímo nebo prostřednictvím členství ve skupině.
   * Pokud již jste zadali, kteří uživatelé by se měla přiřadit toohello aplikace, a pokud selže toofind existující účty pro uživatele Azure AD, Azure AD zřídit nové účty pro ně v aplikaci hello.
2. Jakmile hello počáteční synchronizace byla dokončena, jak je popsáno výše, Azure AD zkontroluje každých 10 minut hello následující změny:
   * Pokud noví uživatelé mají přiřazený toohello aplikace (přímo nebo prostřednictvím členství ve skupinách), pak budou zřízení nového účtu v aplikaci SaaS hello.
   * Pokud byl odebrán přístup uživatele, pak svého účtu v aplikaci SaaS hello budou označeny jako zakázané (uživatele jsou odstraněny nikdy plně, který je chrání před ztrátou dat v případě hello chybné konfigurace).
   * Pokud uživatel byl nedávno přiřazen toohello aplikace a již měla účtu v hello aplikace SaaS, že účet budou označeny jako povolené a některé vlastnosti uživatele může být aktualizován, pokud jsou zastaralé porovnání toohello directory.
   * Došlo ke změně informací o uživateli (například telefonní číslo, umístění kanceláře atd.) v adresáři hello, pak tyto informace budou také aktualizovat v hello aplikaci SaaS.

Další informace o tom, jak jsou mapovány atributy mezi službou Azure AD a aplikace SaaS, najdete v článku hello na [přizpůsobení mapování atributů](active-directory-saas-customizing-attribute-mappings.md).

## <a name="list-of-apps-that-support-automated-user-provisioning"></a>Seznam aplikací, které podporují automatické zřizování uživatelů
Všechny aplikace hello "Doporučený" v galerii aplikací Azure AD hello podporují zřizování automatizované uživatelů. [Zde můžete zobrazit Hello seznam vybraných aplikací.](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured)

Aby aplikaci toosupport automatické zřizování uživatelů se musí nejprve zadejte hello potřeby koncových bodů, které umožňují externí programy tooautomate hello vytváření, údržbu a odebírání uživatelů. Ne všechny aplikace SaaS jsou proto kompatibilní s touto funkcí. Pro aplikace, které to podporují, technický tým služby Azure AD hello bude potom možné toobuild zřizování toothose konektor aplikace, a tento pracovní prioritu podle potřeby hello aktuální a potenciální zákazníky.

Technici hello Azure AD toocontact team toorequest zřizování podpory pro další aplikace, odešlete prosím zprávu prostřednictvím hello [fóru pro zpětnou vazbu Azure Active Directory](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/category/172035-user-provisioning).

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Přizpůsobení mapování atributů pro zřizování uživatelů](active-directory-saas-customizing-attribute-mappings.md)
* [Zapisují se výrazy pro mapování atributů](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Filtry pro zřizování uživatelů oborů](active-directory-saas-scoping-filters.md)
* [Pomocí SCIM tooenable automatické zřizování uživatelů a skupin ze služby Azure Active Directory tooapplications](active-directory-scim-provisioning.md)
* [Účet zřizování oznámení](active-directory-saas-account-provisioning-notifications.md)
* [Seznam kurzů tooIntegrate aplikace SaaS](active-directory-saas-tutorial-list.md)

