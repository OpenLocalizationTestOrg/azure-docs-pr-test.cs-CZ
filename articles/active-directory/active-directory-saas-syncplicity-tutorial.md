---
title: 'Kurz: Azure Active Directory integrace s Syncplicity | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Syncplicity."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 6148112a959232ed24d76d1c7b8773f06568fee7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a>Kurz: Azure Active Directory integrace s Syncplicity

V tomto kurzu zjistíte, jak toointegrate Syncplicity s Azure Active Directory (Azure AD).

Integrace Syncplicity s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooSyncplicity
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSyncplicity (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Syncplicity tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Syncplicity jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Syncplicity z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-syncplicity-from-hello-gallery"></a>Přidání Syncplicity z Galerie hello
tooconfigure hello integrace Syncplicity do Azure AD, je nutné tooadd Syncplicity hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Syncplicity z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Syncplicity**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_search.png)

5. Na panelu výsledků hello vyberte **Syncplicity**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Syncplicity podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Syncplicity je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Syncplicity musí toobe navázat.

V Syncplicity, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Syncplicity, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Syncplicity](#creating-a-syncplicity-test-user)**  -toohave protějšek Britta Simon v Syncplicity, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Syncplicity.

**tooconfigure Azure AD jednotné přihlašování s Syncplicity, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Syncplicity** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_samlbase.png)

3. Na hello **Syncplicity domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.syncplicity.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.syncplicity.com/sp`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory Syncplicity klienta](https://www.syncplicity.com/contact-us) tooget tyto hodnoty. 
 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_certificate.png) 

  
5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_general_400.png)

6. Na hello **Syncplicity konfigurace** klikněte na tlačítko **konfigurace Syncplicity** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_configure.png) 

7. Přihlaste se tooyour **Syncplicity** klienta.

8. V nabídce hello hello nahoře, klikněte na tlačítko **správce**, vyberte **nastavení**a potom klikněte na **vlastní domény a jednotné přihlašování**.
   
    ![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")

9. Na hello **jednotné přihlašování (SSO)** dialogové okno proveďte hello následující kroky:
   
    ![Jednotné přihlašování \(jednotného přihlašování\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")   

    a. V hello **vlastní domény** textovému poli, typ hello název vaší domény.
  
    b. Vyberte **povoleno** jako **jednotné přihlašování stav**.

    c. V hello **Entity Id** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.

    d. V hello **přihlašovací adresa URL stránky** textovému poli, vložte hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.

    e. V hello **adresa URL odhlašovací stránky** textovému poli, vložte hello **Sign-Out URL** který jste zkopírovali z portálu Azure.

    f. V **certifikát zprostředkovatele Identity**, klikněte na tlačítko **zvolte soubor**a pak nahrajte hello certifikát, který jste si stáhli z portálu Azure hello. 

    g. Klikněte na tlačítko **uložit změny**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-syncplicity-test-user"></a>Vytvoření zkušebního uživatele Syncplicity
Pro AAD uživatelé toobe možné toosign v musí být zřízená tooSyncplicity aplikace. Tato část popisuje, jak toocreate AAD uživatelských účtů v Syncplicity.

**tooprovision tooSyncplicity účet uživatele, proveďte následující kroky hello:**

1. Přihlaste se tooyour **Syncplicity** klienta (například: `https://company.Syncplicity.com`).

2. Klikněte na tlačítko **správce** a vyberte **uživatelské účty**.

3. Klikněte na tlačítko **přidat uživatele**.
   
    ![Správa uživatelů](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "Správa uživatelů")

4. Typ hello **e-mailových adres** účtu AAD, tooprovision, vyberte **uživatele** jako **Role**a potom klikněte na **Další**.
   
    ![Informace o účtu](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "účet informace")
   
    >[!NOTE]
    >Držitel účtu AAD Hello získá včetně tooconfirm odkaz e-mailu a aktivaci hello účtu. 
    > 

5. Vyberte skupinu ve vaší společnosti, nový uživatel by měl stane členem, a pak klikněte na **Další**.
   
    ![Členství ve skupinách](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "členství ve skupinách")
   
    >[!NOTE]
    >Pokud nebyly nalezeny žádné skupiny uvedeny, klikněte na tlačítko **Další**. 
    > 

6. Vyberte složky hello chcete vytvořit tooplace pod kontrolou společnosti Syncplicity počítače hello uživatele a pak klikněte na tlačítko **Další**.
   
    ![Složky Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity složek")

>[!NOTE]
>Můžete použít všechny ostatní Syncplicity uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Syncplicity tooprovision AAD uživatelské účty. 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooSyncplicity toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooSyncplicity, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Syncplicity**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.

Když kliknete na dlaždici Syncplicity hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Syncplicity aplikace.
## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_203.png

