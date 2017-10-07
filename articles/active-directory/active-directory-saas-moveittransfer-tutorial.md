---
title: "Kurz: Azure Active Directory integrace s přenosy MOVEit – integrace Azure AD | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a přenos MOVEit – integrace Azure AD."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 5bbe4f2d952bd45c4d58d55ffc3467b4eb871fd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a>Kurz: Azure Active Directory integrace s přenosy MOVEit – integrace Azure AD

V tomto kurzu zjistíte, jak toointegrate MOVEit přenos – integrace Azure AD s Azure Active Directory (Azure AD).

Integrace MOVEit přenos – Azure AD integraci s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooMOVEit přenos – integrace Azure AD.
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooMOVEit přenos – integrace Azure AD (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s přenosy MOVEit – integrace služby Azure AD, je třeba hello následující položky:

- Předplatné služby Azure AD
- Přenos MOVEit – Azure AD integrace jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání MOVEit přenos – integrace Azure AD z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-moveit-transfer---azure-ad-integration-from-hello-gallery"></a>Přidání MOVEit přenos – integrace Azure AD z Galerie hello
integrace hello tooconfigure MOVEit přenos – integrace Azure AD do služby Azure AD, je nutné tooadd MOVEit přenos – integrace Azure AD hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd MOVEit přenos – integrace Azure AD z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **MOVEit přenos – integrace Azure AD**, vyberte **MOVEit přenos – integrace Azure AD** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Přenos MOVEit – integrace Azure AD v seznamu výsledků hello](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části konfiguraci a testování Azure AD jednotné přihlašování s přenosy MOVEit – integrace Azure AD na základě testovací uživatele, nazývá "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow uživatele hello protějškem v MOVEit přenos – integrace Azure AD je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v MOVEit přenos – integrace Azure AD musí toobe navázat.

V MOVEit přenos – integrace služby Azure AD, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testování Azure AD jednotné přihlašování s přenosy MOVEit – integrace služby Azure AD, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření MOVEit přenos – Azure AD integrace testovacího uživatele](#create-a-moveit-transfer---azure-ad-integration-test-user)**  - toohave protějšek Britta Simon při přenosu MOVEit - integraci Azure AD, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v MOVEit přenos – integrace aplikace Azure AD.

**tooconfigure Azure AD jednotné přihlašování s přenosy MOVEit – integrace služby Azure AD, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **MOVEit přenos – integrace Azure AD** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

3. Na hello **MOVEit přenos – integrace Azure AD domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://contoso.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://contoso.com/<tenatid>`

    c. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`    
     
    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL. Najdete tyto hodnoty později v **adresa URL služby zprostředkovatele metadat** části nebo kontaktujte [MOVEit přenos – tým podpory pro Azure AD integrace klienta](https://community.ipswitch.com/s/support) tooget tyto hodnoty.

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_400.png)
    
6. Přihlášení tooyour MOVEit přenos klienta jako správce.

7. V levém navigačním podokně hello, klikněte na tlačítko **nastavení**.

    ![Nastavení oddílu na aplikaci straně](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_000.png)

8. Klikněte na tlačítko **jeden Sign-on** odkaz, což je pod **zásady zabezpečení -> Auth uživatele**.

    ![Zabezpečení straně zásady na aplikace](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_001.png)

9. Klikněte na tlačítko hello adresu URL metadat odkaz toodownload hello dokument metadat.

    ![Adresa URL služby zprostředkovatele metadat](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * Ověřte **entityID** odpovídá **identifikátor** v hello **MOVEit přenos – integrace Azure AD domény a adresy URL** části.
    * Ověřte **AssertionConsumerService** umístění URL odpovídá **adresa URL odpovědi** v hello **MOVEit přenos – integrace Azure AD domény a adresy URL** části.
    
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_007.png)

10. Klikněte na tlačítko **přidat zprostředkovatele Identity** tlačítko tooadd nového poskytovatele federovaných identit.

    ![Přidejte zprostředkovatele Identity](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_003.png)

11. Klikněte na tlačítko **Procházet...**  tooselect hello metadata souboru, který jste si stáhli z portálu Azure a pak klikněte na **přidat zprostředkovatele Identity** tooupload hello stáhli soubor.

    ![Poskytovatele Identity SAML](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_004.png)

12. Vyberte "**Ano**" jako **povoleno** v hello **upravit nastavení federovaný zprostředkovatele Identity...**  a klikněte na tlačítko **Uložit**.

    ![Nastavení zprostředkovatele federovaných identit](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_005.png)

13. V hello **Upravit federované Identity uživatele nastavení zprostředkovatele** proveďte hello následující akce:
    
    ![Upravit nastavení zprostředkovatele federovaných identit](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    a. Vyberte **SAML NameID** jako **přihlašovací jméno**.
    
    b. Vyberte **jiných** jako **úplný název** a v hello **název atributu** textbox vložte hodnotu hello: `http://schemas.microsoft.com/identity/claims/displayname`.
    
    c. Vyberte **jiných** jako **e-mailu** a v hello **název atributu** textbox vložte hodnotu hello: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    d. Vyberte **Ano** jako **automatické vytvoření účtu na Sign-on**.
    
    e. Klikněte na tlačítko **Uložit** tlačítko.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a>Vytvoření MOVEit přenos – Azure AD integrace testovacího uživatele

Hello cílem této části je toocreate uživatel volal Britta Simon v MOVEit přenos – integrace Azure AD. Přenos MOVEit – integrace Azure AD podporuje zřizování za běhu, které jste povolili. Neexistuje žádná položka akce pro vás v této části. Nový uživatel se vytvoří během tooaccess pokusu o přenos MOVEit – integrace Azure AD, pokud ještě neexistuje.

>[!NOTE]
>Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [MOVEit přenos – tým podpory pro Azure AD integrace klienta](https://community.ipswitch.com/s/support).

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooMOVEit přenos – integrace Azure AD toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign tooMOVEit Britta Simon přenos – integrace služby Azure AD, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **MOVEit přenos – integrace Azure AD**.

    ![Hello MOVEit přenos – integrace Azure AD na odkaz v seznamu aplikace hello](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.

Když kliknete na tlačítko hello MOVEit přenos – dlaždici integrace Azure AD v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour MOVEit přenos – integrace aplikace Azure AD. 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_203.png

