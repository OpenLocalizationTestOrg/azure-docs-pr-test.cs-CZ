---
title: 'Kurz: Azure Active Directory integrace s UserEcho | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a UserEcho."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: efe4a94ed6e5d22d153565d4782850eac4dff37b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a>Kurz: Azure Active Directory integrace s UserEcho

V tomto kurzu zjistíte, jak toointegrate UserEcho s Azure Active Directory (Azure AD).

Integrace UserEcho s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooUserEcho
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooUserEcho (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s UserEcho tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- UserEcho jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání UserEcho z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-userecho-from-hello-gallery"></a>Přidání UserEcho z Galerie hello
tooconfigure hello integrace UserEcho do Azure AD, je nutné tooadd UserEcho hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd UserEcho z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **UserEcho**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_search.png)

5. Na panelu výsledků hello vyberte **UserEcho**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s UserEcho podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v UserEcho je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v UserEcho musí toobe navázat.

V UserEcho, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s UserEcho, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele UserEcho](#creating-a-userecho-test-user)**  -toohave protějšek Britta Simon v UserEcho, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci UserEcho.

**tooconfigure Azure AD jednotné přihlašování s UserEcho, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **UserEcho** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_samlbase.png)

3. Na hello **UserEcho domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.userecho.com/`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.userecho.com/saml/metadata/`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory UserEcho klienta](https://feedback.userecho.com/) tooget tyto hodnoty. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_general_400.png)

6. Na hello **UserEcho konfigurace** klikněte na tlačítko **konfigurace UserEcho** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_configure.png) 

7. V jiném okně prohlížeče Přihlaste se jako správce na webu společnosti UserEcho tooyour.

8. Hello nástrojů v horní části hello, klikněte na tlačítko nabídky hello tooexpand název vaše uživatele a pak klikněte na **instalační program**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 

9. Klikněte na tlačítko **integrace**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_07.png) 

10. Klikněte na tlačítko **webu**a potom klikněte na **jednotné přihlašování (typu SAML2)**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_08.png) 

11. Na hello **jednotné přihlašování (SAML)** proveďte hello následující kroky:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_09.png)
    
    a. Jako **povoleno SAML**, vyberte **Ano**.
    
    b. Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z hello portálu Azure do hello **URL jednotné přihlašování SAML** textové pole.
    
    c. Vložení **Sign-Out URL**, který jste zkopírovali z hello portálu Azure do hello **adresy URL vzdálené logoout** textové pole.
    
    d. Otevřete stažený certifikát v poznámkovém bloku hello kopírování obsahu a pak ji vložit do hello **certifikát X.509** textové pole.
    
    e. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-userecho-test-user"></a>Vytvoření zkušebního uživatele UserEcho

Hello cílem této části je toocreate volal Britta Simon v UserEcho uživatele.

**toocreate uživatel volal Britta Simon v UserEcho, proveďte následující kroky hello:**

1. Web společnosti UserEcho tooyour přihlášení jako správce.

2. Hello nástrojů v horní části hello, klikněte na tlačítko nabídky hello tooexpand název vaše uživatele a pak klikněte na **instalační program**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png)

3. Klikněte na tlačítko **uživatelé**, tooexpand hello **uživatelé** části.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_10.png)

4. Klikněte na tlačítko **uživatelé**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_11.png)

5. Klikněte na tlačítko **pozvat nového uživatele**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_12.png)

6. Na hello **pozvat nového uživatele** dialogové okno, proveďte následující kroky hello:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_13.png)

    a. V hello **název** textovému poli, název typu hello uživatele jako Britta Simon.
    
    b.  V hello **e-mailu** jako typ hello e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.
    
    c. Klikněte na tlačítko **pozvat**.

Pozvánka se odesílají tooBritta, která umožňuje jeho toostart pomocí UserEcho. 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooUserEcho toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooUserEcho, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **UserEcho**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.  

Když kliknete na dlaždici UserEcho hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour UserEcho aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_203.png

