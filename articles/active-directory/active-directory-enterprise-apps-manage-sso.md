---
title: "aaaSingle přihlašování správy pro podnikové aplikace v Azure Active Directory hello | Microsoft Docs"
description: "Zjistěte, jak hello toomanage jednotného přihlašování pro podnikové aplikace pomocí Azure Active Directory"
services: active-directory
documentationcenter: 
author: asmalser
manager: femila
editor: 
ms.assetid: bcc954d3-ddbe-4ec2-96cc-3df996cbc899
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: asmalser
ms.openlocfilehash: b0a8e622ab10517b7b69f786406b6e9b9f2e7eaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-single-sign-on-for-enterprise-apps"></a>Správa jednotného přihlašování pro podnikové aplikace
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-enterprise-apps-manage-sso.md)
> * [Portál Azure Classic](active-directory-sso-integrate-saas-apps.md)
> 

Tento článek popisuje, jak toouse hello [portál Azure](https://portal.azure.com) toomanage jeden nastavení přihlášení pro podnikové aplikace. Podnikové aplikace jsou aplikace, které jsou nasazené a použít v rámci vaší organizace. Tento článek se týká hlavně tooapps, které byly přidány z hello [galerii aplikací Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). 

## <a name="finding-your-apps-in-hello-portal"></a>Vyhledání aplikace hello portálu
Všechny podnikové aplikace, které jsou nastavené pro jednotné přihlašování můžete zobrazit a spravovat hello portálu Azure. aplikace Hello lze nalézt v hello **více služeb** &gt; **podnikové aplikace, které** části portálu hello. 

![Okno podnikové aplikace][1]

Vyberte **všechny aplikace** tooview seznam všech aplikací, které byly nakonfigurovány. Výběr aplikace načte hello okna prostředků pro tuto aplikaci, kde je možné zobrazit sestavy pro tuto aplikaci a lze je spravovat celou řadu nastavení.

Vyberte toomanage jeden nastavení přihlášení, **jednotného přihlašování**.

![Okna prostředků aplikace][2]

## <a name="single-sign-on-modes"></a>Režimy přihlášení
Hello **jednotného přihlašování** okno začíná **režimu** nabídce, která umožňuje toobe režimu přihlašování hello nakonfigurované. Hello dostupné možnosti patří:

* **Přihlašování SAML na** – tato možnost je dostupná, pokud aplikace hello podporuje úplné federované jednotné přihlašování s Azure Active Directory pomocí protokolu hello SAML 2.0.
* **Založené na heslech přihlašování** – tato možnost je dostupná, pokud Azure AD podporuje formulář hesla naplnění pro tuto aplikaci.
* **Propojené přihlášení na** -dříve označované jako "Stávající single sign-on", tato možnost umožňuje správci tooplace k aplikaci toothis odkaz v Spouštěč aplikace jejich uživatele přístupový Panel Azure AD nebo Office 365.

Další informace o těchto režimech najdete v tématu [jak funguje jednotné přihlašování pomocí služby Azure Active Directory pracovní](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="saml-based-sign-on"></a>Na základě SAML přihlášení
Hello **přihlašování SAML na** možnost zobrazí okno, které je rozdělena do čtyř částí:

### <a name="domains-and-urls"></a>Domén a adres URL
Toto je, kde jsou všechny podrobnosti o domény a adresy URL aplikace hello přidané tooyour adresář Azure AD. Vyžaduje se všechny vstupy toomake pracovní přihlášení aplikace se zobrazí přímo na úvodní obrazovka, zatímco všechny volitelné vstupy můžete zobrazit výběrem hello **zobrazit upřesňující nastavení adresy URL** zaškrtávací políčko. Úplný seznam podporovaných vstupy Hello zahrnuje:

* **Přihlašovací adresa URL** – kde hello uživatel přejde toothis toosign v aplikaci. Pokud aplikace hello je nakonfigurované tooperform služby jedním spouštěná zprostředkovatele přihlášení, když uživatel přejde toothis adresu URL, poskytovatele služeb hello hello nezbytné tooauthenticate tooAzure AD přesměrování a přihlaste hello uživatele v. Pokud toto pole se vyplní, Azure AD použije tato adresa URL toolaunch hello aplikace z Office 365 a hello přístupový Panel Azure AD. Pokud toto pole je tento parametr vynechán, bude Azure AD místo toho provede zprostředkovatele identity-initiated přihlašování při hello spuštění aplikace z Office 365, hello přístupový Panel Azure AD, nebo z hello Azure AD jeden přihlašování adresy URL.
* **Identifikátor** -tento identifikátor URI musí jednoznačně identifikovat aplikace hello, pro které jedním přihlásit se konfiguruje. Toto je hodnota hello Azure AD odesílá zpět tooapplication jako cílová skupina parametr tokenu SAML hello text hello, a aplikace hello je očekávané toovalidate ho. Tato hodnota se také zobrazí jako hello Entity ID v veškerá metadata SAML poskytované aplikace hello.
* **Adresa URL odpovědi** – adresa URL odpovědi hello je tam, kde aplikace hello očekává tooreceive hello SAML token. Toto je také odkazované tooas hello Assertion příjemce Service (ACS) adresy URL. Po zadání těchto klikněte na tlačítko Další tooproceed toohello další obrazovce. Tato obrazovka poskytuje informace o tom, jaké požadavky toobe na tooenable straně aplikace hello ho tooaccept tokenu SAML z Azure AD.
* **Stav předávání** -stav předávání hello je volitelný parametr, který vám pomůže zjistit hello aplikace, kde tooredirect hello uživatele po dokončení ověřování. Obvykle hello hodnota je platná adresa URL na hello aplikace, ale některé aplikace používají toto pole jinak (viz aplikace hello jednotného přihlašování na dokumentaci). Hello možnost tooset hello předávání stav je nová funkce, která je jedinečný toohello nový portál Azure.

### <a name="user-attributes"></a>Uživatelské atributy
Toto je, kde správci můžete zobrazit a upravit hello atributy, které se odesílají v tokenu SAML hello, Azure AD vydá toohello aplikace mohou uživatelé vždy přihlásit.

Hello pouze atribut upravitelnosti podporována je hello **uživatelský identifikátor** atribut. Hello hodnota tohoto atributu je hello pole ve službě Azure AD, která jednoznačně identifikuje každého uživatele v rámci aplikace hello. Například pokud aplikace hello nasazená pomocí hello "e-mailovou adresu" jako uživatelské jméno hello a jedinečný identifikátor, pak hello by hodnota pole "user.mail" toohello ve službě Azure AD.

### <a name="saml-signing-certificate"></a>Certifikát pro podpis SAML
Tato část uvádí podrobnosti hello hello certifikátu, že Azure AD používá tokeny SAML hello toosign, které jsou vydány toohello aplikaci, kterou každý čas hello uživatel se ověřuje. To umožňuje hello vlastnosti hello aktuální certifikát toobe prověřovány, včetně hello datum vypršení platnosti.

### <a name="application-configuration"></a>Konfigurace aplikace
Koncová část Hello poskytuje dokumentaci hello nebo ovládací prvky požadované tooconfigure hello vlastní aplikace toouse Azure Active Directory jako zprostředkovatele identity.

Hello **konfigurace aplikace** rozevírací nabídce poskytuje nové stručný a embedded pokyny ke konfiguraci aplikace hello. To je další nové funkce jedinečný toohello nový portál Azure.

> [!NOTE]
> Úplný příklad embedded dokumentace najdete v části aplikace hello Salesforce.com. Průběžně je přidáván dokumentace pro další aplikace.
> 
> 

![Vložené dokumentace][3]

## <a name="password-based-sign-on"></a>Založené na heslech přihlašování
Pokud pro aplikace hello podporován, výběr hello založené na heslech režimu jednotné přihlašování a výběrem **Uložit** okamžitě ji nastaví toodo jednotné přihlašování založené na heslech. Další informace o nasazení založené na heslech jednotného přihlašování najdete v tématu [jak funguje jednotné přihlašování pomocí služby Azure Active Directory pracovní](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Založené na heslech přihlašování][4]

## <a name="linked-sign-on"></a>Propojené přihlášení
Pokud pro aplikace hello podporován, výběr hello propojené jednotného přihlašování k režimu vám umožní tooenter hello URL, na kterou budete chtít hello přístupový Panel Azure AD nebo Office 365 tooredirect toowhen uživatelů klikněte na tuto aplikaci. Další informace o propojené přihlášení SSO (dříve označované jako "stávající SSO") najdete v tématu [jak funguje jednotné přihlašování pomocí služby Azure Active Directory pracovní](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Propojené přihlášení][5]

##<a name="feedback"></a>Váš názor

Věříme, že pomocí hello vylepšené možnosti Azure AD. Uchovávejte hello zpětnou vazbu, než dorazí! Zveřejněte vaše názory a návrhy pro zlepšení ve hello **portál pro správu** části našich [fóru pro zpětnou vazbu](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Jsme se vzrušení o vytváření nástrojů nové vlastní položky každý den a používat vaše tooshape pokyny a definovat, co se máme zaměřit příště.

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG
