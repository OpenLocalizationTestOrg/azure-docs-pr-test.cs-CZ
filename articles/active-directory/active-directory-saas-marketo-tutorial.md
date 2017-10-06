---
title: 'Kurz: Azure Active Directory integrace s Marketo | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Marketo."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 87f88cde4f027f99a83c1ab3b318247bb4d658ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a>Kurz: Azure Active Directory integrace s Marketo

V tomto kurzu zjistíte, jak toointegrate Marketo s Azure Active Directory (Azure AD).

Integrace služby Marketo s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooMarketo
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooMarketo (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Marketo, je třeba hello následující položky:

- Předplatné služby Azure AD
- Marketo jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Marketo z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-marketo-from-hello-gallery"></a>Přidání Marketo z Galerie hello
tooconfigure hello integrace Marketo do Azure AD, je nutné tooadd Marketo hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Marketo z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Marketo**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_search.png)

5. Na panelu výsledků hello vyberte **Marketo**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Marketo podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Marketo je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Marketo musí toobe navázat.

V Marketo, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Marketo, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Marketo](#creating-a-marketo-test-user)**  -toohave protějšek Britta Simon v Marketo, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Marketo.

**tooconfigure Azure AD jednotné přihlašování pomocí služby Marketo, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Marketo** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_samlbase.png)

3. Na hello **Marketo domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_url.png)

    a. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://saml.marketo.com/sp`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://login.marketo.com/saml/assertion/\<munchkinid\>`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL. Obraťte se na [tým podpory Marketo](http://investors.marketo.com/contactus.cfm) tooget tyto hodnoty.
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_general_400.png)

6. Na hello **Marketo konfigurace** klikněte na tlačítko **konfigurace Marketo** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_configure.png) 

7. tooget Munchkin Id aplikace, přihlaste se pomocí přihlašovacích údajů správce tooMarketo a proveďte následující akce:
   
    a. Přihlaste se pomocí přihlašovacích údajů správce aplikace tooMarketo.
   
    b. Klikněte na tlačítko hello **správce** tlačítko v horním navigačním podokně hello.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Přejděte toohello integrace nabídky a klikněte na tlačítko hello **Munchkin odkaz**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_11.png)
   
    d. Zkopírujte hello Munchkin Id zobrazí na úvodní obrazovka a dokončit vaše adresa URL odpovědi v Průvodci konfigurací hello Azure AD.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_12.png) 

8. tooconfigure hello jednotné přihlašování v aplikaci hello, postupujte podle hello následujících kroků:
   
    a. Přihlaste se pomocí přihlašovacích údajů správce aplikace tooMarketo.
   
    b. Klikněte na tlačítko hello **správce** tlačítko v horním navigačním podokně hello.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Přejděte toohello integrace nabídky a klikněte na tlačítko **jednotné přihlašování**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_07.png) 
   
    d. Klikněte na tlačítko tooenable hello SAML nastavení **upravit** tlačítko.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_08.png) 
   
    e. **Povolit** nastavení jednotného přihlašování.
   
    f. Vložení hello **SAML Entity ID**, v hello **ID vystavitele** textové pole.
   
    g. V hello **Entity ID** textovému poli, zadejte adresu URL hello jako `http://saml.marketo.com/sp`.
   
    h. Vyberte hello umístění ID uživatele jako **název identifikátor elementu**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > Pokud není uživatelský identifikátor UPN hodnota a hodnota hello změnit na kartě atribut hello.
   
    i. Nahrajte hello certifikát, který jste si stáhli z Průvodce konfigurací služby Azure AD. **Uložit** hello nastavení.
   
    j. Upravte nastavení přesměrování stránky hello.
   
    kB. Vložení hello **SAML jeden přihlašování adresa URL služby** v hello **přihlašovací adresa URL** textové pole.
   
    l. Vložení hello **Sign-Out URL** v hello **adresy URL odhlašovací** textové pole.
   
    m. V hello **chyba URL**, kopie vašeho **adresu URL instance Marketo** a klikněte na tlačítko **Uložit** tlačítko toosave nastavení.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_10.png)

9. tooenable hello jednotného přihlašování pro uživatele, dokončení hello následující akce:
   
    a. Přihlaste se pomocí přihlašovacích údajů správce aplikace tooMarketo.
   
    b. Klikněte na tlačítko hello **správce** tlačítko v horním navigačním podokně hello.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Přejděte toohello **zabezpečení** nabídky a klikněte na tlačítko **nastavení přihlášení**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_13.png)
   
    d. Zkontrolujte hello **vyžadovat jednotné přihlašování** možnost a **Uložit** hello nastavení.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_14.png)

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-marketo-test-user"></a>Vytvoření zkušebního uživatele Marketo

V této části vytvoříte uživatele volal Britta Simon v Marketo. postupujte podle těchto kroků toocreate uživatele v Marketo platformy.

1. Přihlaste se pomocí přihlašovacích údajů správce aplikace tooMarketo.

2. Klikněte na tlačítko hello **správce** tlačítko v horním navigačním podokně hello.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 

3. Přejděte toohello **zabezpečení** nabídky a klikněte na tlačítko **uživatelé a role**
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_19.png)  

4. Klikněte na tlačítko hello **pozvat nového uživatele** odkaz na kartě Uživatelé hello
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_15.png) 

5. V následující informace o aktualizaci hello pozvat nového uživatele Průvodce výplně hello
   
    a. Zadejte uživatele hello **e-mailu** adresu v textovém poli hello
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_16.png)
   
    b. Zadejte hello **křestní jméno** v textovém poli hello
   
    c. Zadejte hello **příjmení** v textovém poli hello
   
    d. Klikněte na **Další**

6. V hello **oprávnění** karty, vyberte hello **userRoles** a klikněte na tlačítko **další**
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_17.png)
7. Klikněte na tlačítko hello **odeslat** tlačítko toosend hello uživatele Pozvánka
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_18.png)

8. Uživatel obdrží e-mailové oznámení hello a má tooclick hello propojení a změňte účet hello tooactivate hello heslo. 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooMarketo toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooMarketo, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Marketo**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Marketo hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Marketo aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_203.png

