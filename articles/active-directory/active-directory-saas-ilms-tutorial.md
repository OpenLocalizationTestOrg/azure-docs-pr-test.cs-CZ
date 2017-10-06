---
title: 'Kurz: Azure Active Directory integrace s iLMS | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a iLMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: da0936de23afcd5a4213aa6f699165f9bfa82c35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a>Kurz: Azure Active Directory integrace s iLMS

V tomto kurzu zjistíte, jak toointegrate iLMS s Azure Active Directory (Azure AD).

Integrace iLMS s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooiLMS
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooiLMS (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s iLMS tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- ILMS jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání iLMS z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-ilms-from-hello-gallery"></a>Přidání iLMS z Galerie hello
tooconfigure hello integrace iLMS do Azure AD, je nutné tooadd iLMS hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd iLMS z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **iLMS**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_search.png)

5. Na panelu výsledků hello vyberte **iLMS**, pak klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s iLMS podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v iLMS je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v iLMS musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v iLMS.

tooconfigure a testu Azure AD jednotné přihlašování s iLMS, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele iLMS](#creating-an-ilms-test-user)**  -toohave protějšek Britta Simon v iLMS, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci iLMS.

**tooconfigure Azure AD jednotné přihlašování s iLMS, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **iLMS** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_samlbase.png)

3. Na hello **iLMS domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **IDP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url.png)

    a. V hello **identifikátor** textovému poli, vložte hello **identifikátor** hodnotu zkopírujete z **poskytovatele služeb** SAML nastaveních v portálu pro správu iLMS oddílu.

    b. V hello **adresa URL odpovědi** textovému poli, vložte hello **koncový bod (URL)** hodnotu zkopírujete z **poskytovatele služeb** části SAML nastavení v portálu pro správu iLMS s následující hello vzor`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

    >[!Note]
    >Tato 123456 je příkladem hodnoty identifikátoru.

4. Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url1.png)

    V hello **přihlašovací adresa URL** textovému poli, vložte hello **koncový bod (URL)** hodnotu zkopírujete z **poskytovatele služeb** části SAML nastavení v portálu pro správu iLMS jako`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`     

5. tooenable JIT zřizování, iLMS aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu. Nakonfigurujte hello následující deklarace identity pro tuto aplikaci. Můžete spravovat hello hodnoty těchto atributů z hello **uživatelské atributy** části na stránce integrace aplikace. Hello následující snímek obrazovky ukazuje příklad pro tento.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/4.png)
    
    Vytvoření **oddělení, oblast** a **dělení** atributy a přidejte název hello těchto atributů v iLMS. Všechny tyto atributy, které jsou uvedené výše se vyžadují.    

    > [!NOTE] 
    > Máte tooenable **vytvořit uživatelský účet Un-recognized** v iLMS toomap těchto atributů. Postupujte podle pokynů hello [sem](http://support.inspiredelearning.com/customer/portal/articles/2204526) tooget představu v konfiguraci atributy hello.

6. V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v hello obrázku výše a provést hello následující kroky:
    
    | Název atributu | Hodnota atributu |
    | ---------------| --------------- |    
    | dělení | User.Department |
    | Oblast | User.state |
    | Oddělení | User.jobtitle |

    a. Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_05.png)
    
    b. V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.
    
    c. Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.
    
    d. Klikněte na tlačítko **Ok**

7. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_certificate.png) 

8. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iLMS-tutorial/tutorial_general_400.png)

9. V okně prohlížeče jiných webových přihlásit tooyour **portál pro správu iLMS** jako správce.

10. Klikněte na tlačítko **SSO:SAML** pod **nastavení** tooopen SAML nastavení a proveďte hello následující kroky:
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/1.png) 

    a. Rozbalte hello **poskytovatele služeb** části a zkopírujte hello **identifikátor** a **koncový bod (URL)** hodnotu.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/2.png) 

    b. V části **zprostředkovatele Identity** klikněte na tlačítko **importovat Metadata**.
    
    c. Vyberte hello **Metadata** soubor stažený z portálu Azure z **SAML podpisový certifikát** části.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig1.png) 

    d. Pokud chcete, aby tooenable JIT zřizování toocreate iLMS účty pro zrušení-rozpoznat uživatele, postupujte podle následujících kroků:
        
       - Zkontrolujte **vytvořte účet bez rozpoznaný uživatel**.
       
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig2.png)

       -  Mapování atributů hello ve službě Azure AD s atributy hello v iLMS. Ve sloupci atributu hello zadejte hello atributy názvu nebo hello výchozí hodnotu.

    e. Přejděte příliš**obchodní pravidla** kartě a provádět hello následující kroky: 
        
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/5.png)

       - Zkontrolujte **vytvořit Un-recognized oblastí, rozdělení a oddělení** toocreate oblastí divizí a oddělení, které už neexistují v hello době jednotné přihlašování.
        
       - Zkontrolujte **aktualizace uživatelského profilu při přihlášení** toospecify jestli aktualizaci profilu hello uživatele s každou jednotné přihlašování. 
        
       - Pokud hello **"Aktualizace prázdné hodnoty pro jiný povinného pole v profilu uživatele"** zaškrtnutá možnost, volitelné profil pole, která jsou prázdné po přihlášení bude také způsobit profil iLMS hello uživatele toocontain prázdné hodnoty těchto polí.
        
       - Zkontrolujte **odeslat E-mail s oznámením chyba** a zadejte e-mailu hello hello uživatele, které chcete tooreceive hello chyba oznámení e-mailu.

11. Klikněte na tlačítko **Uložit** tlačítko toosave hello nastavení.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/save.png)

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
    
### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-ilms-test-user"></a>Vytváření testovacího uživatele iLMS

Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatele v aplikaci hello automaticky vytvoří. JIT bude fungovat, pokud jste klikli hello **vytvořit uživatelský účet Un-recognized** políčko během SAML nastavení konfigurace na portál pro správu iLMS.

Pokud potřebujete toocreate uživatelé ručně, postupujte podle následujících kroků:

1. Přihlaste se na webu společnosti iLMS tooyour jako správce.

2. Klikněte na tlačítko **"Uživatel zaregistrovat"** pod **uživatelé** kartě tooopen **registrovat uživatele** stránky. 
   
   ![Můžete přidat zaměstnance](./media/active-directory-saas-ilms-tutorial/3.png)

3. Na hello **"Uživatel zaregistrovat"** proveďte následující kroky hello.

    ![Můžete přidat zaměstnance](./media/active-directory-saas-ilms-tutorial/create_testuser_add.png)

    a. V hello **křestní jméno** textovému poli, typ hello křestní jméno Britta.
   
    b. V hello **příjmení** textovému poli, typ hello příjmení Simon.

    c. V hello **ID e-mailu** textovému poli, typ hello e-mailovou adresu účtu Britta Simon.

    d. V hello **oblast** rozevíracího seznamu, vyberte hello hodnotu pro oblast.

    e. V hello **dělení** rozevíracího seznamu, vyberte hello hodnotu pro dělení.

    f. V hello **oddělení** rozevíracího seznamu, vyberte hello hodnotu pro oddělení.

    g. Klikněte na **Uložit**.

    > [!NOTE] 
    > Můžete odeslat e-mailu toouser registrace výběrem **odesílání pošty registrace** zaškrtávací políčko.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooiLMS svůj přístup.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooiLMS, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **iLMS**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello iLMS dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour iLMS aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_203.png

