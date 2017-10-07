---
title: "Kurz: Azure Active Directory integrace s Menlo zabezpečení | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Menlo zabezpečení."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9e63fe6b-0ad0-405d-9e41-6a1a40a41df8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: jeedes
ms.openlocfilehash: 193d12eedf31f4f08e1d141936d6e918c36a2109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-menlo-security"></a>Kurz: Azure Active Directory integrace s Menlo zabezpečení

V tomto kurzu zjistíte, jak toointegrate Menlo zabezpečení v Azure Active Directory (Azure AD).

Integrace Menlo zabezpečení s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooMenlo zabezpečení
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooMenlo zabezpečení (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v článku. [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Menlo zabezpečení, je třeba hello následující položky:

- Předplatné služby Azure AD
- Zabezpečení Menlo jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Menlo zabezpečení z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-menlo-security-from-hello-gallery"></a>Přidání Menlo zabezpečení z Galerie hello
tooconfigure hello integraci Menlo zabezpečení do služby Azure AD, je nutné tooadd Menlo zabezpečení hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Menlo zabezpečení z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Menlo zabezpečení**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_search.png)

5. Na panelu výsledků hello vyberte **Menlo zabezpečení**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Menlo zabezpečení založené na testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Menlo zabezpečení je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Menlo zabezpečení musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Menlo zabezpečení.

tooconfigure a testu Azure AD jednotné přihlašování s Menlo zabezpečení, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Menlo zabezpečení](#creating-a-menlo-security-test-user)**  -toohave protějšek Britta Simon v Menlo zabezpečení, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Menlo zabezpečení.

**tooconfigure Azure AD jednotné přihlašování s Menlo zabezpečení, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Menlo zabezpečení** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_samlbase.png)

3. Na hello **Menlo zabezpečení domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.menlosecurity.com/account/login`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné hello. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory zabezpečení klienta Menlo](https://www.menlosecurity.com/menlo-contact) tooget tyto hodnoty. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_400.png)

6. Na hello **konfigurace zabezpečení Menlo** klikněte na tlačítko **konfigurace zabezpečení Menlo** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID**, a **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_configure.png) 

7. tooconfigure jednotného přihlašování na **Menlo zabezpečení** straně, přihlášení toohello **Menlo zabezpečení** webu jako správce.

8. V části **nastavení** přejděte příliš**ověřování** a proveďte následující akce:
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/menlo_user_setup.png)

    a. Zaškrtněte políčko hello **povolení ověřování uživatele pomocí SAML**.

    b. Vyberte **povolit externí přístup** příliš**Ano**.

    c. V části **SAML zprostředkovatele**, vyberte **Azure Active Directory**.

    d. **Koncový bod SAML 2.0** : vložení hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.

    e. **Identifikátor služby (Vystavitel)** : vložení hello **SAML Entity ID** který jste zkopírovali z portálu Azure.

    f. **Certifikát X.509** : Otevřete hello **certifikátu (Base64)** stáhli z portálu Azure hello v programu Poznámkový blok a vložte ho v tomto poli.

    g. Klikněte na tlačítko **Uložit** toosave hello nastavení.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-menlo-security-test-user"></a>Vytvoření zkušebního uživatele Menlo zabezpečení
 
V této části vytvoříte volal Britta Simon v Menlo zabezpečení uživatele. Práce s [tým podpory zabezpečení klienta Menlo](https://www.menlosecurity.com/menlo-contact) tooadd hello uživatelé v platformě Menlo zabezpečení hello. Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování. 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooMenlo zabezpečení Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooMenlo zabezpečení, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Menlo zabezpečení**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování v konfiguraci.

Otevřete okno prohlížeče v "InPrivate" nebo "Incognito" režimu tootrigger nové ověřování.  V aplikaci Internet Explorer použijte kombinaci kláves Ctrl + Shift + P.  V prohlížeči Chrome použijte kombinaci kláves Ctrl + Shift + N.  V okně privátní procházení hello procházet tooa chráněný prostředek a provést přihlášení Azure AD.  Po úspěšném přihlášení bude požadovaný webu přijatá toohello v relaci izolace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_203.png

