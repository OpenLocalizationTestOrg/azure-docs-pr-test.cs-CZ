---
title: "Kurz: Azure Active Directory integrace s vyrovná se se zatížením pro správu vzdělávacího procesu | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a vyrovná se se zatížením pro správu vzdělávacího procesu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: a140a78a3a9474a6372a3ad4fb8251bd2452c990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Kurz: Azure Active Directory integrace s vyrovná se se zatížením pro správu vzdělávacího procesu

V tomto kurzu zjistíte, jak toointegrate Absorb pro správu vzdělávacího procesu s Azure Active Directory (Azure AD).

Integrace vyrovná se se zatížením pro správu vzdělávacího procesu s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má tooAbsorb přístup pro správu vzdělávacího procesu
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAbsorb pro správu vzdělávacího procesu (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v článku. [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s vyrovná se se zatížením pro správu vzdělávacího procesu, je třeba hello následující položky:

- Předplatné služby Azure AD
- Vyrovná se se zatížením pro správu vzdělávacího procesu jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání vyrovná se se zatížením pro správu vzdělávacího procesu z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-absorb-lms-from-hello-gallery"></a>Přidání vyrovná se se zatížením pro správu vzdělávacího procesu z Galerie hello
tooconfigure hello integrace vyrovná se se zatížením pro správu vzdělávacího procesu v tooAzure AD, je nutné tooadd Absorb pro správu vzdělávacího procesu hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Absorb pro správu vzdělávacího procesu z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **vyrovná se se zatížením pro správu vzdělávacího procesu**, vyberte **vyrovná se se zatížením pro správu vzdělávacího procesu** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Vyrovná se se zatížením pro správu vzdělávacího procesu v seznamu výsledků hello](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s vyrovná se se zatížením vzdělávacího procesu na základě testovací uživatele, nazývá "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v vyrovná se se zatížením pro správu vzdělávacího procesu je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v vyrovná se se zatížením pro správu vzdělávacího procesu musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v vyrovná se se zatížením pro správu vzdělávacího procesu.

tooconfigure a testu Azure AD jednotné přihlašování s vyrovná se se zatížením pro správu vzdělávacího procesu, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvořit testovací uživatele s vyrovná se se zatížením pro správu vzdělávacího procesu](#create-an-absorb-lms-test-user)**  -toohave protějšek Britta Simon v vzdělávacího procesu vyrovná se se zatížením, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci vyrovná se se zatížením pro správu vzdělávacího procesu.

**tooconfigure Azure AD jednotné přihlašování s vzdělávacího procesu vyrovná se se zatížením, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **vyrovná se se zatížením pro správu vzdělávacího procesu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. Na hello **vyrovná se se zatížením pro správu vzdělávacího procesu domény a adresy URL** část, proveďte následující kroky hello:

    ![Pro správu vzdělávacího procesu adresy URL jeden přihlašování informace o doméně a vyrovná se se zatížením](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_url.png)

    a. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.myabsorb.com/Account/SAML`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.myabsorb.com/Account/SAML`
     
    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné hello. Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL. Obraťte se na [tým podpory vyrovná se se zatížením klienta pro správu vzdělávacího procesu](https://www.absorblms.com/support) tooget tyto hodnoty. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-absorblms-tutorial/tutorial_general_400.png)
    
7. Na hello **vyrovná se se zatížením konfigurace pro správu vzdělávacího procesu** klikněte na tlačítko **konfigurace vyrovná se se zatížením vzdělávacího procesu** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**

    ![Vyrovná se se zatížením konfigurace pro správu vzdělávacího procesu](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_configure.png) 

8. V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti tooyour vyrovná se se zatížením pro správu vzdělávacího procesu.

9. Klikněte na tlačítko hello **ikonu účtu** na rozhraní správce hello. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/1.png)

10. Klikněte na tlačítko **nastavení portálu**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/2.png)
    
11. Klikněte na tlačítko hello **uživatelé** kartě.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/3.png)

12. Proveďte následující kroky tooaccess hello jednotné přihlašování konfigurační pole hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/4.png)

    a. Vyberte odpovídající hello **režimu**.

    b. Otevřete hello certifikát, který jste si stáhli z portálu Azure v poznámkovém bloku hello odebrat hello **---BEGIN CERTIFICATE---** a **---END CERTIFICATE---** značku a potom vložte hello zbývající obsah Hello **klíč** textové pole.
    
    c. V hello **Vlastnost Id**, vyberte hello odpovídající atribut, který jste nakonfigurovali jako hello identifikátor uživatele v hello Azure AD (například pokud hello userprinciplename je vybrána ve službě Azure AD, pak uživatelské jméno by zde vybraný.)

    d. V hello **přihlašovací adresa URL**, vložte hello **"SAML jeden přihlašování adresa URL služby"** hodnotu zkopírovanou z hello **konfigurovat přihlášení** okno hello portálu Azure.

    e. V hello **adresy URL odhlašovací**, vložte hello **"Adresa URL Sign-Out"** hodnotu zkopírovanou z hello **konfigurovat přihlášení** okno hello portálu Azure.

13. Povolit **"Povolit jenom přihlášení SSO"**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/5.png)

14. Klikněte na tlačítko **"Uložit."**

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-absorblms-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-absorblms-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-absorblms-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-absorblms-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.

### <a name="create-an-absorb-lms-test-user"></a>Vytvořit testovací uživatele s vyrovná se se zatížením pro správu vzdělávacího procesu

Uživatelé toolog tooenable Azure AD v tooAbsorb pro správu vzdělávacího procesu, se musí být zřízená v tooAbsorb pro správu vzdělávacího procesu.  
Pro vyrovná se se zatížením vzdělávacího procesu zřizování je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour vyrovná se se zatížením pro správu vzdělávacího procesu společnosti lokality jako správce.

2. Klikněte na tlačítko **uživatelé** kartě.

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/absorblms_users.png)

3. Klikněte na tlačítko **uživatelé** pod hello **uživatelé** kartě.

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/absorblms_userssub.png)

4.  Vyberte **uživatele** z **přidat nové** rozevíracího seznamu.

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/absorblms_createuser.png)

5. Na hello **přidat uživatele** proveďte hello následující kroky:

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/user.png)

    a. V hello **křestní jméno** textovému poli, typ hello křestní jméno jako Britta.

    b. V hello **příjmení** textovému poli, typ hello příjmení jako Simon.
    
    c. V hello **uživatelské jméno** textovému poli, zadejte jméno uživatele hello jako Britta Simon.

    d. V hello **heslo** textovému poli, zadejte heslo hello Britta Simon.

    e. V hello **Potvrdit heslo** textovému poli, typ hello stejné heslo.
    
    f. Nastavit jej jako **ACTIVE**.   

6. Klikněte na tlačítko **"Uložit."**
 
### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooAbsorb pro správu vzdělávacího procesu Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200]

**tooassign tooAbsorb Britta Simon vzdělávacího procesu, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **vyrovná se se zatížením pro správu vzdělávacího procesu**.

    ![Hello vyrovná se se zatížením pro správu vzdělávacího procesu odkaz v seznamu aplikace hello](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Klikněte na tlačítko hello vyrovná se se zatížením pro správu vzdělávacího procesu dlaždici v hello přístupového panelu, zobrazí se automaticky přihlášeného tooyour vyrovná se se zatížením pro správu vzdělávacího procesu aplikace. Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_203.png

