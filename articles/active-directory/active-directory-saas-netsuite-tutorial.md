---
title: 'Kurz: Azure Active Directory integrace s Netsuite | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Netsuite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 7cf205d5bda5333872fb589e57f4779a8670b595
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Kurz: Azure Active Directory integrace s Netsuite

V tomto kurzu zjistíte, jak toointegrate Netsuite s Azure Active Directory (Azure AD).

Integrace Netsuite s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooNetsuite
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooNetsuite (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Netsuite tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Netsuite jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Netsuite z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-netsuite-from-hello-gallery"></a>Přidání Netsuite z Galerie hello
tooconfigure hello integrace Netsuite do Azure AD, je nutné tooadd Netsuite hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Netsuite z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Netsuite**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_search.png)

5. Na panelu výsledků hello vyberte **Netsuite**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Netsuite podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Netsuite je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Netsuite musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Netsuite.

tooconfigure a testu Azure AD jednotné přihlašování s Netsuite, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Netsuite](#creating-a-netsuite-test-user)**  -toohave protějšek Britta Simon v Netsuite, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Netsuite.

**tooconfigure Azure AD jednotné přihlašování s Netsuite, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Netsuite** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_samlbase.png)

3. Na hello **Netsuite domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_url.png)

    V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzor: `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs``https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    > [!NOTE] 
    > Tato hodnota není skutečné hodnoty. Aktualizace hello hodnotu s hello skutečná adresa URL odpovědi. Obraťte se na [tým podpory Netsuite](http://www.netsuite.com/portal/services/support.shtml) tooget tuto hodnotu.
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_general_400.png)

6. Na hello **Netsuite konfigurace** klikněte na tlačítko **konfigurace Netsuite** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_configure.png) 

7. V prohlížeči otevřete novou kartu a přihlaste se k serveru vaší společnosti Netsuite jako správce.

8. Na hello nástrojů v horní části hello hello stránky, klikněte na **instalace**, pak klikněte na tlačítko **Správce instalace**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

9. Z hello **úlohy nastavení** seznamu, vyberte **integrace**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-integration.png)

10. V hello **spravovat ověřování** klikněte na tlačítko **SAML jednotné přihlašování**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-saml.png)

11. Na hello **SAML instalace** proveďte hello následující kroky:
   
    a. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hodnoty **Stručná referenční příručka** části **konfigurovat přihlášení** a vložte jej do hello **zprostředkovatele Identity Přihlašovací stránka** pole Netsuite.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png)
  
    b. V Netsuite, vyberte **primární metoda ověřování**.

    c. Pro hello pole s názvem bez přípony **metadat zprostředkovatelů Identity SAMLV2**, vyberte **nahrát soubor metadat IDP**. Pak klikněte na tlačítko **Procházet** tooupload hello metadata souboru, který jste stáhli z portálu Azure.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png)

    d. Klikněte na tlačítko **odeslání**.

12. Ve službě Azure AD, klikněte na **zobrazit a upravit všechny ostatní atributy uživatele** zaškrtávací políčko a přidání atributu.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-attributes.png)

13. Pro hello **název atributu** pole, zadejte v `account`. Pro hello **hodnota atributu** pole, zadejte v ID Netsuite účtu. Tato hodnota je konstanta a změňte pomocí účtu. Pokyny, jak toofind vaše ID pro účet jsou popsány níže:

      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-add-attribute.png)

    a. V Netsuite, klikněte na **instalace** nabídce hello v horním navigačním panelu.

    b. Klikněte v části hello **úlohy nastavení** části hello levé navigační nabídce vyberte hello **integrace** a klikněte na **webové služby Předvolby**.

    c. Vaše ID pro účet Netsuite zkopírujte a vložte ho do hello **hodnota atributu** pole ve službě Azure AD.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-account-id.png)

14. Předtím, než mohou uživatelé provádět jednotné přihlašování do Netsuite, je třeba nejprve je přiřadit hello v Netsuite příslušná oprávnění. Postupujte podle pokynů hello níže tooassign tato oprávnění.

    a. V nabídce hello horním navigačním panelu klikněte na tlačítko **instalace**, pak klikněte na tlačítko **Správce instalace**.
      
      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    b. V levém navigačním nabídce hello vyberte **uživatele/role**, pak klikněte na tlačítko **spravovat role**.
      
      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-manage-roles.png)

    c. Klikněte na tlačítko **novou roli**.

    d. Zadejte **název** pro novou roli a vyberte hello **jednoho přihlášení pouze** zaškrtávací políčko.
      
      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-new-role.png)

    e. Klikněte na **Uložit**.

    f. V nabídce hello hello nahoře, klikněte na tlačítko **oprávnění**. Pak klikněte na tlačítko **instalační program**.
      
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-sso.png)

    g. Vyberte **nastavit až SAM jednotné přihlašování**a potom klikněte na **přidat**.

    h. Klikněte na **Uložit**.

    i. V nabídce hello horním navigačním panelu klikněte na tlačítko **instalace**, pak klikněte na tlačítko **Správce instalace**.
      
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    j. V levém navigačním nabídce hello vyberte **uživatele/role**, pak klikněte na tlačítko **spravovat uživatele**.
      
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-manage-users.png)

    kB. Vyberte testovací uživatele. Pak klikněte na tlačítko **upravit**.
      
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-edit-user.png)

    l. V dialogovém okně hello rolí, vyberte roli hello, který jste vytvořili a klikněte na tlačítko **přidat**.
      
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-add-role.png)

    m. Klikněte na **Uložit**.
    
> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_01.png) 

2.  toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**. 

### <a name="creating-a-netsuite-test-user"></a>Vytvoření zkušebního uživatele Netsuite

V této části se uživatel volá Britta Simon vytvoří v Netsuite. Netsuite podporuje za běhu zřizování, který je ve výchozím nastavení povolené.
Neexistuje žádná položka akce pro vás v této části. Pokud uživatel v Netsuite ještě neexistuje, je vytvořen nový pokusíte-li tooaccess Netsuite.


### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooNetsuite toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooNetsuite, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Netsuite**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

tootest jeden přihlašování nastavení, otevřete hello přístupovému panelu na adrese [https://myapps.microsoft.com](https://myapps.microsoft.com/), přihlaste se k hello testovací účet a klikněte na tlačítko **Netsuite**.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfiguraci zřizování uživatelů](active-directory-saas-netsuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_203.png

