---
title: 'Kurz: Azure Active Directory integrace s PagerDuty | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a PagerDuty."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: c3cfbedac3bf075e2d8cd833d5de7ca0bc9468b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Kurz: Azure Active Directory integrace s PagerDuty

V tomto kurzu zjistíte, jak toointegrate PagerDuty s Azure Active Directory (Azure AD).

Integrace PagerDuty s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooPagerDuty
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPagerDuty (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s PagerDuty tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- PagerDuty jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání PagerDuty z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-pagerduty-from-hello-gallery"></a>Přidání PagerDuty z Galerie hello
tooconfigure hello integrace PagerDuty do Azure AD, je nutné tooadd PagerDuty hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd PagerDuty z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **PagerDuty**, vyberte **PagerDuty** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s PagerDuty podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v PagerDuty je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v PagerDuty musí toobe navázat.

V PagerDuty, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s PagerDuty, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele PagerDuty](#create-a-pagerduty-test-user)**  -toohave protějšek Britta Simon v PagerDuty, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci PagerDuty.

**tooconfigure Azure AD jednotné přihlašování s PagerDuty, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **PagerDuty** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_samlbase.png)

3. Na hello **PagerDuty domény a adresy URL** část, proveďte následující kroky hello:

    ![PagerDuty domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.pagerduty.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.pagerduty.com`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory PagerDuty klienta](https://www.pagerduty.com/support/) tooget tyto hodnoty. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-pagerduty-tutorial/tutorial_general_400.png)

6. Na hello **PagerDuty konfigurace** klikněte na tlačítko **konfigurace PagerDuty** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**

    ![Konfigurace PagerDuty](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Pagerduty.

8. V nabídce hello hello nahoře, klikněte na tlačítko **nastavení účtu**.
   
    ![Nastavení účtu](./media/active-directory-saas-pagerduty-tutorial/ic778535.png "nastavení účtu")

9. Klikněte na tlačítko **jednotného přihlašování**.
   
    ![Jednotné přihlašování](./media/active-directory-saas-pagerduty-tutorial/ic778536.png "jednotného přihlašování")

10. Na hello **povolit jednotné přihlašování (SSO)** proveďte hello následující kroky:
   
    ![Povolit jednotné přihlašování](./media/active-directory-saas-pagerduty-tutorial/ic778537.png "povolit jednotné přihlašování")
   
    a. Otevření kódovaného certifikátu kódování base-64 stáhli z portálu Azure v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát X.509** textbox
  
    b. V hello **přihlašovací adresa URL** textovému poli, vložte **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.
  
    c. V hello **adresy URL odhlašovací** textovému poli, vložte **Sign-Out URL** který jste zkopírovali z portálu Azure.
 
    d. Vyberte **zapnout Single Sign-on**.
 
    e. Klikněte na tlačítko **uložit změny**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-pagerduty-test-user"></a>Vytvoření zkušebního uživatele PagerDuty

Uživatelé toolog tooenable Azure AD v tooPagerDuty, se musí být zřízená do PagerDuty.  
V případě hello PagerDuty zřizování je ruční úloha.

>[!NOTE]
>Můžete použít všechny ostatní Pagerduty uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Pagerduty tooprovision Azure Active Directory uživatelské účty.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour **Pagerduty** klienta.

2. V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.

3. Klikněte na tlačítko **přidat uživatele**.
   
    ![Přidání uživatelů](./media/active-directory-saas-pagerduty-tutorial/ic778539.png "přidat uživatele")

4.  Na hello **pozvat váš tým** dialogové okno, proveďte následující kroky hello:
   
    ![Pozvěte váš tým](./media/active-directory-saas-pagerduty-tutorial/ic778540.png "pozvat váš tým")

    a. Typ hello **první a poslední název** uživatele jako **Britta Simon**. 
   
    b. Zadejte **e-mailu** adresu uživatele, například  **brittasimon@contoso.com** .
   
    c. Klikněte na tlačítko **přidat**a potom klikněte na **odesílání žádostí**.
   
    >[!NOTE]
    >Všechny přidaní uživatelé dostanou pozvání toocreate PagerDuty účtu.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooPagerDuty toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200]

**tooassign Britta Simon tooPagerDuty, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **PagerDuty**.

    ![v seznamu aplikace hello Hello PagerDuty odkaz](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello PagerDuty dlaždice v hello přístup Panelyou měli získat automaticky přihlášeného tooyour PagerDuty aplikace.

Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_203.png

