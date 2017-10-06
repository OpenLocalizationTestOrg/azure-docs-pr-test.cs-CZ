---
title: 'Kurz: Azure Active Directory integrace s Replicon | Microsoft Docs'
description: "Zjistěte, jak toouse Replicon s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 4949eaf959265cfa4f732a2b73317fffe6312a17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a>Kurz: Azure Active Directory integrace s Replicon
cílem Hello tohoto kurzu je tooshow hello integrace Azure a Replicon. Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

* Platné předplatné Azure
* Replicon klienta

Po dokončení tohoto kurzu, budou uživatelé hello Azure AD jste přiřadili tooReplicon možné toosingle přihlášení do aplikace hello na váš web společnosti Replicon (služba Zprostředkovatel iniciované přihlašování) nebo pomocí hello [Úvod toohello přístup Panel](active-directory-saas-access-panel-introduction.md).

scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:

1. Povolení integrace aplikace hello pro Replicon
2. Konfigurace jednotného přihlašování (SSO)
3. Konfiguraci zřizování uživatelů
4. Přiřazení uživatelů

![Scénář](./media/active-directory-saas-replicon-tutorial/IC777798.png "scénář")

## <a name="enable-hello-application-integration-for-replicon"></a>Povolit integraci aplikace hello pro Replicon
Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro Replicon.

**Integrace aplikace hello tooenable pro Replicon, proveďte následující kroky hello:**

1. V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.
   
    ![Služby Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "služby Active Directory")
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Aplikace](./media/active-directory-saas-replicon-tutorial/IC700994.png "aplikace")
4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
    ![Přidat aplikaci](./media/active-directory-saas-replicon-tutorial/IC749321.png "přidat aplikaci")
5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
    ![Přidání aplikace z gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "přidat aplikaci z gallerry")
6. V hello **vyhledávacího pole**, typ **Replicon**.
   
    ![Galerie aplikací](./media/active-directory-saas-replicon-tutorial/IC777799.png "galerii aplikací")
7. V podokně výsledků hello, vyberte **Replicon**a potom klikněte na **Complete** tooadd hello aplikace.
   
    ![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")
   
## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování

Hello cílem této části je toooutline jak tooReplicon tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.

**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**

1. V portálu Azure classic, na hello hello **Replicon** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC777801.png "nakonfigurovat jednotné přihlašování")
2. Na hello **jak jste by například uživatelé toosign na tooReplicon** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC777802.png "nakonfigurovat jednotné přihlašování")
3. Na hello **konfigurace adresy URL aplikace** proveďte hello následující kroky:
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-replicon-tutorial/IC777803.png "konfigurovat adresu URL aplikace")
  1. V hello **Replicon přihlašovací adresa URL** textovému poli, zadejte adresu URL Replicon klienta (například: *https://na2.replicon.com/company/saml2/sp-sso/post*).
  2. V hello **adresa URL odpovědi Replicon** textovému poli, zadejte vaše Replicon **AssertionConsumerService** adresa URL (např: *https://global.replicon.com/! / typu saml2 nebo společnosti nebo jednotného přihlašování nebo pozálohovacího*).  
      
     >[!NOTE]
     >Můžete získat adresu URL hello hello Replicon metadata v: **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.
     > 
     > 
 
  3. Klikněte na **Další**.

4. Na hello **nakonfigurovat jednotné přihlašování v Replicon** stránky, toodownload metadata, klikněte na tlačítko **stáhnout metadata**a potom uložte hello metadata ve vašem počítači.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC777804.png "nakonfigurovat jednotné přihlašování")
5. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Replicon.

6. tooconfigure SAML 2.0, proveďte následující kroky hello:
   
    ![Povolit ověřování SAML](./media/active-directory-saas-replicon-tutorial/IC777805.png "ověřování povolit SAML")
  
  1. toodisplay hello **EnableSAML Authentication2** dialogu připojit hello následující adresu URL tooyour, po vašeho klíče společnosti: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**  
    * Následující Hello znázorňuje schéma hello hello úplnou adresu URL:  
   **https://Na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**
   2. Klikněte na tlačítko hello  **+**  tooexpand hello **v20Configuration** části.
   3. Klikněte na tlačítko hello  **+**  tooexpand hello **metaDataConfiguration** části.
   4. Klikněte na tlačítko **zvolit soubor**, tooselect vaší identity zprostředkovatele metadat XML soubor a klikněte na **odeslání**.

7. Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-replicon-tutorial/IC778418.png "nakonfigurovat jednotné přihlašování")
   
## <a name="configure-user-provisioning"></a>Konfiguraci zřizování uživatelů

V pořadí tooenable Azure AD Uživatelé toolog do Replicon musí být zřízená do Replicon.  

V případě hello Replicon zřizování je ruční úloha.

**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**

1. V okně webového prohlížeče Přihlaste se jako správce k serveru vaší společnosti Replicon.
2. Přejděte příliš**správy \> uživatelé**.
   
    ![Uživatelé](./media/active-directory-saas-replicon-tutorial/IC777806.png "uživatelů")
3. Klikněte na tlačítko **+ přidat uživatele**.
   
    ![Přidat uživatele](./media/active-directory-saas-replicon-tutorial/IC777807.png "přidat uživatele")
4. V hello **profil uživatele** část, proveďte následující kroky hello:
   
    ![Profil uživatele](./media/active-directory-saas-replicon-tutorial/IC777808.png "profil uživatele")
   
  1. V hello **přihlašovací jméno** textovému poli, typ hello Azure AD e-mailová adresa uživatele hello Azure AD, bude tooprovision.
  2. Jako **typ ověřování**, vyberte **jednotného přihlašování k**.
  3. V hello **oddělení** textovému poli, zadejte oddělení hello uživatele.
  4. Jako **typ zaměstnance**, vyberte **správce**.
  5. Klikněte na tlačítko **uložit profil uživatele**.

>[!NOTE]
>Můžete použít všechny ostatní Replicon uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Replicon tooprovision AAD uživatelské účty.
> 
> 

## <a name="assign-users"></a>Přiřazení uživatelů
tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.

**tooassign tooReplicon uživatele, proveďte následující kroky hello:**

1. V hello portál Azure classic vytvořte testovací účet.

2. Na hello **Replicon** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
    ![Přiřazení uživatelů](./media/active-directory-saas-replicon-tutorial/IC777809.png "přiřazení uživatelů")

3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.
   
    ![Ano](./media/active-directory-saas-replicon-tutorial/IC767830.png "Ano")

Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu. Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

