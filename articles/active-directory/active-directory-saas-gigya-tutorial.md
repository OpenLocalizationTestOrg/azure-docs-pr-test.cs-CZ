---
title: 'Kurz: Azure Active Directory integrace s Gigya | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Gigya."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2c7d200b-9242-44a5-ac8a-ab3214a78e41
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 1992c5ad09b097563377a488fbf5a375f6511020
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-gigya"></a>Kurz: Azure Active Directory integrace s Gigya

V tomto kurzu zjistíte, jak toointegrate Gigya s Azure Active Directory (Azure AD).

Integrace Gigya s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooGigya
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooGigya (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Gigya tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Gigya jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Gigya z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-gigya-from-hello-gallery"></a>Přidání Gigya z Galerie hello
tooconfigure hello integrace Gigya do Azure AD, je nutné tooadd Gigya hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Gigya z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Gigya**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_search.png)

5. Na panelu výsledků hello vyberte **Gigya**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Gigya podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Gigya je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Gigya musí toobe navázat.

V Gigya, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Gigya, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Gigya](#creating-a-gigya-test-user)**  -toohave protějšek Britta Simon v Gigya, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Gigya.

**tooconfigure Azure AD jednotné přihlašování s Gigya, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Gigya** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_samlbase.png)

3. Na hello **Gigya domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`http://<companyname>.gigya.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://fidm.gigya.com/saml/v2.0/<companyname>`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory Gigya klienta](https://www.gigya.com/support-policy/) tooget tyto hodnoty. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_general_400.png)

6. Na hello **Gigya konfigurace** klikněte na tlačítko **konfigurace Gigya** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Gigya.

8. Přejděte příliš**nastavení \> SAML přihlášení**a potom klikněte na hello **přidat** tlačítko.
   
    ![Přihlašování SAML](./media/active-directory-saas-gigya-tutorial/ic789532.png "SAML přihlášení")

9. V hello **SAML přihlášení** část, proveďte následující kroky hello:
   
    ![Konfigurace SAML](./media/active-directory-saas-gigya-tutorial/ic789533.png "konfigurace SAML")
   
    a. V hello **název** textovému poli, zadejte název pro svou konfiguraci.
   
    b. V **vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure. 
   
    c. V **jeden přihlašování adresa URL služby** textovému poli, vložte hodnotu hello **jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.
   
    d. V **formát ID názvu** textovému poli, vložte hodnotu hello **formát názvu identifikátor** který jste zkopírovali z portálu Azure.
   
    e. Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku stáhli z portálu Azure, hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát X.509** textové pole.
   
    f. Klikněte na tlačítko **uložit nastavení**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-gigya-test-user"></a>Vytvoření zkušebního uživatele Gigya

V pořadí tooenable Azure AD Uživatelé toolog do Gigya musí být zřízená do Gigya.  
V případě hello Gigya zřizování je ruční úloha.

### <a name="tooprovision-a-user-accounts-perform-hello-following-steps"></a>tooprovision uživatelské účty, provádět hello následující kroky:

1. Přihlaste se tooyour **Gigya** společnosti lokality jako správce.

2. Přejděte příliš**správce \> spravovat uživatele**a potom klikněte na **pozvat uživatele**.
   
    ![Správa uživatelů](./media/active-directory-saas-gigya-tutorial/ic789535.png "Správa uživatelů")

3. V dialogovém okně hello pozvat uživatele proveďte následující kroky hello:
   
    ![Uživatele pozvat](./media/active-directory-saas-gigya-tutorial/ic789536.png "pozvat uživatele")
   
    a. V hello **e-mailu** textovému poli, typ hello e-mailový alias chcete tooprovision platný účet služby Azure Active Directory.
    
    b. Klikněte na tlačítko **pozvat uživatele**.
      
    > [!NOTE]
    > Držitel účtu Azure Active Directory Hello obdrží e-mail, který obsahuje účet odkaz tooconfirm hello předtím, než se stane aktivní.
    > 
    

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooGigya toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooGigya, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Gigya**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.

Když kliknete na dlaždici Gigya hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Gigya aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_203.png

