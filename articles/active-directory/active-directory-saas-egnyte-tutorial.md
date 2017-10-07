---
title: 'Kurz: Azure Active Directory integrace s Egnyte | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Egnyte."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: d86fa28a1e7b23474cb76c08aef8539acadaf24d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Kurz: Azure Active Directory integrace s Egnyte

V tomto kurzu zjistíte, jak toointegrate Egnyte s Azure Active Directory (Azure AD).

Integrace Egnyte s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooEgnyte
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooEgnyte (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Egnyte tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Egnyte jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Egnyte z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-egnyte-from-hello-gallery"></a>Přidání Egnyte z Galerie hello
tooconfigure hello integrace Egnyte do Azure AD, je nutné tooadd Egnyte hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Egnyte z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Egnyte**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_search.png)

5. Na panelu výsledků hello vyberte **Egnyte**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Egnyte podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Egnyte je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Egnyte musí toobe navázat.

V Egnyte, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Egnyte, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele Egnyte](#creating-an-egnyte-test-user)**  -toohave protějšek Britta Simon v Egnyte, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Egnyte.

**tooconfigure Azure AD jednotné přihlašování s Egnyte, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Egnyte** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_samlbase.png)

3. Na hello **Egnyte domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_url.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.egnyte.com`

    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory Egnyte klienta](https://www.egnyte.com/corp/contact_egnyte.html) tooget tuto hodnotu. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_general_400.png)

6. Na hello **Egnyte konfigurace** klikněte na tlačítko **konfigurace Egnyte** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti Egnyte tooyour.

8. Klikněte na tlačítko **nastavení**.
   
   ![Nastavení](./media/active-directory-saas-egnyte-tutorial/ic787819.png "nastavení")

9. V nabídce hello, klikněte na tlačítko **nastavení**.

   ![Nastavení](./media/active-directory-saas-egnyte-tutorial/ic787820.png "nastavení")

10. Klikněte na tlačítko hello **konfigurace** a pak klikněte **zabezpečení**.

    ![Zabezpečení](./media/active-directory-saas-egnyte-tutorial/ic787821.png "zabezpečení")

11. V hello **ověření jednotného přihlašování** část, proveďte následující kroky hello:

    ![Jednotné přihlašování na ověřování](./media/active-directory-saas-egnyte-tutorial/ic787822.png "jednotné přihlašování v ověřování")   
    
    a. Jako **ověření jednotného přihlašování**, vyberte **SAML 2.0**.
   
    b. Jako **zprostředkovatele Identity**, vyberte **AzureAD**.
   
    c. Vložení **SAML jeden přihlašování adresa URL služby** zkopírovaných z portálu Azure do hello **adresu URL pro přihlášení zprostředkovatele Identity** textové pole.
   
    d. Vložení **SAML Entity ID** který jste zkopírovali z portálu Azure do hello **ID entity zprostředkovatele Identity** textové pole.
      
    e. Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku stáhli z portálu Azure, hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát zprostředkovatele Identity** textové pole.
   
    f. Jako **výchozí mapování uživatelů**, vyberte **e-mailová adresa**.
   
    g. Jako **použít hodnotu specifické pro doménu vystavitele**, vyberte **zakázáno**.
   
    h. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-egnyte-test-user"></a>Vytváření testovacího uživatele Egnyte

Uživatelé toolog tooenable Azure AD v tooEgnyte, se musí být zřízená do Egnyte. V případě hello Egnyte zřizování je ruční úloha.

**tooprovision uživatelské účty, provádět hello následující kroky:**

1. Přihlaste se tooyour **Egnyte** společnosti lokality jako správce.

2. Přejděte příliš**nastavení \> uživatelé a skupiny**.

3. Klikněte na tlačítko **přidat nové uživatele**a pak vyberte typ hello uživatele chcete tooadd.
   
   ![Uživatelé](./media/active-directory-saas-egnyte-tutorial/ic787824.png "uživatelů")

4. V hello **nové standardní uživatel** část, proveďte následující kroky hello:
   
   ![Nový uživatel standardní](./media/active-directory-saas-egnyte-tutorial/ic787825.png "nové standardní uživatel")   

   a. Typ hello **e-mailu**, **uživatelské jméno**a další podrobnosti chcete tooprovision platný účet služby Azure Active Directory.
   
   b. Klikněte na **Uložit**.
    
    >[!NOTE]
    >Držitel účtu Azure Active Directory Hello obdrží e-mailové oznámení.
    >

>[!NOTE]
>Můžete použít všechny ostatní Egnyte uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Egnyte tooprovision AAD uživatelské účty.
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooEgnyte toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooEgnyte, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Egnyte**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Egnyte hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Egnyte aplikace.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_203.png

