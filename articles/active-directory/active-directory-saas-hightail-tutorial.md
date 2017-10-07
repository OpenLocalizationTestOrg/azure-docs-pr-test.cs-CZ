---
title: 'Kurz: Azure Active Directory integrace s Hightail | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Hightail."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e15206ac-74b0-46e4-9329-892c7d242ec0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 2b36fcf8d5773255fdf89de2dccdceb95c032bd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hightail"></a>Kurz: Azure Active Directory integrace s Hightail

V tomto kurzu zjistíte, jak toointegrate Hightail s Azure Active Directory (Azure AD).

Integrace Hightail s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooHightail
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooHightail (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Hightail tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Hightail jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Hightail z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-hightail-from-hello-gallery"></a>Přidání Hightail z Galerie hello
tooconfigure hello integrace Hightail do Azure AD, je nutné tooadd Hightail hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Hightail z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Hightail**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_search.png)

5. Na panelu výsledků hello vyberte **Hightail**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Hightail podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Hightail je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Hightail musí toobe navázat.

V Hightail, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Hightail, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Hightail](#creating-a-hightail-test-user)**  -toohave protějšek Britta Simon v Hightail, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Hightail.

**tooconfigure Azure AD jednotné přihlašování s Hightail, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Hightail** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_samlbase.png)

3. Na hello **Hightail domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url.png)

     V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL hello jako:`https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`

    > [!NOTE] 
    > Hello předchozí hodnota není skutečné hodnoty. Aktualizujte hodnotu hello s hello skutečná adresa URL odpovědi, který je vysvětlen později v kurzu hello.
 
4. Na hello **Hightail domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **SP iniciované režimu**, proveďte následující kroky hello:
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url1.png)

    a. Klikněte na tlačítko hello **zobrazit upřesňující nastavení adresy URL**.

    b. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL hello jako:`https://www.hightail.com/loginSSO`

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_certificate.png) 

5. Hightail aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu. Nakonfigurujte hello následující deklarace identity pro tuto aplikaci. Můžete spravovat hello hodnoty těchto atributů z hello **"Atrribute"** karta aplikace hello. Hello následující snímek obrazovky ukazuje příklad pro tento. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_attribute.png) 

6. V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:
    
    | Název atributu | Hodnota atributu |
    | ------------------- | -------------------- |
    | FirstName | User.givenName |
    | Příjmení | User.Surname |
    | E-mail | User.Mail |    
    | Identity uživatele | User.Mail |
    
    a. Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_05.png)

    b. V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.

    c. Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.

    d. Nechte hello **Namespace** prázdné.
    
    e. Klikněte na tlačítko **OK**.

7. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_general_400.png)

8. Na hello **Hightail konfigurace** klikněte na tlačítko **konfigurace Hightail** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_configure.png) 

    >[!NOTE] 
    >Před konfigurací hello jednotné přihlašování v aplikaci Hightail, prosím seznamu povolených vaše e-mailovou doménu s Hightail team tak, aby všechny hello uživatelé, kteří používají tuto doménu můžete použít funkci jednotného přihlašování.


9. tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, musíte klienta Hightail toosign na tooyour jako správce.
   
    a. V nabídce hello hello nahoře, klikněte na tlačítko hello **účet** a vyberte **konfigurace SAML**.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png) 

    b. Zaškrtněte políčko hello z **povolit ověřování SAML**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png) 

    c. Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku stáhli z portálu Azure, hello kopírování obsahu ho do schránky a pak ji vložit toohello **SAML podpisový certifikát tokenů** textové pole.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png) 

    d. V hello **autority SAML (zprostředkovatele Identity)** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** zkopírovaných z portálu Azure.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

    e. Pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu** vyberte **"Zprostředkovatele Identity (IdP) iniciované přihlášení"**. Pokud **SP iniciované režimu** vyberte **"Přihlášení iniciované poskytovatelem služeb (SP)"**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

    f. Hello SAML příjemce adresu URL instance, zkopírujte a vložte jej do **adresa URL odpovědi** textového pole v **Hightail domény a adresy URL** části na portálu Azure.
    
    g. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-hightail-test-user"></a>Vytvoření zkušebního uživatele Hightail

Hello cílem této části je toocreate volal Britta Simon v Hightail uživatele. 

Neexistuje žádná položka akce pro vás v této části. Podporuje zřizování uživatelů za běhu v závislosti na vlastní deklarace hello hightail. Pokud jste nakonfigurovali vlastní deklarace hello hello část  **[konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  vyšší, se automaticky vytvoří uživatele v aplikaci hello ještě neexistuje. 

>[!NOTE]
>Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory Hightail](mailto:support@hightail.com). 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooHightail toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooHightail, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Hightail**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.

Když kliknete na dlaždici Hightail hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Hightail aplikace.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png

