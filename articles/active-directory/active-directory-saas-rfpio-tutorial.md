---
title: 'Kurz: Azure Active Directory integrace s RFPIO | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a RFPIO."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: e0c692276276edd8f859e73d81cf54d75a65957a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a>Kurz: Azure Active Directory integrace s RFPIO

V tomto kurzu zjistíte, jak toointegrate RFPIO s Azure Active Directory (Azure AD).

Integrace RFPIO s Azure AD poskytuje hello následující výhody:

- Můžete určovat, kdo ve službě Azure AD, který má přístup tooRFPIO.
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooRFPIO (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě – hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s RFPIO tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD.
- RFPIO jednotného přihlašování na povolené předplatné.

> [!NOTE]
> Nedoporučujeme používat kroky hello tootest v produkčním prostředí v tomto kurzu.

tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:

- Pokud to není nezbytné, nepoužívejte produkční prostředí.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénář, který je popsané v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání RFPIO z Galerie hello.
2. Konfigurace a testování Azure AD jednotného přihlašování.

## <a name="add-rfpio-from-hello-gallery"></a>Přidat RFPIO z Galerie hello
tooconfigure hello integrace RFPIO do Azure AD, je nutné tooadd RFPIO hello Galerie tooyour seznamu spravovaných aplikací SaaS.

### <a name="tooadd-rfpio-from-hello-gallery"></a>tooadd RFPIO z Galerie hello

1. V hello  **[portál Azure](https://portal.azure.com)**, na hello levém navigačním podokně, vyberte hello **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, vyberte hello **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **RFPIO**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_search.png)

5. Na panelu výsledků hello vyberte **RFPIO**a potom vyberte hello **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s RFPIO podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, jaký vztah hello je mezi příslušného uživatele v RFPIO a uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v RFPIO musí toobe navázat.

V RFPIO, přiřadit hodnotu hello **uživatelské jméno** ve službě Azure AD jako hodnota hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s RFPIO, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**– tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#creating-an-azure-ad-test-user)**– tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele RFPIO](#creating-a-rfpio-test-user)**  – toohave protějšek Britta Simon v RFPIO, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**tooenable – Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#testing-single-sign-on)**  – tooverify Pokud hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci RFPIO.

**tooconfigure Azure AD jednotné přihlašování s RFPIO, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **RFPIO** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_samlbase.png)

3. Na hello **RFPIO domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url.png)

    a. V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://www.rfpio.com`

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url1.png)

    b. Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.

    c. V hello **předávání stavu** textového pole zadejte hodnotu řetězce. Obraťte se na [tým podpory RFPIO](https://www.rfpio.com/contact/) tooget tuto hodnotu. 

4. Zkontrolujte **zobrazit upřesňující nastavení adresy URL**. Pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:   

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url2.png)

    V hello **přihlásit na adrese URL** textovému poli, hello zadat adresu URL:`https://www.app.rfpio.com`

5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_general_400.png)

7. V okně prohlížeče jiný web, přihlášení toohello **RFPIO** webu jako správce.

8. Klikněte na rozevírací rohu dolní hello.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app1.png)

9. Klikněte na hello **nastavení organizace**. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app2.png)

10. Klikněte na hello **funkce a integrace**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app4.png)

11. V hello **Konfigurace jednotného přihlašování SAML** klikněte na tlačítko **upravit**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app3.png)

12. V této části proveďte následující akce:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app5.png)
    
    a. Zkopírujte obsah hello hello **stáhnout soubor XML s metadaty** a vložte jej do hello **konfigurace identity** pole.

    > [!NOTE]
    >stáhnout obsah toocopy hello **soubor XML s metadaty** použití **Poznámkový blok ++** nebo správné **editoru XML**. 

    b. Klikněte na tlačítko **ověření**.

    c. Po kliknutím **ověřením**, podle **SAML(Enabled)** tooon.

    d. Klikněte na tlačítko **odeslání**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-rfpio-test-user"></a>Vytvoření zkušebního uživatele RFPIO

Uživatelé toolog tooenable Azure AD v tooRFPIO, se musí být zřízená do RFPIO.  
V případě hello RFPIO zřizování je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour RFPIO společnosti lokality jako správce.

2. Klikněte na rozevírací rohu dolní hello.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app1.png)

3. Klikněte na hello **nastavení organizace**. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app2.png)

4. Klikněte na tlačítko **členové týmu**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app6.png)

5. Klikněte na **přidat členy**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app7.png)

6. V hello **přidat nové členy** části. Proveďte následující akce:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app8.png)

    a. Zadejte **e-mailová adresa** v hello **zadejte jednu e-mailovou na každý řádek** pole.

    b. Vyberte Plese **Role** podle požadavků.

    c. Klikněte na tlačítko **přidat členy**.
        
    > [!NOTE]
    > Držitel účtu Azure Active Directory Hello obdrží e-mailu a dodržuje odkaz tooconfirm svého účtu před stane aktivní.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooRFPIO toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooRFPIO, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **RFPIO**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části otestovat vaše konfigurace Azure AD jeden přihlašování pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello RFPIO dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour RFPIO aplikace.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů o toointegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_203.png

