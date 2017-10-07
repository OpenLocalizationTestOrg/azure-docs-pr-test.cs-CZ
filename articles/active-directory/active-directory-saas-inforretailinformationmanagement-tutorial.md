---
title: "Kurz: Azure Active Directory integrace s prodejní informací – správu informace | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a prodejní informací – informace správy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5ff49168-ef81-4169-8e5e-dc86e24dd5e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 9cd8ab65d41d01832e0611f7f8254aa257120508
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-infor-retail--information-management"></a>Kurz: Azure Active Directory integrace s prodejní informací – informace správy

V tomto kurzu zjistíte, jak toointegrate prodejní informací – informace správy pomocí služby Azure Active Directory (Azure AD).

Integrace informací prodejní – informace správy s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooInfor prodejní – informace správy.
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooInfor prodejní – správy informací (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s prodejní informací – informace správy, je třeba hello následující položky:

- Předplatné služby Azure AD
- Prodejní informací – informace správy jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání informací prodejní – správu informace z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-infor-retail--information-management-from-hello-gallery"></a>Přidání informací prodejní – správu informace z Galerie hello
integrace hello tooconfigure informací prodejní – informace správy do služby Azure AD, je nutné tooadd informací prodejní – informace o správu hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd informací prodejní – správu informace z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **informací prodejní – informace o správu**, vyberte **informací prodejní – informace o správu** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Prodejní informací – informace správy v seznamu výsledků hello](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části konfiguraci a testování Azure AD jednotné přihlašování s prodejní informací – informace správy na základě testovací uživatele, nazývá "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow uživatele hello protějškem v prodejní informací – informace správy je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v prodejní informací – informace správy musí toobe navázat.

V prodejní informací – informace o správě přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testování Azure AD jednotné přihlašování s prodejní informací – informace správy, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření informací prodejní – informace o správu testovacího uživatele](#create-an-infor-retail--information-management-test-user)**  - toohave protějšek Britta Simon v prodejní informací – Správa informace, který je reprezentace toohello propojené služby Azure AD uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v prodejní informací – informace správy aplikací.

**tooconfigure Azure AD jednotné přihlašování s prodejní informací – informace o správě proveďte hello následující kroky:**

1. V portálu Azure, na hello hello **informací prodejní – informace o správu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_samlbase.png)

3. Na hello **informací prodejní – informace o doméně pro správu a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure v rozšíření IDP iniciované režimu hello:

    ![Jednotné přihlašování informace IDP informací prodejní – informace o doméně pro správu a adresy URL](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url.png)

    a. V hello **identifikátor** textové pole, zadejte adresu URL pomocí hello následující vzory: 
    |   |
    | -- |
    | `https://<company name>.mingle.infor.com` |
    | `http://<company name>.mingledev.infor.com` |

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.mingle.infor.com/sp/ACS.saml2`

4. Zkontrolujte **zobrazit upřesňující nastavení adresy URL** a proveďte následující krok, pokud chcete aplikace hello tooconfigure hello **SP** iniciované režimu:

    ![Prodejní informací – informace o doméně pro správu a adresy URL jeden přihlašování informace SP](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url1.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.mingle.infor.com/<company code>`
     
    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL. Obraťte se na [informací prodejní – tým podpory klienta pro správu informace](mailto:innovate@infor.com) tooget tyto hodnoty. 

5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_400.png)
    
7. tooconfigure jednotného přihlašování na **informací prodejní – informace o správu** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[informací prodejní – tým podpory informace správy ](mailto:innovate@infor.com). Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-an-infor-retail--information-management-test-user"></a>Vytvoření informací prodejní – informace o správu testovacího uživatele

V této části vytvoříte uživatele volal Britta Simon v prodejní informací – informace správy. Spojte se s [informací prodejní – tým podpory správy informace](mailto:innovate@infor.com) tooadd hello uživatele v hello informací prodejní – platforma pro správu informace.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooInfor prodejní – informace o správu Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign Britta Simon tooInfor prodejní – informace o správě proveďte hello následující kroky:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **informací prodejní – informace o správu**.

    ![Hello informací prodejní – informace správy na odkaz v seznamu aplikace hello](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello informací prodejní – informace správy dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour prodejní informací – informace správy aplikací.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_203.png

