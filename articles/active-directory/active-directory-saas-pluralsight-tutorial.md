---
title: 'Kurz: Azure Active Directory integrace s Pluralsight | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Pluralsight."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4c3f07d2-4e1f-4ea3-9025-c663f1f2b7b4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: c8394eed79f21fb889816d8dafe2d71187be72b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pluralsight"></a>Kurz: Azure Active Directory integrace s Pluralsight

V tomto kurzu zjistíte, jak toointegrate Pluralsight s Azure Active Directory (Azure AD).

Integrace Pluralsight s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooPluralsight
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPluralsight (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Pluralsight tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Pluralsight jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Pluralsight z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-pluralsight-from-hello-gallery"></a>Přidání Pluralsight z Galerie hello
tooconfigure hello integrace Pluralsight do Azure AD, je nutné tooadd Pluralsight hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Pluralsight z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Pluralsight**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_search.png)

5. Na panelu výsledků hello vyberte **Pluralsight**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Pluralsight podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Pluralsight je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Pluralsight musí toobe navázat.

V Pluralsight, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Pluralsight, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Pluralsight](#creating-a-pluralsight-test-user)**  -toohave protějšek Britta Simon v Pluralsight, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Pluralsight.

**tooconfigure Azure AD jednotné přihlašování s Pluralsight, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Pluralsight** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_samlbase.png)

3. Na hello **Pluralsight domény a adresy URL** část, proveďte následující hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_url.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instance name>.pluralsight.com/sso/<company name>`

    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory Pluralsight klienta](mailto:support@pluralsight.com) tooget tuto hodnotu. 
 


4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_certificate.png) 

5. Hello cílem této části je tooenable Azure AD jednotné přihlašování v hello portál Azure a tooconfigure jednotné přihlašování v aplikaci Pluralsight hello.

    Hello Pluralsight aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování tooyour tokenu atributy konfigurace SAML. Hello následující snímek obrazovky ukazuje příklad pro tento.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_attribute.png)

    >[!NOTE]
    >Můžete také přidat hello **"Jedinečné ID"** atribut s hello odpovídající hodnotu jako EmployeeID nebo něco jiného, který vyhovuje pro vaši organizaci. Všimněte si, že to není povinný atribut hello; ale můžete přidat příliš identifikovat jedinečná uživatelská hello. 

6. hello tooadd požadované **atributy tokenu SAML**, pro každý řádek v tabulce hello níže, proveďte hello následující kroky:
   
   | Název atributu | Hodnota atributu |
   | ---| --- |
   | Jméno |User.givenName |
   | Příjmení |User.Surname |
   | E-mail |User.Mail |
   
   a. Klikněte na tlačítko **přidat atribut uživatele** tooopen hello **přidat uživatele Attribure** dialogové okno.
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_addattribute.png)
  
   b. V hello **název atributu** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.
  
   c. Z hello **hodnota atributu** seznamu, vyberte hello hodnota atributu zobrazený pro tento řádek.
  
   d. Klikněte na tlačítko **OK**.    

7. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_400.png)

8. tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, obraťte se na [Pluralsight Professional služby](mailTo:professionalservices@pluralsight.com) týmu a zadejte soubor stažený metadat hello.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-pluralsight-test-user"></a>Vytvoření zkušebního uživatele Pluralsight

Hello cílem této části je toocreate volal Britta Simon v Pluralsight uživatele. Spojte se s [tým podpory Pluralsight klienta](mailto:support@pluralsight.com) tooadd hello uživatele v hello Pluralsight účtu. 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooPluralsight toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooPluralsight, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Pluralsight**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.

Když kliknete na dlaždici Pluralsight hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Pluralsight aplikace. Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_203.png

