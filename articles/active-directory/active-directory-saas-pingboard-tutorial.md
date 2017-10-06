---
title: 'Kurz: Azure Active Directory integrace s Pingboard | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Pingboard."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 0a916b1f9ef32d8124aa11284d2115bb4fc0bbc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a>Kurz: Azure Active Directory integrace s Pingboard

V tomto kurzu zjistíte, jak toointegrate Pingboard s Azure Active Directory (Azure AD).

Integrace Pingboard s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooPingboard
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPingboard (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Pingboard tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Pingboard jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Pingboard z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-pingboard-from-hello-gallery"></a>Přidání Pingboard z Galerie hello
tooconfigure hello integrace Pingboard do Azure AD, je nutné tooadd Pingboard hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Pingboard z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Pingboard**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_search.png)

5. Na panelu výsledků hello vyberte **Pingboard**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Pingboard podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Pingboard je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Pingboard musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Pingboard.

tooconfigure a testu Azure AD jednotné přihlašování s Pingboard, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Pingboard](#creating-a-pingboard-test-user)**  -toohave protějšek Britta Simon v Pingboard, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci Pingboard.

**tooconfigure Azure AD jednotné přihlašování s Pingboard, proveďte následující kroky hello:**

1. V hello Azure Management portal na hello **Pingboard** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_samlbase.png)

3. Na hello **Pingboard domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **IDP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_url.png)

    a. V hello **identifikátor** textovému poli, hodnota typu hello jako:`http://<entity-id>.pingboard.com/sp`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<entity-id>.pingboard.com/auth/saml/consume`

    > [!NOTE] 
    > Upozorňujeme, že tyto nejsou hello skutečné hodnoty. Máte tooupdate tyto hodnoty pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL. Zde, doporučujeme vám toouse hello jedinečnou hodnotu řetězce v hello identifikátor. Obraťte se na [tým podpory Pingboard klienta](https://support.pingboard.com/) tooget tyto hodnoty. 

4. Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

    a. V hello **přihlašovací adresa URL** textovému poli, hodnota typu hello jako:`http://<sub-domain>.pingboard.com/sign_in`
     
5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_general_400.png)

7. tooconfigure jednotného přihlašování na straně Pingboard, otevřete nové okno prohlížeče a tooyour Pingboard účet pro přihlášení. Správce tooset Pingboard si jednotné přihlašování musí být na.

8. Hello horní nabídce vyberte **aplikace > integrace**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_integration.png)

9.  Na hello **integrace** stránky, vyhledejte hello **"Azure Active Directory"** dlaždici a klikněte na něj.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_aad.png)

10. V hello modální, který následuje dále klikněte na tlačítko **"Konfigurace"**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_configure.png)

11. Na následující stránku hello si všimnete, že "Integrace se službou Azure jednotného přihlašování k dispozici.". Otevřete hello stáhli soubor soubor XML s metadaty v programu Poznámkový blok a vložit hello obsahu v **IDP Metadata**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_sso_configure.png)

12. ověří Hello souboru, a pokud se vše, co je správná, bude nyní povoleno jednotné přihlašování

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-pingboard-test-user"></a>Vytvoření zkušebního uživatele Pingboard

V pořadí tooenable Azure AD Uživatelé toolog do Pingboard musí být zřízená do Pingboard.  
V případě hello Pingboard zřizování je ruční úloha.

**tooprovision uživatelské účty, provádět hello následující kroky:**

1. Přihlaste se tooyour Pingboard společnosti lokality jako správce.

2. Klikněte na tlačítko **"Přidat zaměstnancem"** tlačítko **Directory** stránky.

    ![Můžete přidat zaměstnance](./media/active-directory-saas-pingboard-tutorial/create_testuser_add.png)

3. Na hello **"Přidat zaměstnancem"** dialogové okno proveďte následující kroky hello.

    ![Pozvat uživatele](./media/active-directory-saas-pingboard-tutorial/create_testuser_name.png)

    a. V hello **úplný název** textovému poli, hello úplný název typu Britta Simon.

    b. V hello **e-mailu** textovému poli, typ hello e-mailovou adresu účtu Britta Simon.

    c. V hello **funkce** textovému poli, typ hello pozici Britta Simon.

    d. V hello **umístění** rozevíracího seznamu, vyberte hello umístění Britta Simon.
    
    e. Klikněte na tlačítko **Přidat**.   

4. Obrazovka s potvrzením bude spuštěna tooconfirm hello přidání uživatele.
    
    ![Potvrďte](./media/active-directory-saas-pingboard-tutorial/create_testuser_confirm.png)
        
    > [!NOTE]
    > Držitel účtu Azure Active Directory Hello bude dostávat e-mailu a postupujte podle tooconfirm odkaz svého účtu před stane aktivní.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooPingboard svůj přístup.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooPingboard, proveďte následující kroky hello:**

1. Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Pingboard**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Pingboard hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Pingboard aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_203.png
