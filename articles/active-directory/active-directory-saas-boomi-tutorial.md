---
title: 'Kurz: Azure Active Directory integrace s Boomi | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Boomi."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8e05afa9-2eda-4975-a0cc-6d408065860f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: ce64a4561697d311a8c7b1b244315bb552c5cfb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a>Kurz: Azure Active Directory integrace s Boomi

V tomto kurzu zjistíte, jak toointegrate Boomi s Azure Active Directory (Azure AD).

Integrace Boomi s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooBoomi
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooBoomi (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Boomi tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Boomi jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Boomi z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-boomi-from-hello-gallery"></a>Přidání Boomi z Galerie hello
tooconfigure hello integrace Boomi do Azure AD, je nutné tooadd Boomi hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Boomi z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Boomi**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_search.png)

5. Na panelu výsledků hello vyberte **Boomi**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Boomi podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Boomi je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Boomi musí toobe navázat.

V Boomi, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Boomi, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Boomi](#creating-a-boomi-test-user)**  -toohave protějšek Britta Simon v Boomi, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Boomi.

**tooconfigure Azure AD jednotné přihlašování s Boomi, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Boomi** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_samlbase.png)

3. Na hello **Boomi domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_url.png)

    a. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://platform.boomi.com/sso/<accountname>/saml`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://platform.boomi.com/sso/<accountname>/saml`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL. Obraťte se na [tým podpory Boomi](https://boomi.com/company/contact/) tooget tyto hodnoty.

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_certificate.png)

4. Aplikace Boomi očekává hello SAML kontrolní výrazy ve specifickém formátu. Nakonfigurujte hello následující deklarace identity pro tuto aplikaci. Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace. Hello následující snímek obrazovky ukazuje příklad pro tento.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_attribute.png)

5. V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogu pro každý řádek v tabulce hello níže, proveďte hello následující kroky:

    | Název atributu | Hodnota atributu |
    | -------------- | --------------- |
    | FEDERATION_ID | User.Mail |
    
    a. Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_05.png)
    
    b. V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.
    
    c. Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.
    
    d. Klikněte na tlačítko **OK**.

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_general_400.png)

7. Na hello **Boomi konfigurace** klikněte na tlačítko **konfigurace Boomi** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_configure.png) 

8. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Boomi. 

9. Přejděte příliš**název společnosti** a přejděte příliš**nastavit**.

10. Klikněte na tlačítko hello **možnosti jednotného přihlašování k** kartě a proveďte následující kroky.

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_11.png)

    a. Zkontrolujte **povolit SAML jednotné přihlašování** zaškrtávací políčko.

    b. Klikněte na tlačítko **Import** tooupload hello stáhnout certifikát z Azure AD příliš**certifikát zprostředkovatele Identity**.
    
    c. V hello **adresu URL pro přihlášení zprostředkovatele Identity** textovému poli, vložte hodnotu hello z **SAML jeden přihlašování adresa URL služby** z okna konfigurace aplikace Azure AD.

    d. Jako **Federation Id umístění**, vyberte **federace je v elementu FEDERATION_ID atribut** přepínač. 

    e. Klikněte na tlačítko **Uložit** tlačítko.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-boomi-test-user"></a>Vytvoření zkušebního uživatele Boomi

V pořadí tooenable Azure AD Uživatelé toolog v tooBoomi musí být zřízená do Boomi. V případě hello Boomi zřizování je ruční úloha.

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a>tooprovision uživatelský účet, proveďte následující kroky hello:

1. Přihlaste se tooyour Boomi společnosti lokality jako správce.

2. Po přihlášení přejděte příliš**Správa uživatelů** a přejděte příliš**uživatelé**.

    ![Uživatelé](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "uživatelů")

3. Klikněte na tlačítko  **+**  ikonu a hello **role uživatele přidat/zachování** otevře se dialogové okno.

    ![Uživatelé](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "uživatelů")

    ![Uživatelé](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "uživatelů")

    a. V hello **e-mailová adresa uživatele** jako typ hello e-mailu uživatele k textovému poli, BrittaSimon@contoso.com.
    
    b. V hello **křestní jméno** textovému poli, typ hello křestní jméno uživatele jako Britta.

    c. V hello **příjmení** textovému poli, typ hello příjmení uživatele jako Simon.
    
    d. Zadejte uživatele hello **federace**. Každý uživatel musí mít ID federace, která jednoznačně identifikuje uživatele hello v rámci účtu hello.
    
    e. Přiřadit hello **standardní uživatel** toohello uživatelské role. Vzhledem k tomu, který by mu udělit normální prostředí přístup, jakož i přístup přihlašování není přiřadit role správce hello.
    
    f. Klikněte na **OK**.
    
    > [!NOTE]
    > Hello uživatel neobdrží úvodní oznamovací e-mail obsahující heslo, které můžou být použité toolog v toohello AtomSphere účet, protože heslo je spravován pomocí zprostředkovatele identity hello. Může použít jakékoli jiné Boomi uživatele účtu vytvoření nástroje nebo rozhraní API poskytované Boomi tooprovision AAD uživatelské účty. 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooBoomi toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooBoomi, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Boomi**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Boomi hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Boomi aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_203.png

