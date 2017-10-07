---
title: 'Kurz: Azure Active Directory integrace s Optimizely | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Optimizely."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28ef03e1-9aad-4301-af97-d94e853edc74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 868aefae27ca155d2963f3dcfcd79bbb564b48ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Kurz: Azure Active Directory integrace s Optimizely

V tomto kurzu zjistíte, jak toointegrate Optimizely s Azure Active Directory (Azure AD).

Integrace Optimizely s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooOptimizely
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooOptimizely (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Optimizely tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Optimizely jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Optimizely z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-optimizely-from-hello-gallery"></a>Přidání Optimizely z Galerie hello
tooconfigure hello integrace Optimizely do Azure AD, je nutné tooadd Optimizely hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Optimizely z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Optimizely**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_search.png)

5. Na panelu výsledků hello vyberte **Optimizely**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Optimizely podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Optimizely je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Optimizely musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Optimizely.

tooconfigure a testu Azure AD jednotné přihlašování s Optimizely, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele Optimizely](#creating-an-optimizely-test-user)**  -toohave protějšek Britta Simon v Optimizely, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Optimizely.

**tooconfigure Azure AD jednotné přihlašování s Optimizely, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Optimizely** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_samlbase.png)

3. Na hello **Optimizely domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://app.optimizely.net/<instance name>`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`urn:auth0:optimizely:contoso`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné hello. Aktualizujte hodnotu hello s hello skutečné přihlašovací adresa URL a identifikátor, který je vysvětlen později v kurzu hello. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_general_400.png)

6. Na hello **Optimizely konfigurace** klikněte na tlačítko **konfigurace Optimizely** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_configure.png) 

7. tooconfigure jednotného přihlašování na **Optimizely** straně, obraťte se na správce svého účtu Optimizely a zadejte hello Stáhnout **certifikátu (Base64)**, a **SAML jeden přihlašování adresa URL služby** . 

8. V odpovědi tooyour e-mailu Optimizely vám poskytne hello přihlašovací adresa URL (jednotné přihlašování iniciované SP) a hello hodnot identifikátoru (ID Entity poskytovatele služby).

    a. Kopírování hello **URL jednotné přihlašování iniciované SP** zadaný Optimizely a vložit do hello **přihlašovací adresa URL** textového pole v **Optimizely domény a adresy URL** části na portálu Azure 

    b. Kopírování hello **ID Entity poskytovatele služby** zadaný Optimizely a vložit do hello **identifikátor** textového pole v **Optimizely domény a adresy URL** části na portálu Azure 

9. V okně jiný prohlížeč, přihlášení tooyour Optimizely aplikace.

10. Klikněte na účet pravém rohu název v horní části hello a potom **nastavení účtu**.
   
    ![Azure AD jednotné přihlášení](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

11. Na kartě účet hello, zaškrtněte políčko hello **povolit jednotné přihlašování** pod jednotné přihlašování v hello **přehled** části.
   
    ![Azure AD jednotné přihlášení](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)
    
12. Klikněte na tlačítko **uložit**

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z Britta Simon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-optimizely-test-user"></a>Vytváření testovacího uživatele Optimizely

V této části vytvoříte volal Britta Simon v Optimizely uživatele.

1. Na domovské stránce hello vyberte **spolupracovníci** kartě.

2. tooadd nový projekt toohello spolupracovník, klikněte na tlačítko **nové spolupracovník**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3. Vyplňte hello e-mailovou adresu a přiřadit jim roli. Klikněte na tlačítko **pozvat**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

4. Zobrazí se jim pozvání e-mailu. Pomocí hello e-mailovou adresu, mají toolog v tooOptimizely.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooOptimizely toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooOptimizely, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Optimizely**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Optimizely hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Optimizely aplikace. 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png

