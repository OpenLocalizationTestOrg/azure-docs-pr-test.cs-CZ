---
title: "Kurz: Azure Active Directory integrace s konkrétně | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a konkrétně."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9541d5c4-4c82-4b5b-b01a-6a3f75a2b7a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: b0477ca6fa52a21abea7de458f8a99a01e8c25c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-namely"></a>Kurz: Azure Active Directory integrace s konkrétně

V tomto kurzu zjistíte, jak toointegrate konkrétně v Azure Active Directory (Azure AD).

Konkrétně integraci s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooNamely
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooNamely (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

integrace tooconfigure Azure AD s konkrétně, budete potřebovat hello následující položky:

- Předplatné služby Azure AD
- Konkrétně jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání konkrétně z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-namely-from-hello-gallery"></a>Přidání konkrétně z Galerie hello
tooconfigure hello integrace konkrétně do Azure AD, je nutné tooadd konkrétně z hello Galerie tooyour seznam spravovaných aplikací SaaS.

**tooadd konkrétně z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **konkrétně**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_search.png)

5. Na panelu výsledků hello vyberte **konkrétně**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s, a to podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v konkrétně je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v konkrétně musí toobe navázat.

V konkrétně, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testování Azure AD jednotné přihlašování s konkrétně, budete potřebovat toocomplete hello následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření konkrétně testovací uživatel](#creating-a-namely-test-user)**  -toohave protějšek Britta Simon v konkrétně to znamená propojené toohello reprezentace uživatele Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v konkrétně aplikace.

**tooconfigure Azure AD jednotné přihlašování s konkrétně, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **konkrétně** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_samlbase.png)

3. Na hello **konkrétně domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.namely.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.namely.com/saml/metadata`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory konkrétně klienta](https://www.namely.com/contact/) tooget tyto hodnoty. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_general_400.png)

6. Na hello **konkrétně konfigurace** klikněte na tlačítko **konfigurace konkrétně** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_configure.png) 

7. Přihlašování tooyour v jiném okně prohlížeče, a to společnosti lokality jako správce.

8. V panelu nástrojů hello hello nahoře, klikněte na **společnosti**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

9. Klikněte na tlačítko hello **nastavení** kartě.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 

10. Klikněte na tlačítko **SAML**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 

11. Na hello **SAML nastavení** proveďte hello následující kroky:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png)
 
    a. Klikněte na tlačítko **povolit SAML**. 

    b. V hello **adresa url jednotné přihlašování zprostředkovatele Identity** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.
    
    c. Otevřete stažený certifikát v poznámkovém bloku hello kopírování obsahu a pak ji vložit do hello **certifikát zprostředkovatele Identity** textové pole.
     
    d. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-namely-test-user"></a>Vytváření konkrétně testovací uživatel

Hello cílem této části je toocreate volal Britta Simon v konkrétně uživatele.

**toocreate uživatel volal Britta Simon v konkrétně, proveďte následující kroky hello:**

1. Přihlášení tooyour konkrétně společnosti lokality jako správce.

2. V panelu nástrojů hello hello nahoře, klikněte na **osoby**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

3. Klikněte na tlačítko hello **Directory** kartě.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

4. Klikněte na tlačítko **přidat nové osobě**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_12.png)

5. Na hello **přidat nové osobě** dialogové okno, proveďte následující kroky hello:

    a. V hello **křestní jméno** textovému poli, typ **Britta**.

    b. V hello **příjmení** textovému poli, typ **Simon**.

    c. V hello **e-mailu** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    d. Klikněte na **Uložit**.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooNamely toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooNamely, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **konkrétně**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.

Po kliknutí na tlačítko hello konkrétně dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour konkrétně aplikace

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-namely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png

