---
title: 'Kurz: Azure Active Directory integrace s Veracode | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Veracode."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4fe78050-cb6d-4db9-96ec-58cc0779167f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: d17307b3864b7df8ee55f569d8f962e2e315b936
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veracode"></a>Kurz: Azure Active Directory integrace s Veracode

V tomto kurzu zjistíte, jak toointegrate Veracode s Azure Active Directory (Azure AD).

Integrace Veracode s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooVeracode.
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooVeracode (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Veracode tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Veracode jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidat Veracode z Galerie hello
2. Konfigurace a otestování Azure AD jednotné přihlašování

## <a name="add-veracode-from-hello-gallery"></a>Přidat Veracode z Galerie hello
tooconfigure hello integrace Veracode do Azure AD, je nutné tooadd Veracode hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Veracode z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **Veracode**, vyberte **Veracode** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Veracode v seznamu výsledků hello](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Veracode podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Veracode je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Veracode musí toobe navázat.

V Veracode, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Veracode, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Veracode](#create-a-veracode-test-user)**  -toohave protějšek Britta Simon v Veracode, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Veracode.

**tooconfigure Azure AD jednotné přihlašování s Veracode, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Veracode** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_samlbase.png)

3. Na hello **Veracode domény a adresy URL** části uživatel nemá tooperform žádné kroky jako aplikace hello je už předem integrováno s Azure. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_url.png)

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_certificate.png) 

5. Hello cílem této části je toooutline jak tooVeracode tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.

    Aplikace Veracode očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, abyste tooadd vlastních atributů mapování tooyour **atributy tokenu saml** konfigurace. Hello následující snímek obrazovky ukazuje příklad pro tento.
    
    ![Atributy](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "atributy")

6. mapování atributů tooadd hello vyžaduje, proveďte následující kroky hello:

    | Název atributu | Hodnota atributu |
    |--- |--- |
    | FirstName |User.givenName |
    | Příjmení |User.Surname |
    | E-mailu |User.Mail |
    
    a. Pro každý řádek dat v tabulce hello výše, klikněte na tlačítko **přidat atribut uživatele**.
    
    ![Atributy](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "atributy")
    
    ![Atributy](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "atributy")
    
    b. V hello **název atributu** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.
    
    c. V hello **hodnota atributu** textovému poli, hodnota atributu vyberte hello zobrazený pro tento řádek.
    
    d. Klikněte na tlačítko **OK**.

7. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-veracode-tutorial/tutorial_general_400.png)

8. Na hello **Veracode konfigurace** klikněte na tlačítko **konfigurace Veracode** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID** z hello **Stručná referenční příručka části.**

    ![Konfigurace Veracode](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_configure.png) 

9. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Veracode.

10. V nabídce hello hello nahoře, klikněte na tlačítko **nastavení**a potom klikněte na **správce**.
   
    ![Správa](./media/active-directory-saas-veracode-tutorial/ic802911.png "správy")

11. Klikněte na tlačítko hello **SAML** kartě.

12. V hello **organizace SAML nastavení** část, proveďte následující kroky hello:
   
    ![Správa](./media/active-directory-saas-veracode-tutorial/ic802912.png "správy")
   
    a.  V **vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.
    
    b. Klikněte na svůj certifikát stažený z portálu Azure, tooupload **zvolit soubor**.
   
    c. Vyberte **povolit samoobslužné registrace**.

13. V hello **nastavení samoobslužné registrace** část, proveďte hello následující kroky a pak klikněte na tlačítko **Uložit**:
   
    ![Správa](./media/active-directory-saas-veracode-tutorial/ic802913.png "správy")
   
    a. Jako **nové aktivace uživatele**, vyberte **vyžaduje její aktivace ne**.
   
    b. Jako **aktualizace dat uživatele**, vyberte **předvoleb Veracode uživatelská Data**.
   
    c. Pro **SAML atribut podrobnosti**, vyberte hello následující:
      * **Role uživatele**
      * **Správce zásad**
      * **Kontrolora**
      * **Realizace zabezpečení**
      * **Vedení**
      * **Odesílatel**
      * **Tvůrce**
      * **Všechny kontroly typy**
      * **Členství v týmu**
      * **Výchozí tým**

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-veracode-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-veracode-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-veracode-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-veracode-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-veracode-test-user"></a>Vytvoření zkušebního uživatele Veracode
V pořadí tooenable Azure AD Uživatelé toolog do Veracode musí být zřízená do Veracode. V případě hello Veracode zřizování je automatizaci úloh. Neexistuje žádná položka akce za vás. Uživatelé jsou automaticky vytvářeny v případě potřeby během hello první jeden přihlašování pokus.

> [!NOTE]
> Můžete použít všechny ostatní Veracode uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Veracode tooprovision uživatelské účty Azure AD.
> 

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooVeracode toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign Britta Simon tooVeracode, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Veracode**.

    ![v seznamu aplikace hello Hello Veracode odkaz](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Veracode hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Veracode aplikace.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_203.png

