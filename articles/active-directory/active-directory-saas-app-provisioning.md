---
title: "Automatizované SaaS zřizování uživatelů aplikace ve službě Azure AD | Microsoft Docs"
description: "Úvod do Azure AD můžete použít jak pro automatické zřizování, zrušit a průběžně aktualizovat uživatelské účty napříč různými aplikacemi SaaS jiných výrobců."
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
ms.openlocfilehash: 7cb780117d64d67449146b9757f8162e23e65d1e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatizovat uživatele zajišťování a rušení zajištění pro aplikace SaaS ve službě Azure Active Directory
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>Co je automatické zřizování uživatelů pro aplikace SaaS?
Azure Active Directory (Azure AD) umožňuje automatizovat vytváření, údržbu a odebírání uživatelských identit v cloudu ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) aplikace, jako je Dropbox, Salesforce, ServiceNow a další.

**Tady jsou některé příklady co tato funkce umožňuje, abyste mohli provádět:**

* Automaticky vytvořte nové účty ve správné aplikace SaaS pro nové uživatele, když připojí váš tým.
* Při osoby nevyhnutelnou nechte týmem automaticky deaktivujte účty z aplikací SaaS.
* Zajišťují, aby identit v aplikace SaaS aktuální na základě změn v adresáři.
* Zřídit neuživatelských objekty, například skupin, k aplikacím SaaS, která je podporují.

**Zřizování uživatelů automatizované také zahrnuje následující funkce:**

* Možnost tak, aby odpovídala stávající identit mezi službou Azure AD a aplikace SaaS.
* Možnosti přizpůsobení nápovědě Azure AD podle aktuální konfigurace aplikace SaaS, které vaše organizace aktuálně používá.
* Volitelné e-mailové výstrahy pro zřizování chyby.
* Vytváření sestav a aktivity protokolů, které pomáhají s monitorování a řešení potíží.

## <a name="why-use-automated-provisioning"></a>Proč používat automatického zřizování?
Některé běžné motivace pro použití této funkce patří:

* Aby se zabránilo náklady, umožňuje zvýšit efektivitu a lidské chyby související s ručním procesy pro zřizování.
* Chcete-li okamžitě odebrání identity uživatelů z klíče aplikace SaaS, když opustí organizaci zabezpečit vaši organizaci.
* Snadno importovat číslem hromadně uživatelů do určité aplikaci SaaS.
* Abyste mohli využívat pohodlí nutnosti řešení zřizování, spusťte z stejné zásady přístupu aplikace, které definované pro Azure AD jednotné přihlašování.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
**Jak často Azure AD zápisu directory změn do aplikace SaaS?**

Azure AD kontroluje změny každých pět až deset minut. Pokud aplikace SaaS vrací několik chyb, (například jako v případě přihlašovací údaje správce neplatný), pak v Azure AD postupně snížit jeho četnost maximálně jednou za den, dokud se nevyřeší chyby.

**Jak dlouho bude trvat ke zřízení Moji uživatelé?**

Přírůstkové změny dojít téměř okamžitě, ale pokud chcete zřídit většinu adresáře, pak závisí na počtu uživatelů a skupin, které máte. Malé adresáře jenom pár minut trvat, střední adresáře může trvat několik minut a velmi velkých adresářích může trvat několik hodin.

**Jak můžete sledovat průběh úlohy aktuální zřizování?**

Můžete zkontrolovat sestavu zřizování účtu části sestavy tohoto adresáře. Další možností je přejděte na kartu řídicí panel pro aplikace SaaS, která jsou pro zřizování a vyhledejte v části "Integrace stav" v dolní části stránky.

**Jak poznám, že pokud uživatelům nedaří získat zřízený správně?**

Na konci konfiguraci zřizování je Průvodce existuje možnost přihlášení k odběru e-mailové oznámení pro zřizování selhání. Můžete také zkontrolovat sestavy chyb zřizování zobrazíte uživatele, kteří se nepovedlo zřídit a proč.

**Může Azure AD zapisovat změny z aplikace SaaS zpět do adresáře?**

Pro většinu aplikací SaaS zřizování je pouze odchozí, což znamená, že uživatelé se zapisují z adresáře, do aplikace a změny z aplikace nelze zapisovat zpátky do adresáře. Pro [Workday](https://msdn.microsoft.com/library/azure/dn762434.aspx), ale zřizování je pouze příchozí, což znamená, že uživatelé jsou importovány do adresáře z Workday a podobně, změny v adresáři není získat zpět do pracovního dne.

**Jak mohu odeslat zpětnou vazbu technickému týmu?**

Kontaktujte nás prostřednictvím [fóru pro zpětnou vazbu Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="how-does-automated-provisioning-work"></a>Jak funguje automatické zřizování?
Azure AD zřídí uživatelů k aplikacím SaaS připojením ke zřizování koncové body poskytnutý dodavatelem každé aplikace. Tyto koncové body umožňují službě Azure AD prostřednictvím kódu programu vytvářet, aktualizovat a odebírat uživatele. Níže je stručný přehled různé kroky, které Azure AD potřebné k automatizaci zřizování.

1. Když povolíte zřizování pro aplikace pro první, byly provedeny následující akce:
   * Azure AD se pokusí přiřadit všichni stávající uživatelé v aplikaci SaaS pro jejich odpovídající identit v adresáři. Pokud je nalezena shoda uživatele, jsou *není* automaticky povolené pro jednotné přihlašování. Pořadí pro uživatele tak, aby měl přístup k aplikaci, musí být explicitně přiřazena k aplikaci ve službě Azure AD, buď přímo nebo prostřednictvím členství ve skupině.
   * Pokud jste už zadali, kteří uživatelé by se měla přiřadit k aplikaci, a pokud se nepodaří najít existujících účtů pro uživatele Azure AD, Azure AD zřídit nové účty pro ně v aplikaci.
2. Po dokončení počáteční synchronizace se dokončila, jak je popsáno výše, Azure AD zkontroluje každých 10 minut pro následující změny:
   * Pokud noví uživatelé mají přiřazený k aplikaci (buď přímo nebo prostřednictvím členství ve skupinách), pak budou zřízení nového účtu v aplikaci SaaS.
   * Pokud byl odebrán přístup uživatele, pak svého účtu v aplikaci SaaS budou označeny jako zakázané (uživatele jsou odstraněny nikdy plně, která chrání před ztrátou dat v případě nesprávnou konfiguraci).
   * Pokud uživatel byl nedávno přiřazen k aplikaci a už jako účet v aplikaci SaaS, tomuto účtu budou označeny jako povolené a některé vlastnosti uživatele může být aktualizován, pokud jsou zastaralé, ve srovnání s adresáři.
   * Došlo ke změně informací o uživateli (například telefonní číslo, umístění kanceláře atd.) v adresáři, pak tyto informace bude aktualizováno také v aplikaci SaaS.

Další informace o tom, jak jsou mapovány atributy mezi službou Azure AD a aplikace SaaS, najdete v článku na [přizpůsobení mapování atributů](active-directory-saas-customizing-attribute-mappings.md).

## <a name="list-of-apps-that-support-automated-user-provisioning"></a>Seznam aplikací, které podporují automatické zřizování uživatelů
Všechny aplikace "Doporučený" v galerii aplikací Azure AD, podporují zřizování automatizované uživatelů. [Zde můžete zobrazit seznam vybraných aplikací.](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured)

V pořadí pro aplikace pro podporu zřizování automatizované uživatelů je nejprve musíte zadat potřebné koncových bodů, které umožňují externí programy automatizovat vytváření, údržbu a odebírání uživatelů. Ne všechny aplikace SaaS jsou proto kompatibilní s touto funkcí. Pro aplikace, které to podporují technický tým služby Azure AD bude moct vytvářet zřizování konektor pro tyto aplikace a činnost prioritu podle potřeb aktuální a potenciální zákazníky.

Kontaktujte Azure AD technickému týmu k vyžádání zřizování podpory pro další aplikace, odešlete prosím zprávy prostřednictvím [fóru pro zpětnou vazbu Azure Active Directory](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/category/172035-user-provisioning).

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Přizpůsobení mapování atributů pro zřizování uživatelů](active-directory-saas-customizing-attribute-mappings.md)
* [Zapisují se výrazy pro mapování atributů](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Filtry pro zřizování uživatelů oborů](active-directory-saas-scoping-filters.md)
* [Zapnutí automatického zřizování uživatelů a skupin ze služby Azure Active Directory do aplikací pomocí SCIM](active-directory-scim-provisioning.md)
* [Účet zřizování oznámení](active-directory-saas-account-provisioning-notifications.md)
* [Seznam kurzů k integraci aplikací SaaS](active-directory-saas-tutorial-list.md)

