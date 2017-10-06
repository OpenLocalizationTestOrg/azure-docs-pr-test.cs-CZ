---
title: "aaaManaging aplikací s Azure Active Directory | Microsoft Docs"
description: "Tento článek hello výhod integrace s místními, cloudu a aplikace SaaS Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 95b96f10-2d5c-4b78-8af8-d3657a24140f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: asteen
ms.openlocfilehash: 0016f8b433e101d8a150bc6d9be3931851578241
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-applications-with-azure-active-directory"></a>Správa aplikací pomocí služby Azure Active Directory
Nad rámec hello pracovní postup nebo obsahu firmy mají dva základní požadavky pro všechny aplikace:

1. tooincrease produktivity, aplikace by měl být snadno toodiscover a přístup
2. tooenable zabezpečení a zásad správného řízení, hello organizace potřebuje řízení a dohledu, na který můžete a ve skutečnosti přistupuje každou aplikaci

V hello, world cloudových aplikací, které nejlépe jde tohoto dosáhnout pomocí identity toocontrol "*KDO je povoleno toodo, co*".

V oblasti výpočetních terminologie:

* *Kdo* se označuje jako *identity* -hello správu uživatelů a skupin
* *Co* se označuje jako *správy přístupu* – hello řízení přístupu tooprotected prostředků

Obě součásti společně se označují jako *Identity a řízení přístupu (IAM)*, která je definována pomocí hello [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) skupiny jako "*hello disciplíně zabezpečení, která umožňuje hello vpravo jednotlivce tooaccess hello správné prostředky v hello pravým časy správné důvodů hello*".

V pořádku, co si hello problém? Pokud je IAM *nespravovaná* na jednom místě s integrované řešení:

* Správci identity mají tooindividually vytvářet a aktualizovat uživatelské účty ve všech aplikacích samostatně, aktivitu redundantní a časově náročné.
* Uživatelé mají toomemorize více aplikací hello tooaccess přihlašovací údaje, které potřebují toowork s. V důsledku toho uživatelé mívají toowrite dolů hesel nebo použití jiných řešení pro správu heslo, které zavádí další data bezpečnostní rizika.
* Redundantní, časově náročné aktivity snižte hello množství času uživatelé a správci pracují na obchodní aktivity, které zvýšit řádku dolní vaší firmy.

Ano co obecně brání organizace z přijetí integrované řešení IAM?

* Většinu technických řešení jsou založené na platformách softwaru, které je třeba toobe nasadit a přizpůsobit každé organizace pro své vlastní aplikace.
* Cloudové aplikace jsou často přijata s vyšší rychlostí než IT organizace může integrovat existující IAM řešení.
* Zabezpečení a monitorování nástrojů vyžadují další přizpůsobení a integrace tooachieve komplexní E2E scénáře.

## <a name="azure-active-directory-integrated-with-applications"></a>Azure Active Directory integrovat s aplikacemi
Azure Active Directory je komplexní Identity společnosti Microsoft jako řešení služby (IDaaS) který:

* Jako cloudová služba umožňuje IAM 
* Poskytuje správu centrální přístupu, jednotného přihlašování (SSO) a vytváření sestav 
* Podporuje správu integrovaného přístupu k [tisíce aplikace](https://azure.microsoft.com/marketplace/active-directory/) v galerii aplikací hello, včetně služby Salesforce, Google Apps, pole, Concur a další. 

S Azure Active Directory, všechny aplikace publikujete pro partnery a zákazníky (obchodní nebo příjemce) mají hello stejné možnosti správy identit a přístupu.<br> Díky této může toosignificantly můžete snížit provozní náklady.

Co když potřebujete tooimplement aplikace, která ještě není uvedený v galerii aplikací hello? To je více než nakonfigurovat jednotné přihlašování pro aplikace z Galerie aplikace hello časově náročné, Azure AD poskytuje průvodce, který vám pomůže s konfigurací hello.

Hodnota Hello Azure AD překročí "právě" cloudové aplikace. Můžete ji použít i s místním aplikacím tím, že poskytuje zabezpečený vzdálený přístup. Zabezpečený vzdálený přístup můžete eliminovat hello hello nutné pro sítě VPN nebo jiných implementacích správy tradiční vzdáleného přístupu.

Zadáním správy centrálního přístupu a jednotné přihlašování (SSO) pro všechny aplikace Azure AD poskytuje řešení hello toohello problémy zabezpečení a produktivitu hlavní data.

* Uživatelé můžou používat více aplikací s jeden přihlašování poskytnutí další tooincome čas generování nebo obchodní aktivity operace provádí.
* Správci identity můžete spravovat přístup tooapplications na jednom místě.

je zřejmé Hello výhody pro hello uživatele a pro vaši společnost. Podívejme bližší pohled na hello výhody pro správce identit a hello organizace.

## <a name="integrated-application-benefits"></a>Výhody integrované aplikace
Hello jednotného přihlašování k procesu má dva kroky:

* Ověřování, hello proces ověření identity uživatele hello.
* Autorizace, hello rozhodnutí tooenable nebo blokování přístupu tooa prostředek s zásady přístupu.

Při použití aplikace toomanage Azure AD a povolit jednotné přihlašování:

* Ověřování se provádí na místě (například AD) nebo účet Azure AD hello uživatele.
* Autorizace se spouští na hello Azure AD přiřazení ochrany zásadu a zajistit konzistentní přivětivé uživatelské prostředí a umožňuje přiřazení tooadd, umístění a podmínky MFA na všechny aplikace, bez ohledu na jeho vnitřní funkce.

Je důležité toounderstand, který hello autorizace hello způsob, jak budou přijaty na cílové aplikace hello se liší v závislosti na tom, jak se aplikace hello integrované s Azure AD.

* **Aplikace je předem integrovaných poskytovatelem služby** jako je Office 365 a Azure, jedná se o aplikace založena přímo na Azure AD a spoléhat na něm pro jejich komplexní funkce pro správu identit a přístupu. Přístup k aplikacím toothese je povolená díky directory token a informace o vystavování.
* **Aplikace je předem integrovaných společností Microsoft a vlastních aplikací** Toto jsou nezávislé cloudové aplikace, které spoléhají na adresář interní aplikace a může pracovat nezávisle na Azure AD. Ve vydávající účet aplikace tooan specifické pověření namapované aplikace je povolený přístup k aplikacím toothese. V závislosti na možnosti hello aplikací může být hello pověření federační token nebo uživatelské jméno a heslo pro účet, který byl dříve zajištěny v aplikaci hello.
* **Místní aplikace** aplikacích publikovaných prostřednictvím především povolení přístup k aplikacím tooon místní proxy aplikace hello Azure AD. Tyto aplikace se spoléhají na centrální místní adresář, jako je Windows Server Active Directory. Přístup k aplikacím toothese se aktivuje spuštění hello proxy toodeliver hello aplikace obsahu toohello koncový uživatel respektováním hello místní přihlášení požadavek.

Například pokud se uživatel připojí vaší organizace, musíte toocreate účet pro hello uživatele ve službě Azure AD pro hello primární přihlašování operace. Pokud tento uživatel vyžaduje přístup tooa spravované aplikaci, například služby Salesforce, je také nutné toocreate účet pro tohoto uživatele v Salesforce a tu propojit toohello účet Azure toomake jednotného přihlašování k práci. Když uživatel hello opustí vaši organizaci, je vhodné toodelete hello účet Azure AD a všechny účty protějškem v hello IAM úložiště hello aplikací, které hello uživatel měl přístup k.

## <a name="access-detection"></a>Detekce přístup
V rámci moderní firmy IT oddělení nemají často informace o všech hello cloudové aplikace, které jsou používány. Ve spojení s Cloud App Discovery Azure AD poskytuje řešení toodetect tyto aplikace.

## <a name="account-management"></a>Správa účtů
Tradičně, správě účtů v hello různé aplikace je ruční proces provádí IT nebo podporují osobní v organizaci hello. Azure AD plně automatizované správy účtů mezi všechny zprostředkovatele integrované aplikace služby a tyto aplikace předem integrované společností Microsoft podpora zajišťování automatizované uživatele nebo SAML JIT.

## <a name="automated-user-provisioning"></a>Zřizování automatizované uživatelů
Některé aplikace poskytují rozhraní automatizace pro vytváření a odebrání (nebo deaktivace) účtů. Pokud zprostředkovatele nabízí takového rozhraní, je využít Azure AD. To snižuje provozní náklady, protože úlohy správy dojít automaticky a zvyšuje hello zabezpečení vašeho prostředí, protože se sníží hello riziko neoprávněného přístupu.

## <a name="access-management"></a>Správa přístupu
Pomocí služby Azure AD můžete spravovat přístup k tooapplications pomocí jednotlivých nebo pravidlo řízené přiřazení. Můžete taky delegovat přístup správu toohello příslušní lidé v hello organizace zajistit hello nejlepší dohledu a snížení zatížení hello technickou podporu.

## <a name="on-premises-applications"></a>místní aplikace
Hello součástí proxy aplikací vám umožní toopublish vaši uživatelé tooyour místní aplikace výsledkem obě konzistentní přístup zkušenosti s moderní cloudové aplikace a hello výhody z Azure AD monitorování, vytváření sestav a funkcích zabezpečení .

## <a name="reporting-and-monitoring"></a>Monitorování a vytváření sestav
Azure AD poskytuje předem integrovaných sestav a možnosti, které umožňují tooknow, kdo má přístup tooapplications a když ve skutečnosti používají je monitorování.

## <a name="related-capabilities"></a>Související možnosti
S Azure AD můžete zabezpečit vaše aplikace s zásady granulární přístupu a předem integrované MFA. informace o používání Azure MFA najdete toolearn [Azure MFA](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Začínáme
tooget spuštění integrace aplikací s Azure AD, prohlédněte si hello [Příručka Začínáme integrace Azure Active Directory s aplikacemi, získávání](active-directory-integrating-applications-getting-started.md).

## <a name="see-also"></a>Viz také
[Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

