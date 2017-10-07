---
title: 'Kurz: Azure Active Directory integrace s Coupa | Microsoft Docs'
description: "Zjistěte, jak toouse Coupa s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 87e98573718d27d408c886466a374a987f58faa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Kurz: Azure Active Directory integrace s Coupa
cílem Hello tohoto kurzu je tooshow hello integrace Azure a Coupa.  
Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

* Platné předplatné Azure
* Coupa jednotné přihlašování (SSO) povolené předplatné

Po dokončení tohoto kurzu, budou uživatelé hello Azure AD jste přiřadili tooCoupa možné toosingle přihlášení do aplikace hello pomocí hello [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:

* Povolení integrace aplikace hello pro Coupa
* Konfigurace jednotného přihlašování
* Konfiguraci zřizování uživatelů
* Přiřazení uživatelů

![Scénář](./media/active-directory-saas-coupa-tutorial/IC791897.png "scénář")

## <a name="enable-hello-application-integration-for-coupa"></a>Povolit integraci aplikace hello pro Coupa
Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro Coupa.

**Integrace aplikace hello tooenable pro Coupa, proveďte následující kroky hello:**

1. V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.
   
   ![Služby Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "služby Active Directory")
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
   ![Aplikace](./media/active-directory-saas-coupa-tutorial/IC700994.png "aplikace")
4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
   ![Přidat aplikaci](./media/active-directory-saas-coupa-tutorial/IC749321.png "přidat aplikaci")
5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
   ![Přidání aplikace z gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "přidat aplikaci z gallerry")
6. V hello **vyhledávacího pole**, typ **Coupa**.
   
   ![Galerie aplikací](./media/active-directory-saas-coupa-tutorial/IC791898.png "galerii aplikací")
7. V podokně výsledků hello, vyberte **Coupa**a potom klikněte na **Complete** tooadd hello aplikace.
   
   ![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
   
## <a name="configure-single-sign-on"></a>Konfigurovat jednotné přihlašování

Hello cílem této části je toooutline jak tooCoupa tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.  

Konfigurace jednotného přihlašování pro Coupa vyžaduje tooretrieve hodnotu kryptografický otisk certifikátu. Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak tooretrieve hodnota kryptografického otisku certifikátu](http://youtu.be/YKQF266SAxI).

**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**

1. Přihlaste se na tooyour Coupa společnosti lokality jako správce.
2. Přejděte příliš**instalace \> řízení zabezpečení**.
   
   ![Ovládací prvky zabezpečení](./media/active-directory-saas-coupa-tutorial/IC791900.png "kontrolních mechanismů pro zabezpečení")
3. toodownload hello Coupa metadata souboru tooyour počítače, klikněte na **stahování a import metadat SP**.
   
   ![Metadata Coupa SP](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadat")
4. V okně jiný prohlížeč přihlaste toohello portál Azure classic.
5. Na hello **Coupa** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791902.png "nakonfigurovat jednotné přihlašování")
6. Na hello **jak jste by například uživatelé toosign na tooCoupa** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791903.png "nakonfigurovat jednotné přihlašování")
7. Na hello **konfigurace adresy URL aplikace** proveďte hello následující kroky:
   
   ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-coupa-tutorial/IC791904.png "konfigurovat adresu URL aplikace")   
   1. V hello **přihlašovací adresa URL** textovému poli, zadat adresu URL, které používá vaše uživatele toosign na tooyour Coupa aplikace (například: "*http://company.Coupa.com*").
   2. Otevřete váš stažený soubor metadat Coupa a poté zkopírujte hello **AssertionConsumerService index nebo adresa URL**.
   3. V hello **adresa URL odpovědi Coupa** textovému poli, vložte hello **AssertionConsumerService index nebo adresa URL** hodnotu.
   4. Klikněte na **Další**.
8. Na hello **nakonfigurovat jednotné přihlašování v Coupa** stránky, toodownload váš soubor metadat, klikněte na tlačítko **stáhnout metadata**a potom uložte soubor hello místně na vašem počítači.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791905.png "nakonfigurovat jednotné přihlašování")
9. Na webu společnosti Coupa hello, přejděte příliš**instalace \> řízení zabezpečení**.
   
   ![Ovládací prvky zabezpečení](./media/active-directory-saas-coupa-tutorial/IC791900.png "kontrolních mechanismů pro zabezpečení")
10. V hello **přihlásit pomocí přihlašovacích údajů Coupa** část, proveďte následující kroky hello:  

   ![Přihlaste se pomocí přihlašovacích údajů Coupa](./media/active-directory-saas-coupa-tutorial/IC791906.png "přihlásit pomocí přihlašovacích údajů Coupa") 
   1. Vyberte **Přihlaste se pomocí SAML**.
   2. Klikněte na tlačítko **Procházet** tooupload stažený Azure Active soubor metadat.
   3. Klikněte na **Uložit**.
11. Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.
    
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791907.png "nakonfigurovat jednotné přihlašování")
    
## <a name="configure-user-provisioning"></a>Konfiguraci zřizování uživatelů

V pořadí tooenable Azure AD Uživatelé toolog do Coupa musí být zřízená do Coupa.  

* V případě hello Coupa zřizování je ruční úloha.

**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**

1. Přihlaste se tooyour **Coupa** společnosti lokality jako správce.
2. V nabídce hello hello nahoře, klikněte na tlačítko **instalace**a potom klikněte na **uživatelé**.
   
   ![Uživatelé](./media/active-directory-saas-coupa-tutorial/IC791908.png "uživatelů")
3. Klikněte na možnost **Vytvořit**.
   
   ![Vytvoření uživatelů](./media/active-directory-saas-coupa-tutorial/IC791909.png "vytvoření uživatelů")
4. V hello **vytvořit uživateli** část, proveďte následující kroky hello:
   
   ![Podrobné informace o uživateli](./media/active-directory-saas-coupa-tutorial/IC791910.png "podrobné informace o uživateli")
   
   1. Typ hello **přihlášení**, **křestní jméno**, **příjmení**, **ID přihlášení**, **e-mailu** atributy platný účet služby Azure Active Directory, že který má tooprovision do hello související textových polí.
   2. Klikněte na možnost **Vytvořit**.   
   >[!NOTE]
   >Držitel účtu Azure Active Directory Hello dostane e-mail s účtem odkaz tooconfirm hello předtím, než se stane aktivní. 
   > 

>[!NOTE]
>Můžete použít všechny ostatní Coupa uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Coupa tooprovision AAD uživatelské účty. 
> 

## <a name="assign-users"></a>Přiřazení uživatelů
tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.

**tooassign tooCoupa uživatele, proveďte následující kroky hello:**

1. V hello portál Azure classic vytvořte testovací účet.
2. Na hello ** Coupa ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
   ![Přiřazení uživatelů](./media/active-directory-saas-coupa-tutorial/IC791911.png "přiřazení uživatelů")
3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.
   
   ![Ano](./media/active-directory-saas-coupa-tutorial/IC767830.png "Ano")

Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu. Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

