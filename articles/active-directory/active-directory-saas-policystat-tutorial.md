---
title: 'Kurz: Azure Active Directory integrace s PolicyStat | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a PolicyStat."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 868053cd0d37359fd9b4aeb93dba42cbbaa09845
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a>Kurz: Azure Active Directory integrace s PolicyStat

V tomto kurzu zjistíte, jak toointegrate PolicyStat s Azure Active Directory (Azure AD).

Integrace PolicyStat s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooPolicyStat
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPolicyStat (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s PolicyStat tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- PolicyStat jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání PolicyStat z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-policystat-from-hello-gallery"></a>Přidání PolicyStat z Galerie hello
tooconfigure hello integrace PolicyStat do Azure AD, je nutné tooadd PolicyStat hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd PolicyStat z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **PolicyStat**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_search.png)

5. Na panelu výsledků hello vyberte **PolicyStat**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s PolicyStat podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v PolicyStat je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v PolicyStat musí toobe navázat.

V PolicyStat, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s PolicyStat, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele PolicyStat](#creating-a-policystat-test-user)**  -toohave protějšek Britta Simon v PolicyStat, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci PolicyStat.

**tooconfigure Azure AD jednotné přihlašování s PolicyStat, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **PolicyStat** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_samlbase.png)

3. Na hello **PolicyStat domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.policystat.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.policystat.com/saml2/metadata/`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory PolicyStat klienta](http://www.policystat.com/support/) tooget tyto hodnoty. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_certificate.png) 

5. Hello cílem této části je toooutline jak tooPolicyStat tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.

    Hello PolicyStat aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, abyste tooadd vlastních atributů mapování tooyour **atributy tokenu SAML** konfigurace.  

     Hello následující snímek obrazovky ukazuje příklad tohoto objektu.

     ![Atributy](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "atributy")

6. mapování atributů tooadd hello vyžaduje, proveďte následující kroky hello:

    | Název atributu    |   Hodnota atributu |
    |------------------- | -------------------- |
    | UID | ExtractMailPrefix([mail]) |
    
    a. Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addatribute.png)
    
    b. V hello **název atributu** textovému poli, typ **uid**.

    c. V hello **hodnota atributu** textovému poli, vyberte **ExtractMailPrefix()**.    
   
    d. Z hello **e-mailu** seznamu, vyberte **User.mail**.
    
    e. Klikněte na tlačítko **Ok**

7. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_general_400.png)

8. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti PolicyStat.

9. Klikněte na tlačítko hello **správce** a pak klikněte **konfigurace přihlášení** v levém navigačním podokně.
   
    ![Správce nabídek](./media/active-directory-saas-policystat-tutorial/ic808633.png "správce nabídek")

10. V hello **instalace** vyberte **povolit jeden integrace přihlašování**.
   
    ![Jednotné přihlašování konfigurace](./media/active-directory-saas-policystat-tutorial/ic808634.png "jednotné přihlašování v konfiguraci")

11. Klikněte na tlačítko **konfigurace atributů**a pak na hello **konfigurace atributů** část, proveďte následující kroky hello:
   
    ![Jednotné přihlašování konfigurace](./media/active-directory-saas-policystat-tutorial/ic808635.png "jednotné přihlašování v konfiguraci")
   
    a. V hello **uživatelské jméno atribut** textovému poli, typ **uid**.

    b. V hello **křestní jméno atribut** textovému poli, typ **firstname** uživatele **Britta**.

    c. V hello **poslední atribut Name** textovému poli, typ **lastname** uživatele **Simon**.

    d. V hello **atribut e-mailu** textovému poli, typ **emailaddress** uživatele  **BrittaSimon@contoso.com** .

    e. Klikněte na tlačítko **uložit změny**.

12. Klikněte na tlačítko **si Metadata IDP**a pak na hello **si Metadata IDP** část, proveďte následující kroky hello:
   
    ![Jednotné přihlašování konfigurace](./media/active-directory-saas-policystat-tutorial/ic808636.png "jednotné přihlašování v konfiguraci")
   
    a. Otevřete soubor stažený metadat, hello kopírování obsahu a pak ji vložit do hello **vaše metadat zprostředkovatelů Identity** textové pole.

    b. Klikněte na tlačítko **uložit změny**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-policystat-test-user"></a>Vytvoření zkušebního uživatele PolicyStat

V pořadí tooenable Azure AD Uživatelé toolog do PolicyStat musí být zřízená do PolicyStat.  

PolicyStat podporuje jenom při zřizování uživatelů čas. To znamená, tooadd hello uživatelů není nutné ručně tooPolicyStat. Hello uživatelé budou se přidají automaticky na jejich první přihlášení pomocí jednotného přihlašování.

>[!NOTE]
>Můžete použít všechny ostatní PolicyStat uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované PolicyStat tooprovision uživatelské účty Azure AD.
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooPolicyStat toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooPolicyStat, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **PolicyStat**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici PolicyStat hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour PolicyStat aplikace.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_203.png

