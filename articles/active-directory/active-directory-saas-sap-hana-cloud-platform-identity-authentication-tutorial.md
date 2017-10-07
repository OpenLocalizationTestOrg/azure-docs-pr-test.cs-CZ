---
title: "Kurz: Azure Active Directory integrace s SAP HANA cloudové platformy identitu ověřování | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SAP HANA cloudové platformy identitu ověřování."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 2142770779ddb745555b47fc85b5457b573f9506
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform-identity-authentication"></a>Kurz: Azure Active Directory integrace s SAP HANA cloudové platformy identitu ověřování

V tomto kurzu zjistíte, jak toointegrate SAP HANA cloudové platformy identitu ověřování s Azure Active Directory (Azure AD). Ověření Identity SAP HANA cloudové platformy slouží jako proxy IdP tooaccess SAP aplikací pomocí služby Azure AD jako hlavní IdP hello.

Integrace SAP HANA cloudové platformy identitu ověřování s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooSAP aplikace
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSAP aplikace jednotné přihlašování (SSO) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s SAP HANA cloudové platformy identitu ověřování, je třeba hello následující položky:

- Předplatné služby Azure AD
- A **SAP HANA cloudové platformy identitu ověřování** předplatné povolené jednotné přihlašování


>[!NOTE] 
>tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.
>

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.

Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání ověření Identity SAP HANA cloudové platformy z Galerie hello
2. Konfigurace a testování Azure AD SSO

Než začnete hello technické podrobnosti, je důležité toounderstand hello koncepty, které budete toolook v. Hello SAP HANA cloudové platformy identitu ověřování a Azure Active Directory federation vám umožní tooimplement jednotného přihlašování napříč aplikacím nebo službám chráněným pomocí AAD (jako IdP) s aplikacemi SAP a službám chráněným pomocí SAP HANA Cloudová platforma identita Ověřování.

V současné době SAP HANA cloudové platformy identitu ověřování funguje jako zprostředkovatel Identity Proxy tooSAP – aplikace. Azure Active Directory pak funguje jako hello úvodní zprostředkovatele Identity v rámci této instalace. 

Hello následující diagram znázorňuje toto:    

![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

S tímto nastavením budou klientovi SAP HANA cloudové platformy identitu ověřování nakonfigurované jako důvěryhodné aplikace v Azure Active Directory. 

Všechny aplikace SAP a služby, které mají tooprotect prostřednictvím tímto způsobem jsou následně nakonfigurované v konzole pro správu SAP HANA cloudové platformy identitu ověřování hello!

To znamená, že autorizace pro udělení přístupu tooSAP aplikace a služby potřebám tootake místní v SAP HANA cloudové platformy identitu ověřování pro taková konfigurace (jako názvem na rozdíl od tooconfiguring autorizace v Azure Active Directory).

Nakonfigurováním ověření Identity SAP HANA cloudové platformy jako aplikaci prostřednictvím hello Azure Active Directory Marketplace nepotřebujete tootake péče o konfiguraci potřebné jednotlivé deklaracích / SAML kontrolní výrazy a transformace potřeby tooproduce platné ověřovací token pro aplikace SAP.

>[!NOTE] 
>Obě strany, pouze byl testován aktuálně jednotného přihlašování k webu. Toky potřebné pro aplikaci API nebo rozhraní API API komunikace by měla fungovat, ale nebyly testovány, ještě. Bude být testována jako součást následné aktivity.
>

## <a name="add-sap-hana-cloud-platform-identity-authentication-from-hello-gallery"></a>Přidání ověření Identity SAP HANA cloudové platformy z Galerie hello
tooconfigure hello integrace SAP HANA cloudové platformy identitu ověřování do služby Azure AD, je nutné tooadd SAP HANA cloudové platformy identitu ověřování hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd SAP HANA cloudové platformy identitu ověřování z Galerie hello, proveďte následující kroky hello:**

1. V hello [ **portálu pro správu Azure**](https://portal.azure.com), na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **SAP HANA cloudové platformy identitu ověřování**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_01.png)

5. Na panelu výsledků hello vyberte **SAP HANA cloudové platformy identitu ověřování**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_02.png)


##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části konfiguraci a testování Azure AD přihlášení SSO se SAP HANA cloudové platformy Identity ověřování na základě testovací uživatele, nazývá "Britta Simon".

Pro jednotné přihlašování toowork Azure AD musí tooknow, jaké hello příslušného uživatele v SAP HANA cloudové platformy identitu ověřování je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SAP HANA cloudové platformy identitu ověřování musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v SAP HANA cloudové platformy identitu ověřování.

tooconfigure a testování Azure AD přihlášení SSO se SAP HANA cloudové platformy identitu ověřování, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele SAP HANA cloudové platformy identitu ověřování](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)**  -toohave protějšek Britta Simon v SAP HANA cloudové platformy Identity ověřování, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-sso"></a>Konfigurace Azure AD jednotného přihlašování

V této části můžete povolit jednotné přihlašování Azure AD na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci SAP HANA cloudové platformy identitu ověřování.

Ověření Identity SAP HANA cloudové platformy aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu. Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace. Hello následující snímek obrazovky ukazuje příklad pro tento.

![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_03.png)

**tooconfigure Azure AD přihlášení SSO se SAP HANA cloudové platformy identitu ověřování, proveďte následující kroky hello:**

1. V hello Azure Management portal na hello **SAP HANA cloudové platformy identitu ověřování** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.
 
    ![Konfigurovat jednotné přihlašování][5]

3. V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, pokud vaše aplikace SAP očekává atribut například "jméno". V dialogovém okně atributy tokenu SAML hello přidejte atribut hello "jméno".
 1. Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.
 
    ![Konfigurovat jednotné přihlašování][6]

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_05.png)
 2. V hello **název atributu** textovému poli, zadejte název atributu hello "jméno".
 3. Z hello **hodnota atributu** seznamu, vyberte hello hodnotu atributu "user.givenname".
 4. Klikněte na tlačítko **OK**.

4. Na hello **SAP HANA cloudové platformy identitu ověřování domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_06.png)
 1. V hello **přihlašovací adresa URL** textovému poli, zadejte přihlašovací hello na adresu URL pro aplikaci SAP hello.
 2. V hello **identifikátor** textovému poli, hodnota typu hello následující vzoru:`<entity-id>.accounts.ondemand.com` 
    * Pokud si nejste jisti tuto hodnotu, postupujte podle dokumentace hello SAP HANA cloudové platformy identitu ověřování v [konfiguraci klienta SAML 2.0](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

5. Na hello **konfiguraci Identity ověřování cloudové platformy SAP HANA** klikněte na tlačítko **konfigurace SAP HANA cloudové platformy identitu ověřování** tooopen **konfigurovatpřihlášení** dialogové okno. Potom klikněte na **SAML XML Metadata** a uložte soubor hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_07.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_08.png)

6. tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, přejděte tooSAP HANA cloudové platformy identitu ověřování správy konzoly. Adresa URL Hello má následující vzor hello:`https://<tenant-id>.accounts.ondemand.com/admin`
 * Postupujte podle dokumentace hello na ověření Identity SAP HANA cloudové platformy příliš[konfigurace Microsoft Azure AD jako podnikové poskytovatele Identity v SAP HANA cloudové platformy identitu ověřování](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

7. Na portálu pro správu Azure hello, klikněte na tlačítko **Uložit** tlačítko.
8. Pokračujte hello pouze v případě, že chcete tooadd a povolení jednotného přihlašování pro jinou aplikaci SAP následující kroky. Opakujte kroky v části hello části "Přidání SAP HANA cloudové platformy identitu ověřování z Galerie hello" tooadd jiná instance SAP HANA cloudové platformy identitu ověřování.
9. V hello Azure Management portal na hello **SAP HANA cloudové platformy identitu ověřování** stránky integrace aplikací, klikněte na tlačítko **propojené přihlášení**.

    ![Konfigurovat propojené přihlášení](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)
10. Potom uložte konfiguraci hello.

>[!NOTE] 
>Nová aplikace Hello využije hello Konfigurace jednotného přihlašování pro předchozí aplikaci SAP hello. Zkontrolujte, zda je použití hello stejné podnikové poskytovatelů identit v hello SAP HANA cloudové platformy identitu ověřování konzole pro správu.
>

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v hello názvem Britta Simon nového portálu.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png) 
  1. V hello **název** textovému poli, typ **BrittaSimon**.
  2. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.
  3. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.
  4. Klikněte na možnost **Vytvořit**. 

### <a name="create-a-sap-hana-cloud-platform-identity-authentication-test-user"></a>Vytvoření zkušebního uživatele SAP HANA cloudové platformy identitu ověřování

Nepotřebujete toocreate uživatelé na SAP HANA cloudové platformy identitu ověřování. Uživatelé, kteří jsou v úložišti uživatele hello Azure AD můžete použít funkci jednotného přihlašování k hello.

Ověření Identity SAP HANA cloudové platformy podporuje možnost hello federaci identit. Tato možnost umožňuje toocheck aplikace hello, pokud existují uživatelé hello ověřována hello podnikové identitě zprostředkovatele v hello úložiště systému SAP HANA cloudové platformy identitu ověřování uživatelů. 

Ve výchozím nastavení je hello je zakázána hello možnost federaci identit. Pokud je povoleno federaci identit, jsou pouze hello uživatelé, kteří jsou importovány v SAP HANA cloudové platformy identitu ověřování aplikací mít tooaccess hello. 

Další informace o tom, jak tooenable nebo zakázat federaci identit s SAP HANA cloudové platformy identitu ověřování, najdete v části Povolit federaci identit s SAP HANA cloudové platformy identitu ověřování v [konfigurace federaci identit s hello úložiště systému SAP HANA cloudové platformy identitu ověřování uživatelů. ](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte svůj přístup tooSAP HANA cloudové platformy identitu ověřování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooSAP HANA cloudové platformy identitu ověřování, proveďte následující kroky hello:**

1. Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **SAP HANA cloudové platformy identitu ověřování**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_09.png)

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    

### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.

Když kliknete na dlaždici hello SAP HANA cloudové platformy identitu ověřování v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour SAP HANA cloudové platformy identitu ověřování aplikace.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_06.png

[100]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_203.png