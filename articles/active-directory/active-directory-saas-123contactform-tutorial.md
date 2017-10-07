---
title: 'Kurz: Azure Active Directory integrace s 123ContactForm | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a 123ContactForm."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5211910a-ab96-4709-959a-524c4d57c43e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 931255887845edd1aa7f53b9051a82a2f898e055
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-123contactform"></a>Kurz: Azure Active Directory integrace s 123ContactForm

V tomto kurzu zjistíte, jak toointegrate 123ContactForm s Azure Active Directory (Azure AD).

Integrace 123ContactForm s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup too123ContactForm
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného too123ContactForm (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s 123ContactForm tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- 123ContactForm jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání 123ContactForm z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-123contactform-from-hello-gallery"></a>Přidání 123ContactForm z Galerie hello
tooconfigure hello integrace 123ContactForm do Azure AD, je nutné tooadd 123ContactForm hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd 123ContactForm z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **123ContactForm**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_search.png)

5. Na panelu výsledků hello vyberte **123ContactForm**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s 123ContactForm podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v 123ContactForm je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v 123ContactForm musí toobe navázat.

V 123ContactForm, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s 123ContactForm, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele 123ContactForm](#creating-a-123contactform-test-user)**  -toohave protějšek Britta Simon v 123ContactForm, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci 123ContactForm.

**tooconfigure Azure AD jednotné přihlašování s 123ContactForm, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **123ContactForm** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_samlbase.png)

3. Na hello **123ContactForm domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu**, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/url1.png)

    a. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`

4. Pokud chcete aplikace hello tooconfigure v **SP iniciované režimu**, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/url2.png)

    a. Klikněte na tlačítko hello **zobrazit upřesňující nastavení adresy URL** možnost

    b. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tooupdate budete potřebovat tyto hodnoty z skutečné adresy URL a identifikátor, který je vysvětlen později v kurzu hello.
    
5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/tutorial_general_400.png)

7. tooconfigure jednotného přihlašování na **123ContactForm** straně, přejděte příliš[https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) a provádět hello následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/submit.png) 

    a. V hello **e-mailu** textovému poli, typ hello e-mail uživatele jednofaktorovému hello **BrittaSimon@Contoso.com**.

    b. Klikněte na tlačítko **nahrát** a procházet hello soubor XML s metadaty souboru, který jste si stáhli z portálu Azure.

    c. Klikněte na tlačítko **odeslání formuláře**.

8. Na hello **nakonfigurovat nastavení aplikace služby Microsoft Azure AD jednotné přihlašování –** provést hello následující kroky:
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/url3.png)

    a. Pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu**, kopie hello **IDENTIFIKÁTOR** instance a vložte ji v **identifikátor** textového pole v **123ContactForm domény a adresy URL** části na portálu Azure.
    
    b. Pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu**, kopie hello **adresa URL odpovědi** instance a vložte ji v **adresa URL odpovědi** textového pole v **123ContactForm domény a adresy URL** části na portálu Azure.

    c. Pokud chcete aplikace hello tooconfigure v **SP iniciované režimu**, kopie hello **ON PŘIHLAŠOVACÍ adresa URL** instance a vložte ji v **přihlašovací adresa URL** textového pole v **123ContactForm domény a adresy URL** části na portálu Azure.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-123contactform-test-user"></a>Vytvoření zkušebního uživatele 123ContactForm

Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatele budou vytvořeny v hello aplikace automaticky.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup too123ContactForm toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon too123ContactForm, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **123ContactForm**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici 123ContactForm hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour 123ContactForm aplikace.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_203.png

