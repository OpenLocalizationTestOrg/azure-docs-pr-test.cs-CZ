---
title: 'Kurz: Azure Active Directory integrace s HR2day podle Merces | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a HR2day podle Merces."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 853d08c9-27b1-48d4-b8e7-3705140eb67f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: 257233767e1fddbaf115878cb0455acf61b2f5f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a>Kurz: Azure Active Directory integrace s HR2day podle Merces

V tomto kurzu zjistíte, jak toointegrate HR2day podle Merces službou Azure Active Directory (Azure AD).

Integrace HR2day podle Merces s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooHR2day podle Merces.
- Můžete povolit uživatelům tooautomatically získat podepsaný v tooHR2day Merces s účty služby Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě – hello portálu Azure.

Další informace o integraci aplikací SaaS v Azure AD najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s HR2day podle Merces tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD.
- HR2day podle Merces jednotné přihlašování povolené předplatné.

> [!NOTE]
> Není doporučeno, že produkční prostředí tootest hello pomocí kroků v tomto kurzu.

tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Získání [jeden měsíc bezplatnou zkušební verzi Azure AD](https://azure.microsoft.com/pricing/free-trial/) Pokud ještě nemáte ho.  

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénář, který je zde uvedených zahrnuje dva hlavní stavební bloky:

1. Přidání HR2day podle Merces z Galerie hello.
2. Konfigurace a testování Azure AD jednotného přihlašování.

## <a name="add-hr2day-by-merces-from-hello-gallery"></a>Přidat HR2day podle Merces z Galerie hello
tooconfigure hello integrace HR2day podle Merces do Azure AD, přidejte HR2day podle Merces hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd HR2day podle Merces z Galerie hello hello proveďte následující kroky:**

1. V hello [portál Azure](https://portal.azure.com), na hello levém navigačním podokně, vyberte hello **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, vyberte hello **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **HR2day podle Merces**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_search.png)

5. Na panelu výsledků hello vyberte **HR2day podle Merces**a potom vyberte hello **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s HR2day podle Merces podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování musí Azure AD tooknow, který je hello příslušného uživatele v HR2day podle Merces tooa uživatele ve službě Azure AD. Jinými slovy budete potřebovat tooestablish odkaz mezi uživatele Azure AD a související uživatelské hello v HR2day podle Merces.

V HR2day podle Merces přiřadit hello **uživatelské jméno** ve službě Azure AD příliš **uživatelské jméno** tooestablish hello vztah.

tooconfigure a testu Azure AD jednotné přihlašování s HR2day podle Merces, potřebujete následující stavební bloky hello toocomplete:

1. [Konfigurovat Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on): Povolit toouse vaši uživatelé tuto funkci.
2. [Vytvořit testovací uživatele Azure AD](#creating-an-azure-ad-test-user): Test Azure AD jednotné přihlašování s Britta Simon.
3. [Vytvoření HR2day uživatelem Merces testovací](#creating-an-hr2day-by-merces-test-user): vytvoření protějšek Britta Simon v HR2day podle Merces, která je propojená toohello Azure AD reprezentace uživatele.
4. [Přiřadit hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user): Povolit Britta Simon toouse Azure AD jednotné přihlašování.
5. [Test jednotného přihlašování](#testing-single-sign-on): Ověřte, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaše HR2day Merces aplikací.

**tooconfigure Azure AD jednotné přihlašování s HR2day podle Merces, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **HR2day podle Merces** stránky integrace aplikací, vyberte **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. tooenable jednotné přihlašování, v hello **jednotného přihlašování** dialogové okno, vyberte **režimu** jako **na základě SAML přihlašování**.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_samlbase.png)

3. V hello **HR2day Merces domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_url.png)

    a. V hello **přihlašovací adresa URL** pole, zadejte adresu URL pomocí hello následující vzor: `https://<tenantname>.force.com/<instancename>`.

    b. V hello **identifikátor** pole, zadejte adresu URL pomocí hello následující vzor: `https://hr2day.force.com/<companyname>`.

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte hello skutečné přihlašovací adresa URL a identifikátor. Kontaktujte hello [HR2day tým podpory klienta Merces](mailto:servicedesk@merces.nl) tooget tyto hodnoty. 
 


4. Na hello **SAML podpisový certifikát** vyberte **Certificate(Base64)**a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_certificate.png) 

5. Tato část popisuje, jak tooenable tooHR2day uživatelé tooauthenticate podle Merces ke svému účtu ve službě Azure AD. Budou to provedete pomocí federace, která je založená na hello protokolu SAML.

    Vaše HR2day aplikací Merces očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, abyste tooadd vlastních atributů mapování tooyour SAML token. Hello následující snímek obrazovky ukazuje příklad tohoto objektu. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png)
    
    > [!NOTE] 
    Před konfigurací hello kontrolního výrazu SAML, bude nutné se obrátit hello [HR2day tým podpory Merces klienta](mailto:servicedesk@merces.nl) a požadovat hello hodnotu atributu hello jedinečný identifikátor pro vašeho klienta. Je nutné tuto hodnotu toocomplete hello kroků v další části hello. 

6. V hello **jednotného přihlašování** dialogové okno, v hello **uživatelské atributy** atribut tokenu SAML hello nakonfigurujte, jak ukazuje následující obrázek hello. Proveďte následující kroky hello.
    
      | Název atributu    |   Hodnota atributu |  
    | ------------------- | -------------------- |    
    | ATTR_LOGINCLAIM | spojení ([pošty] "102938475Z", "@" |
    
      a. tooopen hello **přidat atribut** dialogovém okně, vyberte **přidat atribut**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_05.png)

    b. V hello **název** zadejte **ATTR_LOGINCLAIM**.

    c. Z hello **hodnotu** seznamu, vyberte **Join()**.

    d. Z hello **řetězec1** seznamu, vyberte **user.mail**.

    e. Pro **řetězec2**, zadejte hello jedinečný identifikátor, který zajišťuje HR2day týmu.

    f. V hello **oddělovače** zadejte  **@** .
    
    g. Vyberte **Ok**.

7. Vyberte hello **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_general_400.png)

8. V hello **HR2day Merces konfigurace** vyberte **konfigurace HR2day podle Merces** tooopen hello **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out URL**, **SAML Entity ID**, a **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka** části.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_configure.png) 

9. tooconfigure jednotné přihlašování pro vaše aplikace, kontaktujte hello [HR2day tým podpory klienta Merces](mailTo:servicedesk@merces.nl). Připojte hello Stáhnout **Certificate(Base64)** souboru tooyour e-mailu. Také poskytují hello **Sign-Out URL**, **SAML Entity ID**, a **SAML jeden přihlašování adresa URL služby** tak, aby mohly být konfigurovány pro integraci jednotné přihlašování.

    > [!NOTE]
    >Zmínili toohello Merces týmu, který potřebuje této integrace hello Entity ID toobe nastavit pomocí vzoru hello **https://hr2day.force.com/INSTANCENAME**.

    > [!TIP]
    >Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory** > **podnikové aplikace, které** části, vyberte hello **jednotné přihlašování** kartě. Pak vloží hello přístup k dokumentaci prostřednictvím hello **konfigurace** části dolnímu hello. Další informace o funkci hello embedded dokumentace v hello [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, hello proveďte následující kroky:**

1. V hello **portál Azure**, na hello levém navigačním podokně, vyberte hello **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom vyberte **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, vyberte **přidat** hello nahoře dialogového okna hello.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. V hello **uživatele** dialogové okno, hello proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla**a zapište si ji hello heslo.

    d. Vyberte **Vytvořit**.
 
### <a name="create-an-hr2day-by-merces-test-user"></a>Vytvoření HR2day Merces testovací uživatel

Hello cílem této části je toocreate volá Britta Simon v HR2day Merces uživatele. Uživatelé hello tooadd v účtu hello HR2day pracovat s hello [HR2day tým podpory klienta Merces](mailto:servicedesk@merces.nl). 

> [!NOTE]
> Pokud potřebujete toocreate uživatel ručně, obraťte se na hello [HR2day tým podpory klienta Merces](mailto:servicedesk@merces.nl).

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte tooHR2day svůj přístup podle Merces toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooHR2day podle Merces hello proveďte následující kroky:**

1. V hello Azure aplikace hello portálu, otevřete zobrazení, přejděte toohello directory zobrazení a potom přejděte příliš**podnikové aplikace, které**. Potom vyberte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **HR2day podle Merces**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_app.png) 

3. V nabídce hello na levé straně hello vyberte **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Vyberte hello **přidat** tlačítko. Potom v hello **přidat přiřazení** dialogové okno, vyberte **uživatelů a skupin**.

    ![Přiřadit uživatele][203]

5. V hello **uživatelů a skupin** dialogové okno, v hello **uživatelé** seznamu, vyberte **Britta Simon**.

6. Klikněte na tlačítko hello **vyberte** tlačítko.

7. V hello **přidat přiřazení** dialogové okno, vyberte **přiřadit**.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

Hello cílem této části je tootest vaše konfigurace Azure AD jeden přihlašování pomocí hello přístupového panelu.  

Když vyberete hello HR2day podle Merces dlaždice v hello přístupového panelu, můžete získat přihlášeni automaticky tooyour HR2day Merces aplikací.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů o tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png

