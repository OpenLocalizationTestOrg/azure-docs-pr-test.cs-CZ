---
title: 'Kurz: Azure Active Directory integrace s LinkedInSalesNavigator | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a LinkedInSalesNavigator."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 443d302d40d7af16aba5114e00963f23ea8d12d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a>Kurz: Azure Active Directory integrace s LinkedIn prodej Navigátor

V tomto kurzu zjistíte, jak toointegrate LinkedIn prodej Navigátor službou Azure Active Directory (Azure AD).

Integrace LinkedIn prodej Navigátor s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooLinkedIn Navigátor prodeje
- Vaši uživatelé tooautomatically get přihlášeného tooLinkedIn prodej Navigátor (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete další informace o integraci aplikací SaaS v Azure AD tooknow, Procházet [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Navigátor LinkedIn prodej, je třeba hello následující položky:

- Předplatné služby Azure AD
- Organizační jednotky prodej Navigátor LinkedIn jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Pokud to není nezbytné, vyhněte se použití produkční prostředí.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání LinkedIn prodej Navigátor z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-linkedin-sales-navigator-from-hello-gallery"></a>Přidání LinkedIn prodej Navigátor z Galerie hello
tooconfigure hello integrace LinkedIn prodej Navigátor do Azure AD, je nutné tooadd LinkedIn prodej Navigátor hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd LinkedIn prodej Navigátor z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **LinkedIn prodej Navigátor**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

5. Na panelu výsledků hello vyberte **LinkedIn prodej Navigátor**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Navigátor LinkedIn prodeje podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v LinkedIn prodej navigátor je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v LinkedIn prodej Navigátor musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Navigátor prodej LinkedIn.

tooconfigure a testu Azure AD jednotné přihlašování s Navigátor prodej LinkedIn, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele LinkedIn prodej Navigátor](#creating-a-linkedin-sales-navigator-test-user)**  -toohave protějšek Britta Simon v Navigátor LinkedIn prodej, která je propojená toohello Azure AD reprezentace hello uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Navigátor prodej LinkedIn.

**tooconfigure Azure AD jednotné přihlašování s LinkedIn Navigátor prodej, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **LinkedIn prodej Navigátor** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogové okno, v **režimu** vyberte **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

3. V okně prohlížeče jiných webových přihlašování tooyour **LinkedIn prodej Navigátor** webu jako správce.

4. V **centra účtů**, klikněte na tlačítko **globální nastavení** pod **nastavení**. Kromě toho **prodej Navigátor** z rozevíracího seznamu hello.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

5. Klikněte na tlačítko **nebo klepněte sem tooload a zkopírujte jednotlivých polí z formuláře hello** a zkopírujte **Entity Id** a **adresu Url pro přístup k příjemce Assertion (ACS)**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

6. Na portálu Azure v části **LinkedIn prodej Navigátor domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **IDP** iniciované režimu.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    a. V hello **identifikátor** textovému poli, zadejte hello **Entity ID** zkopírovat z portálu LinkedIn 

    b. V hello **adresa URL odpovědi** textovému poli, zadejte hello **adresu Url pro přístup k příjemce Assertion (ACS)** zkopírovat z portálu LinkedIn

7. Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    V hello **přihlašovací adresa URL** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`

8. Vaše **LinkedIn prodej Navigátor** aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, abyste tooadd vlastních atributů mapování tooyour SAML tokenu atributy konfigurace. Hello následující snímek obrazovky ukazuje příklad. Hello výchozí hodnotu **uživatelský identifikátor** je **user.userprincipalname** ale LinkedIn prodej Navigátor očekává, že toobe namapována na hello uživatele e-mailovou adresu. Můžete použít **user.mail** atribut ze seznamu hello nebo použijte hodnotu hello odpovídajícího atributu na základě konfigurace vaší organizace. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/updateusermail.png)
    
9. V **uživatelské atributy** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** a nastavte atributy hello. Hello uživatel potřebuje tooadd čtyři deklarace identity s názvem **e-mailu**, **oddělení**, **firstname**, a **lastname** a hodnota hello je namapována na toobe **user.mail**, **user.department**, **user.givenname**, a **user.surname** v uvedeném pořadí

    | Název atributu | Hodnota atributu |
    | --- | --- |    
    | E-mailu| User.Mail |
    | Oddělení| User.Department |
    | FirstName| User.givenName |
    | Příjmení| User.Surname |
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/userattribute.png)
    
    a. Klikněte na **přidat atribut** tooopen hello atribut dialogu.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    b. V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.
    
    c. Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.
    
    d. Klikněte na tlačítko **Ok**

10. Proveďte následující kroky na hello hello **název** atribut -

    a. Klikněte na hello atribut tooopen hello **Upravit atribut** okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/url_update.png)

    b. Odstranit hodnotu adresy URL hello z hello **obor názvů**.
    
    c. Klikněte na tlačítko **Ok** toosave hello nastavení.

11. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

12. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_400.png)

13. Přejděte příliš**nastavení správce LinkedIn** části. Klikněte na tlačítko **soubor nahrát XML** tooupload hello souboru soubor XML s metadaty, kterou jste si stáhli z portálu Azure hello.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

14. Klikněte na tlačítko **na** tooenable jednotné přihlašování. Jednotné přihlašování stav se změní z **Nepřipojeno** příliš**připojeno**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a>Vytvoření zkušebního uživatele LinkedIn prodej Navigátor

Propojené aplikace Navigátor prodej podporuje pouze v zřizování uživatelů čas (JIT) a po ověření uživatele v aplikaci hello automaticky vytvoří. Aktivovat **automaticky přiřadit licence** tooassign uživatel toohello licence.
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooLinkedIn prodej Navigátor Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign tooLinkedIn Britta Simon Navigátor prodej, proveďte hello následující kroky:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **LinkedIn prodej Navigátor**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici LinkedIn prodej Navigátor hello v hello přístupový Panel, musí být přesměrované tooOrganizational stránku, kde jsou tooprovide osobní podrobnosti o účtu LinkedIn. Ho propojí s vaším účtem obchodní LinkedIn svůj osobní účet. Další informace o hello přístupového panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_203.png

