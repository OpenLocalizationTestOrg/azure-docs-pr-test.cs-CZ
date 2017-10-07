---
title: 'Kurz: Azure Active Directory integrace s Asana | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Asana."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 837e38fe-8f55-475c-87f4-6394dc1fee2b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: jeedes
ms.openlocfilehash: 9ac35dedc809b2b3dfb461d92a5afb066ccdedde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Kurz: Azure Active Directory integrace s Asana

V tomto kurzu zjistíte, jak toointegrate Asana s Azure Active Directory (Azure AD).

Integrace Asana s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooAsana
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAsana (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Asana tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Asana jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Asana z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-asana-from-hello-gallery"></a>Přidání Asana z Galerie hello
tooconfigure hello integrace Asana do Azure AD, je nutné tooadd Asana hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Asana z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **Asana**, vyberte **Asana** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asana-tutorial/tutorial_asana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Asana podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Asana je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Asana musí toobe navázat.

V Asana, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Asana, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvořit testovací uživatele s Asana](#create-an-asana-test-user)**  -toohave protějšek Britta Simon v Asana, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Asana.

**tooconfigure Azure AD jednotné přihlašování s Asana, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Asana** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-asana-tutorial/tutorial_asana_samlbase.png)

3. Na hello **Asana domény a adresy URL** část, proveďte následující kroky hello:

    ![Asana domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-asana-tutorial/tutorial_asana_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadat adresu URL:`https://app.asana.com/`

    b. V hello **identifikátor** textovému poli, hodnota typu:`https://app.asana.com/`
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-asana-tutorial/tutorial_asana_certificate.png)
    
5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-asana-tutorial/tutorial_general_400.png)

6. Na hello **Asana konfigurace** klikněte na tlačítko **konfigurace Asana** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurace Asana](./media/active-directory-saas-asana-tutorial/tutorial_asana_configure.png) 

7. V okně jiný prohlížeč, přihlášení tooyour Asana aplikace. tooconfigure jednotné přihlašování v Asana, nastavení pracovního prostoru hello přístup kliknutím na název pracovního prostoru hello na hello pravém horním rohu úvodní obrazovka. Potom klikněte na  **\<název pracovního prostoru\> nastavení**. 
   
    ![Nastavení jednotného přihlašování k Asana](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

8. Na hello **nastavení organizace** okně klikněte na tlačítko **správy**. Potom klikněte na **členy musí přihlásit pomocí SAML** tooenable hello jednotné přihlašování konfigurace. Hello proveďte hello následující kroky:
   
    ![Konfigurace nastavení jednotný přihlášení](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)  

     a. V hello **přihlašovací adresa URL stránky** textovému poli, vložte hello **SAML jeden přihlašování adresa URL služby**.

     b. Klikněte pravým tlačítkem na certifikát hello si stáhli z portálu Azure a pak otevřete soubor certifikátu hello pomocí poznámkového bloku nebo upřednostňovaný textový editor. Kopie hello obsahu mezi hello začít a hello název certifikátu koncovým a vložte ji do hello **certifikát X.509** textové pole.

9. Klikněte na **Uložit**. Přejděte příliš[Asana Průvodce pro nastavení jednotného přihlašování k](https://asana.com/guide/help/premium/authentication#gl-saml) Pokud potřebujete další pomoc.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-asana-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-asana-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-an-asana-test-user"></a>Vytvořit uživatele s Asana testu

V této části vytvoříte volal Britta Simon v Asana uživatele.

1. Na **Asana**, přejděte toohello **týmy** části na levém panelu hello. Klikněte na tlačítko hello plus tlačítko přihlásit. 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. Zadejte e-mailu hello britta.simon@contoso.com v hello textového pole a pak vyberte **pozvat**.

3. Klikněte na tlačítko **odeslat pozvání**. Nový uživatel Hello obdrží e-mail do své e-mailový účet. Uživatel bude muset toocreate a ověření účtu hello.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooAsana toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200]

**tooassign Britta Simon tooAsana, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Asana**.

    ![v seznamu aplikace hello Hello Asana odkaz](./media/active-directory-saas-asana-tutorial/tutorial_asana_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

Hello cílem této části je tootest vaší služby Azure AD jednotné přihlašování.

Přejděte tooAsana přihlašovací stránku. V textovém poli hello e-mailovou adresu vložit hello e-mailovou adresu britta.simon@contoso.com. Ponechte textové hello hesla v prázdné a pak klikněte na **protokolu v**. Bude přesměrované tooAzure AD přihlašovací stránku. Dokončení přihlašovací údaje Azure AD. Nyní jste přihlášeni Asana.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
