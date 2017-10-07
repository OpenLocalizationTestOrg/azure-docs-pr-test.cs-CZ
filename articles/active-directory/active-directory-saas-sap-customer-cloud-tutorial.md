---
title: "Kurz: Azure Active Directory integrace s cloudem SAP zákazníka. | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SAP cloudu pro zákazníka."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 0525ea81122458ab3ac24a5bdb0b5f628405dd05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a>Kurz: Azure Active Directory integrace s cloudem SAP zákazníka.

V tomto kurzu zjistíte, jak toointegrate SAP cloudu pro zákazníka s Azure Active Directory (Azure AD).

Integrace SAP cloudu pro zákazníka s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooSAP cloudu zákazníka.
- Vaši uživatelé tooautomatically get přihlášeného tooSAP cloudu můžete povolit pro zákazníka (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s SAP cloudu pro zákazníka, je třeba hello následující položky:

- Předplatné služby Azure AD
- Cloud SAP pro zákazníka jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SAP cloudu pro zákazníka z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-sap-cloud-for-customer-from-hello-gallery"></a>Přidání SAP cloudu pro zákazníka z Galerie hello
tooconfigure hello integrace SAP cloudu pro zákazníka do služby Azure AD, musíte pro zákazníka hello Galerie tooyour seznamu spravovaných aplikací SaaS tooadd SAP cloudu.

**tooadd SAP cloudu pro zákazníka z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **SAP cloudu pro zákazníka**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

5. Na panelu výsledků hello vyberte **SAP cloudu pro zákazníka**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části konfiguraci a testování Azure AD jednotné přihlašování s cloudem SAP pro zákazníka na základě testovací uživatele, nazývá "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v cloudu SAP pro zákazníka je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v cloudu SAP pro odběratele musí toobe navázat.

V cloudu SAP pro zákazníka, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testovací Azure AD jednotného přihlášení SAP cloudu pro zákazníka, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření cloudu SAP pro zákazníka testovacího uživatele](#creating-a-sap-cloud-for-customer-test-user)**  -toohave protějšek Britta Simon v cloudu SAP pro zákazníka, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování ve vašem cloudu SAP pro aplikace pro zákazníky.

**tooconfigure Azure AD jednotného přihlášení SAP cloudu pro zákazníka, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **SAP cloudu pro zákazníka** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

3. Na hello **SAP cloudu zákazníka domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server name>.crm.ondemand.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server name>.crm.ondemand.com`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [SAP Cloud pro tým podpory zákazníků klienta](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooget tyto hodnoty. 

4. Na hello **uživatelské atributy** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    a. V **uživatelský identifikátor** seznamu, vyberte hello **ExtractMailPrefix()** funkce.

    b. Z hello **e-mailu** seznamu, vyberte hello atribut uživatele chcete toouse týkající se vaší implementace.
    Například pokud chcete, aby toouse hello EmployeeID jako jedinečný identifikátor uživatele a hodnota atributu hello jsou uloženy v hello ExtensionAttribute2, pak vyberte user.extensionattribute2.  

5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_400.png)

7. Na hello **SAP cloudu pro konfiguraci zákazníka** klikněte na tlačítko **konfigurace cloudu SAP pro zákazníka** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

8. tooget jednotného přihlašování nakonfigurovaná, proveďte hello následující kroky:
   
    a. Přihlášení do cloudu SAP pro zákaznický portál s právy správce.
   
    b. Přejděte toohello **aplikace a běžné úlohy správy uživatele** a klikněte na tlačítko hello **zprostředkovatele Identity** kartě.
   
    c. Klikněte na tlačítko **nového poskytovatele Identity** a soubor XML metadat vyberte hello jste si stáhli z portálu Azure hello. Importováním hello metadata hello systému automaticky nahrává hello dodejka certifikát a certifikát pro šifrování.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    d. Azure Active Directory vyžaduje hello element adresa URL služby Assertion příjemce v žádost SAML hello, takže vyberte hello **zahrnují Assertion příjemce adresa URL služby** zaškrtávací políčko.
   
    e. Klikněte na tlačítko **aktivovat jednotné přihlašování**.
   
    f. Uložte provedené změny.
   
    g. Klikněte na tlačítko hello **Moje systému** kartě.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    h. V **Azure AD adresa URL přihlašování** textovému poli, vložte **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    i. Zadejte, zda zaměstnanec hello ručně vybrat mezi přihlásit pomocí ID uživatele a heslo nebo jednotného přihlašování k výběrem hello **výběr zprostředkovatele Identity ruční**.
   
    j. V hello **jednotného přihlašování k adrese URL** části, zadejte adresu URL hello, který má být používána vaši zaměstnanci toosign systému toohello. 
    V hello **odeslané na adresu URL tooEmployee** seznamu, můžete si vybrat mezi hello následující možnosti:
   
    **Adresa URL jednotného přihlašování**
   
    systém Hello odešle jenom hello normální systému URL toohello zaměstnanců. Zaměstnanec Hello nelze přihlášení pomocí jednotného přihlašování a musí používat heslo nebo místo toho certifikátu.
   
    **ADRESA URL JEDNOTNÉHO PŘIHLAŠOVÁNÍ** 
   
    systém Hello odešle jenom hello zaměstnanec toohello jednotného přihlašování k adrese URL. Hello zaměstnanců může přihlásit pomocí jednotného přihlašování. Prostřednictvím hello IdP přesměruje požadavek na ověření.
   
    **Automatický výběr**
   
    Pokud jednotného přihlašování není aktivní, odešle hello systému hello normální systému URL toohello zaměstnanců. Pokud je aktivní jednotné přihlašování, hello systému zkontroluje, zda hello zaměstnanec má heslo. Pokud heslo je k dispozici, jednotného přihlašování k adrese URL a adresy URL bez jednotného přihlašování k odešlou toohello zaměstnanců. Ale pokud zaměstnanec hello nemá žádné heslo, pouze hello URL jednotného přihlašování k odešle toohello zaměstnanců.
   
    kB. Uložte provedené změny.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a>Vytvoření cloudu SAP pro zákazníka testovacího uživatele

V této části vytvoříte uživatele volat Britta Simon v cloudu SAP pro zákazníka. Spojte se s [SAP Cloud pro tým podpory zákazníků](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooadd hello uživatele v hello SAP cloudu pro platformu zákazníka. 

> [!NOTE]
> Zkontrolujte, zda hodnota NameID by měl odpovídat hello pole pro uživatelské jméno v hello SAP cloudu pro platformu zákazníka.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části je povolit Britta Simon toouse Azure jednotné přihlašování tak, že udělíte přístup tooSAP cloudu pro zákazníka.

![Přiřadit uživatele][200] 

**tooassign tooSAP Britta Simon cloudu pro zákazníka, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **SAP cloudu pro zákazníka**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello SAP cloudu pro dlaždice zákazníka v hello přístupového panelu, měli byste obdržet tooyour automaticky přihlášeného SAP cloudu pro aplikace pro zákazníky.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_203.png

