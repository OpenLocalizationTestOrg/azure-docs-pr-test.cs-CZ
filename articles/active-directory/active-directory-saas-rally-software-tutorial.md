---
title: 'Kurz: Azure Active Directory integrace s technologie Rally softwarem | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a technologie Rally softwaru."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba25fade-e152-42dd-8377-a30bbc48c3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: jeedes
ms.openlocfilehash: c75c8b98ce7fab19964c13de5ad7e19ef3ebd0e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Kurz: Azure Active Directory integrace s technologie Rally softwaru

V tomto kurzu zjistíte, jak toointegrate technologie Rally Software s Azure Active Directory (Azure AD).

Integrace technologie Rally softwaru s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooRally softwaru.
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooRally softwaru (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s technologie Rally softwaru, je třeba hello následující položky:

- Předplatné služby Azure AD
- Technologie Rally softwaru jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání technologie Rally softwaru z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-rally-software-from-hello-gallery"></a>Přidání technologie Rally softwaru z Galerie hello
tooconfigure hello integrace technologie Rally softwaru do služby Azure AD, je nutné tooadd technologie Rally softwaru hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd technologie Rally softwaru z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **technologie Rally softwaru**, vyberte **technologie Rally softwaru** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![V seznamu výsledků hello technologie Rally softwaru](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí technologie Rally Software založený na testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v technologie Rally softwaru je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v softwaru technologie Rally musí toobe navázat.

V softwaru technologie Rally přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s technologie Rally softwaru, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele technologie Rally softwaru](#create-a-rally-software-test-user)**  -toohave protějšek Britta Simon v technologie Rally Software, který je propojený toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci technologie Rally softwaru.

**tooconfigure Azure AD jednotné přihlašování s technologie Rally softwarem, provést hello následující kroky:**

1. V portálu Azure, na hello hello **technologie Rally softwaru** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_samlbase.png)

3. Na hello **technologie Rally softwaru domény a adresy URL** část, proveďte následující kroky hello:

    ![Technologie Rally softwaru adresy URL jeden přihlašování informace o doméně a](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.rally.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.rally.com`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory technologie Rally klientský Software](https://help.rallydev.com/) tooget tyto hodnoty. 
 


4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-rally-software-tutorial/tutorial_general_400.png)

6. Na hello **technologie Rally softwarové konfigurace** klikněte na tlačítko **konfigurace technologie Rally softwaru** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresy URL a SAML Entity ID** z hello **Stručná referenční příručka části.**

    ![Technologie Rally konfigurace softwaru](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_configure.png) 

7. Přihlaste se tooyour **technologie Rally softwaru** klienta.

8. V panelu nástrojů hello hello nahoře, klikněte na **instalace**a potom vyberte **předplatné**.
   
    ![Předplatné](./media/active-directory-saas-rally-software-tutorial/ic769531.png "předplatného")

9. Klikněte na tlačítko hello **akce** tlačítko. Vyberte **upravit předplatné** v hello horní pravé části hello nástrojů.

10. Na hello **předplatné** dialogové okno stránky, proveďte následující kroky hello a pak klikněte na **uložit a zavřít**:
   
    ![Ověřování](./media/active-directory-saas-rally-software-tutorial/ic769542.png "ověřování")
   
    a. Vyberte **technologie Rally nebo jednotného přihlašování k ověřování** z rozevíracího seznamu ověřování.

    b. V hello **adresa URL poskytovatele Identity** textovému poli, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z portálu Azure. 

    c. V hello **jednotného přihlašování k odhlášení** textovému poli, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-rally-software-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-rally-software-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-rally-software-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-rally-software-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-rally-software-test-user"></a>Vytvoření zkušebního uživatele technologie Rally softwaru

Pro Azure AD Uživatelé toobe možné toosign v musí být zřízená toohello technologie Rally softwarová aplikace pomocí jejich názvy uživatele Azure Active Directory.

**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**

1. Přihlaste se tooyour technologie Rally softwaru klienta.

2. Přejděte příliš**instalace \> uživatelé**a potom klikněte na **+ přidat nový**.
   
    ![Uživatelé](./media/active-directory-saas-rally-software-tutorial/ic781039.png "uživatelů")

3. Zadejte název hello v textovém poli hello nového uživatele a pak klikněte na tlačítko **přidat s podrobnostmi**.

4. V hello **vytvořit uživatele** část, proveďte následující kroky hello:
   
    ![Vytvoření uživatele](./media/active-directory-saas-rally-software-tutorial/ic781040.png "vytvoření uživatele")

    a. V hello **uživatelské jméno** textovému poli, název typu hello uživatele jako **Brittsimon**.
   
    b. V **e-mailovou adresu** textovému poli, zadejte e-mailu hello uživatele jako  **brittasimon@contoso.com** .

    c. V **křestní jméno** textové pole, zadejte hello křestní jméno uživatele jako **Britta**.

    d. V **příjmení** text zadejte příjmení uživatele jako hello **Simon**.

    e. Klikněte na tlačítko **uložit a zavřít**.

   >[!NOTE]
   >Můžete použít žádné jiné technologie Rally softwaru uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované technologie Rally softwaru tooprovision uživatelské účty Azure AD.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooRally softwaru Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign Britta Simon tooRally softwaru, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **technologie Rally softwaru**.

    ![odkaz na Software technologie Rally Hello v seznamu aplikace hello](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.

Když kliknete na dlaždici technologie Rally softwaru hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour technologie Rally softwarová aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_203.png

