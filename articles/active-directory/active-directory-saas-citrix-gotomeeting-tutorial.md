---
title: 'Kurz: Azure Active Directory integrace s Citrix GoToMeeting | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Citrix GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7d7897f6-b88e-4dd5-8f3a-e612337b1413
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 46a5da7504806202a5ec29f73c504e772c61bc2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a>Kurz: Azure Active Directory integrace s Citrix GoToMeeting

V tomto kurzu zjistíte, jak toointegrate Citrix GoToMeeting službou Azure Active Directory (Azure AD).

Integrace Citrix GoToMeeting s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooCitrix GoToMeeting
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooCitrix GoToMeeting (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Citrix GoToMeeting, budete potřebovat hello následující položky:

- Předplatné služby Azure AD
- Citrix GoToMeeting jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Citrix GoToMeeting z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-citrix-gotomeeting-from-hello-gallery"></a>Přidání Citrix GoToMeeting z Galerie hello
tooconfigure hello integrace Citrix GoToMeeting do Azure AD, je nutné tooadd Citrix GoToMeeting hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Citrix GoToMeeting z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Citrix GoToMeeting**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_search.png)

5. Na panelu výsledků hello vyberte **Citrix GoToMeeting**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurujete a testu Azure AD jednotné přihlašování s Citrix GoToMeeting podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Citrix GoToMeeting je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Citrix GoToMeeting musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Citrix GoToMeeting.

tooconfigure a testu Azure AD jednotné přihlašování s Citrix GoToMeeting, budete potřebovat následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Citrix GoToMeeting](#creating-a-citrix-gotomeeting-test-user)**  -toohave protějšek Britta Simon v Citrix GoToMeeting, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Citrix GoToMeeting.

**tooconfigure Azure AD jednotné přihlašování s Citrix GoToMeeting, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Citrix GoToMeeting** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_samlbase.png)

3. Na hello **Citrix GoToMeeting domény a adresy URL** části, bez nutnosti tooperform žádné kroky.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_url.png)


3. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_certificate.png) 

4. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_400.png)
    
5. Na hello Citrix GoToMeeting SAML konfigurační oddíl klikněte na konfigurace SAML GoToMeeting Citrix tooopen konfigurovat přihlašování okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

6. V okně jiný prohlížeč, přihlaste tooyour [Citrix organizace Center](https://account.citrixonline.com/organization/administration/).

7. Klikněte na tlačítko hello **zprostředkovatele Identity** kartě a potom proveďte hello následující kroky:  
   
    ![Instalační program SAML](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "instalace SAML")
   
    a. Vyberte **ruční**

    b. V portálu Azure, na hello hello **nakonfigurovat jednotné přihlašování v Citrix GoToMeeting** dialogové okno stránky, kopie hello **SAML jeden přihlašování adresa URL služby** hodnotu a pak ji vložit do hello **přihlašovací stránce Adresa URL** textové pole. 

    c. V portálu Azure, na hello hello **nakonfigurovat jednotné přihlašování v Citrix GoToMeeting** dialogové okno stránky, kopie hello **Sign-Out URL** hodnotu a pak ji vložit do hello **adresy URL odhlašovací stránky**textové pole.

    d. V portálu Azure, na hello hello **nakonfigurovat jednotné přihlašování v Citrix GoToMeeting** dialogové okno stránky, kopie hello **SAML Entity ID** hodnotu a pak ji vložit do hello **ID Entity zprostředkovatele Identity**  textové pole.

    e. tooupload stažený certifikát, klikněte na tlačítko **nahrát certifikát**.

    f. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-citrix-gotomeeting-test-user"></a>Vytvoření zkušebního uživatele Citrix GoToMeeting

V této části se uživatel volá Britta Simon vytvoří v Citrix GoToMeeting. Citrix GoToMeeting podporuje za běhu zřizování, který je ve výchozím nastavení povolené.

Neexistuje žádná položka akce pro vás v této části. Pokud uživatel ještě neexistuje v Citrix GoToMeeting, nový vytvoří při pokusíte tooaccess Citrix GoToMeeting.

>[!Note]
>Pokud potřebujete toocreate uživatel ručně, obraťte se na [tým podpory Citrix GoToMeeting](https://care.citrixonline.com/gotomeeting) 


### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooCitrix GoToMeeting toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooCitrix GoToMeeting, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Citrix GoToMeeting**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu. Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfiguraci zřizování uživatelů](active-directory-saas-citrixgotomeeting-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_203.png

