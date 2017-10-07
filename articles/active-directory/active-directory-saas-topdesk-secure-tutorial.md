---
title: "Kurz: Azure Active Directory integrace s TOPdesk - zabezpečené | Microsoft Docs"
description: "Zjistěte, jak toouse TOPdesk - zabezpečit pomocí služby Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8e149d2d-7849-48ec-9993-31f4ade5fdb4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 10fe420d1691c2845b89c779486ffd6fcd736432
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Kurz: Azure Active Directory integrace s TOPdesk - zabezpečení
cílem Hello tohoto kurzu je tooshow hello integrace Azure a TOPdesk – zabezpečení.  
Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

* Platné předplatné Azure
* A TOPdesk - zabezpečené jednotné přihlašování povolené předplatné

Po dokončení tohoto kurzu, hello Azure AD uživatelům přiřadili jste tooTOPdesk - zabezpečené bude být schopný toosingle přihlášení do aplikace hello na vaše TOPdesk - zabezpečené podnikové lokality (služba Zprostředkovatel iniciované přihlašování), nebo pomocí hello [Úvod toohello přístupový Panel](active-directory-saas-access-panel-introduction.md).

scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:

1. Povolení integrace aplikace hello pro TOPdesk - zabezpečení
2. Konfigurace jednotného přihlašování
3. Konfiguraci zřizování uživatelů
4. Přiřazení uživatelů

![Scénář](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "scénář")

## <a name="enabling-hello-application-integration-for-topdesk---secure"></a>Povolení integrace aplikace hello pro TOPdesk - zabezpečení
Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro TOPdesk - zabezpečené.

### <a name="tooenable-hello-application-integration-for-topdesk---secure-perform-hello-following-steps"></a>Integrace aplikace hello tooenable pro TOPdesk - bezpečný, proveďte hello následující kroky:
1. V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.
   
    ![Služby Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "služby Active Directory")

2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.

3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Aplikace](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "aplikace")

4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
    ![Přidat aplikaci](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "přidat aplikaci")

5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
    ![Přidání aplikace z gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "přidat aplikaci z gallerry")

6. V hello **vyhledávacího pole**, typ **TOPdesk - zabezpečené**.
   
    ![Galerie aplikací](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "galerii aplikací")

7. V podokně výsledků hello, vyberte **TOPdesk - zabezpečené**a potom klikněte na **Complete** tooadd hello aplikace.
   
    ![TOPdesk - zabezpečené](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - zabezpečení")

## <a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
Hello cílem této části je toooutline jak tooenable uživatelé tooauthenticate tooTOPdesk - zabezpečit pomocí svého účtu ve službě Azure AD využívající federaci podle hello protokolu SAML.  
Konfigurace jednotného přihlašování pro TOPdesk – zabezpečení vyžaduje, abyste tooupload soubor logo ikonu. tooget hello soubor ikony, tým podpory TOPdesk kontaktní hello.

### <a name="tooconfigure-single-sign-on-perform-hello-following-steps"></a>tooconfigure jednotné přihlašování, proveďte následující kroky hello:
1. Přihlaste se tooyour **TOPdesk - zabezpečené** společnosti lokality jako správce.
2. V hello **TOPdesk** nabídky, klikněte na tlačítko **nastavení**.
   
    ![Nastavení](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "nastavení")

3. Klikněte na tlačítko **nastavení přihlášení**.
   
    ![Nastavení přihlášení](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "nastavení přihlášení")

4. Rozbalte hello **nastavení přihlášení** nabídce a pak klikněte na tlačítko **Obecné**.
   
    ![Obecné](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "obecné")

5. V hello **zabezpečeného** části hello **SAML přihlášení** konfigurace části, proveďte následující kroky hello:
   
    ![Technické nastavení](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "technické nastavení")
   
    a. Klikněte na tlačítko **Stáhnout** toodownload hello veřejné metadata souboru a poté je uložit místně v počítači.
   
    b. Otevřete soubor metadat hello a vyhledejte hello **AssertionConsumerService** uzlu.
    
    ![Kontrolní výraz příjemce služby](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Assertion příjemce služby")
   
    c. Kopírování hello **AssertionConsumerService** hodnotu.  
      
    > [!NOTE]
    > Bude nutné hello hodnota v hello **konfigurace adresy URL aplikace** později v tomto kurzu.
    > 
    > 

6. V okně prohlížeče jiný web, přihlaste se k vaší **portál Azure classic** jako správce.

7. Na hello **TOPdesk - zabezpečené** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello ** nakonfigurovat jednotné přihlašování ** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "nakonfigurovat jednotné přihlašování")

8. Na hello **jak by vám jako uživatelé toosign na tooTOPdesk - Secure** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "nakonfigurovat jednotné přihlašování")

9. Na hello **konfigurace adresy URL aplikace** proveďte hello následující kroky:
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "konfigurovat adresu URL aplikace")
   
    a. V hello **TOPdesk - zabezpečené přihlašovací adresa URL** textovému poli, adresa URL typu hello používá vaše toosign uživatelů do vašeho TOPdesk - zabezpečené aplikace (například: "*https://qssolutions.topdesk.net*").
   
    b. V hello **TOPdesk – veřejná adresa URL odpovědi** textovému poli, vložte hello **TOPdesk - zabezpečené URL AssertionConsumerService** (například: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
   
    c. Klikněte na **Další**.

10. Na hello **nakonfigurovat jednotné přihlašování v TOPdesk - zabezpečené** stránky, toodownload souboru metadat klikněte na tlačítko **stáhnout metadata**a potom uložte soubor hello místně na vašem počítači.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "nakonfigurovat jednotné přihlašování")

11. toocreate soubor certifikátu, proveďte následující kroky hello:
    
    ![Certifikát](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "certifikátu")
    
    a. Otevřete hello soubor stažený metadat.
    b. Rozbalte hello **RoleDescriptor** uzlu, který má **xsi: type** z **dodáni: ApplicationServiceType**.
    c. Zkopírujte hodnotu hello hello **certifikátu x 509** uzlu.
    d. Uložit hello zkopírovat **certifikátu x 509** hodnota místně na vašem počítači v souboru.

12. Na vaše TOPdesk – zabezpečení společnosti lokality v hello **TOPdesk** nabídky, klikněte na tlačítko **nastavení**.
    
    ![Nastavení](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "nastavení")

13. Klikněte na tlačítko **nastavení přihlášení**.
    
    ![Nastavení přihlášení](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "nastavení přihlášení")

14. Rozbalte hello **nastavení přihlášení** nabídce a pak klikněte na tlačítko **Obecné**.
    
    ![Obecné](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "obecné")

15. V hello **veřejné** klikněte na tlačítko **přidat**.
    
    ![Přidat](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "přidat")

16. Na hello **pomocníka konfigurace SAML** dialogové okno proveďte hello následující kroky:
    
    ![Pomocník pro konfigurace SAML](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "pomocníka konfigurace SAML")
    
    a. tooupload stažené metadata souboru v části **federačních metadat**, klikněte na tlačítko **Procházet**.

    b. tooupload svůj certifikát v části souboru **certifikát (RSA)**, klikněte na tlačítko **Procházet**.

    c. Soubor loga hello tooupload jste získali od týmu podpory TOPdesk hello, v části **Logo ikonu**, klikněte na tlačítko **Procházet**.

    d. V hello **atribut uživatelského jména** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    e. V hello **zobrazovaný název** textovému poli, zadejte název pro svou konfiguraci.

    f. Klikněte na **Uložit**.

17. Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "nakonfigurovat jednotné přihlašování")

## <a name="configuring-user-provisioning"></a>Konfiguraci zřizování uživatelů
V pořadí tooenable Azure AD Uživatelé toolog do TOPdesk - bezpečný, se musí být zřízená do TOPdesk - zabezpečené.  
V případě hello TOPdesk - zabezpečený a zřizování je ruční úloha.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure zřizování uživatelů, proveďte následující kroky hello:
1. Přihlaste se tooyour **TOPdesk - zabezpečené** společnosti lokality jako správce.
2. V nabídce hello hello nahoře, klikněte na tlačítko **TOPdesk \> nový \> soubory podpory \> operátor**.
   
    ![Operátor](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "– operátor")

3. Na hello **operátor New** dialogové okno, proveďte následující kroky hello:
   
    ![Operátor new](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "New – operátor")
   
    a. Klikněte na kartu Obecné hello.
   
    b. V hello **Přezdívka** textbox hello **Obecné** části, typ hello příjmení chcete tooprovision platný účet služby Azure Active Directory.
   
    c. Vyberte **lokality** pro účet hello v hello **umístění** části.
   
    d. V hello **přihlašovací jméno** textbox hello **TOPdesk přihlášení** zadejte přihlašovací jméno uživatele.
   
    e. Klikněte na **Uložit**.

> [!NOTE]
> Můžete použít všechny ostatní TOPdesk - zadáním hodnoty zabezpečené uživatelský účet, nástroje pro tvorbu nebo rozhraní API poskytovaných TOPdesk - Secure tooprovision AAD uživatelské účty.
> 
> 

## <a name="assigning-users"></a>Přiřazení uživatelů
tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.

### <a name="tooassign-users-tootopdesk---secure-perform-hello-following-steps"></a>Uživatelé tooTOPdesk tooassign - zabezpečit, proveďte následující kroky hello:
1. V hello portál Azure classic vytvořte testovací účet.
2. Na hello ** TOPdesk - zabezpečené ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.
   
    ![Přiřazení uživatelů](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "přiřazení uživatelů")

3. Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.
   
    ![Ano](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Ano")

Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu. Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

