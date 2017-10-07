---
title: 'Kurz: Azure Active Directory integrace s LiquidFiles | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a LiquidFiles."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cb517134-0b34-4a74-b40c-5a3223ca81b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 67eb888090f81e0ceb791ed45d564b98fe1eb6d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-liquidfiles"></a>Kurz: Azure Active Directory integrace s LiquidFiles

V tomto kurzu zjistíte, jak toointegrate LiquidFiles s Azure Active Directory (Azure AD).

Integrace LiquidFiles s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooLiquidFiles
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLiquidFiles (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s LiquidFiles tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- LiquidFiles jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání LiquidFiles z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-liquidfiles-from-hello-gallery"></a>Přidání LiquidFiles z Galerie hello
tooconfigure hello integrace LiquidFiles do Azure AD, je nutné tooadd LiquidFiles hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd LiquidFiles z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **LiquidFiles**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_search.png)

5. Na panelu výsledků hello vyberte **LiquidFiles**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s LiquidFiles podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v LiquidFiles je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v LiquidFiles musí toobe navázat.

V LiquidFiles, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s LiquidFiles, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele LiquidFiles](#creating-a-liquidfiles-test-user)**  -toohave protějšek Britta Simon v LiquidFiles, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci LiquidFiles.

**tooconfigure Azure AD jednotné přihlašování s LiquidFiles, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **LiquidFiles** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_samlbase.png)

3. Na hello **LiquidFiles domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<YOUR_SERVER_URL>/saml/init`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<YOUR_SERVER_URL>`

    c. b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<YOUR_SERVER_URL>/saml/consume`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizace tyto hodnoty s hello skutečná adresa URL přihlašování, identifikátor a adresa URL odpovědi. Obraťte se na [tým podpory LiquidFiles klienta](https://www.liquidfiles.com/support.html) tooget tyto hodnoty. 
 
4. Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnota certifikátu.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_400.png)

6. Na hello **LiquidFiles konfigurace** klikněte na tlačítko **konfigurace LiquidFiles** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_configure.png)
 
7. Web společnosti LiquidFiles tooyour přihlášení jako správce.

8. Klikněte na tlačítko **jednotné přihlašování** v hello **správce > Konfigurace** nabídce hello.

9. Na hello **konfigurace přihlášení** proveďte následující kroky hello

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-liquidfiles-tutorial/tutorial_single_01.png)

    a. Jako **jedné přihlašovací na metody**, vyberte **SAML 2**.

    b. V hello **IDP přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.

    c. V hello **adresy URL odhlašovací IDP** textovému poli, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.

    d. V hello **otisků certifikátu IDP** textovému poli, vložte hello **kryptografický OTISK** hodnotu, která jste zkopírovali z portálu Azure...

    e. V textovém poli Formát názvu identifikátor hello, zadejte hodnotu hello **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: emailAddress**.

    f. V hello ověřovacího kontextu textového pole zadejte hodnotu hello **urn: oasis: názvy: tc: SAML:2.0:ac:classes:PasswordProtectedTransport**.

    g. Klikněte na **Uložit**.  

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-liquidfiles-test-user"></a>Vytvoření zkušebního uživatele LiquidFiles

Hello cílem této části je toocreate volal Britta Simon v LiquidFiles uživatele. Spolupracovat s vaší tooget správce serveru LiquidFiles sami přidána jako uživatel před protokolování v tooyour LiquidFiles aplikace.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooLiquidFiles toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooLiquidFiles, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **LiquidFiles**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.

Po kliknutí na tlačítko hello LiquidFiles dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour LiquidFiles aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_203.png

