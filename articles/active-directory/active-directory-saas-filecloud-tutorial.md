---
title: 'Kurz: Azure Active Directory integrace s FileCloud | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a FileCloud."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f39f0ddd-b504-4562-971f-77b88d1e75fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: fe58d01f02d6ce99ee9e2f83e7dc72c4434e13b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-filecloud"></a>Kurz: Azure Active Directory integrace s FileCloud

V tomto kurzu zjistíte, jak toointegrate FileCloud s Azure Active Directory (Azure AD).

Integrace FileCloud s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooFileCloud.
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooFileCloud (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s FileCloud tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- FileCloud jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání FileCloud z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-filecloud-from-hello-gallery"></a>Přidání FileCloud z Galerie hello
tooconfigure hello integrace FileCloud do Azure AD, je nutné tooadd FileCloud hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd FileCloud z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **FileCloud**, vyberte **FileCloud** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![FileCloud v seznamu výsledků hello](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s FileCloud podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v FileCloud je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v FileCloud musí toobe navázat.

V FileCloud, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s FileCloud, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele FileCloud](#create-a-filecloud-test-user)**  -toohave protějšek Britta Simon v FileCloud, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci FileCloud.

**tooconfigure Azure AD jednotné přihlašování s FileCloud, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **FileCloud** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_samlbase.png)

3. Na hello **FileCloud domény a adresy URL** část, proveďte následující kroky hello:

    ![FileCloud domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.filecloudhosted.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.filecloudhosted.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory FileCloud klienta](mailto:support@codelathe.com) tooget tyto hodnoty.

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-filecloud-tutorial/tutorial_general_400.png)

6. Na hello **FileCloud konfigurace** klikněte na tlačítko **konfigurace FileCloud** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID** z hello **Stručná referenční příručka části.**

    ![Konfigurace FileCloud](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_configure.png) 

7. V okně prohlížeče jiný web, klienta FileCloud tooyour přihlášení jako správce.

8. V levém navigačním podokně hello, klikněte na tlačítko **nastavení**. 
   
    ![Nastavení oddílu aplikace na straně](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_000.png)

9. Klikněte na tlačítko **jednotného přihlašování k** karty v části nastavení. 
   
    ![Jeden straně přihlašování karta na aplikace](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_001.png)

10. Vyberte **SAML** jako **výchozí typ jednotného přihlašování k** na **jednotné přihlašování na (SSO) nastavení** panelu.
   
    ![Jeden straně přihlašování nastavení panely na aplikace](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_002.png)

11. Vložení **SAML Entity ID**, který jste zkopírovali z portálu Azure do hello **adresa URL koncového bodu IdP** textové pole.

    ![Adresa URL bodu IDP End textové pole](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_003.png)

12. Otevřete váš soubor stažený metadata v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **IdP metadata** textové pole na **SAML nastavení** panelu.

    ![Část dat Meta IDP na straně aplikace](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_004.png)

13. Klikněte na tlačítko **Uložit** tlačítko.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-filecloud-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-filecloud-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-filecloud-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-filecloud-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-filecloud-test-user"></a>Vytvoření zkušebního uživatele FileCloud

Hello cílem této části je toocreate volal Britta Simon v FileCloud uživatele. FileCloud podporuje za běhu zřizování, který je ve výchozím nastavení povolené. Neexistuje žádná položka akce pro vás v této části. Pokud ještě neexistuje, vytvoří se nový uživatel během pokusu o tooaccess FileCloud.

>[!NOTE]
>Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory FileCloud klienta](mailto:support@codelathe.com). 

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooFileCloud toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign Britta Simon tooFileCloud, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **FileCloud**.

    ![v seznamu aplikace hello Hello FileCloud odkaz](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.

Když kliknete na dlaždici FileCloud hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour FileCloud aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_203.png

