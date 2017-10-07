---
title: "Kurz: Azure Active Directory integrace s Adobe přihlašovací | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Adobe přihlášení."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: b4b07907f30a0890003554a02a76d968400b43ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Kurz: Azure Active Directory integrace s Adobe přihlášení

V tomto kurzu zjistíte, jak toointegrate Adobe přihlaste s Azure Active Directory (Azure AD).

Integrace Adobe přihlášení s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooAdobe přihlášení
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAdobe přihlašovací (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD znakem Adobe, je třeba hello následující položky:

- Předplatné služby Azure AD
- Přihlášení Adobe jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Adobe přihlášení z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-adobe-sign-from-hello-gallery"></a>Přidání Adobe přihlášení z Galerie hello
tooconfigure hello Integrace Adobe registrace do Azure AD, je nutné tooadd Adobe přihlašovací hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Adobe přihlášení z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Adobe přihlašovací**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. Na panelu výsledků hello vyberte **Adobe přihlašovací**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování znakem Adobe podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Adobe přihlášení je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Adobe přihlašovací musí toobe navázat.

V Adobe přihlašovací přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Adobe přihlášení, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele přihlášení Adobe](#creating-an-adobe-sign-test-user)**  -toohave protějšek Britta Simon v Adobe přihlášení, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Adobe přihlášení.

**tooconfigure Azure AD jednotné přihlašování znakem Adobe, proveďte hello následující kroky:**

1. V portálu Azure, na hello hello **Adobe přihlašovací** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. Na hello **Adobe přihlašovací domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.echosign.com/`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.echosign.com`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory Adobe přihlašovací klienta](https://helpx.adobe.com/in/contact/support.html) tooget tyto hodnoty. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_400.png)

6. Na hello **Adobe přihlašovací konfigurace** klikněte na tlačítko **nakonfigurovat přihlašovací Adobe** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_configure.png) 


7. V okně prohlížeče jiný web Přihlaste se jako správce v tooyour Adobe přihlášení na webu společnosti.

8. V nabídce hello hello nahoře, klikněte na tlačítko **účet**a klikněte v navigačním podokně hello na levé straně hello **SAML nastavení** pod **nastavení účtu**.
   
   ![Účet](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "účtu")

9. V hello v oddílu nastavení SAML proveďte hello následující kroky:
   
   ![Nastavení SAML](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "nastavení SAML")
   
   a. Jako **SAML režimu**, vyberte **SAML povinné**.
   
   b. Vyberte **toolog povolit správci EchoSign účtu pomocí svých přihlašovacích údajů EchoSign**.
   
   c. Jako **vytvoření uživatele**, vyberte **automaticky přidat uživatele ověřeného pomocí SAML**.

10. Přesunout, provádění hello následující kroky:

       ![Nastavení SAML](./media/active-directory-saas-adobe-echosign-tutorial/ic789522.png "nastavení SAML")

    a. Vložení **SAML Entity ID**, který jste zkopírovali z portálu Azure do hello **IdP Entity ID** textové pole.
    
    b. Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure do hello **IdP přihlašovací adresa URL** textové pole.
   
    c. Vložení **Sign-Out URL**, který jste zkopírovali z portálu Azure do hello **adresy URL odhlašovací IdP** textové pole.

    d. Otevřete váš stažené **Certificate(Base64)** souborů v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **IdP certifikát** textbox

    e. Klikněte na tlačítko **uložit změny**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-adobe-sign-test-user"></a>Vytváření testovacího uživatele přihlášení Adobe

Uživatelé toolog tooenable Azure AD v tooAdobe přihlášení, se musí být zřízená do Adobe přihlášení. V případě hello Adobe registrace zřizování je ruční úloha.

>[!NOTE]
>Můžete použít jakékoli jiné Adobe přihlášení uživatele účtu vytvoření nástroje nebo rozhraní API poskytované tooprovision Adobe přihlašovací uživatelské účty AAD. 

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour **Adobe přihlašovací** společnosti lokality jako správce.

2. V nabídce hello hello nahoře, klikněte na tlačítko **účet**a klikněte v navigačním podokně hello na levé straně hello **uživatelé a skupiny**a potom klikněte na **vytvořte nového uživatele**.
   
   ![Účet](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "účtu")
   
3. V hello **vytvořit nového uživatele** část, proveďte následující kroky hello:
   
   ![Vytvoření uživatele](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "vytvoření uživatele")
   
   a. Typ hello **e-mailovou adresu**, **křestní jméno**, a **příjmení** z platný účet AAD chcete tooprovision do hello související textových polí.
   
   b. Klikněte na tlačítko **vytvořit uživatele**.

>[!NOTE]
>Držitel účtu Azure Active Directory Hello obdrží e-mail, který obsahuje účet odkaz tooconfirm hello předtím, než se stane aktivní. 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooAdobe přihlašovací Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign tooAdobe Britta Simon přihlášení, proveďte hello následující kroky:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Adobe přihlašovací**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Po kliknutí na tlačítko hello Adobe přihlašovací dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour aplikace Adobe přihlášení.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_203.png

