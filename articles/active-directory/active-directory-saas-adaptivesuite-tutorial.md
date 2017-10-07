---
title: "Kurz: Azure Active Directory integrace s adaptivní Suite | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a adaptivní Suite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: af309c27ab74098c1e229c80adb11c96dc2774fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Kurz: Azure Active Directory integrace s adaptivní Suite

V tomto kurzu zjistíte, jak toointegrate adaptivní Suite a Azure Active Directory (Azure AD).

Integrace adaptivní Suite s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooAdaptive Suite
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAdaptive Suite (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s adaptivní Suite, je třeba hello následující položky:

- Předplatné služby Azure AD
- Adaptivní Suite jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání adaptivní Suite z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-adaptive-suite-from-hello-gallery"></a>Přidání adaptivní Suite z Galerie hello
tooconfigure hello integraci sady adaptivní do služby Azure AD, je nutné tooadd adaptivní Suite hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd adaptivní Suite z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **adaptivní Suite**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_search.png)

5. Na panelu výsledků hello vyberte **adaptivní Suite**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s adaptivní Suite podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v adaptivní Suite je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v sadě adaptivní musí toobe navázat.

V adaptivní Suite přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s adaptivní Suite, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele adaptivní Suite](#creating-an-adaptive-suite-test-user)**  -toohave protějšek Britta Simon adaptivní sady, který je propojený toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci adaptivní Suite.

**tooconfigure Azure AD jednotné přihlašování s adaptivní Suite, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **adaptivní Suite** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_samlbase.png)

3. Na hello **adaptivní Suite domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    >[!NOTE]
    > Tuto hodnotu můžete získat z hello adaptivní Suite je **nastavení jednotného přihlašování SAML** stránky.
    >  

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_400.png)

6. Na hello **adaptivní Suite konfigurace** klikněte na tlačítko **konfigurace adaptivní Suite** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti tooyour adaptivní Suite.

8. Přejděte příliš**správce**.
   
    ![Správce](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "správce")

9. V hello **uživatelů a rolí** klikněte na tlačítko **spravovat nastavení jednotného přihlašování SAML**.
   
    ![Spravovat nastavení jednotného přihlašování SAML](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "spravovat nastavení jednotného přihlašování SAML")

10. Na hello **nastavení jednotného přihlašování SAML** proveďte hello následující kroky:
   
    ![Nastavení jednotného přihlašování SAML](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "nastavení jednotného přihlašování SAML")

    a. V hello **název zprostředkovatele Identity** textovému poli, zadejte název pro svou konfiguraci.
    
    b. Vložení hello **SAML Entity ID** hodnota zkopírována z portálu Azure do hello **zprostředkovatele Identity Entity ID** textové pole.
  
    c. Vložení hello **SAML jeden přihlašování adresa URL služby** hodnota zkopírována z portálu Azure do hello **zprostředkovatele Identity jednotného přihlašování k adrese URL** textové pole.
  
    d. Vložení hello **SAML jeden přihlašování adresa URL služby** hodnota zkopírována z portálu Azure do hello **vlastní adresa URL odhlašovací** textové pole.
  
    e. tooupload stažený certifikát, klikněte na tlačítko **zvolte soubor**.
  
    f. Vyberte pro hello následující:
    * **Id uživatele SAML**, vyberte **adaptivní Statistika uživatelského jména**.
    * **Umístění id uživatele SAML**, vyberte **id uživatele v NameID předmětu**.
    * **Formát SAML NameID**, vyberte **e-mailová adresa**.
    * **Povolit SAML**, vyberte **povolit jednotné přihlašování SAML a přímé přihlášení adaptivní Insights**.
    
    g. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-adaptive-suite-test-user"></a>Vytváření testovacího uživatele adaptivní Suite

Uživatelé toolog tooenable Azure AD v tooAdaptive Suite, se musí být zřízená do adaptivní Suite.  

* V případě hello adaptivní sady zřizování je ruční úloha.

**tooconfigure zřizování uživatelů, proveďte následující kroky hello:** 

1. Přihlaste se tooyour **adaptivní Suite** společnosti lokality jako správce.
2. Přejděte příliš**správce**.
   
   ![Správce](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "správce")
3. V hello **uživatelů a rolí** klikněte na tlačítko **přidat uživatele**.
   
   ![Přidat uživatele](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "přidat uživatele")
4. V hello **nového uživatele** část, proveďte následující kroky hello:
   
   ![Odeslání](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "odeslání")   

   a. Typ hello **název**, **přihlášení**, **e-mailu**, **heslo** platného uživatele Azure Active Directory, kterou chcete tooprovision do hello související textová pole.
  
   b. Vyberte **Role**.
  
   c. Klikněte na tlačítko **odeslání**.

>[!NOTE]
>Můžete použít všechny ostatní adaptivní Suite uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované adaptivní Suite tooprovision AAD uživatelské účty.
>  

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooAdaptive Suite Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooAdaptive Suite, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **adaptivní Suite**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest vaší Microsoft Azure AD Single Sign-On konfigurace pomocí hello přístupového panelu.

Když kliknete na dlaždici adaptivní Suite hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour adaptivní sada aplikací.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_203.png

