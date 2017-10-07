---
title: "Azure AD Connect: Přihlášení uživatele | Microsoft Docs"
description: "Azure AD Connect přihlášení uživatele pro vlastní nastavení."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 547b118e-7282-4c7f-be87-c035561001df
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 7848b419f3855b25cfa074a46779d258bd534bae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-user-sign-in-options"></a>Azure AD Connect uživatelské možnosti přihlášení
Připojení služby Azure Active Directory (Azure AD) umožňuje vaši uživatelé toosign v tooboth cloudu a místních prostředků pomocí hello stejnými hesly. Tento článek popisuje klíčové koncepty pro každý model toohelp identity, že si vyberete hello identitu, která má toouse pro přihlášení tooAzure AD.

Pokud jste již obeznámeni s modelem identity hello Azure AD a chcete další informace o konkrétní metody toolearn, najdete v části hello příslušný odkaz:

* [Synchronizace hesel](#password-synchronization) s [jednotné přihlašování (SSO)](active-directory-aadconnect-sso.md)
* [Předávací ověřování](active-directory-aadconnect-pass-through-authentication.md)
* [Federované jednotné přihlašování (službou Active Directory Federation Services (AD FS))](#federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2)

## <a name="choosing-hello-user-sign-in-method-for-your-organization"></a>Výběr hello uživatele přihlašovací metoda pro vaši organizaci
Pro většinu organizací, které právě mají tooenable uživatele přihlásit tooOffice 365, aplikace SaaS a jiné na základě AD prostředky Azure doporučujeme možnost synchronizace hesla výchozí hello. Některé organizace, ale mají konkrétní důvod, že nejsou možné toouse tuto možnost. Můžete se buď federované možnost přihlášení, například služby AD FS nebo předávací ověřování. Můžete použít následující tabulky toohelp Zkontrolujte správnou volbou hello hello.

Potřebuji příliš| PS pomocí jednotného přihlašování| PA pomocí jednotného přihlašování| SLUŽBA AD FS |
 --- | --- | --- | --- |
Automaticky synchronizujte nové, obraťte se na, účty uživatelů a skupin v cloudu toohello místní služby Active Directory.|x|x|x|
Nastavení klienta pro Office 365 hybridní scénáře.|x|x|x|
Povolte Moje toosign uživatelé v a cloudové služby přístup pomocí hesla pro místní.|x|x|x|
Implementaci jednotného přihlašování pomocí podnikové přihlašovací údaje.|x|x|x|
Ujistěte se, že se žádná hesla ukládat v cloudu hello.||x *|x|
Povolte místní služby Multi-Factor authentication řešení.|||x|

* Prostřednictvím lightweight konektor.

>[!NOTE]
> Předávací ověřování aktuálně má určitá omezení s bohatých klientů. V tématu [předávací ověřování](active-directory-aadconnect-pass-through-authentication.md) další podrobnosti.

### <a name="password-synchronization"></a>Synchronizace hesel
Synchronizace hesla jsou synchronizovány hodnot hash hesel uživatelů z místní služby Active Directory tooAzure AD. Při hesla se změněným nebo resetovaným místní, hello nová hesla jsou synchronizovaná tooAzure AD okamžitě tak, aby vaši uživatelé můžete vždy použít hello stejné heslo pro cloudové a místní prostředků. Hello hesla se nikdy odeslat tooAzure AD nebo uložené ve službě Azure AD ve formátu prostého textu. Můžete používat synchronizaci hesla společně s heslo zpětný zápis tooenable samoobslužného resetování hesla ve službě Azure AD.

Kromě toho můžete povolit [jednotného přihlašování k](active-directory-aadconnect-sso.md) pro uživatele v doméně počítače, které jsou v podnikové síti hello. Pokud se jeden přihlašování, je povoleno pouze pro uživatele musí tooenter toohelp uživatelské jméno je bezpečný přístup k prostředkům cloudu.

![Synchronizace hesel](./media/active-directory-aadconnect-user-signin/passwordhash.png)

Další informace najdete v tématu hello [synchronizace hesel](active-directory-aadconnectsync-implement-password-synchronization.md) článku.

### <a name="pass-through-authentication"></a>Předávací ověřování
S předávací ověřování hello uživatelské heslo bude ověřeno řadič hello místní služby Active Directory. heslo Hello nepotřebuje toobe existuje ve službě Azure Active Directory žádný formulář. To umožňuje místní zásady, třeba hodinu přihlašovací omezení, toobe vyhodnotí během ověřování toocloud služby.

Předávací ověřování používá jednoduchý agenta na počítači připojeném k doméně systému Windows Server 2012 R2 v hello v místním prostředí. Tento agent naslouchá požadavkům ověřování hesla. Nevyžaduje žádné příchozí porty toobe otevřete toohello Internetu.

Kromě toho můžete také povolit jednotné přihlašování pro uživatele v doméně počítače, které jsou v podnikové síti hello. Pokud se jeden přihlašování, je povoleno pouze pro uživatele musí tooenter toohelp uživatelské jméno je bezpečný přístup k prostředkům cloudu.
![Předávací ověřování](./media/active-directory-aadconnect-user-signin/pta.png)

Další informace naleznete v tématu:
- [Předávací ověřování](active-directory-aadconnect-pass-through-authentication.md)
- [Jednotné přihlašování](active-directory-aadconnect-sso.md)

### <a name="federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2"></a>Federace, která používá nový nebo existující farmy se službou AD FS ve Windows serveru 2012 R2
Federované přihlášení uživatelé můžou přihlásit tooAzure založené na AD službám pomocí jejich místních hesel. Když jsou v podnikové síti hello i nemají tooenter jejich hesla. Při použití možnosti hello federační službou AD FS můžete nasadit nové nebo existující farmy se službou AD FS ve Windows serveru 2012 R2. Pokud si zvolíte toospecify existující farmy, nakonfiguruje Azure AD Connect, aby uživatelé mohli podepsat hello vztah důvěryhodnosti mezi farmy a Azure AD.

<center>![Federace se službou AD FS ve Windows serveru 2012 R2](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="deploy-federation-with-ad-fs-in-windows-server-2012-r2"></a>Nasazení federační službou AD FS ve Windows serveru 2012 R2

Pokud nasazujete novou farmu, je třeba:

* Server Windows Server 2012 R2 pro federační server hello.
* Server Windows Server 2012 R2 pro hello Proxy webových aplikací.
* Soubor .pfx pro název vaší služby federačního určený s jedním certifikátem SSL. Příklad: fs.contoso.com.

Pokud provádíte nasazení nové farmy nebo použití existující farmy, je třeba:

* Přihlašovací údaje místního správce na federačních serverech.
* Pověření místního správce na žádném ze serverů pracovní skupiny (není připojený k doméně), že máte v úmyslu toodeploy hello role Proxy webových aplikací na.
* Hello počítač spustit Průvodce hello toobe možné tooconnect tooany jiné počítače, který má tooinstall služby AD FS nebo Proxy webových aplikací na pomocí vzdálené správy systému Windows.

Další informace najdete v tématu [Konfigurace jednotného přihlašování se službou AD FS](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs).

#### <a name="sign-in-by-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>Přihlaste se pomocí dřívější verze služby AD FS nebo řešení třetí strany
Pokud jste již nakonfigurovali cloudu přihlásit pomocí starší verze služby AD FS (například služby AD FS 2.0) nebo zprostředkovatele federování třetích stran, můžete tooskip uživatele přihlásit konfigurace přes Azure AD Connect. To vám umožní tooget hello nejnovější synchronizace a dalším funkcím služby Azure AD Connect při stále pomocí stávajícího řešení pro přihlášení.

Další informace najdete v tématu hello [seznam kompatibility federace třetí strany služby Azure AD](active-directory-aadconnect-federation-compatibility.md).


## <a name="user-sign-in-and-user-principal-name"></a>Přihlášení uživatele a hlavní název uživatele
### <a name="understanding-user-principal-name"></a>Principy hlavní název uživatele
Ve službě Active Directory je příponou hlavního názvu (UPN) uživatele hello výchozí název DNS hello hello domény, které bylo vytvořeno hello uživatelský účet. Ve většině případů je to hello název domény, který je registrovaný jako doménu podnikové hello na hello Internetu. Můžete však přidat další přípony UPN s použitím domény služby Active Directory a vztahy důvěryhodnosti.

Hello UPN hello uživatele má formát hello username@domain. Pro doménu služby Active Directory s názvem "contoso.com", například uživatel s názvem Jan může mít hello UPN "john@contoso.com". Hello UPN hello uživatele je založena na RFC 822. I když hello UPN a sdílení e-mailu hello stejný formát, hello hodnotu hello UPN pro uživatele může nebo nemusí být stejný jako hello e-mailovou adresu uživatele hello hello.

### <a name="user-principal-name-in-azure-ad"></a>Hlavní název uživatele ve službě Azure AD
Průvodce Azure AD Connect Hello používá atribut userPrincipalName hello nebo umožňuje, který zadáte hello toobe atribut (ve vlastní instalaci) používat z místního jako hlavní název uživatele hello ve službě Azure AD. Toto je hello hodnotu, která se používá k podepisování v tooAzure AD. Pokud hodnota hello atribut userPrincipalName hello neodpovídá tooa ověřen domény ve službě Azure AD, Azure AD ho nahradí výchozí. onmicrosoft.com hodnotu.

Každý adresář v Azure Active Directory se dodává s názvem předdefinované domény, hello formátu contoso.onmicrosoft.com, že umožňuje můžete začít používat Azure nebo jiných službách Microsoftu. Můžete zlepšit a zjednodušit hello přihlašování pomocí vlastních domén. Informace o vlastních názvů domén ve službě Azure AD a jak tooverify domény, najdete v části [přidat název tooAzure vaši vlastní doménu služby Active Directory](../add-custom-domain.md#add-your-custom-domain).

## <a name="azure-ad-sign-in-configuration"></a>Konfigurace přihlášení k Azure AD
### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure AD přihlášení konfigurace službou Azure AD Connect
Hello Azure AD přihlášení závisí na tom, jestli může Azure AD shodovat s příponou hlavního názvu uživatele hello uživatele, který se synchronizoval tooone hello vlastní domény, která jsou ověřena v hello adresář Azure AD. Azure AD Connect poskytuje pomoc při konfiguraci nastavení služby Azure AD přihlášení, tak, aby hello přihlášení činnost koncového uživatele v cloudu hello podobné toohello místní prostředí.

Azure AD Connect seznamy hello přípon UPN, které jsou definovány pro domény a pokusů toomatch hello je s vlastní domény ve službě Azure AD. Potom pomáhá s hello příslušné akce, kterou je toobe provedena.
přihlašovací stránku Hello Azure AD uvádí hello přípon UPN, které jsou definovány pro místní služby Active Directory a zobrazí odpovídající stav hello před každou příponu. hodnoty stavu Hello může být jedna z následujících hello:

| Stav | Popis | Akce, které jsou potřeba |
|:--- |:--- |:--- |
| Ověření |Azure AD Connect nalezena že odpovídající ověření domény ve službě Azure AD. Všechny uživatele pro tuto doménu můžete přihlásit pomocí jejich místních přihlašovacích údajů. |Není vyžadována žádná akce. |
| Nebyla ověřena. |Azure AD Connect v Azure AD najít odpovídající vlastní doménu, ale není ověřen. přípona UPN Hello hello uživatelů tato doména bude změnit výchozí toohello. přípona onmicrosoft.com po synchronizaci pokud hello doména není ověřena. | [Ověření vlastní domény hello ve službě Azure AD.](../add-custom-domain.md#verify-the-domain-name-with-azure-ad) |
| Nebyla přidána |Azure AD Connect nenašli tuto příponu UPN korespondenční toohello vlastní doménu. přípona UPN Hello hello uživatelů tato doména bude změnit výchozí toohello. přípona onmicrosoft.com, pokud není hello domény přidat a ověřit v Azure. | [Přidání a ověření vlastní domény, která odpovídá toohello příponu UPN.](../add-custom-domain.md) |

přihlašovací stránku Hello Azure AD uvádí hello přípon UPN, které jsou definovány pro místní služby Active Directory a hello odpovídající vlastní domény ve službě Azure AD s aktuální stav ověření hello. Ve vlastní instalaci, můžete nyní vybrat hello atribut pro hello hlavní název uživatele na hello **přihlášení k Azure AD** stránky.

![Azure AD přihlašovací stránky](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

Můžete kliknout na hello tlačítko toore načítání hello nejnovější stav aktualizace vlastní domény hello z Azure AD.

### <a name="selecting-hello-attribute-for-hello-user-principal-name-in-azure-ad"></a>Výběr hello atribut pro hlavní název uživatele hello ve službě Azure AD
Hello atribut userPrincipalName je atribut hello, který uživatelé používají při přihlášení tooAzure AD a Office 365. Měli byste ověřit hello domén (označované také jako přípony UPN), které se používají ve službě Azure AD před synchronizací uživatelů hello.

Důrazně doporučujeme, abyste hello výchozí atribut userPrincipalName. Pokud tento atribut je nepoužívající a nedá se ověřit, pak je možné tooselect jiný atribut (například e-mail) jako hello atribut, který obsahuje hello přihlášení. To se označuje jako hello alternativní ID. Hodnota atributu alternativní ID Hello musí následovat hello RFC 822 standardní. Můžete vytvořit alternativní ID se heslo jednotné přihlašování i jako řešení přihlašování hello federaci jednotné přihlašování.

> [!NOTE]
> Použití atributu alternativní ID není kompatibilní se všemi úlohami Office 365. Další informace najdete v tématu [konfigurace alternativního přihlašovacího ID](https://technet.microsoft.com/library/dn659436.aspx).
>
>

#### <a name="different-custom-domain-states-and-their-effect-on-hello-azure-sign-in-experience"></a>Stavy jiné vlastní domény a jejich vliv na prostředí hello Azure přihlášení
Je velmi důležité toounderstand hello vztah mezi stavy hello vlastní doménu v adresáři služby Azure AD a hello přípon UPN, které jsou definované v místě. Přejděte prostřednictvím hello různé možné Azure přihlášení při nastavení synchronizace pomocí Azure AD Connect.

Pro hello následující informace, Předpokládejme, že nám nevadí hello (UPN) příponu contoso.com, který je používán hello místního adresáře v rámci UPN – například user@contoso.com.

###### <a name="express-settingspassword-synchronization"></a>Expresní nastavení a hesla synchronizace
| Stav | Vliv na činnost koncového uživatele Azure přihlášení |
|:---:|:--- |
| Nebyla přidána |V takovém případě žádné vlastní domény pro doménu contoso.com přidala hello adresář Azure AD. Uživatelé, kteří mají UPN místně s příponou hello @contoso.com nebude možné toouse jejich toosign místní UPN v tooAzure. Budete místo toho mají toouse nového názvu UPN, které poskytl toothem službou Azure AD přidáním hello přípony pro adresář Azure AD výchozí hello. Například pokud jste synchronizuje azurecontoso.onmicrosoft.com directory uživatelé toohello Azure AD, pak hello místní uživatel user@contoso.com bude mít název UPN user@azurecontoso.onmicrosoft.com. |
| Nebyla ověřena. |V takovém případě máme vlastní domény contoso.com, který je přidaný do adresáře hello Azure AD. Nicméně je ještě nebyl ověřen. Pokud jste pokračujte s synchronizaci uživatelů bez ověření domény hello, pak hello uživatelé nebudou přiřazovány nové hlavní název uživatele Azure AD, stejně jako v hello "Nepřidáno" scénář. |
| Ověření |V takovém případě máme vlastní domény contoso.com, které již přidat a ověřit ve službě Azure AD pro hello přípona UPN. Uživatelé budou moct toouse jejich místní hlavní název uživatele, například user@contoso.com, toosign v tooAzure poté, co jste se synchronizovaly tooAzure AD. |

###### <a name="ad-fs-federation"></a>Federaci služby AD FS
Nelze vytvořit federaci s výchozí hello. doméně onmicrosoft.com v Azure AD nebo neověřené vlastní domény ve službě Azure AD. Když používáte Průvodce hello Azure AD Connect, pokud jste vybrali toocreate neověřené domény federaci se pak Azure AD Connect výzvy k hello nezbytné zaznamenává toobe vytvořit je hostitelem serveru DNS pro doménu hello. Další informace najdete v tématu [vybrané pro federaci ověřte, zda text hello Azure AD domény](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Pokud jste vybrali možnost přihlášení uživatele hello **federační službou AD FS**, pak musí mít vlastní domény toocontinue, vytváření federace ve službě Azure AD. Naše diskusi to znamená, že jsme by měl mít vlastní domény contoso.com, přidat v adresáři hello Azure AD.

| Stav | Vliv na činnost koncového uživatele Azure přihlášení hello |
|:---:|:--- |
| Nebyla přidána |V takovém případě Azure AD Connect nebyl nalezen odpovídající vlastní domény pro doménu contoso.com příponu UPN hello v hello adresář Azure AD. Budete potřebovat tooadd vlastní doménu contoso.com, pokud potřebujete toosign uživatelé v pomocí služby AD FS s jejich místní UPN (jako je user@contoso.com). |
| Nebyla ověřena. |Azure AD Connect v takovém případě vyzve příslušné podrobnosti o tom, jak můžete ověřit doménu v pozdější fázi. |
| Ověření |V takovém případě můžete přejít k tématu s konfigurací hello bez jakékoli další akce. |

## <a name="changing-hello-user-sign-in-method"></a>Změna metoda přihlašování uživatele hello
Metoda přihlašování uživatele hello z federačního, synchronizace hesel nebo předávací ověřování můžete změnit pomocí hello úloh, které jsou k dispozici ve službě Azure AD Connect po počáteční konfiguraci hello služby Azure AD Connect pomocí Průvodce hello. Znovu spusťte Průvodce Azure AD Connect hello a zobrazí se seznam úloh, které můžete provádět. Vyberte **změnit přihlášení uživatele** hello seznamu úloh.

![Změnit přihlášení uživatele](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Na další stránku hello zobrazí se dotaz tooprovide hello pověření pro Azure AD.

![Připojit tooAzure AD](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

Na hello **přihlášení uživatele** vyberte přihlášení hello požadovaného uživatele.

![Připojit tooAzure AD](./media/active-directory-aadconnect-user-signin/changeusersignin2a.png)

> [!NOTE]
> Pokud chcete vytvořit pouze dočasné přepínač toopassword synchronizace, pak vyberte hello **nepřevádějí uživatelské účty** zaškrtávací políčko. Není kontrola hello možnost převede toofederated každý uživatel a může trvat několik hodin.
>
>

## <a name="next-steps"></a>Další kroky
- Další informace o [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
- Další informace o [koncepty návrhu Azure AD Connect](active-directory-aadconnect-design-concepts.md).
