---
title: 'Kurz: Azure Active Directory integrace s ScaleX Enterprise | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ScaleX Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: e398b98d9e0957b5f92c82359651c345d22c3a54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a>Kurz: Azure Active Directory integrace s ScaleX Enterprise

V tomto kurzu zjistíte, jak toointegrate ScaleX Enterprise s Azure Active Directory (Azure AD).

Integrace ScaleX Enterprise s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooScaleX Enterprise
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooScaleX Enterprise (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v článku. Co je přístup k aplikaci a jednotné přihlašování s [Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s ScaleX Enterprise, je třeba hello následující položky:

- Předplatné služby Azure AD
- ScaleX Enterprise jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání ScaleX Enterprise z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-scalex-enterprise-from-hello-gallery"></a>Přidání ScaleX Enterprise z Galerie hello
tooconfigure hello integrace ScaleX Enterprise v tooAzure AD, je nutné tooadd ScaleX Enterprise hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd ScaleX Enterprise z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **ScaleX Enterprise**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

5. Na panelu výsledků hello vyberte **ScaleX Enterprise**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurujete a testovací Azure AD jednotného přihlašování k ScaleX organizace podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello protějšku uživatele v podniku ScaleX je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v podniku ScaleX musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** ve ScaleX podniku.

tooconfigure a testování Azure AD jednotného přihlašování k ScaleX organizace, budete potřebovat následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele ScaleX Enterprise](#creating-a-scalex-enterprise-test-user)**  -toohave protějšek Britta Simon v ScaleX organizace, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci ScaleX Enterprise.

**tooconfigure Azure AD jednotné přihlašování s ScaleX Enterprise, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **ScaleX Enterprise** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

3. Na hello **ScaleX podnikové domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **IDP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    a. V hello **identifikátor** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://platform.rescale.com/saml2/<company id>/`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://platform.rescale.com/saml2/<company id>/acs/`

4. Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    V hello **přihlašovací adresa URL** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://platform.rescale.com/saml2/<company id>/sso/`
     
    > [!NOTE] 
    > Tyto nejsou hello skutečné hodnoty. Tyto hodnoty aktualizujte s hello skutečné identifikátor, adresa URL odpovědi nebo adresa URL přihlašování. Obraťte se na [tým podpory klient systému Enterprise ScaleX](http://info.rescale.com/contact_sales) tooget tyto hodnoty. 

5. Aplikace ScaleX očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste toomodify vlastních atributů mapování tooyour tokenu atributy konfigurace SAML. Klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** hello tooopen políčko vlastní atributy nastavení.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/scalex_attributes.png)
    
    a. Klikněte pravým tlačítkem na atribut hello **název** a klikněte na příkaz odstranit.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/delete_attribute_name.png)

    b. Klikněte na tlačítko **emailaddress** atribut tooopen hello Upravit atribut okno. Změnit svou hodnotu z **user.mail** příliš**user.userprincipalname** a klikněte na tlačítko Ok.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/edit_email_attribute.png)   
    
5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_400.png)
    
7. Na hello **ScaleX podniková konfigurace** klikněte na tlačítko **konfigurace ScaleX Enterprise** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID** a **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

8. tooconfigure jednotného přihlašování na **ScaleX Enterprise** straně, web společnosti ScaleX Enterprise toohello přihlášení jako správce.

9. V nabídce hello v hello horní pravé a vyberte **Contoso správy**.

    > [!NOTE] 
    > Contoso je jenom jako příklad. To by měl být skutečný název vaší společnosti. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/Test_Admin.png) 

10. Vyberte **integrace** hello horní nabídce a výběrem **jednotné přihlašování**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/admin_sso.png) 

11. Vyplňte formulář hello následujícím způsobem:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/scalex_admin_save.png) 
    
    a. Vyberte **"Vytvořit každý uživatel, který může ověřit pomocí jednotného přihlašování."**

    b. **Poskytovatel služeb saml**: Vložit hodnotu hello ***urn: oasis: názvy: tc: SAML:2.0:nameid-formátu: trvalé***

    c. **Název pole zprostředkovatele Identity e-mailu v odpovědi ACS**: Vložit hodnotu hello`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. **ID EntityDescriptor Entity zprostředkovatele identity:** vložení hello **SAML Entity ID** hodnota zkopírována z hello portálu Azure.

    e. **Adresa URL SingleSignOnService zprostředkovatele identity:** vložení hello **SAML jeden přihlašování adresa URL služby** z hello portálu Azure.

    f. **Certifikát identity poskytovatele veřejných X509:** otevřete hello X509 certifikát stažený z hello Azure v programu Poznámkový blok a vložit obsah hello v tomto poli. Zajistěte, že žádná zalomení řádků hello střední hello obsahu certifikátu.
    
    g. Zaškrtněte následující zaškrtávací políčka hello: **povoleno, šifrování NameID a AuthnRequests přihlášení.**

    h. Klikněte na tlačítko **nastavení jednotného přihlašování k aktualizaci** toosave hello nastavení.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-scalex-enterprise-test-user"></a>Vytvoření zkušebního uživatele ScaleX Enterprise

Uživatelé toolog tooenable Azure AD v tooScaleX Enterprise, se musí být zřízená v tooScaleX Enterprise. V případě hello ScaleX podniku zřizování je automatická úloha a nejsou potřeba žádné ruční kroky. Každý uživatel, který může úspěšně ověřit pomocí přihlašovacích údajů jednotné přihlašování se automaticky zřídí na hello ScaleX straně.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte Enterprise tooScaleX přístupu uživatele.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooScaleX Enterprise, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **ScaleX Enterprise**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.

### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Klikněte na tlačítko hello ScaleX Enterprise dlaždici v hello přístupového panelu, zobrazí se automaticky přihlášeného tooyour ScaleX podniková aplikace. Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](https://msdn.microsoft.com/library/dn308586).


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_203.png

