---
title: "tooapps aaaManaging přístup pomocí služby Azure AD | Microsoft Docs"
description: "Popisuje, jak Azure Active Directory umožňuje organizacím toospecify hello aplikace toowhich každý uživatel má přístup."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
ms.assetid: b0829f18-9e57-4107-925d-5f0457d81671
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: b9461b7a1cc8913cd8fb4a4ce0778afe03274935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-access-tooapps"></a>Správa přístupu tooapps
Probíhající přístup správu, využití hodnocení a vytváření sestav pokračovat toobe výzvu po integraci aplikace do systému identity vaší organizace. V mnoha případech správci IT helpdesk mít nebo tootake probíhající aktivní roli při správě aplikací tooyour přístup. V některých případech přiřazení provádí obecné nebo oddělení IT tým. Často rozhodnutí přiřazení hello je určený toobe delegované toohello pracovník s rozhodovací pravomocí, které vyžadují jejich schválení před IT díky hello přiřazení.  Jiných organizací investovat do integraci se stávajícím automatizované přístupu a identit a správy systému, jako je řízení přístupu na základě Role (RBAC) nebo na základě atributů řízení přístupu (ABAC). Integrace hello a vývoj pravidlo zpravidla toobe specializované a nákladné. Monitorování nebo generování sestav na buď způsob správy je vlastní samostatný, nákladná a komplexní investice.

## <a name="how-does-azure-active-directory-help"></a>Jak funguje Azure Active Directory?
 Azure AD podporuje rozsáhlé access management pro nakonfigurovaných aplikací, povolení organizace tooeasily dosáhnout hello správná přístupová práva zásady od automatická, na základě atributů přiřazení (ABAC nebo RBAC scénáře) prostřednictvím delegování a včetně Správa správců. S Azure AD můžete snadno dosáhnout komplexní zásady kombinování více správu modelů pro jednu aplikaci a můžete i znovu použít pravidla správy ve všech aplikacích s hello stejné cílové skupiny.

* [Přidat nové nebo existující aplikace](active-directory-sso-integrate-saas-apps.md)

 Přiřazení aplikace Azure AD se zaměřuje na dva režimy primární přiřazení:

* **Jednotlivé přiřazení** můžete vybrat jednotlivé uživatelské účty a jim udělit přístup správce IT s oprávněními globálního správce adresáře toohello aplikace.
* **Přiřazení na základě skupiny (placené pouze Azure AD)** správce IT s oprávněními globálního správce adresáře můžete přiřadit skupině toohello aplikaci. Přístupu konkrétním uživatelům je určen podle, že ať už jsou členy skupiny hello během hello pokoušejí tooaccess hello aplikace. Jinými slovy správce můžete efektivně vytvořit pravidlo přiřazení s oznámením "hello přiřazené skupiny libovolný aktuální člen má aplikace přístup toohello". Použití této možnosti přiřazení správci mohou těžit z výhod všechny možnosti správy skupin Azure AD, včetně [na základě atributů dynamických skupin](active-directory-accessmanagement-manage-groups.md), externí systém skupiny (například místní služby Active Directory nebo Workday) nebo skupiny řízená správcem nebo řízená na samoobslužných služeb. Jednu skupinu lze snadno přiřadit toomultiple aplikací, zajištění, že aplikace s přiřazení spřažení sdílet pravidla přiřazení snižuje celkové složitost správy hello. Upozorňujeme, že vnořené skupiny členství ve skupinách nejsou podporovány pro přiřazení na základě skupiny tooapplications v tuto chvíli.

Pomocí těchto režimech dvě přiřazení, správci můžete dosáhnout žádné způsob správy žádoucí přiřazení.

S Azure AD použití a vytváření sestav přiřazení jsou plně integrované, povolíte správci tooeasily zprávu o stavu přiřazení a přiřazení chyby a i využití.

## <a name="complex-application-assignment-with-azure-ad"></a>Přiřazení složitých aplikací s Azure AD
Zvažte aplikaci, například služby Salesforce. V mnoha organizacích Salesforce slouží především hello marketing a prodejní organizace. Často členy hello marketingové team vysoce privilegovaný přístup tooSalesforce při členové prodejního týmu hello mít omezený přístup. V mnoha případech široký naplnění informační pracovníci mají omezený přístup toohello aplikace. Pravidla výjimky toothese zkomplikovat záleží. Je často hello vyhrazeny hello marketing nebo prodejní vedení týmy toogrant přístup uživatelů nebo změnit jejich rolí nezávisle na tato obecná pravidla.

S Azure AD může být aplikace, jako je Salesforce předem nakonfigurovaná pro jednotné přihlašování (SSO) a automatické zřizování. Po nakonfigurování aplikace hello správce může trvat hello jednorázová akce toocreate a přiřaďte hello příslušných skupin. V tomto příkladu může správce provést následující přiřazení hello:

* [Dynamické skupiny](active-directory-accessmanagement-manage-groups.md) může být definovaná tooautomatically reprezentovat všichni členové hello prodeje a marketingu týmů pomocí atributů, například oddělení nebo role:
  
  * Všichni členové skupiny marketing by být přiřazena role "marketing" toohello v Salesforce
  * Všichni členové prodejní tým skupiny bude přiřazeno toohello "prodeje" role v Salesforce. Další upřesnění může používat více skupiny, které představují regionální prodejní týmy, které jsou přiřazené role toodifferent Salesforce.
* mechanismus výjimek hello tooenable, skupinu samoobslužné služby by bylo možné vytvořit pro každou roli. Například lze vytvořit skupinu "Salesforce marketingové výjimky" hello jako skupina samoobslužné služby. Hello skupině lze přiřadit toohello Salesforce marketing role a marketing vedení hello můžete provedeny vlastníky. Členové marketingového vedení hello může přidat nebo odebrat uživatele, nastavit zásady spojení, nebo i schválit nebo zamítnout toojoin požadavky jednotlivých uživatelů. Tato možnost je podporována prostřednictvím prostředí příslušné informace pracovního procesu, které nevyžaduje specializované školení pro vlastníky nebo členy.

V takovém případě všechny přiřazené uživatele bude automaticky zřízeného tooSalesforce, protože se jedná o přidané toodifferent skupiny, které by v Salesforce aktualizovat jejich přiřazení role. Uživatelé by být schopný toodiscover a přístup k Salesforce prostřednictvím panel přístupu aplikace Microsoft hello, webovými klienty Office, nebo i tak, že přejdete tootheir organizační Salesforce přihlašovací stránku. Správci by bylo možné tooeasily zobrazení využití a přiřazení stavu pomocí vytvářením sestav Azure AD.

Můžete použít správce [podmíněného přístupu Azure AD](active-directory-conditional-access.md) tooset zásady přístupu pro konkrétní role. Tyto zásady můžete zahrnují mimo hello podnikovém prostředí a i ověřování Multi-Factor Authentication nebo přístup k tooachieve požadavky na zařízení v různých případech je povolen přístup.

## <a name="how-can-i-get-started"></a>Jak můžu začít?
První, pokud již nejsou používání služby Azure AD a jste správcem IT:

* [Vyzkoušejte si to!](https://azure.microsoft.com/trial/get-started-active-directory/) – Můžete si zaregistrovat k bezplatné 30denní zkušební verzi a nasadit řešení první cloudu pomocí tohoto odkazu v části 5 minut.

Azure AD funkce, které umožňují sdílení účet:

* [Přiřazení skupiny](active-directory-accessmanagement-self-service-group-management.md)
* Přidání tooAzure aplikace AD
* Začínáme s přiřazení
* Aplikace přiřazení – nejčastější dotazy
* [Využití aplikace řídicího panelu a sestavách](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Kde můžete další informace?
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Ochrana aplikací pomocí podmíněného přístupu](active-directory-conditional-access.md)
* [Samoobslužné služby skupiny správy nebo SSAA](active-directory-accessmanagement-self-service-group-management.md)

