---
title: 'Kurz: Azure Active Directory integrace s xMatters OnDemand | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a xMatters OnDemand."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 7cc8f9f0d8cefc8a60b9514027437ced50c66242
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Kurz: Azure Active Directory integrace s xMatters OnDemand

V tomto kurzu zjistíte, jak toointegrate xMatters OnDemand službou Azure Active Directory (Azure AD).

Integrace xMatters OnDemand s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooxMatters OnDemand
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooxMatters OnDemand (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s xMatters OnDemand, je třeba hello následující položky:

- Předplatné služby Azure AD
- Předplatné povolené xMatters OnDemand jednotné přihlašování

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání xMatters OnDemand z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-xmatters-ondemand-from-hello-gallery"></a>Přidání xMatters OnDemand z Galerie hello
tooconfigure hello integrace xMatters OnDemand do Azure AD, je nutné tooadd xMatters OnDemand hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd xMatters OnDemand z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **xMatters OnDemand**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_search.png)

5. Na panelu výsledků hello vyberte **xMatters OnDemand**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části konfiguraci a testování Azure AD jednotné přihlašování s xMatters OnDemand založené na testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování, musí Azure AD tooknow jaké hello příslušného uživatele v xMatters OnDemand je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v xMatters OnDemand musí toobe navázat.

V xMatters OnDemand přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s xMatters OnDemand, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele OnDemand xMatters](#creating-a-xmatters-ondemand-test-user)**  -toohave protějšek Britta Simon v xMatters OnDemand, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší xMatters OnDemand aplikace.

**tooconfigure Azure AD jednotné přihlašování s xMatters OnDemand, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **xMatters OnDemand** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_samlbase.png)

3. Na hello **xMatters OnDemand domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_url.png)
    
    a. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:   
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL. Obraťte se na [tým podpory xMatters OnDemand](https://www.xmatters.com/company/contact-us/) tooget tyto hodnoty.

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte hello soubor certifikátu místně jako **c:\\XMatters OnDemand.cer**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_certificate.png)
    
    > [!IMPORTANT]
    > Je třeba tooforward hello certifikát toohello [tým podpory xMatters OnDemand](https://www.xmatters.com/company/contact-us/). certifikát Hello musí toobe odeslaný tým podpory xMatters hello předtím, než můžete dokončit hello jeden přihlašování konfigurace. 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_400.png)

6. Na hello **xMatters OnDemand konfigurace** klikněte na tlačítko **konfigurace xMatters OnDemand** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se tooyour XMatters OnDemand společnosti lokality jako správce.

8. V panelu nástrojů hello hello nahoře, klikněte na **správce**a potom klikněte na **podrobnosti o společnosti** hello navigačním panelu na levé straně hello.
   
    ![Správce](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "správce")

9. Na hello **konfigurace SAML** proveďte hello následující kroky:
   
    ![Konfigurace SAML](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "konfigurace SAML")
   
    a. Vyberte **povolit SAML**.
   
    b. Vložení **SAML Entity ID**, který jste zkopírovali z hello portálu Azure do hello **ID zprostředkovatele Identity** textové pole.
   
    c. Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z hello portálu Azure do hello **jeden přihlašovací adresa URL** textové pole.
   
    d. Vložení **Sign-Out URL**, který jste zkopírovali z hello portálu Azure do hello **jednu adresu URL odhlašovací** textové pole.
   
    e. Na stránce Podrobnosti o společnosti hello hello horní, klikněte na tlačítko **uložit změny**.
    
    ![Podrobnosti o společnosti](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "společnosti podrobnosti")

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-xmatters-ondemand-test-user"></a>Vytvoření zkušebního uživatele OnDemand xMatters

V pořadí tooenable Azure AD Uživatelé toolog v tooXMatters OnDemand musí být zřízená do XMatters OnDemand. V případě hello XMatters OnDemand zřizování je ruční úloha.

### <a name="tooprovision-a-user-accounts-perform-hello-following-steps"></a>tooprovision uživatelské účty, provádět hello následující kroky:
1. Přihlaste se tooyour **XMatters OnDemand** klienta.

2.  Klikněte na tlačítko **uživatelé** kartu a pak klikněte na tlačítko **přidat uživatele**.
  
    ![Uživatelé](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "uživatelů")

3. V hello **přidat uživatele** část, proveďte následující kroky hello:
   
    ![Přidat uživatele](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "přidat uživatele")

    a. Vyberte **Active**.

    b. V hello **ID uživatele** textovému poli, typ hello id uživatele jako Brittasimon@contoso.com.
   
    c. V hello **křestní jméno** textovému poli, typ křestní jméno uživatele hello jako Britta.

    d. V hello **příjmení** textovému poli, zadejte příjmení uživatele hello jako Simon aplikace.
    
    e. V hello **lokality** textovému poli, zadejte platnou lokalitou hello platný Azure AD účet chcete tooprovision.
    
    f. Klikněte na **Uložit**.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooxMatters OnDemand Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign tooxMatters Britta Simon OnDemand, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **xMatters OnDemand**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello xMatters OnDemand dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour xMatters OnDemand aplikace.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_203.png

