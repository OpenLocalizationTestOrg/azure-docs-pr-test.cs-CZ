---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti Autotask | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Autotask síti na pracovišti."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9a7ff71-c389-4169-aafd-d7a505244797
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 7f820f24e8e9493fa2e1c075f2ef61d7eaa84f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-workplace"></a>Kurz: Azure Active Directory integrace s Autotask síti na pracovišti

V tomto kurzu zjistíte, jak toointegrate Autotask síti na pracovišti s Azure Active Directory (Azure AD).

Integrace s Azure AD Autotask síti na pracovišti vám poskytne hello následující výhody:

- Můžete řídit ve službě Azure AD, který má tooAutotask přístup k síti na pracovišti
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAutotask síti na pracovišti (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Autotask síti na pracovišti, je třeba hello následující položky:

- Předplatné služby Azure AD
- Síti na pracovišti Autotask jednotného přihlašování povolené předplatné
- Musíte být správce nebo správce super v síti na pracovišti.
- Musí mít účet správce v hello Azure AD.
- Hello uživatelů, kteří se využívají tato funkce musí mít účty v síti na pracovišti a hello Azure AD a jejich e-mailové adresy pro obě se musí shodovat.

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Autotask síti na pracovišti z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-autotask-workplace-from-hello-gallery"></a>Přidání Autotask síti na pracovišti z Galerie hello
tooconfigure hello integrace síti na pracovišti Autotask do Azure AD, je nutné tooadd síti na pracovišti Autotask hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Autotask síti na pracovišti z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **síti na pracovišti Autotask**, vyberte **síti na pracovišti Autotask** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Autotask síti na pracovišti v hello seznamu výsledků](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurujete a testovací Azure AD jednotné přihlašování se Autotask pracovišti podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v síti na pracovišti Autotask je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v síti na pracovišti Autotask musí toobe navázat.

V síti na pracovišti Autotask, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Autotask síti na pracovišti, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele síti na pracovišti Autotask](#create-an-autotask-workplace-test-user)**  -toohave protějšek Britta Simon v síti na pracovišti Autotask, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Autotask síti na pracovišti.

**tooconfigure Azure AD jednotné přihlašování s Autotask síti na pracovišti, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **síti na pracovišti Autotask** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_samlbase.png)

3. Pokud chcete aplikace hello tooconfigure v **IDP** iniciované režimu, proveďte následující kroky na hello hello **Autotask síti na pracovišti domény a adresy URL** části:

    ![Autotask síti na pracovišti domény a adresy URL jeden přihlašování informace o rozšíření IDP](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url.png)

    a. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`

4. Pokud chcete aplikace hello tooconfigure v **SP** initiated režimu, zkontrolujte **zobrazit upřesňující nastavení adresy URL** a provádět hello následující kroky:

    ![Autotask síti na pracovišti domény a adresy URL jeden přihlašování informace pro poskytovatele služeb](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url1.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.awp.autotask.net/loginsso`
     
    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL. Obraťte se na [tým podpory Autotask pracoviště klienta](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget tyto hodnoty. 

5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_400.png)

7. V okně prohlížeče jiných webových hello přihlásit se tooWorkplace Online pomocí přihlašovacích údajů správce.

    >[!Note]
    >Při konfiguraci hello deklarací identity, bude nutné subdoména toobe zadán. tooconfirm hello správné subdomény, přihlášení tooWorkplace Online. Po přihlášení, zkontrolujte v adrese URL hello Poznámka toohello subdomény.
    >je součástí hello mezi hello "https://" a ".awp.autotask.net/" a musí být v USA, Evropa, certifikační autority nebo au Hello subdomény.

8. Přejděte příliš**konfigurace** > **jednotné přihlašování** a provádět hello následující kroky:

    ![Autotask Single Sign-on konfigurace](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig1.png)
 
    a. Vyberte hello **Metadata souboru XML** možnost a pak nahrajte hello **soubor XML s metadaty** stáhli z portálu Azure.

    b. Klikněte na tlačítko **povolení jednotného přihlašování**.
    
    ![Autotask jednotné přihlášení schválit konfigurace](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig2.png)

    c. Vyberte hello **potvrzuji, tyto informace jsou správné a důvěřovat této IdP** zaškrtávací políčko.

    d. Klikněte na tlačítko **schválit**.
     
>[!Note]
>Pokud potřebujete pomoc s konfigurací Autotask síti na pracovišti, najdete v tématu [tuto stránku](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget pomoc s pracovního účtu.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.

### <a name="create-an-autotask-workplace-test-user"></a>Vytvoření zkušebního uživatele síti na pracovišti Autotask

V této části vytvoříte volal Britta Simon v Autotask uživatele. Spojte se s [tým podpory síti na pracovišti Autotask](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooadd hello uživatelů v síti na pracovišti Autotask platformy hello.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooAutotask přístup k síti na pracovišti.

![Přiřadit role uživatele hello][200] 

**tooassign Britta Simon tooAutotask síti na pracovišti, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Autotask síti na pracovišti**.

    ![Hello síti na pracovišti Autotask odkaz v seznamu aplikace hello](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello síti na pracovišti Autotask dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour aplikace Autotask síti na pracovišti.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_203.png

