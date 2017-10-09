---
title: "Kurz: Azure Active Directory integrace s dojem Spojených států (Non-UltiPro) | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a dojem USA (Non-UltiPro)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b4a8f026-cb5f-41eb-9680-68eddc33565e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 874b5da277b9c68504c4af2ac87ed90d2bbd93b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a>Kurz: Azure Active Directory integrace s dojem Spojených států (Non-UltiPro)

V tomto kurzu zjistíte, jak toointegrate dojem Spojených států (Non-UltiPro) s Azure Active Directory (Azure AD).

Integrace dojem USA (Non-UltiPro) s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooPerception USA (Non-UltiPro).
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPerception USA (Non-UltiPro) (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s dojem Spojených států (Non-UltiPro), je třeba hello následující položky:

- Předplatné služby Azure AD
- Dojem USA (Non-UltiPro) jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání dojem USA (Non-UltiPro) z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-perception-united-states-non-ultipro-from-hello-gallery"></a>Přidání dojem USA (Non-UltiPro) z Galerie hello
tooconfigure hello integrace z dojem Spojených států (Non-UltiPro) do Azure AD, musíte tooadd dojem USA (Non-UltiPro) hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd dojem Spojených států (Non-UltiPro) z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **dojem USA (Non-UltiPro)**, vyberte **dojem USA (Non-UltiPro)** z panelu výsledků klikněte **přidat** tooadd tlačítko aplikace Hello.

    ![Dojem Spojených států (Non-UltiPro) v seznamu výsledků hello](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části konfiguraci a testování Azure AD jednotné přihlašování s dojem USA (Non-UltiPro) podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v dojem Spojených států (Non-UltiPro) je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v dojem Spojených států (Non-UltiPro) musí toobe navázat.

V dojem USA (Non-UltiPro), přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testování Azure AD jednotné přihlašování s dojem Spojených států (Non-UltiPro), je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele dojem USA (Non-UltiPro)](#create-a-perception-united-states-non-ultipro-test-user)**  -toohave protějšek Britta Simon v dojem Spojené státy americké (Non-UltiPro), je toohello propojené služby Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci dojem USA (Non-UltiPro).

**tooconfigure Azure AD jednotné přihlašování s dojem Spojených států (Non-UltiPro), proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **dojem USA (Non-UltiPro)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_samlbase.png)

3. Na hello **doméně dojem USA (Non-UltiPro) a adresy URL** část, proveďte následující kroky hello:

    ![Dojem USA (Non-UltiPro) domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_url.png)

    a. V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://perception.kanjoya.com/sp`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://perception.kanjoya.com/sso?idp=<entity_id>`

    > [!NOTE] 
    > Hodnota Hello není skutečné. Aktualizujte hodnotu hello s hello skutečná adresa URL odpovědi, který je vysvětlen později v kurzu hello.
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_400.png)

6. Na hello **konfigurace dojem USA (Non-UltiPro)** klikněte na tlačítko **konfigurace dojem Spojených států (Non-UltiPro)** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID** z hello **Stručná referenční příručka části.**

    a. Hello **dojem USA (Non-UltiPro)** aplikace vyžaduje hello **SAML Entity ID** kódovaný identifikátor uri toobe hodnotu, kterou jste zkopírovali,. Hodnota uri kódovaný hello tooget, použijte hello následující odkaz:**http://www.url-encode-decode.com/**.

    b. Po získání hello uri šifrovanou hodnotu ho spojovat se hello **adresa URL odpovědi** jak je uvedeno níže -

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    c. Vložení hello větší než hodnota v hello **adresa URL odpovědi** textového pole v **doméně dojem USA (Non-UltiPro) a adresy URL** části.

    ![Konfigurace dojem USA (bez UltiPro)](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_configure.png) 

7. V jiném okně prohlížeče Přihlaste se jako správce na webu společnosti tooyour dojem USA (Non-UltiPro).

8. Na hlavním panelu nástrojů hello klikněte na **nastavení účtu**.

    ![Dojem uživatele USA (Non-UltiPro)](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

9. Na hello **nastavení účtu** proveďte hello následující kroky:

    ![Dojem uživatele USA (Non-UltiPro)](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    a. V hello **název společnosti** textovému poli, název typu hello hello **společnosti**.
    
    b. V hello **název účtu** textovému poli, název typu hello hello **účet**.

    c. V **výchozí odpovědi tooEmail** textového pole, platný typ hello **e-mailu**.

    d. Vyberte **zprostředkovatele Identity jednotného přihlašování k** jako **SAML 2.0**.

10. Na hello **Konfigurace jednotného přihlašování k** proveďte hello následující kroky:

    ![SSOConfig dojem USA (bez UltiPro)](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    a. Vyberte **SAML NameID typ** jako **e-MAILU**.

    b. V hello **název konfigurace jednotného přihlašování k** textovému poli, název typu hello vaše **konfigurace**.
    
    c. V **název zprostředkovatele Identity** textovému poli, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z portálu Azure. 

    d. V **SAML domény textbox**, zadejte hello domény jako  **@contoso.com** .

    e. Klikněte na **nahrát znovu** tooupload hello **soubor XML s metadaty** souboru.

    f. Klikněte na tlačítko **aktualizace**.


> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
  
### <a name="create-a-perception-united-states-non-ultipro-test-user"></a>Vytvoření zkušebního uživatele dojem USA (Non-UltiPro)

V této části vytvoříte uživatele volat Britta Simon v dojem Spojených států (Non-UltiPro). Práce s [tým podpory dojem USA (Non-UltiPro)](http://www.ultimatesoftware.com/Contact/ContactUs) tooadd hello uživatelé v platformě hello dojem USA (Non-UltiPro).

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooPerception USA (Non-UltiPro) toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign Britta Simon tooPerception USA (Non-UltiPro), proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **dojem USA (Non-UltiPro)**.

    ![odkaz Hello dojem USA (Non-UltiPro) v seznamu aplikace hello](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici hello dojem USA (Non-UltiPro) v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour dojem USA (Non-UltiPro) aplikace.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_203.png

