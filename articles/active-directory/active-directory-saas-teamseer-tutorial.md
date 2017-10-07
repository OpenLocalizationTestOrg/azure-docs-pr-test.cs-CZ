---
title: 'Kurz: Azure Active Directory integrace s TeamSeer | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a TeamSeer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 876d13e446115acd50b01c7f44db99357045e429
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Kurz: Azure Active Directory integrace s TeamSeer

V tomto kurzu zjistíte, jak toointegrate TeamSeer s Azure Active Directory (Azure AD).

Integrace TeamSeer s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooTeamSeer
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTeamSeer (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s TeamSeer tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- TeamSeer jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání TeamSeer z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-teamseer-from-hello-gallery"></a>Přidání TeamSeer z Galerie hello
tooconfigure hello integrace TeamSeer v tooAzure AD, je nutné tooadd TeamSeer hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd TeamSeer z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **TeamSeer**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_search.png)

5. Na panelu výsledků hello vyberte **TeamSeer**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s TeamSeer podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v TeamSeer je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v TeamSeer musí toobe navázat.

V TeamSeer, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s TeamSeer, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele TeamSeer](#creating-a-teamseer-test-user)**  -toohave protějšek Britta Simon v TeamSeer, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci TeamSeer.

**tooconfigure Azure AD jednotné přihlašování s TeamSeer, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **TeamSeer** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_samlbase.png)

3. Na hello **TeamSeer domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_url.png)

     V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.teamseer.com/<companyid>`

    > [!NOTE] 
    > Hodnota Hello není skutečné. Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory TeamSeer klienta](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) tooget hello hodnotu. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_general_400.png)

6. Na hello **TeamSeer konfigurace** klikněte na tlačítko **konfigurace TeamSeer** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_configure.png)

7. V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti TeamSeer tooyour.

8. Přejděte příliš**HR správce**.
   
    ![Správce HR](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR správce")

9. Klikněte na tlačítko **instalační program**.
   
    ![Instalační program](./media/active-directory-saas-teamseer-tutorial/ic789635.png "instalační program")

10. Klikněte na tlačítko **nastavit podrobné informace o poskytovateli SAML**.
   
    ![Nastavení SAML](./media/active-directory-saas-teamseer-tutorial/ic789636.png "nastavení SAML")

11. V hello v části Podrobnosti o zprostředkovatele SAML proveďte hello následující kroky:
   
    ![Nastavení SAML](./media/active-directory-saas-teamseer-tutorial/ic789637.png "nastavení SAML")   

    a. Vložení hello **jeden přihlašování adresa URL služby** hodnota v toohello **URL** textové pole.
          
    b. Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky tooyour a pak ji vložit toohello **veřejný certifikát IdP** textové pole.

12. toocomplete hello zprostředkovatele konfigurace SAML, proveďte následující kroky hello:
    
    ![Nastavení SAML](./media/active-directory-saas-teamseer-tutorial/ic789638.png "nastavení SAML") 

    a. V hello **testovací e-mailové adresy**, zadejte hello testovacího uživatele e-mailovou adresu. 
  
    b. V hello **vystavitele** textovému poli, typ hello URL vystavitele hello poskytovatele služeb. 
  
    c. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-teamseer-test-user"></a>Vytvoření zkušebního uživatele TeamSeer

Uživatelé toolog tooenable Azure AD v tooTeamSeer, se musí být zřízená v tooShiftPlanning. V případě hello TeamSeer zřizování je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour **TeamSeer** společnosti lokality jako správce.

2. Proveďte hello následující kroky:
   
    ![Správce HR](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR správce")  
 
    a. Přejděte příliš**HR správce \> uživatelé**.
  
    b. Klikněte na tlačítko **spusťte Průvodce nového uživatele hello**.

3. V hello **podrobné informace o uživateli** část, proveďte následující kroky hello:
   
    ![Podrobné informace o uživateli](./media/active-directory-saas-teamseer-tutorial/ic789641.png "podrobné informace o uživateli")

    a. Typ hello **křestní jméno**, **Přezdívka**, **uživatelské jméno (e-mailovou adresu)** z platný účet AAD chcete tooprovision v toohello související textových polí.
  
    b. Klikněte na **Další**.

4. Postupujte podle hello na obrazovce pokyny pro přidání nového uživatele a klikněte na tlačítko **Dokončit**.

>[!NOTE]
>Můžete použít všechny ostatní TeamSeer uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované TeamSeer tooprovision uživatelské účty Azure AD. 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooTeamSeer toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooTeamSeer, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **TeamSeer**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu. Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_203.png

