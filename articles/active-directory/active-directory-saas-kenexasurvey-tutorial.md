---
title: "Kurz: Azure Active Directory integrace s IBM Kenexa průzkum Enterprise | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a IBM Kenexa průzkum Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: cf7ed886b4418ac396ca7056827ee10fd7a19ef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>Kurz: Azure Active Directory integrace s IBM Kenexa průzkum Enterprise

V tomto kurzu zjistíte, jak toointegrate IBM Kenexa průzkum Enterprise s Azure Active Directory (Azure AD).

Integrace IBM Kenexa průzkum Enterprise s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooIBM Kenexa průzkum Enterprise.
- Přihlášení uživatele tooautomatically tooIBM Kenexa průzkum Enterprise můžete povolit pomocí jednotného přihlašování (SSO) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě: hello portálu Azure.

Pokud chcete více informací o softwaru tooknow jako integraci aplikace služby (SaaS) s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s IBM Kenexa průzkum Enterprise, je třeba hello následující položky:

- Předplatné služby Azure AD
- Předplatné služby IBM Kenexa průzkum Enterprise jednotného přihlašování

> [!NOTE]
> Při testování hello kroky v tomto kurzu, doporučujeme vám, nepoužívejte provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu Azure AD jednotného přihlašování k testování v testovacím prostředí. Hello scénáři uvedeném v kurzu hello se skládá ze dvou hlavních stavebních bloků:

* Přidání IBM Kenexa průzkum Enterprise z Galerie hello
* Konfigurace a testování Azure AD SSO

## <a name="add-ibm-kenexa-survey-enterprise-from-hello-gallery"></a>Přidat IBM Kenexa průzkum Enterprise z Galerie hello
tooconfigure hello integrace IBM Kenexa průzkum Enterprise do Azure AD, přidejte IBM Kenexa průzkum Enterprise hello Galerie tooyour seznamu spravovaných aplikací SaaS.

tooadd IBM Kenexa průzkum Enterprise z Galerie hello hello následující:

1. V hello [portál Azure](https://portal.azure.com), v levém podokně text hello, klikněte na hello **Azure Active Directory** tlačítko. 

    ![tlačítko Azure Active Directory Hello][1]

2. Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd aplikaci, klikněte na tlačítko hello **novou aplikaci** tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **IBM Kenexa průzkum Enterprise**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_search.png)

5. V seznamu výsledků hello vyberte **IBM Kenexa průzkum Enterprise**a potom klikněte na hello **přidat** tlačítko tooadd hello aplikace.

    ![IBM Kenexa průzkum Enterprise v seznamu výsledků hello](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD přihlášení SSO se IBM Kenexa průzkum Enterprise podle testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování toowork musí tooidentify hello IBM Kenexa průzkum Enterprise uživatele protějšku ve službě Azure AD. Jinými slovy Azure AD potřeba vytvořit vztah propojení mezi uživatele Azure AD a související uživatelské ve IBM Kenexa průzkum podniku.

tooestablish hello odkaz vztahu přiřadit hodnotu hello hello **uživatelské jméno** ve IBM Kenexa průzkum podniku jako hodnota hello hello **uživatelské jméno** ve službě Azure AD.

tooconfigure a testování Azure AD přihlášení SSO se IBM Kenexa průzkum Enterprise, dokončení hello stavebních bloků v následujících dvou částech hello.

### <a name="configure-azure-ad-sso"></a>Konfigurovat Azure AD jednotného přihlašování

V této části Povolení jednotného přihlašování Azure AD v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci IBM Kenexa průzkum Enterprise díky hello následující:

1. V portálu Azure, na hello hello **IBM Kenexa průzkum Enterprise** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![IBM Kenexa průzkum Enterprise nakonfigurovat jeden přihlašování odkaz][4]

2. V hello **jednotného přihlašování** dialogové okno, v hello **režimu** vyberte **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_samlbase.png)

3. V hello **IBM Kenexa průzkum podnikové domény a adresy URL** část, proveďte následující kroky hello:

    ![IBM Kenexa průzkum podnikové domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_url.png)

    a. V hello **identifikátor** textovému poli, zadejte adresu URL s hello následující vzoru:`https://surveys.kenexa.com/<companycode>`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL s hello následující vzoru:`https://surveys.kenexa.com/<companycode>/tools/sso.asp`

    > [!NOTE] 
    > Hello předchozí hodnoty nejsou skutečné. Je aktualizovat skutečným identifikátorem hello a adresa URL odpovědi. tooobtain hello skutečnými hodnotami, kontaktujte hello [tým podpory IBM Kenexa průzkum Enterprise](https://www.ibm.com/support/home/?lnk=fcw).

4. V části **SAML podpisový certifikát**, klikněte na tlačítko **certifikátu (Base64)**a potom uložte hello certifikát souboru tooyour počítače.

    ![odkaz ke stažení certifikátu (Base64) Hello](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_certificate.png) 

    Hello IBM Kenexa průzkum podniková aplikace očekává tooreceive hello zabezpečení kontrolní výrazy Markup Language (SAML) kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování toohello konfigurace vaší atributů tokenu SAML. Hello hello uživatelský identifikátor deklarace identity v odpovědi hello musí odpovídat hello ID jednotné přihlašování, který je nakonfigurovaný v systému Kenexa hello. toomap hello identifikátor příslušné uživatele ve vaší organizaci jako jednotného přihlašování k Internetu Datagram Protocol (IDP), práce s hello [tým podpory IBM Kenexa průzkum Enterprise](https://www.ibm.com/support/home/?lnk=fcw). 

    Ve výchozím nastavení Azure AD Nastaví uživatelský identifikátor hello jako hodnota hlavní název (UPN) uživatele hello. Můžete změnit tuto hodnotu na hello **atribut** kartě, jak ukazuje následující snímek obrazovky hello. integrace Hello funguje pouze po dokončení hello mapování správně.
    
    ![Dialogové okno uživatelské atributy Hello](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_attribute.png) 

5. Klikněte na **Uložit**.

    ![Hello nakonfigurovat jednotné přihlašování tlačítko Uložit](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_400.png)

6. tooopen hello **konfigurovat přihlášení** okno, v části **IBM Kenexa průzkum podniková konfigurace**, klikněte na tlačítko **konfigurace IBM Kenexa průzkum Enterprise**. 
 
    ![odkaz Konfigurace IBM Kenexa průzkum Enterprise Hello](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_configure.png)

7. Kopírování hello **Sign-Out URL**, **SAML Entity ID**, a **SAML jeden přihlašovací adresa URL služby** hodnoty z hello **Stručná referenční příručka** části.

8. V hello **konfigurovat přihlášení** okno, v části **Stručná referenční příručka**, kopie hello **Sign-Out URL**, **SAML Entity ID**, a  **SAML jeden přihlašovací adresa URL služby** hodnoty.

9. tooconfigure jednotného přihlašování na hello **IBM Kenexa průzkum Enterprise** straně, odesílat hello Stáhnout **certifikátu (Base64)**, **Sign-Out URL**, **SAML Entity ID**, a **SAML jeden přihlašovací adresa URL služby** hodnoty toohello [tým podpory IBM Kenexa průzkum Enterprise](https://www.ibm.com/support/home/?lnk=fcw).

> [!TIP]
> Může odkazovat tooa stručným verzi tyto pokyny v hello [portál Azure](https://portal.azure.com) při nastavujete aplikace hello. Po přidání aplikace hello z hello **služby Active Directory** > **podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotného přihlašování** kartě a potom přejdete na Hello vložených dokumentace prostřednictvím hello **konfigurace** části na konci hello. toolearn Další informace o funkci embedded dokumentace hello, najdete v části [Azure AD vložených dokumentaci](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
V této části vytvoříte testovacího uživatele Britta Simon v hello portálu Azure pomocí tohoto postupu hello následující:

![Vytvořit testovací uživatele Azure AD][100]

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-an-ibm-kenexa-survey-enterprise-test-user"></a>Vytvořit testovací uživatele s IBM Kenexa průzkum Enterprise

V této části vytvoříte uživatele volal Britta Simon v IBM Kenexa průzkum Enterprise. 

toocreate uživatele v hello systému IBM Kenexa průzkum Enterprise a mapy hello ID jednotného přihlašování pro ně, můžete pracovat s hello [tým podpory IBM Kenexa průzkum Enterprise](https://www.ibm.com/support/home/?lnk=fcw). Tato hodnota ID jednotné přihlašování musí být také mapovat toohello hodnota identifikátoru uživatele z Azure AD. Můžete změnit toto výchozí nastavení na hello **atribut** kartě.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolit uživatele Britta Simon toouse jednotného přihlašování k Azure tak, že udělíte přístup tooIBM Kenexa průzkum Enterprise.

![Přiřadit role uživatele hello][200] 

uživatel tooassign Britta Simon tooIBM Kenexa průzkum Enterprise, hello následující:

1. V hello portálu Azure, otevřete hello **aplikace** zobrazit, přejděte toohello **Directory** zobrazit, vyberte možnost **podnikové aplikace, které**a pak klikněte na tlačítko **všechny aplikace**.

    ![Hello "podnikové aplikace" a "Všechny aplikace" odkazy][201] 

2. V hello **aplikace** seznamu, vyberte **IBM Kenexa průzkum Enterprise**.

    ![Hello IBM Kenexa průzkum Enterprise odkaz v seznamu aplikace hello](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_app.png) 

3. V levém podokně hello, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202] 

4. Klikněte na tlačítko hello **přidat** tlačítko a pak na hello **přidat přiřazení** podokně, vyberte **uživatelů a skupin**.

    ![Podokno Přidat přidružení Hello][203]

5. V hello **uživatelů a skupin** dialogové okno, v hello **uživatelé** seznamu, vyberte **Britta Simon**.

6. V hello **uživatelů a skupin** dialogovém okně klikněte na hello **vyberte** tlačítko.

7. V hello **přidat přiřazení** dialogovém okně klikněte na hello **přiřadit** tlačítko.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části otestovat konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.

Když kliknete na tlačítko hello **IBM Kenexa průzkum Enterprise** dlaždice v hello přístupového panelu, můžete by měl být automaticky přihlášeni v tooyour IBM Kenexa průzkum podniková aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů toointegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png

 
