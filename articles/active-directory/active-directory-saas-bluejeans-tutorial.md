---
title: 'Kurz: Azure Active Directory integrace s BlueJeans | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a BlueJeans."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: jeedes
ms.openlocfilehash: 67613303a9f854afbf4619418cc1607d329caf94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Kurz: Azure Active Directory integrace s BlueJeans

V tomto kurzu zjistíte, jak toointegrate BlueJeans s Azure Active Directory (Azure AD).

Integrace BlueJeans s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooBlueJeans
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooBlueJeans (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s BlueJeans tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- BlueJeans jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání BlueJeans z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-bluejeans-from-hello-gallery"></a>Přidání BlueJeans z Galerie hello
tooconfigure hello integrace BlueJeans do Azure AD, je nutné tooadd BlueJeans hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd BlueJeans z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **BlueJeans**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_search.png)

5. Na panelu výsledků hello vyberte **BlueJeans**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s BlueJeans podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v BlueJeans je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v BlueJeans musí toobe navázat.

V BlueJeans, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s BlueJeans, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele BlueJeans](#creating-a-bluejeans-test-user)**  -toohave protějšek Britta Simon v BlueJeans, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci BlueJeans.

**tooconfigure Azure AD jednotné přihlašování s BlueJeans, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **BlueJeans** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. Na hello **BlueJeans domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.BlueJeans.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.BlueJeans.com`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory BlueJeans klienta](https://support.bluejeans.com/contact) tooget tyto hodnoty. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_general_400.png)

6. Na hello **BlueJeans konfigurace** klikněte na tlačítko **konfigurace BlueJeans** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, adresy URL pro změnu hesla a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. V okně prohlížeče jiných webových přihlásit tooyour **BlueJeans** společnosti lokality jako správce.

8. Přejděte příliš**správce \> nastavení skupiny \> zabezpečení**.
   
   ![Správce](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "správce")

9. V hello **zabezpečení** část, proveďte následující kroky hello:
   
   ![Jednotné přihlašování SAML na](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "jednotného přihlašování k SAML")   
   
   a. Vyberte **jednotného přihlašování k SAML**.
  
   b. Vyberte **zapněte automatické zřizování**.

10. Přesunout s hello následující kroky:

    ![Certifikát cesta](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "certifikátu cesta")
    
    a. Klikněte na tlačítko **zvolit soubor**a pak nahrajte hello stáhnout certifikát.
   
    b. Vložení **SAML jeden přihlašování adresa URL služby** do hello **přihlašovací adresa URL** textové pole.
   
    c. Vložení **heslo změnit adresu URL** do hello **heslo změnit adresu URL** textové pole.
   
    d. Vložení **Sign-Out URL** do hello **adresy URL odhlašovací** textové pole.

11. Přesunout s hello následující kroky:
    
    ![Uložit změny](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "uložit změny")
    
    a. V hello **id uživatele** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
   
    b. V hello **e-mailu** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
   
    c. Klikněte na tlačítko **uložit změny**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-bluejeans-test-user"></a>Vytvoření zkušebního uživatele BlueJeans

Uživatelé toolog tooenable Azure AD v tooBlueJeans, se musí být zřízená do BlueJeans.  

V případě BlueJeans zřizování je ruční úloha.

**tooprovision uživatelské účty, provádět hello následující kroky:**

1. Přihlaste se tooyour **BlueJeans** společnosti lokality jako správce.

2. Přejděte příliš**správce \> spravovat uživatele \> přidat uživatele**.
   
   ![Správce](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "správce")
   
   >[!IMPORTANT]
   >Hello **přidat uživatele** karta je dostupná pouze v případě, v hello **karta zabezpečení**, **zapněte automatické zřizování** není zaškrtnuta. 
   
3. V hello **přidat uživatele** část, proveďte následující kroky hello:

    ![Přidat uživatele](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "přidat uživatele")
    
    a. Zadejte **uživatelské jméno BlueJeans**, **e-mailová adresa**, **BlueJeans schůzku ID**, **moderátora hesla**, **úplný název** , hello **společnosti** z platný účet AAD chcete tooprovision do hello související textových polí.
    
    b. Klikněte na tlačítko **přidat uživatele**.

>[!NOTE]
>Můžete použít všechny ostatní BlueJeans uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované BlueJeans tooprovision AAD uživatelské účty. 
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooBlueJeans toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooBlueJeans, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **BlueJeans**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello BlueJeans dlaždici v hello přístupového panelu, měli byste obdržet přihlašovací stránku BlueJeans aplikace.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_203.png

