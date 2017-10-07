---
title: 'Kurz: Azure Active Directory integrace s Salesforce | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a služby Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 1d848518ee30910e051cdc4746c599219f3b5a3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Kurz: Azure Active Directory integrace s Salesforce

V tomto kurzu zjistíte, jak toointegrate Salesforce službou Azure Active Directory (Azure AD).

Integrace služby Salesforce s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooSalesforce
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSalesforce (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD pomocí služby Salesforce, je třeba hello následující položky:

- Předplatné služby Azure AD
- Služby Salesforce jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Salesforce z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-salesforce-from-hello-gallery"></a>Přidání Salesforce z Galerie hello
tooconfigure hello integrace služby Salesforce do Azure AD, je nutné tooadd Salesforce hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Salesforce z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Salesforce**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_search.png)

5. Na panelu výsledků hello vyberte **Salesforce**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Salesforce podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Salesforce je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Salesforce musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Salesforce.

tooconfigure a testování Azure AD jednotné přihlašování pomocí služby Salesforce, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Salesforce](#creating-a-salesforce-test-user)**  -toohave protějšek Britta Simon v Salesforce, které je propojené toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Salesforce.

**tooconfigure Azure AD jednotné přihlašování pomocí služby Salesforce, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Salesforce** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_samlbase.png)

3. Na hello **Salesforce domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_url.png)

    V hello **přihlašovací adresa URL** textovému poli, typ hello hodnotu pomocí hello následující vzoru: 
   * Účet organizace:`https://<subdomain>.my.salesforce.com`
   * Vývojářský účet:`https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné hello. Tyto hodnoty aktualizujte s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory služby Salesforce klienta](https://help.salesforce.com/support) tooget tyto hodnoty. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_general_400.png)

6. Na hello **Salesforce konfigurace** klikněte na tlačítko **konfigurace Salesforce** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z hello **Stručná referenční příručka části.** 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS>
7.  V prohlížeči otevřete novou kartu a přihlaste se tooyour správce účtu Salesforce.

8.  V části hello **správce** navigačním podokně klikněte na tlačítko **ovládacích prvků zabezpečení** tooexpand hello související části. Pak klikněte na tlačítko **nastavení jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png)

9.  Na hello **nastavení jednotného přihlašování** klikněte na tlačítko hello **upravit** tlačítko.
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)

      > [!NOTE]
      > Pokud jste nelze tooenable jednotné přihlašování v nastavení účtu služby Salesforce, může být nutné toocontact [tým podpory služby Salesforce klienta](https://help.salesforce.com/support). 

10. Vyberte **povoleno SAML**a potom klikněte na **Uložit**.

      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png)
11. tooconfigure SAML jeden přihlašování nastavení, klikněte na tlačítko **nový**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png)

12. Na hello **SAML jeden přihlašování nastavení upravit** vyberte hello následující konfigurace:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png)

    a. Pro hello **název** pole, zadejte popisný název pro tuto konfiguraci. Poskytuje hodnotu pro **název** automaticky vyplnit hello **název rozhraní API** textové pole.

    b. Vložení **SMAL Entity ID** hodnotu do hello **vystavitele** pole v Salesforce.

    c. V hello **textové pole Entity Id**, zadejte název domény vaší služby Salesforce pomocí hello následující vzoru:
      
      * Účet organizace:`https://<subdomain>.my.salesforce.com`
      * Vývojářský účet:`https://<subdomain>-dev-ed.my.salesforce.com`
      
    d. Klikněte na tlačítko **Procházet** nebo **zvolit soubor** tooopen hello **zvolit soubor tooUpload** dialogovém okně, vyberte svůj certifikát Salesforce a pak klikněte na tlačítko **otevřete**tooupload hello certifikátu.

    e. Pro **typ Identity SAML**, vyberte **kontrolní výraz obsahuje uživatelské jméno uživatele salesforce.com**.

    f. Pro **umístění Identity SAML**, vyberte **identita je v elementu NameIdentifier hello hello předmětu – příkaz**

    g. Vložení **jeden přihlašování adresa URL služby** do hello **adresu URL pro přihlášení zprostředkovatele Identity** pole v Salesforce.
    
    h. Pro **zprostředkovatele iniciované žádosti vazby služby**, vyberte **přesměrování protokolu HTTP**.
    
    i. Nakonec klikněte na **Uložit** tooapply SAML jeden přihlašování nastavení.

13. V levém navigačním podokně hello v Salesforce, klikněte na tlačítko **Správa domén** tooexpand hello související části a pak klikněte na **Moje domény**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png)

14. Projděte dolů toohello **konfiguraci ověřování** části a klikněte na tlačítko hello **upravit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png)

15. V hello **ověřovací služby** vyberte hello popisný název konfigurace jednotného přihlašování SAML a pak klikněte na tlačítko **Uložit**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Pokud je vybrána více než jedna služba ověřování, jsou uživatelé výzvami tooselect služby ověřování, která se jako toosign se při inicializaci tooyour přihlašování Salesforce prostředí. Pokud nechcete, aby bylo toohappen, pak byste měli **nechte nezaškrtnuté všechny ostatní služby ověřování**.
<CE>    
> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V levém navigačním podokně hello v hello **portál Azure**, klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-salesforce-test-user"></a>Vytvoření zkušebního uživatele Salesforce

V této části se uživatel volá Britta Simon vytvoří v Salesforce. Salesforce podporuje za běhu zřizování, který je ve výchozím nastavení povolené.
Neexistuje žádná položka akce pro vás v této části. Pokud uživatel v Salesforce ještě neexistuje, je vytvořen nový pokusíte-li tooaccess Salesforce.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooSalesforce toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooSalesforce, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Salesforce**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

tootest jeden přihlašování nastavení, otevřete hello přístupovému panelu na adrese [https://myapps.microsoft.com](https://myapps.microsoft.com/), pak se přihlaste do hello testovací účet a klikněte na tlačítko **Salesforce**.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfiguraci zřizování uživatelů](active-directory-saas-salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_203.png

