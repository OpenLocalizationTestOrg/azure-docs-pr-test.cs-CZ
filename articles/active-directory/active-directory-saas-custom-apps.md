---
title: "aaaConfigure Azure AD jednotného přihlašování pro aplikace | Microsoft Docs"
description: "Zjistěte, jak připojit tooself služby tooAzure aplikace služby Active Directory pomocí SAML a jednotné přihlašování založené na heslech"
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: 0d42eb0c-6d3f-4557-9030-e88e86709a19
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.reviewer: luleon
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 002a19a6c7ad25ea2f3b9c6a7c7874ed2be23cce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-single-sign-on-tooapplications-that-are-not-in-hello-azure-active-directory-application-gallery"></a>Konfigurace jednoho přihlášení tooapplications které nejsou v galerii aplikací Azure Active Directory hello
Tento článek se týká funkce, která umožňuje správci tooconfigure jeden přihlašování tooapplications nejsou k dispozici v galerii aplikací Azure Active Directory hello *bez nutnosti psaní kódu*. Tato funkce byla vydána z 18. listopadu 2015 technical preview a je součástí [Azure Active Directory Premium](active-directory-editions.md). Pokud místo toho hledáte vývojáře pokyny najdete v části toointegrate vlastních aplikací s Azure AD prostřednictvím kódu, [scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md).

Hello galerii aplikací Azure Active Directory poskytuje seznam aplikací, které jsou známé toosupport forma jednotné přihlašování s Azure Active Directory, jak je popsáno v [v tomto článku](active-directory-appssoaccess-whatis.md). Jakmile (jako IT specialista nebo systémový integrátor ve vaší organizaci) naleznete aplikace hello chcete tooconnect, abyste mohli začít podle postupujte podle hello podrobné pokyny uvedené v hello Azure management portal tooenable jednotné přihlašování.

Zákazníci s [Azure Active Directory Premium](active-directory-editions.md) licence získat také tyto další funkce:

* Samoobslužné integrace každou aplikaci, která podporuje poskytovatele identity SAML 2.0 (spouštěná SP nebo spouštěná IdP)
* Samoobslužné integrace webové aplikace, který má k HTML na přihlašovací stránce pomocí [jednotné přihlašování založené na heslech](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* Připojení aplikace, které používají protokol hello SCIM pro zřizování uživatelů samoobslužné služby ([zde popsané](active-directory-scim-provisioning.md))
* Možnost tooadd odkazy tooany aplikace hello [Spouštěč aplikace Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) nebo hello [přístupový panel Azure AD](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)

To může zahrnovat pouze aplikací SaaS použít ale ještě nebyly na zahrnuté toohello galerii aplikací Azure AD, ale třetí strany webové aplikace, které vaše organizace nasadil tooservers, kterou řídíte, buď v hello cloudu nebo místně.

Tyto možnosti, také známé jako *šablony integrace aplikace*, poskytovat založených na standardech spojovací body pro aplikace, které podporují SAML, SCIM nebo ověřování pomocí formulářů a zahrnuje nastavení pro kompatibilitu s mnoho aplikací a flexibilní možnosti. 

## <a name="adding-an-unlisted-application"></a>Přidání aplikace neuvedené
tooconnect aplikace pomocí šablony integrace aplikací, přihlaste se k hello portál pro správu Azure pomocí účtu správce služby Azure Active Directory a procházet toohello **služby Active Directory > [Directory] > aplikace**vyberte **přidat**a potom **přidat aplikaci z Galerie hello**. 

![][1]

Galerie hello aplikací, můžete přidat aplikace neuvedené pomocí hello **vlastní** kategorie na levé straně hello nebo výběrem hello **přidání aplikace neuvedené** odkaz, který se zobrazí v hledání hello výsledků, pokud požadované aplikace nebyl nalezen. Po zadání název vaší aplikace, můžete nakonfigurovat hello jeden přihlašování možnosti a chování. 

**Rychlý tip**: jako osvědčený postup použijte toosee toocheck funkce vyhledávání hello, pokud aplikace hello již existuje v galerii aplikací hello. Pokud je nalezen hello aplikace a její popis uvádí "jednotné přihlašování", pak aplikace hello je podporována pro federované jednotné přihlašování. 

![][2]

Přidání aplikace tímto způsobem poskytuje velmi podobné prostředí toohello, jednu pro předběžně integrované aplikace k dispozici. toostart, vyberte **nakonfigurovat jednotné přihlašování**. úvodní obrazovka další uvede hello následující tři možnosti pro konfiguraci jednotné přihlašování, které jsou popsané v následující části hello.

![][3]

## <a name="azure-ad-single-sign-on"></a>Azure AD jednotné přihlášení
Vyberte tuto možnost tooconfigure na základě SAML ověřování pro aplikaci hello. To vyžaduje, aby hello podporu aplikace SAML 2.0 a na tom, jak toouse hello funkce SAML hello aplikace před pokračováním by shromažďovat informace. Po výběru **Další**, bude výzvami tooenter tři různé adresy URL odpovídající toohello koncových bodů SAML pro aplikace hello. 

![][4]

Jsou to:

* **Přihlašovací adresa URL (spouštěná SP pouze)** – kde hello uživatel přejde toothis toosign v aplikaci. Aplikace hello je nakonfigurována jedním spouštěná poskytovatele služby tooperform přihlásit, pak když uživatel přejde toothis adresu URL, poskytovatele služeb hello bude hello tooauthenticate tooAzure AD nezbytné přesměrování a přihlásit uživatele hello v. Pokud toto pole se vyplní, Azure AD použije tato adresa URL toolaunch hello aplikace z Office 365 a hello přístupový Panel Azure AD. Pokud toto pole je ommited, a poté Azure AD bude místo toho proveďte zprostředkovatele identity-initiated přihlašování při hello spuštění aplikace z Office 365, hello přístupový Panel Azure AD, nebo z hello Azure AD jeden přihlašování adresy URL (copiable z karty řídicí panel hello).
* **URL vystavitele** -URL vystavitele hello jedinečně identifikoval hello aplikace, pro které jedním přihlásit se konfiguruje. Toto je hodnota text hello, Azure AD odešle back tooapplication jako hello **cílovou skupinu** parametr hello tokenu SAML a aplikace hello je očekávané toovalidate ho. Tato hodnota se rovněž zobrazuje jako hello **Entity ID** v veškerá metadata SAML poskytované aplikace hello. Podívejte se do dokumentace aplikace hello SAML podrobnosti k tomu, co je Entity ID nebo hodnota cílovou skupinu. Dole je příklad, jak se zobrazí v hello SAML token vrácený toohello aplikace hello URL cílové skupiny:

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **Adresa URL odpovědi** – adresa URL odpovědi hello je tam, kde aplikace hello očekává tooreceive hello SAML token. Toto je také hello odkazované tooas **Assertion příjemce Service (ACS) adresy URL**. Podívejte se do dokumentace aplikace hello SAML podrobnosti na to, co je jeho adresa URL odpovědi tokenu SAML nebo adresa URL služby ACS.
  Po zadání těchto klikněte na tlačítko **Další** tooproceed toohello další obrazovce. Tato obrazovka poskytuje informace o tom, jaké požadavky toobe na tooenable straně aplikace hello ho tooaccept tokenu SAML z Azure AD. 

![][5]

Hodnot, které jsou požadovány lišit v závislosti na aplikaci hello, takže podívejte se do dokumentace aplikace hello SAML podrobnosti. Hello **přihlašování** a **odhlášení** adresa URL služby obě řešení toohello stejný koncový bod, který je koncový bod zpracování požadavků hello SAML pro vaší instanci Azure AD. URL vystavitele Hello je hello hodnotu, která se zobrazí jako "Vystavitele" hello uvnitř hello aplikace vydaných toohello tokenu SAML. 

Po vaší aplikace byla nakonfigurovaná, klikněte na tlačítko **Další** tlačítko a pak hello **Complete** tooclose hello dialogu. 

## <a name="assigning-users-and-groups-tooyour-saml-application"></a>Přiřazení uživatelů a skupin tooyour SAML aplikace
Jakmile vaše aplikace nakonfigurovaná toouse Azure AD jako zprostředkovatele identity na základě SAML, už je téměř Hotovo tootest. Jako ovládací prvek zabezpečení Azure AD nevydá token, což jim toosign do aplikace hello Pokud kterým byl udělen přístup pomocí služby Azure AD. Uživatelé mohou mít udělen přístup přímo nebo prostřednictvím skupiny, které jsou členy. 

tooassign aplikaci tooyour uživatele nebo skupinu, klikněte na tlačítko hello **přiřadit uživatele** tlačítko. Vyberte hello uživatele nebo skupiny chcete tooassign a potom vyberte hello **přiřadit** tlačítko. 

![][6]

Přiřazení uživatele umožní Azure AD tooissue token pro uživatele hello, jakož i způsobuje dlaždice pro tuto aplikaci tooappear panelu hello uživatele přístup. Dlaždici aplikace se také zobrazí v Spouštěč aplikace hello Office 365, pokud uživatel hello používá Office 365. 

Můžete nahrávat logo dlaždice pro aplikace hello pomocí hello **nahrát Logo** na hello tlačítko **konfigurace** kartu pro aplikace hello. 

### <a name="customizing-hello-claims-issued-in-hello-saml-token"></a>Přizpůsobení hello deklarace identity vystavené v tokenu SAML hello
Když se uživatel ověřuje toohello aplikace, Azure AD vydá token toohello aplikace SAML, který obsahuje informace (nebo deklarace identity) o hello uživateli, který jedinečně identifikuje je. Ve výchozím nastavení to zahrnuje uživatelské jméno, e-mailová adresa, jméno a příjmení hello uživatele. 

Můžete zobrazit nebo upravit hello deklarace identity odeslat v hello aplikace toohello tokenu SAML pod hello **atributy** kartě. 

![][7]

Existují dvě možné důvody, proč může být nutné tooedit hello deklarace identity vystavené v tokenu SAML hello: •hello aplikace byla zapsána toorequire jinou sadu deklarací identity identifikátory URI nebo deklarací identity, hodnoty •Your aplikace nasazený způsobem, který vyžaduje hello NameIdentifier deklarace toobe něco jiného než hello uživatelské jméno (hlavní název uživatele NEBOLI) uložené ve službě Azure Active Directory. 

Informace o tom, jak tooadd a úpravy deklarací pro tyto scénáře, podívejte se na to [článku o přizpůsobení deklarace identity](active-directory-saml-claims-customization.md). 

### <a name="testing-hello-saml-application"></a>Testování aplikace hello SAML
Po hello SAML adresy URL a certifikát byly nakonfigurovány v Azure AD a hello aplikace, uživatele nebo skupiny mají přiřazený toohello aplikaci v Azure a hello deklarace identity byly zkontrolovány a upravovat v případě potřeby, pak uživatel hello je připraven toosign do hello aplikace. 

tootest, jednoduše přihlásit se k hello přístupový panel Azure AD v https://myapps.microsoft.com pomocí uživatelského účtu, že jste přiřadili toohello aplikace a potom klikněte na dlaždici hello tookick aplikace hello vypnout hello jeden přihlašování procesu. Alternativně můžete procházet přímo toohello adresa URL přihlašování pro aplikace hello a přihlásit se z ní. 

Ladění tipy, najdete [článek věnovaný tomu, jak toodebug na základě SAML jeden přihlašování tooapplications](active-directory-saml-debugging.md) 

## <a name="password-single-sign-on"></a>Heslo jednotné přihlašování
Vyberte tuto možnost tooconfigure [založené na heslech jednotné přihlašování](active-directory-appssoaccess-whatis.md) pro webovou aplikaci, která má přihlašovací stránku HTML. Založené na heslech SSO také heslo odkazované tooas vaulting, vám umožní toomanage uživatel přístup a hesla tooweb aplikace, které nepodporují federaci identit. Je také užitečné v případech, kde několika uživatelům musí tooshare jeden účet, například účty aplikace sociálních médií tooyour organizace. 

Po výběru **Další**, bude adresa URL hello výzvami tooenter hello aplikace založené na webu přihlašovací stránky. Všimněte si, že to musí být hello stránky, která zahrnuje hello uživatelské jméno a heslo vstupních polí. Spustí jednou zadané, Azure AD proces tooparse hello přihlašovací stránka pro vstup uživatelské jméno a heslo vstup. Pokud proces hello není úspěšné, pak provede vás alternativní procesem instalace rozšíření prohlížeče (vyžaduje Internet Explorer, Chrome nebo Firefox), které vám umožní toomanually zachycení hello pole.

Jakmile zachytí přihlašovací stránku hello, může přiřadit uživatele a skupiny a zásady přihlašovacích údajů můžete nastavit stejně jako regulární [heslo jednotného přihlašování k aplikacím](active-directory-appssoaccess-whatis.md).

Poznámka: Můžete nahrávat logo dlaždice pro aplikace hello pomocí hello **nahrát Logo** na hello tlačítko **konfigurace** kartu pro aplikace hello. 

## <a name="existing-single-sign-on"></a>Existující jednotné přihlašování
Vyberte tuto možnost tooadd portál odkazu tooan aplikace tooyour organizace přístupový Panel Azure AD nebo Office 365. Místo Azure AD pro ověřování můžete použít tento tooadd odkazy toocustom webových aplikací, které používají Azure Active Directory Federation Services (nebo jiné služby federation service). Nebo můžete přidat přímých odkazů toospecific SharePoint stránky nebo jiných webových stránek, že chcete tooappear na panelů přístup uživatelů. 

Po výběru **Další**, bude výzvami tooenter hello URL toolink aplikace hello k. Po dokončení, uživatelé a skupiny přiřazeni toohello aplikace, což způsobí, že tooappear aplikace hello v hello [Spouštěč aplikace Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) nebo hello [přístupový panel Azure AD](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) pro tyto uživatele.

Poznámka: Můžete nahrávat logo dlaždice pro aplikace hello pomocí hello **nahrát Logo** na hello tlačítko **konfigurace** kartu pro aplikace hello.

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Jak tooCustomize deklarace identity vystavené v hello tokenu SAML pro Pre-Integrated aplikace](active-directory-saml-claims-customization.md)
* [Řešení potíží s na základě SAML jednotné přihlašování](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png
