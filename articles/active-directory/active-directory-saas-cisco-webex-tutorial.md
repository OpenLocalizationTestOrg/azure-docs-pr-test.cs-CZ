---
title: 'Kurz: Azure Active Directory integrace s Cisco Webex | Microsoft Docs'
description: "Zjistěte, jak toouse Cisco Webex s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 9fc11e58a7acaa6fbfb32eeccbfbf85984950e67
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Kurz: Azure Active Directory integrace s Cisco Webex
cílem Hello tohoto kurzu je tooshow hello integrace Azure a Cisco Webex.  
Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

* Platné předplatné Azure
* Klient Cisco Webex

Po dokončení tohoto kurzu, hello uživatele Azure AD, které jste přiřadili tooCisco Webex bude možné toosingle přihlášení do aplikace hello ve vaší lokalitě společnosti Cisco Webex (služba Zprostředkovatel iniciované přihlašování) nebo pomocí hello [toohello Úvod Přístup k panelu](active-directory-saas-access-panel-introduction.md).

scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:

* Povolení integrace aplikace hello pro Cisco Webex
* Konfigurace jednotného přihlašování (SSO)
* Konfiguraci zřizování uživatelů
* Přiřazení uživatelů

![Scénář](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "scénář")

## <a name="enable-hello-application-integration-for-cisco-webex"></a>Povolit integraci aplikace hello pro Cisco Webex
Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro Cisco Webex.

**Integrace aplikace hello tooenable pro Cisco Webex proveďte hello následující kroky:**

1. V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.
   
   ![Služby Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "služby Active Directory")
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
   ![Aplikace](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "aplikace")
4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
   ![Přidat aplikaci](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "přidat aplikaci")
5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
   ![Přidání aplikace z gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "přidat aplikaci z gallerry")
6. V hello **vyhledávacího pole**, typ **Cisco Webex**.
   
   ![Galerie aplikací](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "galerii aplikací")
7. V podokně výsledků hello, vyberte **Cisco Webex**a potom klikněte na **Complete** tooadd hello aplikace.
   
   ![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
   
## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování

Hello cílem této části je toooutline jak tooenable uživatelé tooauthenticate tooCisco Webex ke svému účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.  

V rámci tohoto postupu jsou požadované toocreate kódování base-64 kódovaného certifikátu. Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak tooconvert binární certifikátů do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**

1. V portálu Azure classic, na hello hello **Cisco Webex** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "nakonfigurovat jednotné přihlašování")
2. Na hello **jak jste by například uživatelé toosign na tooCisco Webex** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "nakonfigurovat jednotné přihlašování")
3. Na hello **konfigurace adresy URL aplikace** stránky, proveďte hello následující kroky a pak klikněte na tlačítko **Další**.
   
   ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "konfigurovat adresu URL aplikace")   
   1. V hello **Sing – adresa URL** textovému poli, zadejte adresu URL klienta Cisco Webex (např: *http://contoso.webex.com*).
   2. V hello **adresa URL odpovědi Webex Cisco** textovému poli, typ vaše **Cisco Webex AssertionConsumerService URL** (např: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company* ).
4. Na hello **nakonfigurovat jednotné přihlašování v Cisco Webex** stránky, toodownload svůj certifikát, klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu hello ve vašem počítači.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "nakonfigurovat jednotné přihlašování")
5. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Cisco Webex.
6. V nabídce hello hello nahoře, klikněte na tlačítko **Správa webu**.
   
   ![Správa lokalit](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "lokality správy")
7. V hello **spravovat lokality** klikněte na tlačítko **Konfigurace jednotného přihlašování k**.
   
   ![Konfigurace jednotného přihlašování k](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "Konfigurace jednotného přihlašování")
8. V hello federovaného jednotného přihlašování konfiguraci webové části proveďte hello následující kroky:
   
   ![Konfigurace jednotného přihlašování federovaného](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "federovaného jednotného přihlašování k konfigurace")  
   1. Z hello **Federation protokol** seznamu, vyberte **SAML 2.0**.
   2. Vytvoření **kódování Base-64** soubor stažený certifikát.  
    >[!TIP]
    >Další podrobnosti najdete v tématu [jak tooconvert binární certifikátů do textového souboru](http://youtu.be/PlgrzUZ-Y1o)
    >  
   3. V programu Poznámkový blok a potom hello kopírování obsahu je otevření kódovaného certifikátu kódování base-64.
   4. Klikněte na tlačítko **importovat Metadata SAML**a potom vložte vaše kódování base-64 kódovaného certifikátu.
   5. V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, kopie hello **URL vystavitele** hodnotu a pak ji vložit do hello **vystavitele pro SAML (IdP ID)** textové pole.
   6. V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, kopie hello **vzdálené adresy URL pro přihlášení** hodnotu a pak ji vložit do hello **zákazníka jednotného přihlašování služby přihlášení Adresa URL** textové pole.
   7. Z hello **NameID formátu** seznamu, vyberte **e-mailová adresa**.
   8. V hello **AuthnContextClassRef** textovému poli, typ **urn: oasis: názvy: tc: SAML:2.0:ac:classes:Password**.
   9. V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, kopie hello **vzdálené adresy URL odhlašovací** hodnotu a pak ji vložit do hello **zákazníka jednotného přihlašování služby odhlášení Adresa URL** textové pole.
   10. Klikněte na tlačítko **aktualizace**.
9. V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v Cisco Webex** dialogové okno stránky, vyberte hello potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete**.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "nakonfigurovat jednotné přihlašování")
   
## <a name="configure-user-provisioning"></a>Konfiguraci zřizování uživatelů

V pořadí tooenable Azure AD Uživatelé toolog do Cisco Webex musí být zřízená do Cisco Webex.  

* V případě hello Cisco Webex zřizování je ruční úloha.

**tooprovision uživatelské účty, provádět hello následující kroky:**

1. Přihlaste se tooyour **Cisco Webex** klienta.
2. Přejděte příliš**spravovat uživatele \> přidat uživatele**.
   
   ![Přidání uživatelů](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "přidat uživatele")
3. V hello části přidat uživatele proveďte následující kroky hello:
   
   ![Přidat uživatele](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "přidat uživatele")   
   1. Jako **typ účtu**, vyberte **hostitele**.
   2. Zadejte informace o hello stávajícího uživatele Azure AD do hello následující textových polí: **křestní jméno, příjmení**, **uživatelské jméno**, **e-mailu**, **heslo**, **Potvrzení hesla**.
   3. Klikněte na tlačítko **Přidat**.

>[!NOTE]
>Můžete použít všechny ostatní Cisco Webex uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Cisco Webex tooprovision AAD uživatelské účty. 
> 

## <a name="assign-users"></a>Přiřazení uživatelů
tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.

**tooassign uživatelé tooCisco Webex, proveďte následující kroky hello:**

1. V hello portál Azure classic vytvořte testovací účet.
2. Na hello **Cisco Webex** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
   ![Přiřazení uživatelů](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "přiřazení uživatelů")
3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.
   
   ![Ano](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Ano")

Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu. Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

