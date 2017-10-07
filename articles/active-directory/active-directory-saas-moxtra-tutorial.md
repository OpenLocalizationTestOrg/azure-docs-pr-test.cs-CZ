---
title: 'Kurz: Azure Active Directory integrace s Moxtra | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Moxtra."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 82e2fcc390ba508e86a3992ec1c81d0a0ffed96b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a>Kurz: Azure Active Directory integrace s Moxtra

V tomto kurzu zjistíte, jak toointegrate Moxtra s Azure Active Directory (Azure AD).

Integrace Moxtra s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooMoxtra
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooMoxtra (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Moxtra tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Moxtra jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Moxtra z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-moxtra-from-hello-gallery"></a>Přidání Moxtra z Galerie hello
tooconfigure hello integrace Moxtra do Azure AD, je nutné tooadd Moxtra hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Moxtra z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Moxtra**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_search.png)

5. Na panelu výsledků hello vyberte **Moxtra**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Moxtra podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Moxtra je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Moxtra musí toobe navázat.

V Moxtra, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Moxtra, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Moxtra](#creating-a-moxtra-test-user)**  -toohave protějšek Britta Simon v Moxtra, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Moxtra.

**tooconfigure Azure AD jednotné přihlašování s Moxtra, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Moxtra** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_samlbase.png)

3. Na hello **Moxtra domény a adresy URL** část, proveďte následující krok hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_url.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://www.moxtra.com/service/#login`

4. Aplikace Moxtra očekává hello SAML kontrolní výrazy ve specifickém formátu. Nakonfigurujte hello následující deklarace identity pro tuto aplikaci. Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace. Hello následující snímek obrazovky ukazuje příklad pro tuto konfiguraci. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_attributes.png)
    
5. V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:
    
    | Název atributu | Hodnota atributu |
    | ------------------- | -------------------- |    
    | FirstName | User.givenName |
    | Příjmení | User.Surname |
    | idpid    | < SAML Entity ID > 

    > [!Note]
    > Hello hodnotu **idpid** atribut není skutečné. Můžete získat skutečnou hodnotu hello z **Stručná referenční příručka** oddílu pod **Moxtra konfigurace**.
    
    a. Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_04.png)

    b. V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_05.png)

    c. Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.

    d. Klikněte na tlačítko **OK**.
    
5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_general_400.png)

7. Na hello **Moxtra konfigurace** klikněte na tlačítko **konfigurace Moxtra** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_configure.png) 

8. V jiném okně prohlížeče Přihlaste se jako správce na webu společnosti Moxtra tooyour.

9. V panelu nástrojů hello na levé straně hello, klikněte na **konzoly pro správu > SAML jednotné přihlašování**a potom klikněte na **nový**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_06.png) 

10. Na hello **SAML** proveďte hello následující kroky:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_08.png)   
 
    a. V hello **název** textovému poli, zadejte název pro svou konfiguraci (například: *SAML*). 
  
    b. V hello **IdP Entity ID** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure. 
 
    c. V **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure. 
 
    d. V hello **AuthnContextClassRef** textovému poli, typ **urn: oasis: názvy: tc: SAML:2.0:ac:classes:Password**. 
 
    e. V hello **NameID formátu** textovému poli, typ **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: emailAddress**. 
 
    f. Otevřete certifikát, který jste si stáhli z portálu Azure v poznámkovém bloku hello obsah zkopírujte a vložte jej do hello **certifikát** textové pole.    
 
    g. Do pole pro e-mailovou doménu hello SAML zadejte vaše e-mailovou doménu SAML.    
  
    >[!NOTE]
    >toosee hello kroky tooverify hello domény, klikněte na tlačítko hello "**i**" níže.

    h. Klikněte na tlačítko **aktualizace**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-moxtra-test-user"></a>Vytvoření zkušebního uživatele Moxtra

Hello cílem této části je toocreate volal Britta Simon v Moxtra uživatele.

**toocreate uživatel volal Britta Simon v Moxtra, proveďte následující kroky hello:**

1. Přihlaste se na tooyour Moxtra společnosti lokality jako správce.

2. V panelu nástrojů hello na levé straně hello, klikněte na **konzoly pro správu > Správa uživatelů**a potom **přidat uživatele**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_10.png) 

3. Na hello **přidat uživatele** dialogové okno, proveďte následující kroky hello:
  
    a. V hello **křestní jméno** textovému poli, typ **Britta**.
  
    b. V hello **příjmení** textovému poli, typ **Simon**.
  
    c. V hello **e-mailu** textovému poli, typ Britta na e-mailová adresa, aby se pro stejné jako na portálu Azure.
  
    d. V hello **dělení** textovému poli, typ **Dev**.
  
    e. V hello **oddělení** textovému poli, typ **IT**.
  
    f. Vyberte **správce**.
  
    g. Klikněte na tlačítko **Přidat**.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooMoxtra toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooMoxtra, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Moxtra**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Moxtra hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Moxtra aplikace.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_203.png

