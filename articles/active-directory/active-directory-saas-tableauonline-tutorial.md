---
title: 'Kurz: Azure Active Directory integrace s Tableau Online | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Tableau Online."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 590b2674270c340b4750c7b6feeaf4f0df4bf853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Kurz: Azure Active Directory integrace s Tableau Online

V tomto kurzu zjistíte, jak toointegrate Tableau Online se službou Azure Active Directory (Azure AD).

Integrace Tableau Online s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooTableau Online
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTableau Online (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Tableau Online, je třeba hello následující položky:

- Předplatné služby Azure AD
- Tableau Online jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Tableau Online z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-tableau-online-from-hello-gallery"></a>Přidání Tableau Online z Galerie hello
tooconfigure hello integrace Tableau Online do služby Azure AD, je nutné tooadd Tableau Online hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Tableau Online z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Tableau Online**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_search.png)

5. Na panelu výsledků hello vyberte **Tableau Online**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Tableau Online na základě testovací uživatele, nazývá "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Tableau Online je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Tableau Online musí toobe navázat.

V Tableau Online přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Tableau Online, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Tableau Online](#creating-a-tableau-online-test-user)**  -toohave protějšek Britta Simon v Tableau Online, které je propojené toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Tableau Online.

**tooconfigure Azure AD jednotné přihlašování s Tableau Online, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Tableau Online** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_samlbase.png)

3. Na hello **Tableau Online domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_url.png)
    
    a. V hello **přihlašovací adresa URL** textovému poli, hello zadat adresu URL:`https://sso.online.tableau.com`

    b. V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://sso.online.tableau.com/public/sp/<instancename>`

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_general_400.png)

6. V okně jiný prohlížeč, přihlášení tooyour Tableau Online aplikace. Přejděte příliš**nastavení** a potom **ověřování**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)
    
7. tooenable SAML, v části **typy ověřování** části. Zkontrolujte hello **jednotné přihlašování s SAML** zaškrtávací políčko.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

8. Posuňte se dolů, dokud **soubor Import metadat do Tableau Online** části.  Klikněte na tlačítko Procházet a importovat soubor hello metadat, které jste si stáhli z Azure AD. Potom klikněte na **použít**.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

9. V hello **odpovídat kontrolní výrazy** část, vložte hello odpovídající zprostředkovatele Identity kontrolní název pro **e-mailová adresa**, **křestní jméno**, a **příjmení** . tooget tyto informace z Azure AD: 
  
    a. V hello portálu Azure, přejděte na hello **Tableau Online** stránky integrace aplikace.
    
    b. V části atributy hello vyberte hello **"Zobrazit a upravit všechny ostatní atributy uživatele"** zaškrtávací políčko. 
    
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/attributesection.png)
      
    c. Zkopírujte hodnotu hello oboru názvů pro tyto atributy: givenname, e-mailu a příjmení pomocí hello následující kroky:

   ![Azure AD jednotné přihlášení](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)
    
    d. Klikněte na tlačítko **user.givenname** hodnota 
    
    e. Zkopírujte hodnotu hello z hello **obor názvů** textové pole.

   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/attributesection2.png)

    f. toocopy hello namesapce hodnoty pro e-mailu hello a příjmení podle předchozích kroků hello.

    g. Přepínač toohello Tableau Online aplikace a pak nastavte hello **Tableau Online atributy** části následujícím způsobem:
     * E-mailu: **e-mailu** nebo **userprincipalname**
     * Křestní jméno: **givenname**
     * Příjmení: **Přezdívka**
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-tableau-online-test-user"></a>Vytvoření Tableau Online zkušebního uživatele

V této části vytvoříte uživatele volal Britta Simon v Tableau Online.

1. Na **Tableau Online**, klikněte na tlačítko **nastavení** a potom **ověřování** části. Posuňte se dolů příliš**vybrat uživatele** části. Klikněte na tlačítko **přidat uživatele** a potom **zadejte e-mailové adresy**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. Vyberte **přidat uživatele pro jednotné přihlašování (SSO) ověřování**. V hello **zadejte e-mailové adresy** přidat textové polebritta.simon@contoso.com
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)
3. Klikněte na možnost **Vytvořit**.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooTableau Online toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooTableau Online, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Tableau Online**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.

Po kliknutí na tlačítko hello Tableau Online dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Tableau Online aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png

