---
title: 'Kurz: Azure Active Directory integrace s TINFOIL SECURITY | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a TINFOIL SECURITY."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 1a3fa9880d9e026c2d6d6548188df2269ff69139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Kurz: Azure Active Directory integrace s TINFOIL SECURITY

V tomto kurzu zjistíte, jak toointegrate TINFOIL SECURITY s Azure Active Directory (Azure AD).

Integrace TINFOIL SECURITY s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooTINFOIL zabezpečení
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTINFOIL zabezpečení (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s TINFOIL SECURITY, budete potřebovat hello následující položky:

- Předplatné služby Azure AD
- TINFOIL SECURITY jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidat TINFOIL SECURITY z Galerie hello
2. Konfigurace a otestování Azure AD jednotné přihlašování

## <a name="add-tinfoil-security-from-hello-gallery"></a>Přidat TINFOIL SECURITY z Galerie hello
tooconfigure hello integrace TINFOIL SECURITY do Azure AD, je nutné tooadd TINFOIL SECURITY hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd TINFOIL SECURITY z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **TINFOIL SECURITY**, vyberte **TINFOIL SECURITY** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![TINFOIL SECURITY z Galerie](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s TINFOIL SECURITY podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v TINFOIL SECURITY je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v TINFOIL SECURITY musí toobe navázat.

V TINFOIL SECURITY přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s TINFOIL SECURITY, budete potřebovat následující stavební bloky hello toocomplete:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele TINFOIL SECURITY](#create-a-tinfoil-security-test-user)**  -toohave protějšek Britta Simon v TINFOIL SECURITY, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci TINFOIL SECURITY.

**tooconfigure Azure AD jednotné přihlašování s TINFOIL SECURITY, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **TINFOIL SECURITY** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Přihlášení na základě SAML](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_samlbase.png)

3. Na hello **TINFOIL zabezpečení domény a adresy URL** část, hello uživatel nemá tooperform žádné kroky jako aplikace hello je už předem integrováno s Azure.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_url.png)


4. Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnotu.

    ![Certifikát pro podpis SAML části](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_certificate.png) 

5. mapování atributů tooadd hello vyžaduje, proveďte následující kroky hello:
    
    ![Atributy](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "atributy")
    
    | Název atributu    |   Hodnota atributu |
    | ------------------- | -------------------- |
    | ID účtu | UXXXXXXXXXXXXX |
    
    a. Klikněte na tlačítko **přidat atribut uživatele**.
    
    ![Přidat atribut](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "atributy")
    
    ![Přidat atribut](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "atributy")
    
    b. V hello **název atributu** textovému poli, typ **accountid**.
    
    c. V hello **hodnota atributu** textovému poli, vložte hello účet ID hodnotu, která budete mít později v kurzu hello.
    
    d. Klikněte na tlačítko **OK**.    

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Tlačítko Uložit](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_400.png)

7. Na hello **TINFOIL SECURITY Configuration** klikněte na tlačítko **konfigurace TINFOIL SECURITY** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurace zabezpečení aplikace TINFOIL](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_configure.png) 

8. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti TINFOIL SECURITY.

9. V panelu nástrojů hello hello nahoře, klikněte na **Můj účet**.
   
    ![Řídicí panel](./media/active-directory-saas-tinfoil-security-tutorial/ic798971.png "řídicí panel")

10. Klikněte na tlačítko **zabezpečení**.
   
    ![Zabezpečení](./media/active-directory-saas-tinfoil-security-tutorial/ic798972.png "zabezpečení")

11. Na hello **jednotné přihlašování** konfigurace proveďte hello následující kroky:
   
    ![Jednotné přihlašování](./media/active-directory-saas-tinfoil-security-tutorial/ic798973.png "jednotného přihlašování")
   
    a. Vyberte **povolit SAML**.
   
    b. Klikněte na tlačítko **ruční konfigurace**.
   
    c. V **SAML Post URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure
   
    d. V **otisků certifikátu SAML** textovému poli, vložte hodnotu hello **kryptografický otisk** který jste zkopírovali z **SAML podpisový certifikát** části.
  
    e. Kopírování **vaše ID účtu** a vložte hodnotu hello v **hodnota atributu** textového pole pod **přidat atribut** části na portálu Azure.
   
    f. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Uživatelé a skupiny -> všichni uživatelé ](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Uživatel](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-tinfoil-security-test-user"></a>Vytvoření zkušebního uživatele TINFOIL SECURITY

V pořadí tooenable Azure AD Uživatelé toolog do TINFOIL SECURITY musí být zřízená do TINFOIL SECURITY. V případě hello TINFOIL SECURITY zřizování je ruční úloha.

**tooget uživatele zřízený, proveďte následující kroky hello:**

1. Pokud je uživatel hello součástí účet podnikové sítě, je třeba příliš[kontaktujte tým podpory TINFOIL SECURITY hello](https://www.tinfoilsecurity.com/contact) tooget hello uživatelský účet vytvořený.

2. Pokud je uživatel hello běžný uživatel TINFOIL SECURITY SaaS, můžete přidat uživatele hello spolupracovník tooany lokalit hello uživatele. Tento aktivační události procesu toosend, toohello pozvánku k zadané e-mailu toocreate nový uživatelský účet TINFOIL SECURITY.

> [!NOTE]
> Můžete použít všechny ostatní TINFOIL SECURITY uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované TINFOIL SECURITY tooprovision uživatelské účty Azure AD.
> 
> 

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooTINFOIL zabezpečení Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooTINFOIL zabezpečení, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **TINFOIL SECURITY**.

    ![Vyberte TINFOIL SECURITY](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici TINFOIL SECURITY hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour aplikace TINFOIL SECURITY. Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_203.png

