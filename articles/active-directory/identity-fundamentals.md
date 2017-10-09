---
title: "aaaFundamentals správy identit Azure | Microsoft Docs"
description: 
keywords: 
author: jeffgilb
manager: femila
ms.reviewr: jsnow
ms.author: jeffgilb
ms.date: 7/5/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.openlocfilehash: a9710b8e543cdbb2f78ea9e3f83b183e1983b31d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="fundamentals-of-azure-identity-management"></a>Základy správy identit Azure
Jako více společnosti digitální prostředky za provozu mimo hello podnikové sítě, v cloudu hello a na zařízeních, skvělé cloudových identit a přístupu řešení pro správu se stává stále nezbytné. Cloudové identity jsou nyní hello nejlepší způsob, jak toomaintain řízení over a získat přehled o tom, jak a kdy uživatelé přístup k podnikovým aplikacím a datům.

Microsoft má byla zabezpečení cloudových identit pro přes deset a teď se [Azure Active Directory (AD)](https://docs.microsoft.com/azure/active-directory/active-directory-editions), jsou k dispozici tooyou tyto stejné systémy ochrany. S Azure AD můžete správcům snadno zajistit odpovědnosti za uživatele a správce s lepší zabezpečení a zásad správného řízení než kdy dřív.

Azure AD Premium je cloudových identit a přístupu řešení pro správu pomocí funkce rozšířené ochrany umožňující jednu zabezpečené identitu pro všechny aplikace, ochranu identity (vylepšit určením hello [Microsoft intelligence zabezpečení grafu](https://www.microsoft.com/en-us/security/intelligence)) a Privileged Identity Management. Jenom další monitorování nebo generování sestav nástroj Azure AD Premium můžete chránit identit uživatelů v reálném čase a povolit je toocreate přístupu na základě riziko, adaptivní zásady tooprotect dat vaší organizace.

Podívejte se na následující krátké video pro rychlý přehled o správu identit Azure AD a ochrany:
<iframe width="560" height="315" src="https://www.youtube.com/embed/9LGIJ2-FKIM" frameborder="0" allowfullscreen></iframe>

Společnost Microsoft poskytuje nejen identitou, která přebírá everywhere, ale taky sadu nástrojů tooautomate, zabezpečení a správě IT v rámci vaší organizace. I po hello nástupem cloud computing je stále potřebují toomanage a řízení IT úkolů, jako jsou helpdesk volá tooreset hesla uživatele uživatel skupiny správy a žádosti o aplikace. Komplikují další věci, zaměstnanci jsou nyní přináší svá osobní zařízení toowork a pomocí aplikace SaaS ve snadno dostupné. Díky tomu Údržba kontrolu nad svých aplikací mezi datovými centry podnikové a veřejný cloud platformy významné problémy.

[!INCLUDE [identity](../../includes/azure-ad-licenses.md)]

## <a name="connect-on-premises-active-directory-with-azure-ad-and-office-365"></a>Připojit místní službu Active Directory s Azure AD a Office 365
Organizace, které provedli velké investice v místní službě Active Directory můžete rozšířit tyto investice toohello cloudu díky integraci svých místních adresářů se Azure AD do [hybridní Správa identit](https://docs.microsoft.com/azure/active-directory/active-directory-hybrid-identity-design-considerations-overview). Díky tomu umožňuje uživatelům zvýšit produktivitu tím, že poskytuje společnou identitu pro přístup k prostředkům bez ohledu na umístění. Uživatelé a organizace můžou pak pomocí jednotného přihlašování (SSO) tooaccess místních prostředků a cloudových služeb, jako je například Office 365.

[Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) je budete potřebovat integrace hello tooget provést pouze nástroj hello. Azure AD Connect poskytuje možnosti toosupport potřebuje vaše synchronizaci identit a nahrazuje starší verze nástrojů pro integraci identity například DirSync a Azure AD Sync. S Azure AD Connect správu identit a synchronizace mezi místními a Azure AD je povolená díky:

- Synchronizace – tato komponenta odpovídá za vytváření uživatelů, skupin a dalších objektů. Je také zajišťuje, aby se, že informace o identitě místních uživatelů a skupin shodovaly hello cloudu. Zpětný zápis hesla lze povoleno tookeep místních adresářů synchronizované, když uživatel aktualizuje hesla ve službě Azure AD.
- Služba AD FS - federace se nachází volitelné funkce poskytované přes Azure AD Connect, které můžou být použité tooconfigure hybridní prostředí pomocí místní infrastruktury služby AD FS. Federační slouží organizace tooaddress komplexních nasazení, jako je například jednotné přihlašování vynucení přihlášení zásady služby Active Directory a čipové karty nebo třetích stran vícefaktorového ověřování.
- Monitorování stavu – [Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health) může poskytovat robustní monitorování a poskytují centrální umístění na portálu Azure tooview hello této aktivity.

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>Zvýšení produktivity a snížit náklady na technickou podporu s samoobslužné služby a jeden přihlašování

Zaměstnanci jsou zvýšit produktivitu, pokud mají jeden tooremember uživatelské jméno a heslo a konzistentního prostředí z každé zařízení. Budou také ušetřit čas při mohou provádět samoobslužné služby úkoly, jako je [resetování zapomenuté heslo](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) nebo pro požadování přístupu tooan aplikace bez čekání na požádat o pomoc hello helpdesk.

Azure AD [rozšiřuje místní služby Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) do cloudu hello povolení uživatelé toouse jejich primární účtu organizace pro svá zařízení připojených k doméně, prostředkům společnosti a všechny webové hello a aplikace SaaS jejich třeba toouse tooget svou práci udělat. Kromě toho toonot s tooremember více sad uživatelská jména a hesla, přístup k aplikaci uživatelů lze také automaticky zřizovat (nebo zrušte zřízený) na základě jejich členství ve skupinách organizace a jejich stav jako zaměstnanec. A můžete řídit přístup k pro galerii aplikace nebo pro své vlastní místní aplikace, které jste vyvinuté a publikované prostřednictvím hello [Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

## <a name="manage-and-control-access-toocorporate-resources"></a>Spravovat a řídit přístup k prostředkům toocorporate
Microsoft identit a přístupu řešení Nápověda pro správu IT chránit tooapplications přístup a prostředky v podnikovém datovém centru hello a do cloudu hello povolení další úrovně ověřování, jako [služby Multi-Factor authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) a [zásady podmíněného přístupu](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Monitorování podezřelé aktivity přes pokročilé zabezpečení vytváření sestav a auditování, výstrahy, pomáhá zmírnit potenciální potíže se zabezpečením.

Zásady podmíněného přístupu v Azure AD Premium získáte, hello enterprise Admins, hello možnost toocreate na základě zásad pravidel přístupu pro všechny Azure AD připojené aplikaci (aplikace SaaS, vlastní aplikace běžící v cloudu nebo na místní hello webových aplikací). Azure AD vyhodnotí tyto zásady v reálném čase a vynucuje je vždy, když uživatel pokusí tooaccess aplikace. Zásady ochrany identit Azure povolit tooautomatically proveďte akce. Pokud je zjištěno podezřelou aktivitu. Tyto akce mohou obsahovat blokování přístupu toousers vysoce rizikové, vynucování služby Multi-Factor authentication a ohrožený resetuje heslo uživatele, pokud to vypadá přihlašovací údaje.


## <a name="azure-active-directory-privileged-identity-management"></a>Správa privilegovaných identit v Azure Active Directory

[Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started), součástí hello Azure Active Directory Premium P2 nabídky, vám umožní toodiscover, omezte a monitorujte účty pro správu a jejich přístup tooresources v Azure Active Directory a jiných Služeb Microsoft online services. Pomáhá také můžete spravovat přístup pro správu na vyžádání hello přesný dobu, které potřebujete.

Správa privilegovaných identit můžete vynutit oprávnění správce na vyžádání, tak, aby správci požadovat vícefaktorové ověřený, dočasný zvýšení svá oprávnění na předem nakonfigurovaná dobu před jejich účty vrátit tooa normální Stav uživatele.

## <a name="benefits-of-azure-identity"></a>Výhody identit Azure

Správa identit Azure můžete:

-   Vytvářet a spravovat jedinou identitu pro každého uživatele v celé organizaci, udržování uživatelé, skupiny a zařízení synchronizace s [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

-   Zadejte přístup přihlášení tooyour aplikace včetně tisícům předem integrovaných aplikací SaaS nebo poskytnout zabezpečený vzdálený přístup pomocí hello aplikace SaaS tooon místní [Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

-   Povolit aplikaci přístup k zabezpečení vynucením založeného na pravidlech [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) pro místní i cloudové aplikace.

-   Zvýšení produktivity uživatelů s [samoobslužného resetování hesel](https://docs.microsoft.com/azure/active-directory/active-directory-passwords)a skupiny a aplikace přístup k žádosti o pomocí hello [MyApps portál](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-user-help).

-   Využít výhod hello [vysokou dostupnost a spolehlivost](https://docs.microsoft.com/azure/architecture/resiliency/high-availability-azure-applications) služby po celém světě, podnikové úrovni, cloudových identit a přístupu řešení pro správu.

## <a name="next-steps"></a>Další kroky
[Další informace o řešení Azure identity](https://docs.microsoft.com/azure/active-directory/understand-azure-identity-solutions)