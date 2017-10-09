---
title: 'Kurz: Azure Active Directory integrace s ServiceNow | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ServiceNow a ServiceNow Express."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 1d8eb99e-8ce5-4ba4-8b54-5c3d9ae573f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: df6a07dd1aa437198fbdb9d0a04ea14f3a320249
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Kurz: Azure Active Directory integrace s ServiceNow
V tomto kurzu zjistíte, jak toointegrate ServiceNow a ServiceNow Express s Azure Active Directory (Azure AD).

Integrace s Azure AD ServiceNow a ServiceNow Express poskytuje hello následující výhody:

* Můžete řídit v Azure AD, který má přístup tooServiceNow a ServiceNow Express
* Můžete povolit uživatelům tooautomatically get přihlášeného tooServiceNow a ServiceNow Express (jednotné přihlášení) s jejich účty Azure AD
* Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
tooconfigure integrace Azure AD s ServiceNow a ServiceNow Express, je třeba hello následující položky:

* Předplatné služby Azure AD
* Pro ServiceNow, instanci nebo klienta ServiceNow Calgary verze nebo vyšší
* Pro instance ServiceNow Express, Helsinkách verze ServiceNow Express nebo vyšší
* Hello ServiceNow klienta musí mít hello [více jeden znak na modul Plugin poskytovatele](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) povolena. To lze provést [odesílá se žádost o službu](https://hi.service-now.com). 

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.
> 
> 

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

* Provozním prostředí byste neměli používat, pokud je to nutné.
* Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání ServiceNow z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování pro ServiceNow a ServiceNow Express

## <a name="adding-servicenow-from-hello-gallery"></a>Přidání ServiceNow z Galerie hello
tooconfigure hello integrace ServiceNow a ServiceNow Express do Azure AD, je nutné tooadd ServiceNow hello Galerie tooyour seznamu spravovaných aplikací SaaS. 

**tooadd ServiceNow z Galerie hello, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**. 
   
    ![Active Directory][1]
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Aplikace][2]
4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
    ![Aplikace][3]
5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
    ![Aplikace][4]
6. Hello vyhledávacího pole zadejte **ServiceNow**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)
7. V podokně výsledků hello, vyberte **ServiceNow**a potom klikněte na **Complete** tooadd hello aplikace.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s ServiceNow a ServiceNow Express podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v ServiceNow je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v ServiceNow musí toobe navázat.
Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v ServiceNow. tooconfigure a testu Azure AD jednotné přihlašování s ServiceNow, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování pro ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Konfigurace Azure AD jednotné přihlašování pro expresní ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow-express)**  -tooenable toouse vaši uživatelé tuto funkci.
3. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytvoření zkušebního uživatele ServiceNow](#creating-a-servicenow-test-user)**  -toohave protějšek Britta Simon v ServiceNow, která je jí reprezentace toohello propojené služby Azure AD.
5. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
6. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

> [!NOTE]
> Pokud chcete, aby tooconfigure ServiceNow vynechte krok 2. Podobně pokud chcete, aby tooconfigure ServiceNow Express vynechte krok 1.
> 
> 

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>Konfigurace Azure AD jednotné přihlášení pro ServiceNow
1. Na portálu classic hello Azure AD na hello **ServiceNow** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno .
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749323.png "nakonfigurovat jednotné přihlašování")

2. Na hello **jak jste by například uživatelé toosign na tooServiceNow** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749324.png "nakonfigurovat jednotné přihlašování")

3. Na hello **nakonfigurovat nastavení aplikace** proveďte hello následující kroky:
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/IC769497.png "konfigurovat adresu URL aplikace")
   
    a. v hello **ServiceNow přihlašovací adresa URL** textové pole, zadejte adresu URL používá vaše aplikace ServiceNow uživatelé tooyour toosign na následující vzor hello: `https://<instance-name>.service-now.com`.
   
    b. V hello **identifikátor** textové pole, zadejte adresu URL používá vaše aplikace ServiceNow uživatelé tooyour toosign na následující vzor hello: `https://<instance-name>.service-now.com`.
   
    c. Klikněte na **Další**

4. toohave Azure AD automaticky nakonfigurovat ServiceNow pro ověřování na základě SAML, zadejte název instance ServiceNow, uživatelské jméno správce a heslo správce v hello **automaticky nakonfigurovat jednotné přihlašování** formuláři a klikněte na tlačítko  *Konfigurace*. Všimněte si, že hello správce uživatelské jméno musí mít hello **security_admin** přiřazenou v ServiceNow pro tento toowork roli. Jinak toomanually nakonfigurovat ServiceNow toouse Azure AD jako poskytovatele identity SAML, klikněte na **ručně nakonfigurovat hello aplikaci pro jednotné přihlašování**, pak klikněte na tlačítko **Další** a dokončení hello Následující kroky.
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "konfigurovat adresu URL aplikace")

5. Na hello **nakonfigurovat jednotné přihlašování v ServiceNow** klikněte na tlačítko **stažení certifikátu**, uložte soubor certifikátu hello místně na vašem počítači.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749325.png "nakonfigurovat jednotné přihlašování")

6. Přihlášení tooyour ServiceNow aplikace jako správce.

7. Aktivovat hello *integrace - více jeden přihlašování instalační program zprostředkovatele* modulu plug-in podle následující hello další kroky:
   
    a. V navigačním podokně hello na levé straně hello přejděte příliš**definice systému** části a pak klikněte na **modulů plug-in**.
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "aktivovat modulu plug-in")
   
    b. Vyhledejte *integrace - více jeden přihlašování instalační program zprostředkovatele*.
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "aktivovat modulu plug-in")
   
    c. Vyberte modul plug-in hello. Rigth klikněte a vyberte **aktivovat nebo upgradovat**.
   
    d. Klikněte na tlačítko hello **aktivovat** tlačítko.

8. V navigačním podokně hello na levé straně hello, klikněte na tlačítko **vlastnosti**.  
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "konfigurovat adresu URL aplikace")

9. Na hello **více jednotného přihlašování k vlastnosti zprostředkovatele** dialogové okno, proveďte následující kroky hello:
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "konfigurovat adresu URL aplikace")
   
    a. Jako **povolit zprostředkovatel vícenásobného jednotného přihlašování k**, vyberte **Ano**.
   
    b. Jako **povolit protokolování ladění získali hello zprostředkovatel vícenásobného jednotného přihlašování k integraci**, vyberte **Ano**.
   
    c. V **hello pole hello uživatele tabulky, který...**  textovému poli, typ **uživatelské_jméno**.
   
    d. Klikněte na **Uložit**.

10. V navigačním podokně hello na levé straně hello, klikněte na tlačítko **x509 certifikáty**.
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "nakonfigurovat jednotné přihlašování")

11. Na hello **certifikáty X.509** dialogové okno, klikněte na tlačítko **nový**.
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "nakonfigurovat jednotné přihlašování")

12. Na hello **certifikáty X.509** dialogové okno, proveďte následující kroky hello:
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "nakonfigurovat jednotné přihlašování")
    
     a. Klikněte na možnost **Nové**.
    
     b. V hello **název** textovému poli, zadejte název pro svou konfiguraci (například: **TestSAML2.0**).
    
     c. Vyberte **Active**.
    
     d. Jako **formátu**, vyberte **PEM**.
    
     e. Jako **typ**, vyberte **důvěřovat certifikátu úložiště**.
    
     f. Otevření certifikátu kódováním Base64 v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **PEM certifikát** textové pole.
    
     g. Klikněte na tlačítko **aktualizace**.

13. V navigačním podokně hello na levé straně hello, klikněte na tlačítko **zprostředkovatelů Identity**.
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "nakonfigurovat jednotné přihlašování")

14. Na hello **zprostředkovatelů Identity** dialogové okno, klikněte na tlačítko **nový**:
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "nakonfigurovat jednotné přihlašování")

15. Na hello **zprostředkovatelů Identity** dialogové okno, klikněte na tlačítko **aktualizaci1 typu SAML2?**:
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "nakonfigurovat jednotné přihlašování")

16. V dialogovém okně Vlastnosti typu SAML2 aktualizaci1 hello proveďte následující kroky hello:
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "nakonfigurovat jednotné přihlašování")

    a. v hello **název** textovému poli, zadejte název pro svou konfiguraci (například: **SAML 2.0**).

    b. V hello **pole uživatelského** textovému poli, typ **e-mailu** nebo **uživatelské_jméno**, v závislosti na tom, která pole se používá toouniquely identifikace uživatelů ve vašem nasazení ServiceNow. 

    > [!NOTE] 
    > Můžete buď ID uživatele hello Azure AD (hlavní název uživatele) tooemit configue Azure AD nebo hello e-mailovou adresu jako jedinečný identifikátor v tokenu SAML hello hello podle budete toohello **ServiceNow > atributy > jednotné přihlašování** část Hello klasický portál Azure a mapování hello požadovaného pole toohello **nameidentifier** atribut. Hodnota Hello uložené pro vybraný atribut hello ve službě Azure AD (například hlavní název uživatele) musí odpovídat hello hodnotou uloženou v ServiceNow hello zadaná pole (například uživatelské_jméno)

    c. Na portálu classic hello Azure AD, zkopírujte hello **ID zprostředkovatele Identity** hodnotu a pak ji vložit do hello **adresa URL poskytovatele Identity** textové pole.

    d. Na portálu classic hello Azure AD, zkopírujte hello **adresa URL žádosti o ověření** hodnotu a pak ji vložit do hello **AuthnRequest zprostředkovatele Identity** textové pole.

    e. Na portálu classic hello Azure AD, zkopírujte hello **jednu adresu URL služby Sign-Out** hodnotu a pak ji vložit do hello **SingleLogoutRequest zprostředkovatele Identity** textové pole.

    f. V hello **domovské stránky ServiceNow** textovému poli, typ hello URL domovské stránce instance ServiceNow.

    > [!NOTE] 
    > domovskou stránku instance ServiceNow Hello je tvořen vaše **URL klienta ServieNow** a **/navpage.do** (např:`https://fabrikam.service-now.com/navpage.do`).

    g. V hello **Entity ID / vystavitele** textovému poli, zadat adresu URL hello klienta služby ServiceNow.

    h. V hello **cílovou skupinu URL** textovému poli, zadat adresu URL hello klienta služby ServiceNow. 

    i. V hello **protokol vazby pro hello na IDP SingleLogoutRequest** textovému poli, typ **urn: oasis: názvy: tc: SAML:2.0:bindings:HTTP-přesměrování**.

    j. Hello NameID zásad textové pole, zadejte **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: neurčené**.

    kB. Zrušte výběr **vytvořit AuthnContextClass**.

    l. V hello **AuthnContextClassRef metoda**, typ `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. To je potřeba jenom Pokud jste cloudu pouze organizace. Pokud používáte místní služby AD FS nebo vícefaktorového ověřování pro ověřování pak byste neměli konfigurovat tuto hodnotu. 

    m. V **hodin zkreslit** textovému poli, typ **60**.

    n. Jako **jednoho přihlášení na skriptu**, vyberte **MultiSSO_SAML2_Update1**.

    o. Jako **x509 certifikát**vyberte hello certifikátů, které jste vytvořili v předchozím kroku hello.

    p. Klikněte na tlačítko **odeslání**. 

1. Na portálu classic hello Azure AD, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Další**. 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "nakonfigurovat jednotné přihlašování")

2. Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "nakonfigurovat jednotné přihlašování")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>Konfigurace Azure AD jednotné přihlášení pro ServiceNow Express
1. Na portálu classic hello Azure AD na hello **ServiceNow** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno .
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749323.png "nakonfigurovat jednotné přihlašování")

2. Na hello **jak jste by například uživatelé toosign na tooServiceNow** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749324.png "nakonfigurovat jednotné přihlašování")

3. Na hello **nakonfigurovat nastavení aplikace** proveďte hello následující kroky:
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/IC769497.png "konfigurovat adresu URL aplikace")
   
    a. v hello **ServiceNow přihlašovací adresa URL** textové pole, zadejte adresu URL používá vaše aplikace ServiceNow uživatelé tooyour toosign na následující vzor hello: `https://<instance-name>.service-now.com`.
   
    b. V hello **URL vystavitele** textové pole, zadejte adresu URL používá vaše aplikace ServiceNow uživatelé tooyour toosign na následující hello vzor `https://<instance-name>.service-now.com`.
   
    c. Klikněte na **Další**

4. Klikněte na tlačítko **ručně nakonfigurovat hello aplikaci pro jednotné přihlašování**, pak klikněte na tlačítko **Další** a dokončení hello následující kroky.
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "konfigurovat adresu URL aplikace")

5. Na hello **nakonfigurovat jednotné přihlašování v ServiceNow** klikněte na tlačítko **stažení certifikátu**, uložit soubor certifikátu hello místně na vašem počítači a pak klikněte na tlačítko **Další**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC749325.png "nakonfigurovat jednotné přihlašování")

6. Přihlášení tooyour aplikace ServiceNow Express jako správce.

7. V navigačním podokně hello na levé straně hello, klikněte na tlačítko **jednotné přihlašování**.  
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "konfigurovat adresu URL aplikace")

8. Na hello **jednotné přihlašování** dialogové okno, klikněte na ikonu konfigurace hello na hello horní, pravé a nastavte hello následující vlastnosti:
   
    ![Konfigurovat adresu URL aplikace](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "konfigurovat adresu URL aplikace")
   
    a. Přepnutí **povolit zprostředkovatel vícenásobného jednotného přihlašování k** toohello vpravo.
   
    b. Přepnutí **ladění povolení protokolování pro hello zprostředkovatel vícenásobného jednotného přihlašování k integraci** toohello vpravo.
   
    c. V **hello pole hello uživatele tabulky, který...**  textovému poli, typ **uživatelské_jméno**.
9. Na hello **jednotné přihlašování** dialogové okno, klikněte na tlačítko **přidat nový certifikát**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "nakonfigurovat jednotné přihlašování")
10. Na hello **certifikáty X.509** dialogové okno, proveďte následující kroky hello:
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "nakonfigurovat jednotné přihlašování")
    
    a. V hello **název** textovému poli, zadejte název pro svou konfiguraci (například: **TestSAML2.0**).
    
    b. Vyberte **Active**.
    
    c. Jako **formátu**, vyberte **PEM**.
    
    d. Jako **typ**, vyberte **důvěřovat certifikátu úložiště**.
    
    e. Vytvořte soubor kódováním Base64 z stažený certifikát.
    
    > [!NOTE]
    > Další podrobnosti najdete v tématu [jak tooconvert binární certifikátů do textového souboru](http://youtu.be/PlgrzUZ-Y1o).
    > 
    > 
    
    f. Otevření certifikátu kódováním Base64 v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **PEM certifikát** textové pole.
    
    g. Klikněte na tlačítko **aktualizace**.
11. Na hello **jednotné přihlašování** dialogové okno, klikněte na tlačítko **přidat nové rozšíření IdP**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "nakonfigurovat jednotné přihlašování")
12. Na hello **přidat nového poskytovatele Identity** dialogové okno, v části **konfigurace zprostředkovatele Identity**, proveďte následující kroky hello:
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "nakonfigurovat jednotné přihlašování")

    a. v hello **název** textovému poli, zadejte název pro svou konfiguraci (například: **SAML 2.0**).

    b. Na portálu classic hello Azure AD, zkopírujte hello **ID zprostředkovatele Identity** hodnotu a pak ji vložit do hello **adresa URL poskytovatele Identity** textové pole.

    c. Na portálu classic hello Azure AD, zkopírujte hello **adresa URL žádosti o ověření** hodnotu a pak ji vložit do hello **AuthnRequest zprostředkovatele Identity** textové pole.

    d. Na portálu classic hello Azure AD, zkopírujte hello **jednu adresu URL služby Sign-Out** hodnotu a pak ji vložit do hello **SingleLogoutRequest zprostředkovatele Identity** textové pole.

    e. Jako **certifikát zprostředkovatele Identity**vyberte hello certifikátů, které jste vytvořili v předchozím kroku hello.


1. Klikněte na tlačítko **Upřesnit nastavení**a v části **další vlastnosti zprostředkovatele Identity**, proveďte následující kroky hello:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "nakonfigurovat jednotné přihlašování")
   
    a. V hello **protokol vazby pro hello na IDP SingleLogoutRequest** textovému poli, typ **urn: oasis: názvy: tc: SAML:2.0:bindings:HTTP-přesměrování**.
   
    b. V hello **NameID zásad** textovému poli, typ **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: neurčené**.    
   
    c. V hello **AuthnContextClassRef metoda**, typ **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.
   
    d. Zrušte výběr **vytvořit AuthnContextClass**.

2. V části **další vlastnosti zprostředkovatele služby**, proveďte následující kroky hello:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "nakonfigurovat jednotné přihlašování")
   
    a. V hello **domovské stránky ServiceNow** textovému poli, typ hello URL domovské stránce instance ServiceNow.
   
    > [!NOTE]
    > domovskou stránku instance ServiceNow Hello je tvořen vaše **URL klienta ServieNow** a **/navpage.do** (např: `https://fabrikam.service-now.com/navpage.do`).
    > 
    > 
   
    b. V hello **Entity ID / vystavitele** textovému poli, zadat adresu URL hello klienta služby ServiceNow.
   
    c. V hello **identifikátor URI cílové skupiny** textovému poli, zadat adresu URL hello klienta služby ServiceNow. 
   
    d. V **hodin zkreslit** textovému poli, typ **60**.
   
    e. V hello **pole uživatelského** textovému poli, typ **e-mailu** nebo **uživatelské_jméno**, v závislosti na tom, která pole se používá toouniquely identifikace uživatelů ve vašem nasazení ServiceNow.
   
    > [!NOTE]
    > Můžete buď ID uživatele hello Azure AD (hlavní název uživatele) tooemit configue Azure AD nebo hello e-mailovou adresu jako jedinečný identifikátor v tokenu SAML hello hello podle budete toohello **ServiceNow > atributy > jednotné přihlašování** část Hello klasický portál Azure a mapování hello požadovaného pole toohello **nameidentifier** atribut. Hodnota Hello uložené pro vybraný atribut hello ve službě Azure AD (například hlavní název uživatele) musí odpovídat hello hodnotou uloženou v ServiceNow hello zadaná pole (například uživatelské_jméno)
    > 
    > 
   
    f. Klikněte na **Uložit**. 

3. Na portálu classic hello Azure AD, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Další**. 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "nakonfigurovat jednotné přihlašování")

4. Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "nakonfigurovat jednotné přihlašování")

## <a name="configuring-user-provisioning"></a>Konfiguraci zřizování uživatelů
Hello cílem této části je toooutline jak tooenable zřizování uživatelů služby Active Directory uživatele tooServiceNow účty.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure zřizování uživatelů, proveďte následující kroky hello:
1. V hello Azure classic portálu pro správu, na hello **ServiceNow** stránky integrace aplikací, klikněte na tlačítko **konfiguraci zřizování uživatelů**. 
   
    ![Zřizování uživatelů](./media/active-directory-saas-servicenow-tutorial/IC769498.png "zřizování uživatelů")

2. Na hello **zadejte vaše zřizování automatické uživatelů ServiceNow pověření tooenable** zadejte hello následující nastavení:
   
     a. V hello **název Instance ServiceNow** textovému poli, název instance ServiceNow hello typu.
   
     b. V hello **uživatelské jméno správce ServiceNow** textovému poli, název typu hello hello ServiceNow účet správce.
   
     c. V hello **heslo správce ServiceNow** textovému poli, zadejte hello heslo pro tento účet.
   
     d. Klikněte na tlačítko **ověření** tooverify konfiguraci.
   
     e. Klikněte na tlačítko hello **Další** tlačítko tooopen hello **další kroky** stránky.
   
     f. Pokud chcete tooprovision aplikace toothis všechny uživatele, vyberte možnost "**automaticky zřizovat všechny uživatelské účty v aplikaci toothis directory hello**". 
   
    ![Další kroky](./media/active-directory-saas-servicenow-tutorial/IC698804.png "další kroky")
   
     g. Na hello **další kroky** klikněte na tlačítko **Complete** toosave konfiguraci.

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
V této části vytvoříte na portálu classic hello názvem Britta Simon testovacího uživatele.

![Vytvořit uživatele Azure AD][20]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.

3. Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 
   
    a. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.
   
    b. V hello uživatelské jméno **textbox**, typ **BrittaSimon**.
   
    c. Klikněte na **Další**.

6. Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 
   
   a. V hello **křestní jméno** textovému poli, typ **Britta**.  
   
   b. V hello **příjmení** textovému poli, typ, **Simon**.
   
   c. V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.
   
   d. V hello **Role** seznamu, vyberte **uživatele**.
   
   e. Klikněte na **Další**.

7. Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 
   
    a. Poznamenejte si hodnotu hello hello **nové heslo**.
   
    b. Klikněte na **Dokončit**.   

### <a name="creating-a-servicenow-test-user"></a>Vytvoření zkušebního uživatele ServiceNow
V této části vytvoříte uživatele volal Britta Simon v ServiceNow. V této části vytvoříte uživatele volal Britta Simon v ServiceNow. Pokud nevíte jak tooadd uživatele ve vaší ServiceNow nebo ServiceNow Express účet, kontaktujte tým podpory ServiceNow.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele
V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooServiceNow svůj přístup.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooServiceNow, proveďte následující kroky hello:**

1. Na portálu classic hello, klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **ServiceNow**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

3. V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.
   
    ![Přiřadit uživatele][203] 

4. V seznamu hello všechny uživatele, vyberte **Britta Simon**.

5. V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.
   
    ![Přiřadit uživatele][205]

### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování
Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.

Když kliknete na dlaždici ServiceNow hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour ServiceNow aplikace.

## <a name="additional-resources"></a>Další zdroje
* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
