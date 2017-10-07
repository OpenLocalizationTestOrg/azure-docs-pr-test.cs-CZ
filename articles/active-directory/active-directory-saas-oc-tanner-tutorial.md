---
title: "Kurz: Azure Active Directory integrace s O.C. Nováková - AppreciateHub | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a O.C. Nováková - AppreciateHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dee8fbca-0b60-4a21-8917-1fb6919de5a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: 45052cf56e35746d7df5910162e40e3bbcad1aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a>Kurz: Azure Active Directory integrace s O.C. Nováková - AppreciateHub

V tomto kurzu zjistíte, jak toointegrate O.C. Nováková - AppreciateHub s Azure Active Directory (Azure AD).

Integrace O.C. Nováková - AppreciateHub s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooO.C. Nováková - AppreciateHub
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooO.C. Nováková - AppreciateHub (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s O.C. Nováková - AppreciateHub, budete potřebovat hello následující položky:

- Předplatné služby Azure AD
- O.C. Nováková - AppreciateHub jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání O.C. Nováková - AppreciateHub z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-oc-tanner---appreciatehub-from-hello-gallery"></a>Přidání O.C. Nováková - AppreciateHub z Galerie hello
integrace hello tooconfigure O.C. Nováková - AppreciateHub do služby Azure AD, je nutné tooadd O.C. Nováková - AppreciateHub hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd O.C. Nováková - AppreciateHub z Galerie hello provést hello následující kroky:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **O.C. Nováková - AppreciateHub**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_search.png)

5. Na panelu výsledků hello vyberte **O.C. Nováková - AppreciateHub**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s O.C. Nováková - AppreciateHub podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování musí Azure AD tooknow hello příslušného uživatele v O.C. Nováková - AppreciateHub je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v O.C. Nováková - AppreciateHub musí toobe navázat.

V O.C. Nováková - AppreciateHub, přiřaďte hello hodnotu hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testování Azure AD jednotné přihlašování s O.C. Nováková - AppreciateHub, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření O.C. Nováková - AppreciateHub testovacího uživatele](#creating-a-oc-tanner---appreciatehub-test-user)**  -toohave protějšek Britta Simon v O.C. Nováková - AppreciateHub, která je propojená toohello reprezentace uživatele Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší O.C. Nováková - AppreciateHub aplikace.

**tooconfigure Azure AD jednotné přihlašování s O.C. Nováková - AppreciateHub, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **O.C. Nováková - AppreciateHub** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_samlbase.png)

3. Na hello **O.C. Nováková - AppreciateHub domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_url.png)

    a. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.appreciatehub.com/fed/sp/authnResponse20`

    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte tuto hodnotu s hello skutečná adresa URL odpovědi. Obraťte se na [O.C. Nováková - tým podpory AppreciateHub](mailto:sso@octanner.com) tooget tuto hodnotu.

    b. Soubor metadat otevřete hello pomocí hello následující odkaz: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).
   
    c. Vyhledejte hello **md:AssertionConsumerService** uzlu. 
   
    d. Zkopírujte hodnotu hello hello **umístění** atribut. 
   
    ![Konfigurovat nastavení aplikace][12]
   
    e. V hello **přihlašovací adresa URL** textovému poli, po hello hodnoty, které jste získali v předchozím kroku hello.

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_400.png)

6. tooconfigure jednotného přihlašování na **O.C. Nováková - AppreciateHub** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[O.C. Nováková - tým podpory AppreciateHub](mailto:sso@octanner.com).

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a>Vytváření O.C. Nováková - AppreciateHub testovacího uživatele

Hello cílem této části je toocreate volal Britta Simon v O.C. uživatele Nováková - AppreciateHub.

**toocreate uživatel volal Britta Simon v O.C. Nováková - AppreciateHub, proveďte následující kroky hello:**

Požádejte vaše [O.C. Nováková - tým podpory AppreciateHub](mailto:sso@octanner.com) toocreate uživatele, který má jako hello atribut nameID stejnou hodnotu jako uživatelské jméno hello Britta Simon ve službě Azure AD.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooO.C toouse Britta Simon Azure jednotné přihlašování. Nováková - AppreciateHub.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooO.C. Nováková - AppreciateHub, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **O.C. Nováková - AppreciateHub**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.  
Když kliknete na tlačítko hello O.C. Nováková - AppreciateHub dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour O.C. Nováková - AppreciateHub aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png

[100]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png

