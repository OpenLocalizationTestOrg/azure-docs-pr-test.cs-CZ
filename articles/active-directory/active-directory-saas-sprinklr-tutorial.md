---
title: 'Kurz: Azure Active Directory integrace s Sprinklr | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Sprinklr."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 14b467c72d4a453ed7ad248eadcdade710f105af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Kurz: Azure Active Directory integrace s Sprinklr

V tomto kurzu zjistíte, jak toointegrate Sprinklr s Azure Active Directory (Azure AD).

Integrace Sprinklr s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooSprinklr
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSprinklr (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Sprinklr tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Sprinklr jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Sprinklr z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-sprinklr-from-hello-gallery"></a>Přidání Sprinklr z Galerie hello
tooconfigure hello integrace Sprinklr do Azure AD, je nutné tooadd Sprinklr hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Sprinklr z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Sprinklr**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_search.png)

5. Na panelu výsledků hello vyberte **Sprinklr**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Sprinklr podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Sprinklr je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Sprinklr musí toobe navázat.

V Sprinklr, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Sprinklr, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Sprinklr](#creating-a-sprinklr-test-user)**  -toohave protějšek Britta Simon v Sprinklr, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Sprinklr.

**tooconfigure Azure AD jednotné přihlašování s Sprinklr, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Sprinklr** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_samlbase.png)

3. Na hello **Sprinklr domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.sprinklr.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.sprinklr.com`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizujte hodnotu hello s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory Sprinklr klienta](https://www.sprinklr.com/contact-us/) tooget tyto hodnoty. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/tutorial_general_400.png)

6. Na hello **Sprinklr konfigurace** klikněte na tlačítko **konfigurace Sprinklr** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

7. V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti Sprinklr tooyour.

8. Přejděte příliš**správy \> nastavení**.
   
    ![Správa](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "správy")

9. Přejděte příliš**spravovat partnera \> jednotné přihlašování** v levém podokně hello.
   
    ![Správa partnera](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "spravovat partnera")

10. Klikněte na tlačítko **+ jednotné přihlašování, doplňky**.
   
    ![Jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "jednotného přihlašování")

11. Na hello **jednotného přihlašování** proveďte hello následující kroky:
   
    ![Jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "jednotného přihlašování")

    a. V hello **název** textovému poli, zadejte název pro svou konfiguraci (například: *WAADSSOTest*).

    b. Vyberte **povoleno**.

    c. Vyberte **používat nový certifikát jednotného přihlašování k**.
             
    e. Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát zprostředkovatele Identity** textové pole.

    f. Vložení hello **SAML Entity ID** hodnotu, která jste zkopírovali z portálu Azure do hello **Entity Id** textové pole.

    g. Vložení hello **SAML jeden přihlašování adresa URL služby** hodnotu, která jste zkopírovali z portálu Azure do hello **adresu URL pro přihlášení zprostředkovatele Identity** textové pole.

    h. Vložení hello **Sign-Out URL** hodnotu, která jste zkopírovali z portálu Azure do hello **adresa URL odhlašovací zprostředkovatele Identity** textové pole.
     
    i. Jako **typ ID uživatele SAML**, vyberte **kontrolní výraz obsahuje uživatele "uživatelské jméno s sprinklr.com**.

    j. Jako **umístění ID uživatele SAML**, vyberte **ID uživatele je v elementu identifikátor jména hello hello subjektu příkaz**.

    kB. Klikněte na **Uložit**.
       
    ![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-sprinklr-test-user"></a>Vytvoření zkušebního uživatele Sprinklr

1. Přihlaste se tooyour Sprinklr společnosti lokality jako správce.

2. Přejděte příliš**správy \> nastavení**.
   
    ![Správa](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "správy")

3. Přejděte příliš**spravovat klienta \> uživatelé** v levém podokně hello.
   
    ![Nastavení](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "nastavení")

4. Klikněte na tlačítko **přidat uživatele**.
   
    ![Nastavení](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "nastavení")

5. Na hello **upravit uživatele** dialogové okno, proveďte následující kroky hello:
   
    ![Úprava uživatele](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "úpravy uživatele") 

    a. V hello **e-mailu**, **křestní jméno** a **příjmení** textových polí, informace o typu hello chcete tooprovision účtu uživatele Azure AD.

    b. Vyberte **heslo zakázáno**.

    c. Vyberte **jazyk**.

    d. Vyberte **typ uživatele**.

    e. Klikněte na tlačítko **aktualizace**.
   
     >[!IMPORTANT]
     >**Heslo zakázáno** musí být vybraný tooenable toolog uživatele v pomocí zprostředkovatele Identity. 
     
6. Přejděte příliš**Role**a poté proveďte hello následující kroky:
   
    ![Partner role](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "Partner role")

    a. Z hello **globální** seznamu, vyberte **všechny\_oprávnění**.  

    b. Klikněte na tlačítko **aktualizace**.

>[!NOTE]
>Můžete použít všechny ostatní Sprinklr uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Sprinklr tooprovision uživatelské účty Azure AD. 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooSprinklr toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooSprinklr, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Sprinklr**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello Sprinklr dlaždice v hello přístupového panelu, můžete získat aplikaci Sprinklr automaticky přihlášeného tooyour Další informace o hello přístupového panelu, najdete v části [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_203.png

