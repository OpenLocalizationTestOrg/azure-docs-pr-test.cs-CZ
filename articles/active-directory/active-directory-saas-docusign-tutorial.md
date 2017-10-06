---
title: 'Kurz: Azure Active Directory integrace s DocuSign | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a DocuSign."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: e4ef40b8f5af20d811d8d806d2bd7e2039c55052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Kurz: Azure Active Directory integrace s DocuSign

V tomto kurzu zjistíte, jak toointegrate DocuSign s Azure Active Directory (Azure AD).

Integrace DocuSign s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooDocuSign
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooDocuSign (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s DocuSign tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- DocuSign jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání DocuSign z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-docusign-from-hello-gallery"></a>Přidání DocuSign z Galerie hello
tooconfigure hello integrace DocuSign do Azure AD, je nutné tooadd DocuSign hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd DocuSign z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **DocuSign**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_search.png)

5. Na panelu výsledků hello vyberte **DocuSign**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s DocuSign podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v DocuSign je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v DocuSign musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v DocuSign.

tooconfigure a testu Azure AD jednotné přihlašování s DocuSign, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele DocuSign](#creating-a-docusign-test-user)**  -toohave protějšek Britta Simon v DocuSign, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci DocuSign.

**tooconfigure Azure AD jednotné přihlašování s DocuSign, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **DocuSign** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_samlbase.png)

3. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base 64)** a potom uložte soubor certifikátu v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_certificate.png) 

4. Na hello **DocuSign konfigurace** části portálu Azure, klikněte na tlačítko **konfigurace DocuSign** tooopen přihlašování konfigurace okna. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_configure.png)

5. V okně prohlížeče jiný web, přihlášení tooyour **portál pro správu DocuSign** jako správce.

6. V navigační nabídce hello na levé straně hello, klikněte na **domény**.
   
    ![Konfigurace jednotného přihlašování][51]

7. V pravém podokně hello, klikněte na tlačítko **deklarace identity domény**.
   
    ![Konfigurace jednotného přihlašování][52]

8. Na hello **deklarace identity domény** dialogové okno, ve hello **název domény** textovému poli, zadejte doménu vaší společnosti a pak klikněte na tlačítko **deklarace identity**. Ujistěte se, že ověření hello domény a stav hello je aktivní.
   
    ![Konfigurace jednotného přihlašování][53]

9. V nabídce na levé straně hello, klikněte na tlačítko **poskytovatelů identit**  
   
    ![Konfigurace jednotného přihlašování][54]
10. V pravém podokně hello, klikněte na **přidat zprostředkovatele Identity**. 
   
    ![Konfigurace jednotného přihlašování][55]

11. Na hello **nastavení zprostředkovatele Identity** proveďte hello následující kroky:
   
    ![Konfigurace jednotného přihlašování][56]

    a. V hello **název** textovému poli, zadejte jedinečný název pro svou konfiguraci. Nepoužívejte mezery.

    b. Vložení **SAML Entity ID** do hello **vystavitele zprostředkovatele Identity** textové pole.

    c. Vložení **SAML jeden přihlašování adresa URL služby** do hello **adresu URL pro přihlášení zprostředkovatele Identity** textové pole.

    d. Vložení **Sign-Out URL** do hello **adresa URL odhlašovací zprostředkovatele Identity** textové pole.

    e. Vyberte **přihlásit ověřovacího požadavku**.

    f. Jako **odeslání ověřovacího požadavku pomocí**, vyberte **POST**.

    g. Jako **odeslán požadavek na odhlášení podle**, vyberte **získat**.

12. V hello **vlastní atribut mapování** zvolte pole hello chcete toomap s Azure AD deklarací identity. V tomto příkladu hello **emailaddress** deklarace identity je namapována na hodnotu hello **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**. Je hello výchozí název deklarace identity z Azure AD pro deklarace identity e-mailu. 
   
    > [!NOTE]
    > Použití hello odpovídající **uživatelský identifikátor** toomap hello uživatele z mapování uživatelů tooDocuSign Azure AD. Vyberte hello správné pole a zadejte odpovídající hodnotu hello na základě nastavení vaší organizace.
          
    ![Konfigurace jednotného přihlašování][57]

13. V hello **certifikát zprostředkovatele Identity** klikněte na tlačítko **přidat certifikát**a pak nahrajte certifikát hello jste si stáhli z portálu Azure AD.   
   
    ![Konfigurace jednotného přihlašování][58]

14. Klikněte na **Uložit**.

15. V hello **zprostředkovatelů Identity** klikněte na tlačítko **akce**a potom klikněte na **koncové body**.   
   
    ![Konfigurace jednotného přihlašování][59]
 
16. V hello **zobrazit SAML 2.0 koncové body** části na **portál pro správu DocuSign**, proveďte následující kroky hello:
   
    ![Konfigurace jednotného přihlašování][60]
   
    a. Kopírování hello **adresa URL vystavitele pro zprostředkovatele služby**a vložte do hello **identifikátor** textové pole na **DocuSign domény a adresy URL** části hello Azure portálu následující hello vzor: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.
   
    b. Kopírování hello **adresa URL služby zprostředkovatele přihlášení**a vložte do hello **přihlašovací adresa URL** textové pole na **DocuSign domény a adresy URL** části hello Azure portálu následující hello vzor: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/`.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_url.png)
      
    c.  Klikněte na tlačítko **zavřít**
    
17. Na portálu Azure hello, klikněte na tlačítko **Uložit**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png)

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-docusign-test-user"></a>Vytvoření zkušebního uživatele DocuSign

Aplikace podporuje **těsně v zřizování uživatelů čas** a po ověření uživatele v aplikaci hello automaticky vytvoří.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooDocuSign svůj přístup.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooDocuSign, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **DocuSign**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici DocuSign hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour DocuSign aplikace.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfiguraci zřizování uživatelů](active-directory-saas-docusign-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png
[100]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_203.png

