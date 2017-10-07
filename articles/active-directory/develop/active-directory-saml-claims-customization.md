---
title: "aaaCustomizing deklarace identity vystavené v tokenu hello SAML pro předběžně integrované aplikace v Azure Active Directory | Microsoft Docs"
description: "Další způsob vystavování deklarací identity hello toocustomize v hello tokenu SAML pro předběžně integrované aplikace v Azure Active Directory"
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: a376318929472403e799f02fdd3fbddc91d0a70c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customizing-claims-issued-in-hello-saml-token-for-pre-integrated-apps-in-azure-active-directory"></a>Přizpůsobují se deklarace identity vystavené v hello tokenu SAML pro předběžně integrované aplikace v Azure Active Directory
Dnes Azure Active Directory podporuje tisícům předem integrovaných aplikací v hello Azure AD Application Gallery, včetně přes 360, které podporují jednotné přihlašování pomocí protokolu hello SAML 2.0. Když se uživatel ověřuje tooan aplikací prostřednictvím služby Azure AD pomocí SAML, Azure AD odešle token toohello aplikaci (přes HTTP POST). A pak aplikace hello ověří a použije hello tokenu toolog hello uživatele v místo výzvy k zadání uživatelského jména a hesla. Tyto tokeny SAML obsahují informace o uživateli hello známé jako "deklarace identity".

Ve vztahu identity Předčítání, "deklarace identity" je informace, které stavy zprostředkovatele identity o uživateli uvnitř tokenu hello, vydaného pro tohoto uživatele. V [tokenu SAML](http://en.wikipedia.org/wiki/SAML_2.0), tato data je obvykle součástí hello příkaz atribut SAML. Hello jedinečné ID uživatele je obvykle reprezentována hello SAML subjektu také nazývané jako název identifikátor.

Ve výchozím nastavení vydá Azure Active Directory aplikaci tooyour tokenu SAML, která obsahuje deklaraci NameIdentifier s hodnotou hello uživatelské jméno uživatele (hlavní název uživatele NEBOLI) ve službě Azure AD. Tato hodnota může jedinečně identifikovat hello uživatele. Hello SAML token také obsahuje další deklarace obsahující hello uživatele e-mailová adresa, jméno a příjmení.

tooview nebo upravit hello deklarací identity vystavené v hello SAML token toohello aplikace, otevřete hello aplikace na portálu Azure. Potom vyberte hello **zobrazit a upravit všechny ostatní atributy uživatele** zaškrtnout políčko hello **uživatelské atributy** oddílu aplikace hello.

![Uživatelské atributy oddílu][1]

Existují dvě možné důvody, proč může být nutné tooedit hello deklarace identity vystavené v tokenu SAML hello:
* aplikace Hello byla zapsána toorequire jinou sadu deklarací identity identifikátory URI nebo hodnot deklarací identity.
* nasazení aplikace Hello způsobem, který vyžaduje hello NameIdentifier deklarace identity toobe něco jiného než hello uživatelské jméno (hlavní název uživatele NEBOLI) uložené ve službě Azure Active Directory.

Můžete upravit všechny hodnoty deklarací výchozí hello. Vyberte řádek hello deklarace identity v tabulce atributy tokenu SAML hello. Tím se otevře hello **Upravit atribut** části a pak můžete upravit název deklarace identity, hodnotu a přidružený hello deklaraci oboru názvů.

![Upravit atribut uživatele][2]

Rovněž můžete odebrat deklarace (jiné než NameIdentifier) pomocí hello kontextové nabídky, otevře se kliknutím na hello **...**  ikonu.  Můžete také přidat nové deklarace pomocí hello **přidat atribut** tlačítko.

![Upravit atribut uživatele][3]

## <a name="editing-hello-nameidentifier-claim"></a>Úpravy hello NameIdentifier deklarace identity
toosolve hello problém, kde byla nasazena aplikace hello pomocí jiné uživatelské jméno, klikněte na hello **uživatelský identifikátor** rozevírací nabídku v hello **uživatelské atributy** části. Tato akce poskytuje dialogové okno s několik možností:

![Upravit atribut uživatele][4]

V hello rozevíracího seznamu vyberte **user.mail** tooset hello NameIdentifier deklarace identity e-mailovou adresu uživatele hello toobe v adresáři hello. Nebo vyberte **user.onpremisessamaccountname** název účtu SAM tooset toohello uživatele, který se synchronizovaly z místní služby Azure AD.

Můžete také použít speciální hello **ExtractMailPrefix()** funkce tooremove hello příponu domény z hello e-mailovou adresu, název účtu SAM nebo hlavní název uživatele hello. To vyextrahuje pouze první část hello hello uživatele název předávány prostřednictvím (například "joe_smith" místo joe_smith@contoso.com).

![Upravit atribut uživatele][5]

Jsme nyní jste také přidali hello **join()** funkce toojoin hello ověřené doméně s hello hodnota identifikátoru uživatele. Když vyberete funkce join() hello hello **uživatelský identifikátor** vyberte nejdřív hello uživatelský identifikátor jako jako e-mailovou adresu nebo uživatele hlavní název a potom hello druhý rozevírací nabídky vyberte ověřené domény. Pokud vyberete hello e-mailovou adresu s hello ověřené domény, pak Azure AD extrahuje hello uživatelského jména z hello první hodnota joe_smith z joe_smith@contoso.com a jeho připojení s contoso.onmicrosoft.com. Viz následující ukázka hello:

![Upravit atribut uživatele][6]

## <a name="adding-claims"></a>Přidání deklarace identity
Při přidání deklarace identity, můžete zadat název atributu hello, (která nepotřebuje výhradně toofollow vzor URI podle specifikace SAML hello). Nastavte hello hodnota tooany uživatele atribut, který je uložen v adresáři hello.

![Přidat atribut uživatele][7]

Například můžete potřebovat toosend hello oddělení, které hello uživatele patří tooin organizace jako deklarace identity (například prodej). Zadejte název deklarací hello podle očekávání hello aplikací a potom vyberte **user.department** jako hodnota hello.

> [!NOTE]
> Pokud pro daného uživatele neexistuje žádná hodnota uložené pro vybraný atribut, není se této deklarace identity vystavené v tokenu hello.

> [!TIP]
> Hello **user.onpremisesecurityidentifier** a **user.onpremisesamaccountname** podporují jenom při synchronizaci dat uživatele z místní služby Active Directory pomocí hello [Azure Nástroje služby AD Connect](../active-directory-aadconnect.md).

## <a name="restricted-claims"></a>Deklarace identity s omezeným přístupem

Existují některé s omezeným přístupem deklarace identity v SAML. Pokud přidáte tyto deklarace, Azure AD, nebude odesílat tyto deklarace identity. Následují hello SAML omezená sada deklarací identity:

    | Typ deklarace identity (URI) |
    | ------------------- |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/Expiration |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/Expired |
    | http://schemas.microsoft.com/identity/Claims/accesstoken |
    | http://schemas.microsoft.com/identity/Claims/openid2_id |
    | http://schemas.microsoft.com/identity/Claims/identityprovider |
    | http://schemas.microsoft.com/identity/Claims/objectidentifier |
    | http://schemas.microsoft.com/identity/Claims/PUID |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/NameIdentifier [MR1] |
    | http://schemas.microsoft.com/identity/Claims/tenantid |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/AuthenticationInstant |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/AuthenticationMethod |
    | http://schemas.microsoft.com/accesscontrolservice/2010/07/Claims/identityprovider |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/groups |
    | http://schemas.microsoft.com/Claims/groups.Link |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/role |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/wids |
    | http://schemas.microsoft.com/2014/09/devicecontext/Claims/iscompliant |
    | http://schemas.microsoft.com/2014/02/devicecontext/Claims/isknown |
    | http://schemas.microsoft.com/2012/01/devicecontext/Claims/ismanaged |
    | http://schemas.microsoft.com/2014/03/psso |
    | http://schemas.microsoft.com/Claims/authnmethodsreferences |
    | http://schemas.xmlsoap.org/ws/2009/09/identity/Claims/actor |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/samlissuername |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/confirmationkey |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsaccountname |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/primarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/primarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/authorizationdecision |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/Authentication |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/SID |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/denyonlyprimarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/denyonlyprimarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/denyonlysid |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/denyonlywindowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsdeviceclaim |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsfqbnversion |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowssubauthority |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsuserclaim |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/x500distinguishedname |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/UPN |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/GroupSID |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/SPN |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/ispersistent |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/privatepersonalidentifier |
    | http://schemas.microsoft.com/identity/Claims/scope |

## <a name="next-steps"></a>Další kroky
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](../active-directory-apps-index.md)
* [Konfigurace jednoho přihlášení tooapplications které nejsou v galerii aplikací Azure Active Directory hello](../active-directory-saas-custom-apps.md)
* [Řešení potíží s na základě SAML jednotné přihlašování](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/user-attribute-section.png
[2]: ./media/active-directory-saml-claims-customization/edit-claim-name-value.png
[3]: ./media/active-directory-saml-claims-customization/delete-claim.png
[4]: ./media/active-directory-saml-claims-customization/user-identifier.png
[5]: ./media/active-directory-saml-claims-customization/extractemailprefix-function.png
[6]: ./media/active-directory-saml-claims-customization/join-function.png
[7]: ./media/active-directory-saml-claims-customization/add-attribute.png