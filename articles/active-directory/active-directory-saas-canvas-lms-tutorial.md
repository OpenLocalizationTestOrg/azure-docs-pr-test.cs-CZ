---
title: "Kurz: Azure Active Directory integrace s plátno pro správu vzdělávacího procesu | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a plátno pro správu vzdělávacího procesu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: jeedes
ms.openlocfilehash: 8f4a09266a108e2c92326b0909dd0650b1c84d6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Kurz: Azure Active Directory integrace s plátno pro správu vzdělávacího procesu

V tomto kurzu zjistíte, jak toointegrate plátno s Azure Active Directory (Azure AD).

Integrace plátno s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooCanvas
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooCanvas (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD plátno tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Plátně jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání plátno z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-canvas-from-hello-gallery"></a>Přidání plátno z Galerie hello
tooconfigure hello integrace plátno do Azure AD, je nutné tooadd plátno hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd plátno z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **plátno**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_search.png)

5. Na panelu výsledků hello vyberte **plátno**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování plátno podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello plátno je tooa uživatelem ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello plátno musí toobe navázat.

V plátně přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s plátno, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele plátno](#creating-a-canvas-test-user)**  -toohave protějšek Britta Simon plátno, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci plátno.

**tooconfigure Azure AD jednotné přihlašování plátno, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **plátno** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_samlbase.png)

3. Na hello **plátno domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.instructure.com`

    b. V hello **identifikátor** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://<tenant-name>.instructure.com/saml2`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory plátno klienta](https://community.canvaslms.com/community/help) tooget tyto hodnoty. 
 
4. Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnota certifikátu.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_400.png)

6. Na hello **plátno konfigurace** klikněte na tlačítko **konfigurace plátno** tooopen **konfigurovat přihlášení** okno. Kopírování hello **heslo změnit adresu URL, adresa URL Sign-Out, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_configure.png) 
 
7. V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti tooyour plátno.

8. Přejděte příliš**kurzy \> účty spravované \> Microsoft**.
   
    ![Plátno](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "plátno")

9. V navigačním podokně hello na levé straně hello vyberte **ověřování**a potom klikněte na **přidat nová konfigurace SAML**.
   
    ![Ověřování](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "ověřování")

10. Na stránce aktuální integrace hello proveďte následující kroky hello:
   
    ![Aktuální integrace](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "aktuální integrace")

    a. V **IdP Entity ID** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.

    b. V **protokolu na adresu URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.

    c. V **adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.

    d. V **odkaz změnit heslo** textovému poli, vložte hodnotu hello **heslo změnit adresu URL** který jste zkopírovali z portálu Azure. 

    e. V **otisků prstů certifikátů** textovému poli, vložte hello **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.      
        
    f. Z hello **přihlášení atribut** seznamu, vyberte **NameID**.

    g. Z hello **identifikátor formátu** seznamu, vyberte **emailAddress**.

    h. Klikněte na tlačítko **uložit nastavení ověřování**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-canvas-test-user"></a>Vytvoření zkušebního uživatele plátno

Uživatelé toolog tooenable Azure AD v tooCanvas, se musí být zřízená plátno.

V případě plátně zřizování uživatelů je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour **plátno** klienta.

2. Přejděte příliš**kurzy \> účty spravované \> Microsoft**.
   
   ![Plátno](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "plátno")

3. Klikněte na tlačítko **uživatelé**.
   
   ![Uživatelé](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "uživatelů")

4. Klikněte na tlačítko **přidat nového uživatele**.
   
   ![Uživatelé](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "uživatelů")

5. V hello přidat nového uživatele dialogové okno stránky proveďte následující kroky hello:
   
   ![Přidat uživatele](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "přidat uživatele")
   
   a. V hello **úplný název** textovému poli, zadejte název hello uživatele jako **BrittaSimon**.

   b. V hello **e-mailu** textovému poli, zadejte e-mailu hello uživatele jako  **brittasimon@contoso.com** .

   c. V hello **přihlášení** textovému poli, zadejte hello uživatele Azure AD e-mailovou adresu jako  **brittasimon@contoso.com** .

   d. Vyberte **e-mailem hello uživatel o vytvoření tohoto účtu**.

   e. Klikněte na tlačítko **přidat uživatele**.

>[!NOTE]
>Můžete použít všechny ostatní plátno uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované plátno tooprovision AAD uživatelské účty.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooCanvas toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooCanvas, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **plátno**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici plátno hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour plátno aplikace.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_203.png

