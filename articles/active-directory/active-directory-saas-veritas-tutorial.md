---
title: "Kurz: Integrace Azure Active Directory této společnosti Enterprise Vault.cloud jednotného přihlašování k | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a jednotné přihlašování Vault.cloud Enterprise této společnosti."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 1037e70515686091460ac41e9e5a7951639bb520
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veritas-enterprise-vaultcloud-sso"></a>Kurz: Integrace Azure Active Directory pomocí této společnosti Enterprise Vault.cloud jednotného přihlašování

V tomto kurzu zjistíte, jak toointegrate této společnosti Enterprise Vault.cloud jednotné přihlašování s Azure Active Directory (Azure AD).

Integrace této společnosti Enterprise Vault.cloud jednotné přihlašování s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooVeritas Enterprise Vault.cloud SSO
- Vaši uživatelé tooautomatically get přihlášeného tooVeritas Enterprise Vault.cloud SSO (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD pomocí jednotného přihlašování Vault.cloud Enterprise této společnosti, potřebujete hello následující položky:

- Předplatné služby Azure AD
- Této společnosti Enterprise Vault.cloud jednotného přihlašování k jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání této společnosti Enterprise Vault.cloud jednotného přihlašování z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-veritas-enterprise-vaultcloud-sso-from-hello-gallery"></a>Přidání této společnosti Enterprise Vault.cloud jednotného přihlašování z Galerie hello
tooconfigure hello integrace této společnosti Enterprise Vault.cloud přihlašování do služby Azure AD, je nutné tooadd této společnosti Enterprise Vault.cloud SSO hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd této společnosti Enterprise Vault.cloud jednotného přihlašování z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **této společnosti Enterprise Vault.cloud jednotného přihlašování k**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_search.png)

5. Na panelu výsledků hello vyberte **této společnosti Enterprise Vault.cloud jednotného přihlašování k**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí této společnosti Enterprise Vault.cloud jednotného přihlašování na základě testovací uživatele, nazývá "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v této společnosti Enterprise Vault.cloud jednotné přihlašování je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v této společnosti Enterprise Vault.cloud jednotné přihlašování musí toobe navázat.

V této společnosti Enterprise Vault.cloud jednotné přihlašování, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testování Azure AD jednotné přihlašování pomocí jednotného přihlašování Vault.cloud Enterprise této společnosti, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele této společnosti Enterprise Vault.cloud jednotného přihlašování k](#creating-a-veritas-enterprise-vaultcloud-sso-test-user)**  -toohave protějšek Britta Simon SSO Vault.cloud Enterprise této společnosti, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v této společnosti Enterprise Vault.cloud jednotného přihlašování k aplikaci.

**tooconfigure Azure AD jednotné přihlašování pomocí jednotného přihlašování Vault.cloud Enterprise této společnosti, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **této společnosti Enterprise Vault.cloud jednotného přihlašování k** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_samlbase.png)

3. Na hello **této společnosti Enterprise Vault.cloud jednotného přihlašování k doméně a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_url.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`
    
    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory této společnosti Enterprise Vault.cloud jednotné přihlašování klienta](https://www.veritas.com/support/.html) tooget tuto hodnotu. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_general_400.png)

6. Na hello **konfiguraci této společnosti Enterprise Vault.cloud jednotného přihlašování k** klikněte na tlačítko **konfigurace této společnosti Enterprise Vault.cloud SSO** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_configure.png) 

7. tooconfigure jednotného přihlašování na **této společnosti Enterprise Vault.cloud jednotného přihlašování k** straně, je nutné stáhnout hello toosend **Certificate(Base64)** a **SAML jeden přihlašování adresa URL služby**příliš[tým podpory této společnosti Enterprise Vault.cloud jednotného přihlašování k](https://www.veritas.com/support/.html).

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-veritas-enterprise-vaultcloud-sso-test-user"></a>Vytvoření zkušebního uživatele této společnosti Enterprise Vault.cloud jednotného přihlašování

V této části vytvoříte uživatele volat Britta Simon SSO Vault.cloud Enterprise. Práce s [tým podpory této společnosti Enterprise Vault.cloud jednotného přihlašování k](https://www.veritas.com/support/.html) pro přidání uživatelů hello hello Enterprise Vault.cloud jednotného přihlašování k platformě. Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooVeritas Enterprise Vault.cloud jednotného přihlašování k toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooVeritas Enterprise Vault.cloud jednotné přihlašování, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **této společnosti Enterprise Vault.cloud jednotného přihlašování k**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici hello této společnosti Enterprise Vault.cloud jednotné přihlašování v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour této společnosti Enterprise Vault.cloud jednotného přihlašování k aplikaci.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_203.png

