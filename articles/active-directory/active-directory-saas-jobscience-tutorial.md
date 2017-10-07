---
title: 'Kurz: Azure Active Directory integrace s Jobscience | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Jobscience."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 77282dcc-bbe2-4728-953d-adb4ab6a713b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 4a4c78aad6d324795a15a9569542afc23b4716d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Kurz: Azure Active Directory integrace s Jobscience

V tomto kurzu zjistíte, jak toointegrate Jobscience s Azure Active Directory (Azure AD).

Integrace Jobscience s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooJobscience
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooJobscience (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Jobscience tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Jobscience jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Jobscience z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-jobscience-from-hello-gallery"></a>Přidání Jobscience z Galerie hello
tooconfigure hello integrace Jobscience do Azure AD, je nutné tooadd Jobscience hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Jobscience z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Jobscience**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_search.png)

5. Na panelu výsledků hello vyberte **Jobscience**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Jobscience podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Jobscience je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Jobscience musí toobe navázat.

V Jobscience, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Jobscience, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Jobscience](#creating-a-jobscience-test-user)**  -toohave protějšek Britta Simon v Jobscience, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Jobscience.

**tooconfigure Azure AD jednotné přihlašování s Jobscience, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Jobscience** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_samlbase.png)

3. Na hello **Jobscience domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_url.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`http://<company name>.my.salesforce.com`
    
    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování. Tato hodnota získat [tým podpory Jobscience klienta](https://www.jobscience.com/support) nebo z hello jednotného přihlašování k profilu bude vytvořena, což je vysvětleno později v kurzu hello. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_general_400.png)

6. Na hello **Jobscience konfigurace** klikněte na tlačítko **konfigurace Jobscience** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_configure.png) 

7. Přihlaste se tooyour Jobscience společnosti lokality jako správce.

8. Přejděte příliš**instalační program**.
   
   ![Instalační program](./media/active-directory-saas-jobscience-tutorial/IC784358.png "instalační program")

9. V levém navigačním podokně hello v hello **Správa** klikněte na tlačítko **Správa domén** tooexpand hello související části a pak klikněte na **Moje domény** tooopen hello  **Moje doména** stránky. 
   
   ![Moje doména](./media/active-directory-saas-jobscience-tutorial/ic767825.png "Moje doména")

10. tooverify, který vaše doména byla nastavena správně, ujistěte se, že je v "**krok 4 nasazené tooUsers**" a zkontrolovat vaše "**Moje nastavení domény**".

    ![Domény nasazen tooUser](./media/active-directory-saas-jobscience-tutorial/ic784377.png "tooUser nasazené v doméně")

11. Na webu společnosti Jobscience hello, klikněte na tlačítko **ovládacích prvků zabezpečení**a potom klikněte na **nastavení jednotného přihlašování**.
    
    ![Ovládací prvky zabezpečení](./media/active-directory-saas-jobscience-tutorial/ic784364.png "kontrolních mechanismů pro zabezpečení")

12. V hello **nastavení jednotného přihlašování** část, proveďte následující kroky hello:
    
    ![Jednotné přihlašování v nastavení](./media/active-directory-saas-jobscience-tutorial/ic781026.png "jednotné přihlašování v nastavení")
    
    a. Vyberte **povoleno SAML**.

    b. Klikněte na možnost **Nové**.

13. Na hello **SAML jeden přihlašování nastavení upravit** dialogové okno, proveďte následující kroky hello:
    
    ![SAML jeden přihlašování nastavení](./media/active-directory-saas-jobscience-tutorial/ic784365.png "SAML jeden přihlašování nastavení")
    
    a. V hello **název** textovému poli, zadejte název pro svou konfiguraci.

    b. V **vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z portálu Azure.

    c. V hello **Entity Id** textovému poli, typu`https://salesforce-jobscience.com`

    d. Klikněte na tlačítko **Procházet** tooupload váš certifikát služby Azure AD.

    e. Jako **typ Identity SAML**, vyberte **kontrolní výraz obsahuje hello federace z objektu uživatele hello**.

    f. Jako **umístění Identity SAML**, vyberte **identita je v elementu NameIdentfier hello hello subjektu příkaz**.

    g. V **adresu URL pro přihlášení zprostředkovatele Identity** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.

    h. V **adresa URL odhlašovací zprostředkovatele Identity** textovému poli, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.

    i. Klikněte na **Uložit**.

14. V levém navigačním podokně hello v hello **Správa** klikněte na tlačítko **Správa domén** tooexpand hello související části a pak klikněte na **Moje domény** tooopen hello  **Moje doména** stránky. 
    
    ![Moje doména](./media/active-directory-saas-jobscience-tutorial/ic767825.png "Moje doména")

15. Na hello **Moje domény** stránku hello **Branding přihlašovací stránky** klikněte na tlačítko **upravit**.
    
    ![Branding přihlašovací stránky](./media/active-directory-saas-jobscience-tutorial/ic767826.png "Branding přihlašovací stránky")

16. Na hello **Branding přihlašovací stránky** stránku hello **ověřovací služby** část, hello název vaší **nastavení jednotného přihlašování SAML** se zobrazí. Vyberte ji a pak klikněte na tlačítko **Uložit**.
    
    ![Branding přihlašovací stránky](./media/active-directory-saas-jobscience-tutorial/ic784366.png "Branding přihlašovací stránky")

17. tooget hello SP zahájí jednotného přihlašování na adresu URL pro přihlášení, klikněte na hello **nastavení jednotného přihlašování** v hello **ovládacích prvků zabezpečení** části nabídky.

    ![Ovládací prvky zabezpečení](./media/active-directory-saas-jobscience-tutorial/ic784368.png "kontrolních mechanismů pro zabezpečení")
    
    Klikněte na tlačítko hello jednotného přihlašování k profilu, který jste vytvořili v předchozí krok text hello. Tato stránka zobrazuje hello jednotného přihlašování na adrese URL vaší společnosti (například [https://companyname.my.salesforce.com?so=companyid](https://companyname.my.salesforce.com?so=companyid).    

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-jobscience-test-user"></a>Vytvoření zkušebního uživatele Jobscience

V pořadí tooenable Azure AD Uživatelé toolog v tooJobscience musí být zřízená do Jobscience. V případě hello Jobscience zřizování je ruční úloha.

>[!NOTE]
>Můžete použít všechny ostatní Jobscience uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Jobscience tooprovision Azure Active Directory uživatelské účty.
>  
        
**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**

1. Přihlaste se tooyour **Jobscience** společnosti lokality jako správce.

2. Přejděte tooSetup.
   
   ![Instalační program](./media/active-directory-saas-jobscience-tutorial/ic784358.png "instalační program")
3. Přejděte příliš**spravovat uživatele \> uživatelé**.
   
   ![Uživatelé](./media/active-directory-saas-jobscience-tutorial/ic784369.png "uživatelů")
4. Klikněte na tlačítko **nového uživatele**.
   
   ![Všichni uživatelé](./media/active-directory-saas-jobscience-tutorial/ic784370.png "všichni uživatelé")
5. Na hello **upravit uživatele** dialogové okno, proveďte následující kroky hello:
   
   ![Úprava uživatele](./media/active-directory-saas-jobscience-tutorial/ic784371.png "Úprava uživatele")
   
   a. V hello **křestní jméno** textovému poli, zadejte jméno uživatele hello jako Britta.
   
   b. V hello **příjmení** textovému poli, zadejte příjmení uživatele hello jako Simon.
   
   c. V hello **Alias** textovému poli, zadejte název aliasu hello uživatele jako brittas.

   d. V hello **e-mailu** jako typ hello e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.

   e. V hello **uživatelské jméno** textové pole, zadejte uživatelské jméno uživatele jako Brittasimon@contoso.com.

   f. V hello **Přezdívka** textovému poli, zadejte název nick uživatele jako Simon.

   g. Klikněte na **Uložit**.

    
> [!NOTE]
> Držitel účtu Azure Active Directory Hello obdrží e-mailu a dodržuje odkaz tooconfirm svého účtu před stane aktivní.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooJobscience toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooJobscience, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Jobscience**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Jobscience hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Jobscience aplikace.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_203.png

