---
title: 'Kurz: Azure Active Directory integrace s Dropbox pro firmy | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Dropbox pro firmy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 3f33a43ca8fbd60486d7a400ae8246af768376ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Kurz: Azure Active Directory integrace s Dropbox pro firmy

V tomto kurzu zjistíte, jak toointegrate Dropbox pro firmy s Azure Active Directory (Azure AD).

Integrace Dropbox pro firmy s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooDropbox pro firmy
- Vaši uživatelé tooautomatically get přihlášeného tooDropbox můžete povolit pro firmy (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Dropbox pro firmy, musíte hello následující položky:

- Předplatné služby Azure AD
- Dropbox pro obchodní jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Dropbox pro firmy z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-dropbox-for-business-from-hello-gallery"></a>Přidání Dropbox pro firmy z Galerie hello
integrace hello tooconfigure Dropbox pro firmy do služby Azure AD, musíte pro firmy hello Galerie tooyour seznamu spravovaných aplikací SaaS tooadd Dropbox.

**tooadd Dropbox pro firmy z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Dropbox pro firmy**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_search.png)

5. Na panelu výsledků hello vyberte **Dropbox pro firmy**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Dropbox pro firmy na základě testovací uživatele, nazývá "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Dropbox pro firmy je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Dropbox pro firmy musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Dropbox pro firmy.

tooconfigure a testu Azure AD jednotné přihlašování s Dropbox pro firmy, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření Dropbox pro obchodní testovacího uživatele](#creating-a-dropbox-for-business-test-user)**  -toohave protějšek Britta Simon v Dropbox pro firmy, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování do vaší schránky pro obchodní aplikace.

**tooconfigure Azure AD jednotné přihlašování s Dropbox pro firmy, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Dropbox pro firmy** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. Na hello **Dropbox obchodní domény a adresy URL** část, proveďte následující kroky hello:

    a. Přihlášení tooyour Dropbox pro obchodní klienta. 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "nakonfigurovat jednotné přihlašování")
   
    b. V navigačním podokně hello na levé straně hello, klikněte na tlačítko **konzoly pro správu**. 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "nakonfigurovat jednotné přihlašování")
   
    c. Na hello **konzoly pro správu**, klikněte na tlačítko **ověřování** v levém navigačním podokně hello. 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "nakonfigurovat jednotné přihlašování")
   
    d. V hello **jednotného přihlašování** vyberte **povolit jednotné přihlašování**a potom klikněte na **Další** tooexpand v této části.  
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "nakonfigurovat jednotné přihlašování")
   
    e. Zkopírujte adresu URL hello vedle příliš**uživatelé se mohou přihlásit zadáním e-mailové adresy nebo můžete přejít přímo na**. 
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769513.png)
    
    f. Na portálu Azure, v hello hello **přihlašovací adresa URL** textovému poli, hello vložte adresu URL.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url.png)

     V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.dropbox.com/sso/<id>`

    > [!NOTE] 
    > Tato hodnota není skutečné hodnoty. Aktualizujte hodnotu hello s hello skutečná adresa URL přihlašování můžete získat z jejich části jednom přihlášení. Obraťte se na [Dropbox pro klienta obchodní tým podpory](https://www.dropbox.com/business/contact) tooget tuto hodnotu. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_400.png)

6. Na hello **Dropbox pro konfiguraci obchodní** klikněte na tlačítko **Dropbox nakonfigurovat pro firmy** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. tooconfigure jednotného přihlašování na **Dropbox pro firmy** straně, přejděte na vaší schránky pro obchodní klienta v hello **jednotného přihlašování** části hello **ověřování** stránky, Proveďte hello následující kroky: 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "nakonfigurovat jednotné přihlašování")
   
    a. Klikněte na tlačítko **požadované**.
   
    b. V portálu Azure, na hello hello **konfigurovat přihlášení** okno, kopie hello **SAML jeden přihlašování adresa URL služby** hodnotu a pak ji vložit do hello **přihlašovací adresa URL** textové pole.

    c. Klikněte na tlačítko **vyberte certifikát**a potom vyhledejte tooyour **Base64 kódovaného certifikátu souboru**.

    d. Klikněte na tlačítko **uložit změny** toocomplete hello konfiguraci na vaší schránky pro obchodní klienta.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_01.png) 

2.  toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-dropbox-for-business-test-user"></a>Vytváření Dropbox pro obchodní testovacího uživatele

V této části se uživatel volá Britta Simon vytvoří v Dropbox pro firmy. Dropbox pro firmy podporuje za běhu zřizování, který je ve výchozím nastavení povolené.

Neexistuje žádná položka akce pro vás v této části. Pokud uživatel ještě neexistuje v Dropbox pro firmy, je vytvořen nový pokusíte-li tooaccess Dropbox pro firmy.

>[!Note]
>Pokud potřebujete toocreate uživatel ručně, obraťte se na [Dropbox pro tým podpory obchodní klienta](https://www.dropbox.com/business/contact) 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooDropbox pro firmy Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign tooDropbox Britta Simon pro firmy, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Dropbox pro firmy**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko Dropbox hello pro firmy dlaždici v hello přístupového panelu, měli byste obdržet přihlašovací stránku vaší schránky pro obchodní aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfiguraci zřizování uživatelů](active-directory-saas-dropboxforbusiness-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_203.png

