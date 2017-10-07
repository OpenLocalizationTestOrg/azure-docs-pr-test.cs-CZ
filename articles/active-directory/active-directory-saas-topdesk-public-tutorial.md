---
title: "Kurz: Azure Active Directory integrace s TOPdesk - veřejné | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a TOPdesk - veřejné."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: ef0dd06157ecc3b33814590039f5cbae64e8c916
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a>Kurz: Azure Active Directory integrace s TOPdesk - veřejné

V tomto kurzu zjistíte, jak toointegrate TOPdesk - veřejné službou Azure Active Directory (Azure AD).

Integrace TOPdesk - veřejné s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooTOPdesk - veřejné.
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTOPdesk - veřejné (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s TOPdesk - veřejné, budete potřebovat hello následující položky:

- Předplatné služby Azure AD
- TOPdesk - veřejné jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání TOPdesk - veřejné z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-topdesk---public-from-hello-gallery"></a>Přidání TOPdesk - veřejné z Galerie hello
integrace hello tooconfigure TOPdesk - veřejné do služby Azure AD, je nutné tooadd TOPdesk - veřejné hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd TOPdesk - veřejné z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **TOPdesk - veřejné**, vyberte **TOPdesk - veřejné** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![TOPdesk - veřejné v seznamu výsledků hello](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s TOPdesk – veřejné podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v TOPdesk – veřejné je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v TOPdesk - veřejné musí toobe navázat.

V TOPdesk - Public, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a test Azure AD jednotné přihlašování s TOPdesk - veřejné, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření TOPdesk - veřejné testovacího uživatele](#create-a-topdesk---public-test-user)**  - toohave protějšek Britta Simon v TOPdesk - Public, který je propojený toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší TOPdesk - veřejné aplikace.

**tooconfigure Azure AD jednotné přihlašování s TOPdesk - veřejné, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **TOPdesk - veřejné** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_samlbase.png)

3. Na hello **TOPdesk - veřejnou doménu a adresy URL** část, proveďte následující kroky hello:

    ![Jednotné přihlašování informace TOPdesk - veřejnou doménu a adresy URL](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.topdesk.net`
    
    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.topdesk.net/tas/public/login/verify`

    c. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.topdesk.net/tas/public/login/saml`
     
    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL. Adresa URL odpovědi je explaned později v kurzu. Obraťte se na [TOPdesk - tým podpory veřejné klienta](https://help.topdesk.com/saas/enterprise/user/) tooget tyto hodnoty.  

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_400.png)
    
6. Na hello **TOPdesk - veřejné konfigurace** klikněte na tlačítko **konfigurace TOPdesk - veřejné** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![TOPdesk - veřejné konfigurace](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_configure.png) 

7. Přihlaste se tooyour **TOPdesk - veřejné** společnosti lokality jako správce.

8. V hello **TOPdesk** nabídky, klikněte na tlačítko **nastavení**.
   
    ![Nastavení](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "nastavení")

9. Klikněte na tlačítko **nastavení přihlášení**.
   
    ![Nastavení přihlášení](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "nastavení přihlášení")

10. Rozbalte hello **nastavení přihlášení** nabídce a pak klikněte na tlačítko **Obecné**.
   
    ![Obecné](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "obecné")

11. V hello **veřejné** části hello **SAML přihlášení** konfigurace části, proveďte následující kroky hello:
   
    ![Technické nastavení](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "technické nastavení")
   
    a. Klikněte na tlačítko **Stáhnout** toodownload hello veřejné metadata souboru a poté je uložit místně v počítači.
   
    b. Otevřete soubor metadat hello stáhli a vyhledejte hello **AssertionConsumerService** uzlu.

    ![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")
   
    c. Kopírování hello **AssertionConsumerService** hodnotu, vložte tuto hodnotu hello **adresa URL odpovědi** textového pole v **TOPdesk - veřejnou doménu a adresy URL** části.      
   
12. toocreate soubor certifikátu, proveďte následující kroky hello:
    
    ![Certifikát](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "certifikátu")
    
    a. Otevřete hello stáhnout soubor metadat z portálu Azure.
    
    b. Rozbalte hello **RoleDescriptor** uzlu, který má **xsi: type** z **dodáni: ApplicationServiceType**.
    
    c. Zkopírujte hodnotu hello hello **certifikátu x 509** uzlu.
    
    d. Uložit hello zkopírovat **certifikátu x 509** hodnota místně na vašem počítači v souboru.

13. V hello **veřejné** klikněte na tlačítko **přidat**.
    
    ![Přihlašování SAML](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML přihlášení")

14. Na hello **pomocníka konfigurace SAML** dialogové okno proveďte hello následující kroky:
    
    ![Pomocník pro konfigurace SAML](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "pomocníka konfigurace SAML")
    
    a. tooupload stažené metadata souboru z portálu Azure, v části **federačních metadat**, klikněte na tlačítko **Procházet**.

    b. tooupload svůj certifikát v části souboru **certifikát (RSA)**, klikněte na tlačítko **Procházet**.

    c. Soubor loga hello tooupload jste získali od týmu podpory TOPdesk hello, v části **Logo ikonu**, klikněte na tlačítko **Procházet**.

    d. V hello **atribut uživatelského jména** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. V hello **zobrazovaný název** textovému poli, zadejte název pro svou konfiguraci.

    f. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-topdesk---public-test-user"></a>Vytvoření TOPdesk - veřejné testovacího uživatele

V pořadí tooenable Azure AD Uživatelé toolog do TOPdesk - veřejné, musí být zřízená do TOPdesk - veřejné.  
V případě hello TOPdesk - veřejné, zřizování je ruční úloha.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure zřizování uživatelů, proveďte následující kroky hello:
1. Přihlaste se tooyour **TOPdesk - veřejné** společnosti lokality jako správce.

2. V nabídce hello hello nahoře, klikněte na tlačítko **TOPdesk \> nový \> soubory podpory \> osoba**.
   
    ![Osoba](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "osoba")

3. V dialogovém okně hello nové osobě proveďte následující kroky hello:
   
    ![Nové osobě](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "nové osobě")
   
    a. Klikněte na kartu Obecné hello.

    b. V hello **Přezdívka** textovému poli, zadejte příjmení uživatele hello jako Simon
 
    c. Vyberte **lokality** pro účet hello.
 
    d. Klikněte na **Uložit**.

> [!NOTE]
> Můžete použít jiné TOPdesk – nástroje pro tvorbu veřejné uživatelského účtu nebo rozhraní API poskytovaných TOPdesk - veřejné tooprovision Azure AD uživatelské účty.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooTOPdesk - veřejné Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign Britta Simon tooTOPdesk - veřejné, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **TOPdesk - veřejné**.

    ![Hello TOPdesk - veřejné odkaz v seznamu aplikace hello](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello TOPdesk - veřejné dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour TOPdesk - veřejné aplikace.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_203.png

