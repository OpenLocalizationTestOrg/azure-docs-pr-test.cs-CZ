---
title: 'Kurz: Azure Active Directory integrace se serverem Tableau | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Tableau serveru."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: feb2087bd6ae6ddcb920901e6719688fc95ae287
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Kurz: Azure Active Directory integrace se serverem Tableau

V tomto kurzu zjistíte, jak toointegrate Tableau serveru se službou Azure Active Directory (Azure AD).

Integrace serveru Tableau s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooTableau serveru
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTableau serveru (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Tableau serveru, je třeba hello následující položky:

- Předplatné služby Azure AD
- Serveru Tableau jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání serveru Tableau z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-tableau-server-from-hello-gallery"></a>Přidání serveru Tableau z Galerie hello
tooconfigure hello integrace Tableau serveru do Azure AD, je nutné tooadd Tableau Server hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Tableau serveru z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Tableau Server**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_search.png)

5. Na panelu výsledků hello vyberte **Tableau Server**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Tableau serveru podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello Tableau serveru je tooa uživatelem ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello Tableau serveru musí toobe navázat.

V serveru Tableau přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Tableau serveru, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele serveru Tableau](#creating-a-tableau-server-test-user)**  -toohave protějšek Britta Simon Tableau serveru, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Tableau serveru.

**tooconfigure Azure AD jednotné přihlašování se serverem Tableau, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Tableau Server** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_samlbase.png)

3. Na hello **Tableau Server domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://azure.<domain name>.link`
    
    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://azure.<domain name>.link`

    c. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://azure.<domain name>.link/wg/saml/SSO/index.html`
     
    > [!NOTE] 
    > Hello předchozí hodnoty nejsou skutečné hodnoty. Později aktualizujte hello hodnoty s hello skutečná adresa URL a identifikátor ze stránky konfigurace Tableau Server hello. 

4. Aplikace serveru tableau očekává, že hello SAML kontrolní výrazy ve specifickém formátu. Nakonfigurujte hello následující deklarace identity pro tuto aplikaci. Můžete spravovat hello hodnoty těchto atributů z hello **"Atributy uživatele"** části na stránce integrace aplikace. Hello následující snímek obrazovky ukazuje příklad pro hello stejné.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/3.png)
    
5. V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v hello obrázku výše a provést hello následující kroky:
    
    | Název atributu | Hodnota atributu |
    | ---------------| --------------- |    
    | uživatelské jméno | *User.DisplayName* |

    a. Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_05.png)
    
    b. V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.
    
    c. Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.
    
    d. Klikněte na tlačítko **Ok**


6. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_certificate.png) 

7. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS>
8. tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, musíte klienta toosign na tooyour Tableau serveru jako správce.
   
   a. V konfiguraci serveru Tableau hello, klikněte na tlačítko hello **SAML** kartě.
  
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 
  
   b. Zaškrtněte políčko hello z **pomocí SAML pro jednotné přihlašování**.
   
   c. Tableau Server návratovou adresu URL – adresy URL, která uživatelé Tableau serveru budou například http://tableau_server hello. Se nedoporučuje používat http://localhost. Použijete adresu URL s koncové lomítko (například http://tableau_server/) není podporována. Kopírování **Tableau Server návratovou adresu URL** a vložte ji tooAzure AD **přihlašovací adresa URL** textového pole v **Tableau Server domény a adresy URL** části.
   
   d. SAML entity ID – hello entity ID jednoznačně identifikuje vaši toohello instalace serveru Tableau IdP. Můžete zadat vaše adresa URL serveru Tableau znovu sem, pokud chcete, ale nemá toobe vaše adresa URL serveru Tableau. Kopírování **SAML entity ID** a vložte ji tooAzure AD **identifikátor** textového pole v **Tableau Server domény a adresy URL** části.
     
   e. Klikněte na tlačítko hello **exportovat soubor metadat** a otevře ji v hello textový editor aplikace. Vyhledejte adresu URL služby příjemce Assertion pomocí Http Post a Index 0 a adresy URL hello kopírování. Nyní vložte jej tooAzure AD **adresa URL odpovědi** textového pole v **Tableau Server domény a adresy URL** části.
   
   f. Vyhledejte soubor federačních metadat stáhli z portálu Azure a nahrajte ho v hello **soubor metadat SAML Idp**.
   
   g. Klikněte na tlačítko hello **OK** tlačítka na stránku konfigurace serveru Tableau hello.
   
    >[!NOTE] 
    >Zákazník mít tooupload některý z certifikátů v konfiguraci jednotné přihlašování SAML Tableau Server hello a budou získat ignorovat v hello toku jednotné přihlašování.
    >Pokud jste třeba pomoci konfigurace SAML na serveru Tableau pak naleznete v článku toothis [konfigurace SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).
    >
<CE>

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-tableau-server-test-user"></a>Vytvoření zkušebního uživatele Tableau serveru

Hello cílem této části je toocreate uživatele volat Britta Simon Tableau serveru. Je nutné tooprovision všichni uživatelé hello hello Tableau serveru. 

Shodovat s tímto uživatelským jménem uživatele hello hello hodnotu, která jste nakonfigurovali ve vlastní atribut hello Azure AD **uživatelské jméno**. S hello správné mapování hello integrace by měla fungovat [konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).

>[!NOTE]
>Pokud potřebujete toocreate uživatel ručně, musíte toocontact hello Tableau serveru správce ve vaší organizaci.
> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooTableau Server toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooTableau serveru, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Tableau Server**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Tableau Server hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Tableau serverová aplikace.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png

