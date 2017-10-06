---
title: "Kurz: Integrace Azure Active Directory pomocí jednotného přihlašování SAML pro Jira podle řešení GmbH | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a jednotné přihlašování SAML pro Jira podle řešení GmbH."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 20e18819-e330-4e40-bd8d-2ff3b98e035f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: a3436a9aa25640e931a61b5ba4a62611e6e07890
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-jira-by-resolution-gmbh"></a>Kurz: Integrace Azure Active Directory pomocí jednotného přihlašování SAML pro Jira podle řešení GmbH

V tomto kurzu zjistíte, jak toointegrate jednotné přihlašování SAML pro Jira podle řešení GmbH službou Azure Active Directory (Azure AD).

Integrace jednotné přihlašování SAML pro Jira podle řešení GmbH s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, kdo má přístup tooSAML jednotné přihlašování pro Jira tak řešení GmbH
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSAML jednotné přihlašování pro Jira řešení GmbH (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD pomocí jednotného přihlašování SAML pro Jira podle řešení GmbH tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Jednotné přihlašování SAML pro Jira podle řešení GmbH jednotné přihlašování v předplatném povolené

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání jednotné přihlašování SAML pro Jira podle řešení GmbH z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-saml-sso-for-jira-by-resolution-gmbh-from-hello-gallery"></a>Přidání jednotné přihlašování SAML pro Jira podle řešení GmbH z Galerie hello
tooconfigure hello integrace přihlašování SAML pro Jira podle řešení GmbH do Azure AD, musíte pro Jira podle řešení GmbH hello Galerie tooyour seznamu spravovaných aplikací SaaS tooadd jednotné přihlašování SAML.

**tooadd jednotné přihlašování SAML pro Jira podle řešení GmbH z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **jednotné přihlašování SAML pro Jira podle řešení GmbH**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_search.png)

5. Na panelu výsledků hello vyberte **jednotné přihlašování SAML pro Jira podle řešení GmbH**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML pro Jira pomocí řešení, které GmbH podle testovacího uživatele názvem "Britta Simon."

Azure AD pro toowork jeden přihlašování, musí tooknow uživatele co hello protějškem v jednotné přihlašování SAML pro Jira podle řešení GmbH je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a hello související uživatele v jednotné přihlašování SAML pro Jira podle řešení GmbH musí toobe navázat.

V jednotné přihlašování SAML pro Jira podle řešení GmbH, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testování Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML Jira podle řešení GmbH, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření jednotné přihlašování SAML pro Jira uživatelem testovací GmbH řešení](#creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user)**  -toohave protějšek Britta Simon v jednotné přihlašování SAML pro Jira podle řešení GmbH, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší jednotné přihlašování SAML pro Jira podle řešení GmbH aplikace.

**tooconfigure Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML pro Jira podle řešení GmbH, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **jednotné přihlašování SAML pro Jira podle řešení GmbH** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_samlbase.png)

3. Na hello **jednotné přihlašování SAML Jira podle řešení GmbH domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_url_1.png)

    a. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server-base-url>/plugins/servlet/samlsso`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server-base-url>/plugins/servlet/samlsso`

4. Zkontrolujte **zobrazit upřesňující nastavení adresy URL**. Pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_url_2.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL. Obraťte se na [tým podpory jednotné přihlašování SAML pro Jira podle řešení GmbH klienta](https://www.resolution.de/go/support) tooget tyto hodnoty. 

5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/tutorial_general_400.png)
    
7. V okně prohlížeče jiných webových přihlásit tooyour **jednotné přihlašování SAML pro Jira pomocí portálu pro správu GmbH řešení** jako správce.

8. Pozastavte ukazatel myši na ikonu a klikněte na tlačítko hello **doplňky**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/addon1.png)

9. Jste přesměrovaného tooAdministrator stránky. Zadejte hello **heslo** a klikněte na tlačítko **potvrdit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/addon2.png)

10. Karta části doplňky, klikněte na tlačítko **najít nové rozšíření**. Hledání **SAML jednotné přihlašování na (SSO) pro JIRA** a klikněte na tlačítko **nainstalovat** tooinstall tlačítko hello nové zásuvný modul SAML.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/addon7.png)

11. Instalace modulu plug-in Hello se spustí. Klikněte na **Zavřít**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/addon8.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/addon9.png)

12. Klikněte na **Manage** (Spravovat).

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/addon10.png)
    
13. Klikněte na tlačítko **konfigurace** tooconfigure hello nového modulu plug-in.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/addon11.png)

14. Na **konfigurace modulu plug-in SingleSignOn SAML** klikněte na tlačítko **přidat další zprostředkovatele Identity** tlačítko tooconfigure hello nastavení zprostředkovatele Identity.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/addon4.png)

15. Proveďte následující kroky na této stránce:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/addon5.png)
 
    a. Přidat **název** z hello zprostředkovatele Identity (např. Azure AD).
    
    b. Přidat **popis** z hello zprostředkovatele Identity (např. Azure AD).

    c. Klikněte na tlačítko **XML** a vyberte hello **Metadata** souboru, který jste si stáhli z portálu Azure.

    d. Klikněte na tlačítko **zatížení** tlačítko.

    e. Přečte hello IdP metadata a naplní pole hello jako zvýrazněná hello snímku obrazovky.   

16. Klikněte na tlačítko **uložit nastavení** tlačítko toosave hello nastavení.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/addon6.png)

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user"></a>Vytváření jednotné přihlašování SAML pro Jira řešení GmbH testovací uživatel

Uživatelé toolog tooenable Azure AD v tooSAML jednotné přihlašování pro Jira podle řešení GmbH, se musí být zřízená do jednotné přihlašování SAML pro Jira podle řešení GmbH.  
V jednotné přihlašování SAML pro Jira podle řešení GmbH zřizování je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour jednotné přihlašování SAML pro Jira řešení GmbH společnosti lokalita jako správce.

2. Pozastavte ukazatel myši na ikonu a klikněte na tlačítko hello **Správa uživatelů**.

    ![Můžete přidat zaměstnance](./media/active-directory-saas-samlssojira-tutorial/user1.png) 

3. Jsou přesměrovaného tooAdministrator přístup stránky tooenter **heslo** a klikněte na tlačítko **potvrdit** tlačítko.

    ![Můžete přidat zaměstnance](./media/active-directory-saas-samlssojira-tutorial/user2.png) 

4. V části **Správa uživatelů** oddíl, klikněte na **vytvořit uživatele**.

    ![Můžete přidat zaměstnance](./media/active-directory-saas-samlssojira-tutorial/user3.png) 

5. Na hello **"Vytvořit nový uživatel"** dialogové okno proveďte hello následující kroky:

    ![Můžete přidat zaměstnance](./media/active-directory-saas-samlssojira-tutorial/user4.png) 

    a. V hello **e-mailová adresa** jako typ hello e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.

    b. V hello **úplný název** textovému poli, úplný název typu hello uživatele jako Britta Simon.

    c. V hello **uživatelské jméno** jako typ hello e-mailu uživatele k textovému poli, Brittasimon@contoso.com.

    d. V hello **heslo** textovému poli, zadejte hello heslo uživatele.

    e. Klikněte na tlačítko **vytvořit uživateli**.   

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooSAML jednotné přihlašování pro Jira podle řešení GmbH Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooSAML jednotné přihlašování pro Jira podle řešení GmbH, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **jednotné přihlašování SAML pro Jira podle řešení GmbH**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello jednotné přihlašování SAML pro Jira pomocí řešení GmbH dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour jednotné přihlašování SAML pro Jira podle řešení GmbH aplikace.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_203.png

