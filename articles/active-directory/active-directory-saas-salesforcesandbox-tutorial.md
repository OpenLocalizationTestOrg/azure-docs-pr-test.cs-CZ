---
title: "Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a izolovaného prostoru služby Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 7539f08356568a17ebfcee2764bbbefa129b0553
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce

V tomto kurzu zjistíte, jak toointegrate Salesforce izolovaného prostoru se službou Azure Active Directory (Azure AD).

Integrace služby Salesforce izolovaného prostoru s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooSalesforce izolovaného prostoru
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSalesforce izolovaného prostoru (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Salesforce izolovaného prostoru, je třeba hello následující položky:

- Předplatné služby Azure AD
- Izolovaného prostoru Salesforce jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání izolovaného prostoru Salesforce z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-salesforce-sandbox-from-hello-gallery"></a>Přidání izolovaného prostoru Salesforce z Galerie hello
tooconfigure hello integrace služby Salesforce izolovaného prostoru do Azure AD, je nutné tooadd izolovaného prostoru Salesforce hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd izolovaného prostoru Salesforce z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **izolovaného prostoru Salesforce**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_search.png)

5. Na panelu výsledků hello vyberte **izolovaného prostoru Salesforce**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurujete a testu Azure AD jednotné přihlašování s izolovaného prostoru Salesforce podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow uživatele hello protějškem v izolovaném prostoru Salesforce je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v izolovaném prostoru Salesforce musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v izolovaném prostoru služby Salesforce.

tooconfigure a testu Azure AD jednotné přihlašování s Salesforce izolovaného prostoru, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele izolovaného prostoru Salesforce](#creating-a-salesforce-sandbox-test-user)**  -toohave protějšek Britta Simon v Salesforce izolovaného prostoru, který je propojený toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Salesforce izolovaného prostoru.

**tooconfigure Azure AD jednotné přihlašování s Salesforce izolovaného prostoru, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **izolovaného prostoru Salesforce** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. Na hello **Salesforce izolovaného prostoru domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_url.png)

     V hello **přihlašovací adresa URL** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://<subdomain>.my.salesforce.com`

    > [!NOTE] 
    > Tato hodnota není skutečné hello. Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory služby Salesforce izolovaného prostoru klienta](https://help.salesforce.com/support) tooget tuto hodnotu.


4. Pokud jste již nakonfigurovali jednotné přihlašování pro jiná instance Salesforce izolovaného prostoru v adresáři, pak je také potřeba nakonfigurovat hello **identifikátor** toohave hello stejnou hodnotu jako hello **přihlásit na adrese URL**. 
    
    >[!Note]
    >Hello **identifikátor** pole naleznete kontrolou hello **zobrazit upřesňující nastavení** zaškrtávací políčko je na hello **konfigurace adresy URL aplikace** stránku hello dialogového okna 


5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_400.png)

7. Na hello **Salesforce izolovaného prostoru konfigurace** klikněte na tlačítko **konfigurace izolovaného prostoru Salesforce** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS>
8. V prohlížeči otevřete novou kartu a přihlaste se tooyour účet správce izolovaného prostoru služby Salesforce.

9. V nabídce hello hello nahoře, klikněte na tlačítko **instalační program**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781024.png)
10. V navigačním podokně hello na levé straně hello, klikněte na tlačítko **ovládacích prvků zabezpečení**a potom klikněte na **nastavení jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781025.png)
11. V hello část nastavení jednotného přihlašování, proveďte následující kroky hello: ![nakonfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)
     
     a.  Vyberte **povoleno SAML**. 

     b.  Klikněte na možnost **Nové**.

12. V hello oddílu SAML jeden přihlašování nastavení proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781027.png)

    textového pole Název hello a.In, název typu hello hello konfigurace (např: *SPSSOWAAD\_Test*). 

    b. Vložení **SMAL Entity ID** hodnotu do hello **vystavitele** textové pole.

    c. V hello **Entity Id** textovému poli, typ **https://test.salesforce.com** Pokud je první instance Salesforce izolovaného prostoru, které přidáváte tooyour directory hello. Pokud jste již přidali instance Salesforce karantény, pak pro hello **Entity ID** typu v hello **přihlašovací adresa URL**, což by mělo být v tomto formátu:`http://company.my.salesforce.com`  
 
    d. Klikněte na tlačítko **Procházet** tooupload hello stáhnout certifikát.  

    e. Jako **typ Identity SAML**, vyberte **kontrolní výraz obsahuje hello federace z objektu uživatele hello**.
 
    f. Jako **umístění Identity SAML**, vyberte **identita je v elementu NameIdentifier hello hello subjektu příkaz**.

    g. Vložení **jeden přihlašování adresa URL služby** do hello **adresu URL pro přihlášení zprostředkovatele Identity** textové pole. 

    h. SFDC nepodporuje SAML odhlášení.  Jako alternativní řešení, vložte 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' do hello **adresa URL odhlašovací zprostředkovatele Identity** textové pole.

    i. Jako **zprostředkovatele iniciované žádosti vazby služby**, vyberte **HTTP POST**. 

    j. Klikněte na **Uložit**.

### <a name="enable-your-domain"></a>Povolit doménu
V této části se předpokládá, že jste již vytvořili domény.  Další informace najdete v tématu [definování váš název domény](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**tooenable doménu, proveďte následující kroky hello:**

1. V levém navigačním podokně hello, klikněte na **Správa domén**a pak klikněte na tlačítko **Moje domény.**
   
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781029.png)
   
   >[!NOTE]
   >Přesvědčte se, že doménu je správně nakonfigurováno. 

2. V hello **nastavení přihlašovací stránky** klikněte na tlačítko **upravit**, pak jako **ověřovací služby**, vyberte název hello hello SAML jeden přihlašování nastavení z předchozí hello část a nakonec klikněte na tlačítko **Uložit**.
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781030.png)

Jakmile máte doménu nakonfigurován, uživatelé měli používat hello domény adresy URL toologin toohello Salesforce izolovaného prostoru.  

Hodnota hello tooget hello adresy URL, klikněte na tlačítko hello jednotného přihlašování k profilu, který jste vytvořili v předchozí části hello.    

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-salesforce-sandbox-test-user"></a>Vytvoření zkušebního uživatele izolovaného prostoru Salesforce

V této části se uživatel volá Britta Simon vytvoří v izolovaného prostoru služby Salesforce. Izolovaný prostor Salesforce podporuje zřizování za běhu, který je ve výchozím nastavení povolené.
Neexistuje žádná položka akce pro vás v této části. Pokud uživatel v izolovaném prostoru Salesforce ještě neexistuje, je vytvořen nový pokusíte-li tooaccess Salesforce izolovaného prostoru.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooSalesforce izolovaného prostoru Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign tooSalesforce Britta Simon izolovaného prostoru, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **izolovaného prostoru Salesforce**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Pokud chcete tootest nastavení jednotného přihlašování, otevřete hello přístupového panelu. Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfiguraci zřizování uživatelů](active-directory-saas-salesforce-sandbox-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_203.png

