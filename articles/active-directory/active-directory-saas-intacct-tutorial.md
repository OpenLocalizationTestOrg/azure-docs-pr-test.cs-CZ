---
title: 'Kurz: Azure Active Directory integrace s Intacct | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Intacct."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 3500039615166c2f61fb408d85bb82dfaefba134
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a>Kurz: Azure Active Directory integrace s Intacct

V tomto kurzu zjistíte, jak toointegrate Intacct s Azure Active Directory (Azure AD).

Integrace Intacct s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooIntacct
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooIntacct (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Intacct tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Intacct jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Intacct z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-intacct-from-hello-gallery"></a>Přidání Intacct z Galerie hello
tooconfigure hello integrace Intacct do Azure AD, je nutné tooadd Intacct hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Intacct z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Intacct**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_search.png)

5. Na panelu výsledků hello vyberte **Intacct**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Intacct podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Intacct je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Intacct musí toobe navázat.

V Intacct, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Intacct, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele Intacct](#creating-an-intacct-test-user)**  -toohave protějšek Britta Simon v Intacct, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Intacct.

**tooconfigure Azure AD jednotné přihlašování s Intacct, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Intacct** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_samlbase.png)

3. Na hello **Intacct domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_url.png)

    V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte tuto hodnotu s hello skutečná adresa URL odpovědi. Obraťte se na [tým podpory Intacct](https://us.intacct.com/support) tooget tuto hodnotu.

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_general_400.png)

6. Na hello **Intacct konfigurace** klikněte na tlačítko **konfigurace Intacct** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_configure.png) 

7. V okně prohlížeče jiný web přihlaste jako správce tooyour Intacct společnosti lokality.

8. Klikněte na tlačítko hello **společnosti** a pak klikněte **informace společnosti**.

    ![Společnosti](./media/active-directory-saas-intacct-tutorial/ic790037.png "společnosti")

9. Klikněte na tlačítko hello **zabezpečení** a pak klikněte **upravit**.

    ![Zabezpečení](./media/active-directory-saas-intacct-tutorial/ic790038.png "zabezpečení")

10. V hello **jednotné přihlašování (SSO)** část, proveďte následující kroky hello:

    ![Jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/ic790039.png "jednotné přihlašování")

    a. Vyberte **povolení jednotného přihlašování na**.

    b. Jako **typ zprostředkovatele Identity**, vyberte **SAML 2.0**.

    c. V **URL vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.
   
    d. V **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.

    e. Otevřete váš **kódování base-64** kódování certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát** pole.
   
    f. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-intacct-test-user"></a>Vytváření testovacího uživatele Intacct

tooset přidávání uživatelů Azure AD, se můžete přihlásit tooIntacct, se musí být zřízená do Intacct. Pro Intacct zřizování je ruční úloha.

**tooprovision uživatelské účty, proveďte následující kroky hello:**

1. Přihlaste se tooyour **Intacct** klienta.

2. Klikněte na tlačítko hello **společnosti** a pak klikněte **uživatelé**.

    ![Uživatelé](./media/active-directory-saas-intacct-tutorial/ic790041.png "uživatelů")
3. Klikněte na tlačítko hello **přidat** kartě.

    ![Přidat](./media/active-directory-saas-intacct-tutorial/ic790042.png "přidat")
4. V hello **informace o uživateli** část, proveďte následující kroky hello:

    ![Informace o uživateli](./media/active-directory-saas-intacct-tutorial/ic790043.png "informace o uživateli")

    a. Zadejte hello **ID uživatele**, hello **příjmení**, **křestní jméno**, hello **e-mailová adresa**, hello **název**, a hello **Phone** účtu Azure AD, které chcete do hello tooprovision **informace o uživateli** části.

    b. Vyberte hello **oprávnění správce** účtu Azure AD, které chcete tooprovision.
   
    c. Klikněte na **Uložit**. Držitel účtu Azure AD Hello obdrží e-mailu a dodržuje odkaz tooconfirm svého účtu před stane aktivní.

>[!NOTE]
>tooprovision Azure AD uživatelské účty, můžete použít jiné nástroje pro tvorbu Intacct uživatelského účtu nebo rozhraní API, které jsou poskytovány Intacct.
        
### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooIntacct toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooIntacct, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Intacct**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části otestovat vaše konfigurace Azure AD jeden přihlašování pomocí hello přístupového panelu.

Když kliknete na dlaždici Intacct hello v hello přístupového panelu, by měl být automaticky přihlášeni tooyour Intacct aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_203.png

