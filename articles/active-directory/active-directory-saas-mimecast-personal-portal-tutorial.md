---
title: "Kurz: Azure Active Directory integrace s Mimecast osobní portál | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Mimecast osobní portál."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: ee2a8edcab36f295732ac1ebe641ed7fcfc1f2a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Kurz: Azure Active Directory integrace s Mimecast osobní portálu

V tomto kurzu zjistíte, jak toointegrate Mimecast osobní portálu v Azure Active Directory (Azure AD).

Integrace portálu osobní Mimecast s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooMimecast osobní portálu
- Vaši uživatelé tooautomatically get přihlášeného tooMimecast osobní portálu (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Mimecast osobní portál, je třeba hello následující položky:

- Předplatné služby Azure AD
- Portálu osobní Mimecast jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání osobní portálu Mimecast z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-mimecast-personal-portal-from-hello-gallery"></a>Přidání osobní portálu Mimecast z Galerie hello
tooconfigure hello integrace Mimecast osobní portálu do Azure AD, je nutné tooadd Mimecast osobní portál hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Mimecast osobní portál z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Mimecast osobní portál**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_search.png)

5. Na panelu výsledků hello vyberte **osobní portál Mimecast**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Mimecast osobní portál podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello osobní portálu Mimecast je tooa uživatelem ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello osobní portálu Mimecast musí toobe navázat.

Osobní portálu Mimecast přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Mimecast osobní portál, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele osobní portál Mimecast](#creating-a-mimecast-personal-portal-test-user)**  -toohave protějšek Britta Simon Mimecast osobní portálu, který je propojený toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Mimecast osobní portál.

**tooconfigure Azure AD jednotné přihlašování s Mimecast osobní portál, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **osobní portál Mimecast** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_samlbase.png)

3. Na hello **Mimecast osobní portál domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru: 
    | |     
    | ----------------------------------------|
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|
    | |
   
    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:

    | |     
    | --- |
    | `https://webmail-us.mimecast.com/sso/<companyname>`|
    | `https://webmail-uk.mimecast.com/sso/<companyname>`|    
    | `https://webmail-za.mimecast.com/sso/<companyname>`|
    | `https://webmail.mimecast-offshore.com/sso/<companyname>`|
    ||                                                 
    
    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory Mimecast osobní portál klienta](https://www.mimecast.com/customer-success/technical-support/) tooget tyto hodnoty. 
 


4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_400.png)

6. Na hello **osobní konfigurace portálu Mimecast** klikněte na tlačítko **nakonfigurovat portál osobní Mimecast** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se k portálu osobní Mimecast jako správce.

8. Přejděte příliš**služby \> aplikace**.
   
    ![Aplikace](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794998.png "aplikace")

9. Klikněte na tlačítko **ověřování profily**.
   
    ![Profily ověřování](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794999.png "profily ověřování")

10. Klikněte na tlačítko **nový profil ověřování**.
   
    ![Nový profil ověřování](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795000.png "nový profil ověřování")

11. V hello **profil ověření** část, proveďte následující kroky hello:
   
    ![Profil ověření](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795001.png "profil ověření")
   
    a. V hello **popis** textovému poli, zadejte název pro svou konfiguraci.
   
    b. Vyberte **vynutit ověřování SAML pro portál Mimecast osobní**.
   
    c. Jako **zprostředkovatele**, vyberte **Azure Active Directory**.
   
    d. V **URL vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.
   
    e. V **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.
   
    f. V **adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.

    g. Otevřete váš **kódování base-64** kódování certifikátu v poznámkovém bloku stáhli z portálu Azure, hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát zprostředkovatele Identity (Metadata)** textové pole.

    h. Vyberte **povolit jednotné přihlašování na**.
   
    i. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-mimecast-personal-portal-test-user"></a>Vytvoření zkušebního uživatele Mimecast osobní portálu

V pořadí tooenable Azure AD Uživatelé toolog Mimecast osobní portálu musí být zřízená Mimecast osobní portálu. V případě hello osobní portálu Mimecast zřizování je ruční úloha.

Než můžete vytvořit uživatele, musíte tooregister domény.

**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**

1. Přihlaste se tooyour **Mimecast osobní portál** jako správce.

2. Přejděte příliš**adresáře \> interní**.
   
    ![Adresáře](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795003.png "adresáře")

3. Klikněte na tlačítko **zaregistrovat nové domény**.
   
    ![Zaregistrujte novou doménu](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795004.png "zaregistrovat nové domény")

4. Po vytvoření nové domény, klikněte na tlačítko **novou adresu**.
   
    ![Nové adresy](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795005.png "novou adresu")

5. V hello adresu dialogové okno Nový, provádět hello kroků platný Azure AD účet chcete tooprovision:
   
    ![Uložit](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795006.png "uložit")
   
    a. V hello **e-mailovou adresu** textovému poli, typ **e-mailovou adresu** hello uživatele jako  **BrittaSimon@contoso.com** .
    
    b. V hello **globální název** textovému poli, typ hello **uživatelské jméno** jako **BrittaSimon**.

    c. V hello **heslo**, a **Potvrdit heslo** textová pole, typ hello **heslo** hello uživatele.
   
    b. Klikněte na **Uložit**.

>[!NOTE]
>Můžete použít jiné nástroje pro tvorbu účet uživatele Mimecast osobní portál nebo rozhraní API poskytovaných osobní portál Mimecast tooprovision Azure AD uživatelské účty. 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooMimecast osobní portál Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooMimecast osobní portál, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Mimecast osobní portál**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování
V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici osobní portál Mimecast hello v hello přístupového panelu, měli byste obdržet aplikace automaticky přihlášeného tooyour Mimecast osobní portálu. Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_203.png

