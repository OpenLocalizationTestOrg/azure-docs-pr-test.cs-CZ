---
title: 'Kurz: Azure Active Directory integrace s Datahug | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Datahug."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5c0dc1ea-7ff4-4554-b60b-0f2fa9f5abaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: jeedes
ms.openlocfilehash: 79ccb939c7a19720bcf696af141f747db00c8a2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-datahug"></a>Kurz: Azure Active Directory integrace s Datahug

V tomto kurzu zjistíte, jak toointegrate Datahug s Azure Active Directory (Azure AD).

Integrace Datahug s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooDatahug
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooDatahug (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v článku. [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Datahug tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Datahug jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Datahug z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-datahug-from-hello-gallery"></a>Přidání Datahug z Galerie hello
tooconfigure hello integrace Datahug do Azure AD, je nutné tooadd Datahug hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Datahug z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Datahug**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_search.png)

5. Na panelu výsledků hello vyberte **Datahug**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Datahug podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Datahug je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Datahug musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Datahug.

tooconfigure a testu Azure AD jednotné přihlašování s Datahug, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Datahug](#creating-a-datahug-test-user)**  -toohave protějšek Britta Simon v Datahug, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Datahug.

**tooconfigure Azure AD jednotné přihlašování s Datahug, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Datahug** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_samlbase.png)

3. Na hello **Datahug domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_ur1.png)

    a. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://apps.datahug.com/identity/<uniqueID>`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://apps.datahug.com/identity/<uniqueID>/acs`

4. Zkontrolujte **zobrazit upřesňující nastavení adresy URL**. Pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_url2.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://apps.datahug.com/`
     
    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné hello. Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL. Zde, doporučujeme vám toouse hello jedinečnou hodnotu řetězce v hello identifikátor a adresa URL odpovědi. Obraťte se na [tým podpory Datahug klienta](http://datahug.com/about/contact-us/) tooget tyto hodnoty. 

5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_certificate.png) 

6.  Zkontrolujte **"Zobrazit upřesňující nastavení podpisový certifikát"** a provádět hello následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_cert.png)

    a. V **podepisování možnost**, vyberte **kontrolního výrazu SAML přihlašovací**.
    
    b. V **podpisového algoritmu**, vyberte **SHA1**.
 
7. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_general_400.png)
    
8. Na hello **Datahug konfigurace** klikněte na tlačítko **konfigurace Datahug** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID** a **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_configure.png) 

9. tooconfigure jednotného přihlašování na **Datahug** straně, je nutné stáhnout hello toosend **soubor XML s metadaty**, **SAML Entity ID** a **SAML-služby přihlášení Adresa URL** příliš[Datahug podporu](http://datahug.com/about/contact-us/). Nastavují tuto aplikaci toohave hello jednotné přihlašování SAML připojení správně nastavena na obou stranách.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-datahug-test-user"></a>Vytvoření zkušebního uživatele Datahug

Uživatelé toolog tooenable Azure AD v tooDatahug, se musí být zřízená do Datahug.  
Při zřizování Datahug, je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour Datahug společnosti lokality jako správce.

2. Pozastavte ukazatel myši nad hello **ozubené kolo** v horním pravém rohu hello a klikněte na **nastavení**
   
   ![Můžete přidat zaměstnance](./media/active-directory-saas-datahug-tutorial/1.png)

3. Zvolte **osoby** a klikněte na tlačítko hello **přidat uživatele** karta

    ![Můžete přidat zaměstnance](./media/active-directory-saas-datahug-tutorial/2.png)

4. Zadejte e-mailu hello hello osoby, které byste chtěli toocreate na účtu a klikněte na tlačítko **přidat**.

    ![Můžete přidat zaměstnance](./media/active-directory-saas-datahug-tutorial/3.png)

    > [!NOTE] 
    > Můžete odeslat e-mailu toouser registrace výběrem **odeslání Uvítacího e-mailu** zaškrtávací políčko.  
    > Při vytváření účtu služby Salesforce Neodesílat hello uvítací e-mail.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooDatahug toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooDatahug, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Datahug**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.
Když kliknete na dlaždici Datahug hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Datahug aplikace. Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_203.png

