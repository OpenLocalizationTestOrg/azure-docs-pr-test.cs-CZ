---
title: 'Kurz: Azure Active Directory integrace s LearnUpon | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a LearnUpon."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: fdb9c62172327a539f0459c98aa20e63fa441e4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Kurz: Azure Active Directory integrace s LearnUpon

V tomto kurzu zjistíte, jak toointegrate LearnUpon s Azure Active Directory (Azure AD).

Integrace LearnUpon s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooLearnUpon
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLearnUpon (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s LearnUpon tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- LearnUpon jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání LearnUpon z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-learnupon-from-hello-gallery"></a>Přidání LearnUpon z Galerie hello
tooconfigure hello integrace LearnUpon do Azure AD, je nutné tooadd LearnUpon hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd LearnUpon z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **LearnUpon**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_search.png)

5. Na panelu výsledků hello vyberte **LearnUpon**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s LearnUpon podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v LearnUpon je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v LearnUpon musí toobe navázat.

V LearnUpon, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s LearnUpon, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele LearnUpon](#creating-a-learnupon-test-user)**  -toohave protějšek Britta Simon v LearnUpon, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci LearnUpon.

**tooconfigure Azure AD jednotné přihlašování s LearnUpon, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **LearnUpon** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_samlbase.png)

3. Na hello **LearnUpon domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_url.png)

    V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.learnupon.com/saml/consumer`

    > [!NOTE] 
    > Upozorňujeme, že se nejedná hello skutečné hodnoty. Máte tooupdate tuto hodnotu s hello skutečná adresa URL odpovědi. Obraťte se na tooget tuto hodnotu [tým podpory LearnUpon](https://www.learnupon.com/features/support/).



4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Raw)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_general_400.png)

6. Na hello **LearnUpon konfigurace** klikněte na tlačítko **konfigurace LearnUpon** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_configure.png) 

7. Otevřete jiné instance prohlížeče a přihlášení do LearnUpon s účtem správce. 

8. Klikněte na tlačítko hello **nastavení** kartě.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png)

9. Klikněte na tlačítko **jednotné přihlašování – SAML**a potom klikněte na **obecné nastavení** tooconfigure SAML nastavení.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 

10. V hello **obecné nastavení** část, proveďte následující kroky hello:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png)  
  
    a. Vyberte **povoleno**.

    b. Vyberte **verze** jako **2.0**.

    c. Vyberte **přeskočit podmínky** jako **ne**.

    d. V hello **vystavení tokenu SAML název parametru** textovému poli, název typu hello požadavek post parametr toohello výše uvedené adresy URL příjemce SAML obsahující hello kontrolního výrazu SAML toobe ověřit a ověřovat – například  **SAMLResponse**.

    e. V hello **formát názvu identifikátor** textovému poli, typ hello hodnotu, která určuje, kde ve vaši uživatelé hello kontrolního výrazu SAML identifikátor (e-mailovou adresu) nachází – například **urn: oasis: názvy: tc: SAML:1.1:nameid-formát: emailAddress**.
  
    f. V hello **identifikovat umístění poskytovatele** textové pole, typ hello hodnotu, která určuje, kde hello uživatelům jsou odeslány tooif kliknou na ikonu vašeho nahrané z obrazovky přihlášení k portálu Azure.
  
    g. V hello **Odhlásit se adresa URL** textovému poli, vložte hello **Sign-Out URL** který jste zkopírovali z hello portálu Azure.
    
    h. Klikněte na tlačítko **spravovat prstem výtisků**a pak nahrajte hello prstu stažené certifikátu.

11. Klikněte na tlačítko **uživatelská nastavení**a poté proveďte hello následující kroky:
   
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png)  
 
    a. V hello **křestní jméno identifikátor formátu** textovému poli, typ hello hodnotu, která sděluje nám v vaší kontrolního výrazu SAML hello uživatelé firstname – například nachází: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
  
    b. V hello **poslední formát názvu identifikátor** textovému poli, typ hello hodnotu, která sděluje nám v vaší kontrolního výrazu SAML hello uživatelé lastname – například nachází: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-learnupon-test-user"></a>Vytvoření zkušebního uživatele LearnUpon

Hello cílem této části je toocreate volal Britta Simon v LearnUpon uživatele. LearnUpon podporuje za běhu zřizování, který je ve výchozím nastavení povolené.

Neexistuje žádná položka akce pro vás v této části. Pokud ještě neexistuje, se během pokusu o tooaccess LearnUpon vytvoří nového uživatele. [Konfigurace Azure AD jednotné přihlášení](#configuring-azure-ad-single-single-sign-on).

>[!NOTE]
>Pokud potřebujete toocreate uživatelé ručně, je nutné toocontact [tým podpory LearnUpon](https://www.learnupon.com/features/support/). 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooLearnUpon toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooLearnUpon, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **LearnUpon**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici LearnUpon hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour LearnUpon aplikace.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png

