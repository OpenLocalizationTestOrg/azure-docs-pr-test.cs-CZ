---
title: 'Kurz: Azure Active Directory integrace s SmarterU | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SmarterU."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 95fe3212-d052-4ac8-87eb-ac5305227e85
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: a3b81b557c47e31f09e61bcf75dd23f370e642e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Kurz: Azure Active Directory integrace s SmarterU

V tomto kurzu zjistíte, jak toointegrate SmarterU s Azure Active Directory (Azure AD).

Integrace SmarterU s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooSmarterU
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSmarterU (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s SmarterU tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- SmarterU jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SmarterU z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-smarteru-from-hello-gallery"></a>Přidání SmarterU z Galerie hello
tooconfigure hello integrace SmarterU do Azure AD, je nutné tooadd SmarterU hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd SmarterU z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **SmarterU**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_search.png)

5. Na panelu výsledků hello vyberte **SmarterU**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s SmarterU podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v SmarterU je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SmarterU musí toobe navázat.

V SmarterU, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s SmarterU, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele SmarterU](#creating-a-smarteru-test-user)**  -toohave protějšek Britta Simon v SmarterU, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci SmarterU.

**tooconfigure Azure AD jednotné přihlašování s SmarterU, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **SmarterU** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_samlbase.png)

3. Na hello **SmarterU domény a adresy URL** část, proveďte následující kroky hello: 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_url.png)

    V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://www.smarteru.com/`

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smarteru-tutorial/tutorial_general_400.png)

6. V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti SmarterU tooyour.

7. V panelu nástrojů hello hello nahoře, klikněte na **nastavení účtu**.
   
    ![Nastavení účtu](./media/active-directory-saas-smarteru-tutorial/IC777326.png "nastavení účtu")

8. Na stránce konfigurace účtu hello proveďte následující kroky hello:
   
    ![Externí ověřování](./media/active-directory-saas-smarteru-tutorial/IC777327.png "externí ověřování") 
 
      a. Vyberte **povolit externí autorizace**.
  
      b. V hello **hlavní přihlášení řízení** části, vyberte hello **SmarterU** kartě.
  
      c. V hello **výchozí přihlašovací jméno** části, vyberte hello **SmarterU** kartě.
  
      d. Vyberte **povolit Okta**.
  
      e. Zkopírujte obsah hello hello stažené metadata souboru a pak ji vložit do hello **Okta Metadata** textové pole.
  
      f. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-smarteru-test-user"></a>Vytvoření zkušebního uživatele SmarterU

Uživatelé toolog tooenable Azure AD v tooSmarterU, se musí být zřízená do SmarterU.

Při zřizování SmarterU, je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour **SmarterU** klienta.

2. Přejděte příliš**uživatelé**.

3. V části hello uživatele proveďte hello následující kroky:
   
    ![Nový uživatel](./media/active-directory-saas-smarteru-tutorial/IC777329.png "nového uživatele")  

    a. Klikněte na tlačítko **+ uživatele**.
    
    b. Typ hello související hodnoty atributu hello Azure AD uživatelského účtu do textových polí následující hello: **primární e-mailovou**, **ID zaměstnance**, **heslo**,  **Ověřte heslo**, **křestní jméno**, **Přezdívka**.
    
    c. Klikněte na tlačítko **Active**. 
    
    d. Klikněte na **Uložit**.

>[!NOTE]
>Můžete použít všechny ostatní SmarterU uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované SmarterU tooprovision AAD uživatelské účty.
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooSmarterU toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooSmarterU, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **SmarterU**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.
 
Když kliknete na dlaždici SmarterU hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour SmarterU aplikace.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_203.png

