---
title: 'Kurz: Azure Active Directory integraci s Workday | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a během pracovního dne."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: aaa41e372e45f464c4540a70fc79c98dbc06d6b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Kurz: Azure Active Directory integraci s Workday

V tomto kurzu zjistíte, jak toointegrate Workday s Azure Active Directory (Azure AD).

Integrace Workday s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooWorkday
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWorkday (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure Azure AD integraci s Workday, je třeba hello následující položky:

- Předplatné služby Azure AD
- Workday jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Workday z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-workday-from-hello-gallery"></a>Přidání Workday z Galerie hello
tooconfigure hello integrace Workday do Azure AD, musíte tooadd Workday hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Workday z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Workday**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/tutorial_workday_search.png)

5. Na panelu výsledků hello vyberte **Workday**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/tutorial_workday_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workday podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Workday je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello ve Workday musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** ve Workday.

tooconfigure a testu Azure AD jednotné přihlašování s Workday, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Workday](#creating-a-workday-test-user)**  -toohave protějšek Britta Simon ve Workday, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci během pracovního dne.

**tooconfigure Azure AD jednotné přihlašování s Workday, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Workday** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_samlbase.png)

3. Na hello **Workday domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, hodnota typu hello jako:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné hello. Tyto hodnoty aktualizujte hello skutečná adresa URL přihlašování a adresa URL odpovědi. Vaše adresa URL odpovědi musí mít například subdoména: www, wd2, wd3, wd3 impl, wd5, wd5 impl). Pomocí něco podobného jako "*http://www.myworkday.com*" funguje, ale "*http://myworkday.com*" neexistuje. Obraťte se na [tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html) tooget tyto hodnoty. 
 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_general_400.png)

6. Na hello **Workday konfigurace** klikněte na tlačítko **konfigurace Workday** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS>
7. V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti tooyour Workday.

8. Přejděte příliš**nabídky \> Workbench**.
   
    ![Workbench](./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")

9. Přejděte příliš**správy účtů**.
   
    ![Účet správy](./media/active-directory-saas-workday-tutorial/IC782924.png "účet správy")

10. Přejděte příliš**upravit nastavení klienta – zabezpečení**.
   
    ![Upravit zabezpečení klienta](./media/active-directory-saas-workday-tutorial/IC782925.png "upravit klienta zabezpečení")

11. V hello **adresy URL pro přesměrování** část, proveďte následující kroky hello:
   
    ![Adresy URL pro přesměrování](./media/active-directory-saas-workday-tutorial/IC7829581.png "adresy URL pro přesměrování")
   
    a. Klikněte na tlačítko **přidejte řádek**.
   
    b. V hello **adresy URL přesměrování při přihlášení** textové pole a hello **adresu URL pro přesměrování mobilní** textovému poli, typ hello **přihlašovací adresa URL** jste zadali na hello **Workday domény a Adresy URL** části hello portálu Azure.
   
    c. V portálu Azure, na hello hello **konfigurovat přihlášení** okno, kopie hello **Sign-Out URL**a pak ji vložit do hello **adresy URL přesměrování odhlašovací** textové pole.
   
    d.  V **prostředí** textovému poli, název typu hello prostředí.  

    >[!NOTE]
    > Hello hodnotu atributu hello prostředí je vázaný toohello hodnotu adresy URL hello klienta:  
    >-Pokud název domény hello URL klienta Workday hello začíná impl například: *https://impl.workday.com/\<klienta\>/login-saml2.htmld*), hello **prostředí** musí být nastaven tooImplementation.  
    >– Pokud je název domény hello začíná něco jiného, je nutné toocontact [tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html) tooget hello odpovídající **prostředí** hodnotu.

12. V hello **SAML instalace** část, proveďte následující kroky hello:
   
    ![Instalační program SAML](./media/active-directory-saas-workday-tutorial/IC782926.png "instalace SAML")
   
    a.  Vyberte **povolit ověřování SAML**.
   
    b.  Klikněte na tlačítko **přidejte řádek**.

13. V části zprostředkovatelů Identity SAML hello proveďte hello následující kroky:
   
    ![Zprostředkovatelé Identity SAML](./media/active-directory-saas-workday-tutorial/IC7829271.png "zprostředkovatelů Identity SAML")
   
    a. Do textového pole Název zprostředkovatele Identity hello, zadejte název zprostředkovatele (například: *SPInitiatedSSO*).
   
    b. V portálu Azure, na hello hello **konfigurovat přihlášení** okno, kopie hello **SAML Entity ID** hodnotu a pak ji vložit do hello **vystavitele** textové pole.
   
    c. Vyberte **povolit Workday Initiated odhlášení**.
   
    d. V portálu Azure, na hello hello **konfigurovat přihlášení** okno, kopie hello **Sign-Out URL** hodnotu a pak ji vložit do hello **adresy URL žádosti o odhlášení** textové pole.

    e. Klikněte na tlačítko **certifikátu veřejného klíče zprostředkovatele Identity**a potom klikněte na **vytvořit**. 

    ![Vytvoření](./media/active-directory-saas-workday-tutorial/IC782928.png "vytvořit")

    f. Klikněte na tlačítko **vytvořit x509 veřejný klíč**. 

    ![Vytvoření](./media/active-directory-saas-workday-tutorial/IC782929.png "vytvořit")


14. V hello **veřejný klíč zobrazení x509** část, proveďte následující kroky hello: 
   
    ![Veřejný klíč zobrazení x509](./media/active-directory-saas-workday-tutorial/IC782930.png "zobrazení x509 veřejný klíč") 
   
    a. V hello **název** textovému poli, zadejte název pro svůj certifikát (například: *PPE\_SP*).
   
    b. V hello **platný z** textovému poli, typ hello platnost od hodnota atributu vašeho certifikátu.
   
    c.  V hello **platný k** textovému poli, hodnota platná tooattribute hello typu certifikátu.
   
    > [!NOTE]
    > Můžete získat platný hello od data a hello platný toodate z hello stáhnout certifikát poklepáním.  Hello data jsou uvedeny v části hello **podrobnosti** kartě.
    > 
    >
   
    d.  V programu Poznámkový blok a potom hello kopírování obsahu je otevření kódovaného certifikátu kódování base-64.
   
    e.  V hello **certifikát** textovému poli, hello vložit obsah schránky.
   
    f.  Klikněte na **OK**.

15. Proveďte hello následující kroky: 
   
    ![Konfigurace jednotného přihlašování k](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "Konfigurace jednotného přihlašování")
   
    a.  Povolit hello **x509 pár privátního klíče**.
   
    b.  V hello **ID zprostředkovatele služby** textovému poli, typ **http://www.workday.com**.
   
    c.  Vyberte **povolit SP spustil ověřování SAML**.
   
    d.  V portálu Azure, na hello hello **konfigurovat přihlášení** okno, kopie hello **SAML jeden přihlašování adresa URL služby** hodnotu a pak ji vložit do hello **IdP adresa URL služby jednotného přihlašování k** textové pole.
   
    e. Vyberte **není Deflate žádosti o ověření spouštěná SP**.
   
    f. Jako **metoda žádosti o podpis ověřování**, vyberte **SHA256**. 
   
    ![Metoda ověřování žádost o podpis](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "metodu ověřování žádost o podpis") 
   
    g. Klikněte na **OK**. 
   
    ![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE>

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
>

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-workday-test-user"></a>Vytvoření zkušebního uživatele Workday

tooget testovacího uživatele zřízené do Workday, budete potřebovat toocontact hello [tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html).
Hello [tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html) pro vás vytvoří hello uživatele.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooWorkday toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooWorkday, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Workday**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu. Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfiguraci zřizování uživatelů](active-directory-saas-workday-inbound-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workday-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workday-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workday-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workday-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workday-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workday-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workday-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workday-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workday-tutorial/tutorial_general_203.png

