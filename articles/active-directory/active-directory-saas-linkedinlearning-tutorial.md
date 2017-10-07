---
title: 'Kurz: Azure Active Directory integrace s LinkedIn Learning | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a LinkedIn Learning."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: jeedes
ms.openlocfilehash: 14610a25132ed0ccf5892cad6ccc4e1ef03ff280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a>Kurz: Azure Active Directory integrace s LinkedIn Learning

V tomto kurzu zjistíte, jak toointegrate LinkedIn Learning s Azure Active Directory (Azure AD).

Integrace LinkedIn Learning s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooLinkedIn učení
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLinkedIn Learning (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s LinkedIn Learning, je třeba hello následující položky:

- Předplatné služby Azure AD
- LinkedIn Learning jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání LinkedIn Learning z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-linkedin-learning-from-hello-gallery"></a>Přidání LinkedIn Learning z Galerie hello
tooconfigure hello integrace LinkedIn Learning do Azure AD, je nutné tooadd LinkedIn Learning hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd LinkedIn Learning z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **LinkedIn Learning**. Na panelu výsledků klikněte na **LinkedIn Learning** tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s LinkedIn Learning podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v LinkedIn Learning je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v LinkedIn Learning musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v LinkedIn Learning.

tooconfigure a testu Azure AD jednotné přihlašování s LinkedIn Learning, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele LinkedIn Learning](#creating-a-linkedin-learning-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci LinkedIn Learning.

**tooconfigure Azure AD jednotné přihlašování s LinkedIn Learning, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **LinkedIn Learning** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedin_01.png)

3. V okně prohlížeče jiný web, klienta LinkedIn Learning tooyour přihlášení jako správce.

4. V **centra účtů**, klikněte na tlačítko **globální nastavení** pod **nastavení**. Kromě toho **učení – výchozí** z rozevíracího seznamu hello.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

5. Klikněte na tlačítko **nebo klepněte sem tooload a zkopírujte jednotlivých polí z formuláře hello** a zkopírujte **Entity Id** a **adresu Url pro přístup k příjemce Assertion (ACS)**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

6. Na portálu Azure v části **LinkedIn Learning domény a adresy URL**, proveďte následující kroky, pokud chcete, aby tooconfigure jednotné přihlašování hello v **IdP iniciované** režimu

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    a. V hello **identifikátor** textovému poli, zadejte hello **Entity ID** zkopírovat z portálu LinkedIn 

    b. V hello **adresa URL odpovědi** textovému poli, zadejte hello **adresu Url pro přístup k příjemce Assertion (ACS)** zkopírovat z portálu LinkedIn

7. Pokud chcete, aby tooconfigure jednotné přihlašování v **iniciované SP**, klikněte na možnost zobrazit Advanced adresy URL v části Konfigurace hello a nakonfigurovat následující vzor hello hello přihlašovací adresa URL:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
8. Aplikace LinkedIn Learning očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování tooyour tokenu atributy konfigurace SAML. Hello následující snímek obrazovky ukazuje příklad pro tento. Výchozí hodnota Hello **uživatelský identifikátor** je **user.userprincipalname** ale LinkedIn Learning očekává tento toobe namapována na hello uživatele e-mailovou adresu. K tomu můžete použít **user.mail** atribut ze seznamu hello nebo použijte hodnotu hello odpovídajícího atributu na základě konfigurace vaší organizace. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/updateusermail.png)
    
9. V **uživatelské atributy** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** a nastavte atributy hello. Hello uživatel potřebuje tooadd čtyři deklarace identity s názvem **e-mailu**, **oddělení**, **firstname**, a **lastname** a hodnota hello je namapována na toobe **user.mail**, **user.department**, **user.givenname**, a **user.surname** v uvedeném pořadí

    | Název atributu | Hodnota atributu |
    | --- | --- |
    | E-mailu| User.Mail |    
    | Oddělení| User.Department |
    | FirstName| User.givenName |
    | Příjmení| User.Surname |
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/userattribute.png)
    
    a. Klikněte na tlačítko **přidat atribut** tooopen hello atribut dialogu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_04.png)

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_05.png)
    
    b. V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.
    
    c. Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.
    
    d. Klikněte na tlačítko **Ok**

10. Proveďte následující kroky na hello hello **název** atribut -

    a. Klikněte na hello atribut tooopen hello **Upravit atribut** okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinLearning-tutorial/url_update.png)

    b. Odstranit hodnotu adresy URL hello z hello **obor názvů**.
    
    c. Klikněte na tlačítko **Ok** toosave hello nastavení.

11. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png) 

12. Klikněte na **Uložit**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_400.png)

13. Přejděte příliš**nastavení správce LinkedIn** části. Nahrajte soubor hello XML, který jste si stáhli z portálu Azure hello kliknutím na možnost soubor nahrát XML hello.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

14. Klikněte na tlačítko **na** tooenable jednotné přihlašování. Jednotné přihlašování stav se změní z **Nepřipojeno** příliš**připojeno**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**. 

### <a name="creating-a-linkedin-learning-test-user"></a>Vytvoření zkušebního uživatele LinkedIn Learning

Propojené Learning aplikace podporuje. Jenom při zřizování uživatelů čas a po ověření uživatele se automaticky vytvoří v aplikace hello. Na stránce Nastavení správce hello na přepínači portálu flip hello LinkedIn Learning hello **automaticky přiřadit licence** tooactive tooenable pouze v době zřizování a to také přiřadit licence toohello uživatele.
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinLearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části můžete povolit Britta Simon toouse Azure jednotné přihlašování a udělení přístupu tooLinkedIn učení.

![Přiřadit uživatele][200] 

**tooassign tooLinkedIn Britta Simon učení, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **LinkedIn Learning**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici LinkedIn Learning hello v hello přístupového panelu, měli byste obdržet hello Azure přihlašovací stránku a na po úspěšném přihlášení, měli byste obdržet do své aplikace LinkedIn Learning.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_203.png