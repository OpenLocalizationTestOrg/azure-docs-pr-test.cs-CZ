---
title: "aaaAzure Active Directory – nejčastější dotazy | Microsoft Docs"
description: "Azure Active Directory nejčastější dotazy k odpovědi na otázky o přístupu tooaccess Azure a Azure Active Directory, Správa hesel služby a aplikace."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: b8207760-9714-4871-93d5-f9893de31c8f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/16/2017
ms.author: markvi
ms.openlocfilehash: 63c30c4aeda4551bf02c6b968f98cded5a3b2c16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-faq"></a>Nejčastější dotazy ke službě Azure Active Directory
Azure Active Directory (Azure AD) je komplexní řešení Identity jako služby (IDaaS), které pokrývá všechny prvky identity, řízení přístupu a zabezpečení.

Další informace najdete v tématu [Co je Azure Active Directory?](active-directory-whatis.md).


## <a name="access-azure-and-azure-active-directory"></a>Přístup ke službě Azure a Azure Active Directory
**Otázka: Proč se zobrazila "žádné předplatné nenalezeno" při tooaccess Azure AD v hello portál Azure classic?**

**Odpověď:** tooaccess hello portál Azure classic, každý uživatel potřebuje oprávnění má předplatné Azure. Pokud máte placené předplatné služeb Office 365 nebo Azure AD, přejděte příliš[http://aka.ms/accessAAD](http://aka.ms/accessAAD) pro krok jednorázové aktivaci. Jinak, budete potřebovat bezplatný tooactivate [účet Azure](https://azure.microsoft.com/pricing/free-trial/) nebo placené předplatné.

Další informace naleznete v tématu:

* [Jak je předplatné Azure propojeno se službou Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Spravovat hello adresáře pro předplatné služeb Office 365 ve službě Azure](active-directory-manage-o365-subscription.md)

- - -
**Otázka: co je hello vztah mezi službou Azure AD, Office 365 a Azure?**

**Odpověď:** Azure AD poskytuje běžné funkce identity a přístupu tooall webové služby. Ať používáte Office 365, Microsoft Azure, Intune, nebo jiné, můžete se už používá Azure AD toohelp zapnout správu přihlašování a přístupu pro všechny tyto služby.

Všichni uživatelé, kteří jsou nastaveni tak toouse webových služeb jsou definovány jako uživatelské účty v jedné nebo více instancí služby Azure AD. Těmto účtům můžete nastavit přístup k bezplatným funkcím služby Azure AD, například ke cloudovým aplikacím.

Placené služby AD Azure, jako je Enterprise Mobility + Security, doplňují ostatní webové služby, např. Office 365 nebo Microsoft Azure o komplexní řešení správy a zabezpečení celého podniku.
- - -
**Otázka: Proč může přihlášení toohello portál Azure ale není hello portál Azure classic?**

**Odpověď:** hello portál Azure nevyžaduje platné předplatné, a portálu classic hello vyžadují platné předplatné.  Pokud nemáte předplatné, nemůžete se přihlásit toohello portálu classic.
- - -
**Otázka: co jsou hello rozdíly mezi správce předplatného a správce adresáře?**

**Odpověď:** ve výchozím nastavení, můžete se přiřazuje role správce předplatného hello při registraci v Azure. Správce předplatného můžete použít účet Microsoft nebo pracovní nebo školní účet hello adresář, který hello předplatné Azure je přidružen.  Tato role je autorizovaný toomanage služeb v hello portálu Azure.

Pokud ostatní potřebují toosign v a získat přístup ke službám pomocí pomocí hello stejného předplatného, můžete je přidat jako pomocní správci. Tato role má hello stejný přístup oprávnění jako dobrý den, Správce služby, ale nelze změnit přidružení hello odběry tooAzure adresářů.  Další informace o Správci předplatného najdete v tématu [jak tooadd nebo změna role Správce služby Azure](../billing-add-change-azure-subscription-administrator.md) a [asociování předplatných Azure se službou Azure Active Directory](active-directory-how-subscriptions-associated-directory.md).


Azure AD má jinou sadu správce rolí toomanage hello adresáře a funkce související s identity.  Tyto správce bude mít přístup k funkcím toovarious v hello portál Azure nebo hello portál Azure classic. role správce Hello Určuje, co mohou provádět, jako je vytvoření nebo úprava uživatelů, přiřazení role správců tooothers, resetovat uživatelská hesla, spravovat uživatelské licence nebo spravovat domény.  Další informace o správcích adresáře služby Azure AD a jejich rolích najdete v tématu [Přiřazení rolí správce ve službě Azure Active Directory](active-directory-assign-admin-roles.md).

Kromě toho placené služby AD Azure, jako je Enterprise Mobility + Security, doplňují ostatní webové služby, např. Office 365 nebo Microsoft Azure o komplexní řešení správy a zabezpečení celého podniku.

- - -
**Otázka: Existuje sestava, která ukazuje, kdy vyprší platnost mé uživatelské licence Azure AD?**

**Odpověď:** Ne.  Aktuálně není k dispozici.

- - -

## <a name="get-started-with-hybrid-azure-ad"></a>Začínáme s hybridní službou Azure AD


**Otázka: Jak opustím tenanta, když jsem přidán jako spolupracovník?**

**Odpověď:** při tooanother organizace klienta jsou přidány jako spolupracovníka, můžete použít hello "klienta přepínači" v horním pravém tooswitch hello mezi klienty.  V současné době neexistuje žádný způsob, jak tooleave hello pozvání organizace a společnost Microsoft pracuje na poskytnutí tuto funkci.  Dokud nebude tato funkce je dostupná, můžete požádat hello pozvání tooremove organizace vám v jejich klienta.
- - -
**Otázka: jak můžete připojit Moje místní adresář tooAzure AD?**

**Odpověď:** připojíte tooAzure directory vaše místní AD pomocí Azure AD Connect.

Další informace najdete v článku [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).

- - -
**Otázka: Jak mám nastavit jednotné přihlašování mezi místním adresářem a cloudovými aplikacemi?**

**Odpověď:** stačí tooset si jednotné přihlašování (SSO) mezi místním adresářem a službou Azure AD. Tak dlouho, dokud cloudové aplikace přistupujete přes Azure AD, hello služba automaticky povede uživatele toocorrectly ověřování pomocí jejich místních přihlašovacích údajů.

Implementaci jednotného přihlašování z místního prostředí lze snadno nastavit pomocí federačních řešení, jako je například služba Active Directory Federation Services (AD FS), nebo konfigurací synchronizace hodnot hash hesel. Obě možnosti můžete snadno nasadit pomocí Průvodce konfigurací hello Azure AD Connect.

Další informace najdete v článku [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).

- - -
**Otázka: Poskytuje Azure AD samoobslužný portál pro uživatele v naší organizaci?**

**Odpověď:** Ano, Azure AD poskytuje hello [přístupový Panel Azure AD](http://myapps.microsoft.com) pro uživatelskou samoobsluhu a přístup k aplikaci. Pokud jste zákazníkem služby Office 365, najdete mnoho hello stejné funkce portálu hello Office 365.

Další informace najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

- - -
**Otázka: Pomůže mi Azure AD spravovat místní infrastrukturu?**

**Odpověď:** Ano. edice Azure AD Premium Hello vám poskytne Azure AD Connect Health. Azure AD Connect Health pomáhá monitorovat a proniknout do vaší identity místní infrastruktury a hello synchronizační služby.  

Další informace najdete v tématu [monitorování vaší místní infrastruktury identit a synchronizace služeb v cloudu hello](active-directory-aadconnect-health.md).  

- - -
## <a name="password-management"></a>Správa hesel
**Otázka: Je možné použít zpětný zápis hesla služby Azure AD bez synchronizace hesla? (V tomto scénáři je možné toouse Azure AD samoobslužné resetování hesla (SSPR) s heslo zpětný zápis a ne úložiště hesel v cloudu hello?)**

**Odpověď:** není nutné toosynchronize vaší služby Active Directory hesla tooAzure AD tooenable zpětný zápis. Ve federovaném prostředí Azure AD jednotné přihlašování (SSO) spoléhá na hello místní adresář tooauthenticate hello uživatele. Tento scénář nevyžaduje hello místní heslo toobe sledovat ve službě Azure AD.

- - -
**Otázka: jak dlouho trvá heslo toobe, zapisovat zpátky tooActive na místní adresáře?**

**Odpověď:** Zpětný zápis hesla funguje v reálném čase.

Další informace najdete v tématu [Začínáme se správou hesel](active-directory-passwords-getting-started.md).

- - -
**Otázka: Je možné použít zpětný zápis hesel, která spravuje správce?**

**Odpověď:** Ano, pokud máte zpětný zápis hesla povolena, hello heslo provádí správce zapíšou se operace zpět tooyour v místním prostředí.  

Další odpovědi najdete informace o toopassword [nejčastější dotazy se správou hesel](active-directory-passwords-faq.md).
- - -
**Otázka: Co mám dělat, když nepamatuji si stávající heslo Office 365 nebo Azure AD při pokusu o toochange hesla?**

**Odpověď:** Pro takové situace existuje řada možností.  Pokud je k dispozici, použijte samoobslužné resetování hesla.  Fungování samoobslužného resetování hesla závisí na jeho konfiguraci.  Další informace najdete v tématu [jak hello heslo resetovat portálu pracovní](active-directory-passwords-best-practices.md).

Pro uživatele služeb Office 365, může správce resetovat heslo hello pomocí hello kroků uvedených v [resetovat hesla uživatelů](https://support.office.com/en-us/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US).

Pro účty Azure AD můžete správci resetování hesla pomocí jedné z následujících hello:

- [Resetování účtů v hello portálu Azure](active-directory-users-reset-password-azure-portal.md)
- [Resetování účtů portálu classic hello](active-directory-create-users-reset-password.md)
- [Pomocí PowerShellu](/powershell/module/msonline/set-msoluserpassword?view=azureadps-1.0)


- - -
## <a name="security"></a>Zabezpečení
**Otázka: Uzamknou se účty po určitém počtu neúspěšných pokusů o přihlášení, nebo se používá složitější strategie?**</br>
Používáme sofistikovanější účty toolock strategie.  To je založené na hello IP hello požadavku a zadaná hesla hello. Doba trvání Hello hello uzamčení se taky zvýší podle hello pravděpodobnost, že se jedná o útoku.  

**Otázka: určité (běžné) hesla získat odmítne kvůli hello zprávy 'Toto heslo bylo použité toomany časy', to získáte toopasswords použít v aktuální službě active directory hello?**</br>
Vztahuje se toopasswords, které jsou globálně společné, například všechny varianty "Password" a "123456".

**Otázka: Budou všechny žádosti o přihlášení z podezřelých zdrojů (botnety, koncový bod tor) blokované v případě tenanta B2C, nebo to vyžaduje tenanta edice Basic nebo Premium?**</br>
Máme bránu, která filtruje požadavky a nabízí určitou ochranu před botnety a která se používá pro všechny tenanty B2C.

## <a name="application-access"></a>Přístup k aplikaci
**Otázka: Kde najdu seznam aplikací, které jsou předem integrovány se službou Azure AD a jejími funkcemi?**

**Odpověď:** Azure AD má více než 2 600 předem integrovaných aplikací od společnosti Microsoft, poskytovatelů služeb aplikací a partnerů. Všechny předem integrované aplikace podporují jednotné přihlašování. Jednotné přihlašování umožňuje využívat tooaccess vaše firemní přihlašovací údaje vaší aplikace. Některé aplikace hello také podporují automatické zřizování a jeho rušení.

Úplný seznam předem integrovaných aplikací hello najdete v tématu hello [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).

- - -
**Otázka: Co když hello aplikace, které je potřeba není hello Azure AD Marketplace?**

**Odpověď:** Se službou Azure AD Premium můžete přidávat a konfigurovat libovolné aplikace. V závislosti na funkcích aplikace a předvolbách můžete nakonfigurovat jednotné přihlašování a automatické zřizování.  

Další informace naleznete v tématu:

* [Konfigurace jednoho přihlášení tooapplications které nejsou v galerii aplikací Azure Active Directory hello](active-directory-saas-custom-apps.md)
* [Pomocí SCIM tooenable automatické zřizování uživatelů a skupin ze služby Azure Active Directory tooapplications](active-directory-scim-provisioning.md)

- - -
**Otázka: jak uživatelé přihlásit tooapplications pomocí Azure AD?**

**Odpověď:** Azure AD poskytuje několik způsobů pro uživatele tooview a přístupu k aplikacím, jako například:

* panel přístupu Hello Azure AD
* Spouštěč aplikace Hello Office 365
* Přímé přihlášení toofederated aplikace
* Přímé odkazy toofederated, založené na heslech, nebo existující aplikace

Další informace najdete v tématu [nasazení služby Azure AD integrovaných aplikací toousers](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

- - -
**Otázka: co je hello různými způsoby Azure AD umožňuje ověřování a tooapplications přihlášení?**

**Odpověď:** Azure AD podporuje mnoho standardizovaných protokolů pro ověřování a autorizaci, například SAML 2.0, OpenID Connect, OAuth 2.0 a WS-Federation. Azure AD podporuje funkce ukládání hesel do trezoru a automatického přihlašování pro aplikace, které podporují pouze ověření na základě formuláře.   

Další informace naleznete v tématu:

* [Scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md)
* [Protokoly ověřování pro Active Directory](https://msdn.microsoft.com/library/azure/dn151124.aspx)
* [Jak funguje jednotné přihlašování pomocí služby Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

- - -
**Otázka: Je možné přidat místní aplikace?**

**Odpověď:** proxy aplikace služby Azure AD poskytují snadný a bezpečný přístup tooon místní webové aplikace, které zvolíte. Aplikace v hello můžete používat stejným způsobem jako přístup váš software jako služba (SaaS) aplikace ve službě Azure AD. Není nutné pro sítě VPN nebo toochange infrastruktura vaší sítě.  

Další informace najdete v tématu [jak tooprovide zabezpečený vzdálený přístup tooon místní aplikace](active-directory-application-proxy-get-started.md).

- - -
**Otázka: Jak můžu vyžadovat vícefaktorové ověřování pro uživatele, kteří používají určitou aplikaci?**

**Odpověď:** Díky podmíněnému přístupu ke službě Azure AD můžete ke každé aplikaci přiřadit jedinečné zásady přístupu. V zásadách můžete požadovat použití vícefaktorového ověřování vždy nebo pokud uživatelé nejsou připojené toohello místní sítě.  

Další informace najdete v tématu [zabezpečení přístupu tooOffice 365 a další aplikace připojeným tooAzure služby Active Directory](active-directory-conditional-access.md).

- - -
**Otázka: Co je automatické zřizování uživatelů pro aplikace SaaS?**

**Odpověď:** tooautomate hello vytváření, údržbu a odebírání uživatelských identit v mnoha oblíbených cloudových aplikací SaaS používání Azure AD.

Další informace najdete v tématu [automatizace zřizování uživatelů a rušení zajištění tooSaaS aplikací s Azure Active Directory](active-directory-saas-app-provisioning.md).

- - -
**Otázka: Je možné vytvořit zabezpečené připojení LDAP se službou Azure Active Directory?**

**Odpověď:** Ne. Azure AD nepodporuje protokol LDAP hello.
