---
title: 'Kurz: Azure Active Directory integrace s Cezanne HR softwarem | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Cezanne HR softwarem a Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fb8f95bf-c3c1-4e1f-b2b3-3b67526be72d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 3675acd8871d62c2277def8074f7aa39ac46e2a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-cezanne-hr-software"></a>Kurz: Integrate Azure Active Directory s Cezanne HR softwaru

V tomto kurzu zjistíte, jak toointegrate Cezanne HR software s Azure Active Directory (Azure AD).

Integrace Cezanne HR softwaru s Azure AD poskytuje následující výhody hello. Můžete:

- Řízení ve službě Azure AD, který má přístup tooCezanne HR softwaru.
- Povolte přihlášení uživatelů tooautomatically tooCezanne HR software s jednotné přihlašování (SSO) s jejich účty Azure AD.
- Spravovat účty v jednom centrálním místě: hello portálu Azure.

toolearn Další informace o softwaru, služba (SaaS) aplikace integraci s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace služby Azure AD s Cezanne HR softwaru, je třeba hello následující položky:

- Předplatné služby Azure AD
- Software Cezanne HR předplatné povolené jednotné přihlašování

> [!NOTE]
> Doporučujeme tootest hello kroky v tomto kurzu, nepoužívejte provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:

- Nepoužívejte produkční prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu Azure AD jednotného přihlašování k testování v testovacím prostředí. 

Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

* Přidání softwaru Cezanne HR z Galerie hello
* Konfigurace a testování Azure AD SSO

## <a name="add-cezanne-hr-software-from-hello-gallery"></a>Přidat software Cezanne HR z Galerie hello
integrace hello tooconfigure Cezanne HR softwaru do služby Azure AD, přidejte Cezanne HR softwaru hello Galerie tooyour seznamu spravovaných aplikací SaaS.

tooadd Cezanne HR softwaru z Galerie hello hello následující:

1. V hello  **[portál Azure](https://portal.azure.com)**, v levém podokně text hello, vyberte hello **Azure Active Directory** tlačítko. 

    ![tlačítko "Azure Active Directory" Hello][1]

2. Vyberte **podnikové aplikace, které** > **všechny aplikace**.

    ![Hello "Všechny aplikace" odkaz][2]
    
3. tooadd novou aplikaci, hello horní části hello **všechny aplikace** dialogové okno, vyberte **novou aplikaci**.

    ![Hello "Nové aplikace" tlačítko][3]

4. Hello vyhledávacího pole zadejte **Cezanne HR softwaru**.

    ![Hello vyhledávacího pole](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_search.png)

5. V seznamu výsledků hello vyberte **Cezanne HR softwaru** a pak vyberte hello **přidat** tlačítko tooadd hello aplikace.

    ![seznam výsledků Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části můžete nakonfigurovat a otestovat jednotného přihlašování k AD Azure s Cezanne HR software založený na testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování toowork musí tooknow hello Cezanne HR softwaru protějšku toohello uživatele Azure AD. Jinými slovy je potřeba vytvořit vztah propojení mezi uživatele Azure AD a související uživatelské hello v hello Cezanne HR softwaru.

tooestablish hello odkaz vztahu hello přiřazení softwaru Cezanne HR **uživatelské jméno** hodnotu jako hello Azure AD **uživatelské jméno** hodnotu.

tooconfigure a testování Azure AD jednotného přihlašování pomocí softwaru Cezanne HR dokončení hello následující stavební bloky.

### <a name="configure-azure-ad-sso"></a>Konfigurovat Azure AD jednotného přihlašování

V této části můžete povolení jednotného přihlašování Azure AD v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Cezanne HR softwaru pomocí tohoto postupu hello následující:

1. V portálu Azure, na hello hello **Cezanne HR softwaru** stránky integrace aplikací, vyberte **jednotného přihlašování**.

    ![příkaz "Jednotného přihlašování" Hello][4]

2. tooenable jednotné přihlašování, v hello **jednotného přihlašování** dialogové okno, vyberte hello **režimu** jako **na základě SAML přihlašování**.
 
    ![pole "Režim" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. V části **Cezanne HR softwaru domény a adresy URL**, hello následující:

    ![část "Cezanne HR softwaru domény adresy URL a" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    a. V hello **přihlašovací adresa URL** pole, zadejte adresu URL, která má hello následující syntaxi:`https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`

    b. V hello **adresa URL odpovědi** pole, zadejte adresu URL, která má hello následující syntaxi:`https://w3.cezanneondemand.com:443/<tenantid>`    
     
    > [!NOTE] 
    > Hello předchozí hodnoty nejsou skutečné. Adresa URL hello skutečné odpovědi a hello přihlašovací adresa URL je aktualizujte. tooobtain hello hodnoty, kontaktujte hello [tým podpory klientský software Cezanne HR](mailto:info@cezannehr.com).

4. V části **SAML podpisový certifikát**, vyberte **certifikátu (Base64)**a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Hello část "SAML podpisový certifikát"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. Vyberte **Uložit**.

    ![tlačítko "Uložit" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)
    
6. V části **Cezanne HR softwarové konfigurace**, vyberte **konfigurace softwaru HR Cezanne** tooopen hello **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID** a **SAML-služby přihlášení** adresa URL z hello **Stručná referenční příručka** části.

    ![Hello "Cezanne HR softwaru konfigurace"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png) 

7. V okně prohlížeče jiných webových přihlaste tooyour Cezanne HR softwaru klienta jako správce.

8. V levém podokně hello vyberte **nastavení systému**. Vyberte **nastavení zabezpečení** > **jednotné přihlašování konfigurace**.

    ![odkaz "Jednoho přihlášení konfigurace" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

9. V hello **povolit uživatelům toolog pomocí hello následující služby Jednotné přihlašování (SSO)** podokně, vyberte hello **SAML 2.0** zaškrtávací políčko a vyberte hello **Upřesnit konfiguraci** možnost.

    ![Jednotné přihlašování možnosti služeb](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

10. Vyberte **přidat nové**.

    ![tlačítko "Přidat nové" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

11. V části **zprostředkovatelů Identity SAML 2.0**, hello následující:

    ![část "Zprostředkovatelů Identity SAML 2.0" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    a. V hello **zobrazovaný název** zadejte hello název zprostředkovatele identity.

    b. V hello **identifikátor Entity** pole, vložte hello **SAML Entity ID** který jste zkopírovali ze hello portálu Azure. 

    c. V hello **SAML vazby** pole se seznamem, vyberte **POST**.

    d. V hello **koncový bod služby tokenu zabezpečení** pole, vložte hello **SAML-služby přihlášení** adresu URL, kterou jste zkopírovali z hello portálu Azure. 
    
    e. V hello **název atributu ID uživatele** zadejte `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    f. tooupload hello stáhnout certifikát z Azure AD, vyberte hello **nahrát** tlačítko.
    
    g. Vyberte **OK**. 

12. Vyberte **Uložit**.

    ![tlačítko "Uložit" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> Při nastavování aplikace hello, si můžete přečíst stručným verzi hello předchozích pokynů v hello [portál Azure](https://portal.azure.com). Po přidání aplikace hello z hello **služby Active Directory** > **podnikové aplikace, které** části, vyberte hello **jednotného přihlašování** kartě. Potom přístup hello vložených dokumentace z hello **konfigurace** části. 

toolearn Další informace o funkci embedded dokumentace hello, najdete v části [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
V této části vytvoříte testovacího uživatele Britta Simon v hello portálu Azure.

![Hello testovacího uživatele Britta Simon][100]

toocreate testovacího uživatele ve službě Azure AD, hello následující:

1. V hello **portál Azure**, v levém podokně text hello, vyberte hello **Azure Active Directory** tlačítko.

    ![tlačítko "Azure Active Directory" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, vyberte **uživatelů a skupin** > **všichni uživatelé**.
    
    ![Hello "Všichni uživatelé" odkaz](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png) 
    
    Hello **všichni uživatelé** otevře se dialogové okno.

3. tooopen hello **uživatele** dialogové okno, vyberte **přidat**.
 
    ![tlačítko "Přidat" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png) 

4. V hello **uživatele** dialogové okno pole, hello následující:
 
    ![Dialogové okno "User" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png) 

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole, zadejte uživatele Britta Simon **e-mailová adresa**.

    c. Vyberte hello **zobrazit hesla** zaškrtávací políčko a potom Poznámka hello hodnotu, která byla vygenerována v hello **heslo** pole.

    d. Vyberte **Vytvořit**.
 
### <a name="create-a-cezanne-hr-software-test-user"></a>Vytvoření zkušebního uživatele Cezanne HR softwaru

Uživatelé toosign tooenable Azure AD v softwaru tooCezanne oddělení lidských zdrojů, se musí být zřízená do Cezanne HR softwaru. V případě hello Cezanne HR softwaru zřizování je ruční úloha.

Poskytnutí uživatelského účtu pomocí tohoto postupu hello následující:

1.  Přihlaste se tooyour Cezanne HR softwaru společnosti lokality jako správce.

2.  V levém podokně hello vyberte **nastavení systému** > **spravovat uživatele** > **přidat nové uživatele**.

    ![odkaz "Přidat nový uživatel" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "nového uživatele")

3.  V části **osoba podrobnosti**, hello následující:

    ![Hello části osoba detaily.](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "nového uživatele")
    
    a. Nastavit **interní uživatele** jako **OFF**.
    
    b. V hello **křestní jméno** pole, typ hello křestní jméno uživatele, například **Britta**.  
 
    c. V hello **příjmení** pole, typ hello příjmení uživatele, například **Simon**.
    
    d. V hello **e-mailu** hello uživatele e-mailovou adresu, zadejte například Brittasimon@contoso.com.

4.  V části **informace o účtu**, hello následující:

    ![Hello část "Informace o účtu"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "nového uživatele")
    
    a. V hello **uživatelské jméno** hello uživatele e-mailovou adresu, zadejte například Brittasimon@contoso.com.
    
    b. V hello **heslo** zadejte heslo uživatele hello.
    
    c. V hello **Role zabezpečení** vyberte **HR Professional**.
    
    d. Vyberte **OK**.

5. Na hello **jednotného přihlašování** na kartě hello **SAML 2.0 identifikátory** vyberte **přidat nové**.

    ![tlačítko "Přidat nové" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "uživatele")

6. V hello **zprostředkovatele Identity** vyberte zprostředkovatele identity. V hello **uživatelský identifikátor** zadejte hello e-mailovou adresu pro testovací uživatele Britta Simon na účet.

    ![Hello polí "Zprostředkovatele Identity" a "Uživatelský identifikátor"](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "uživatele")
    
7. Vyberte **Uložit**.

    ![tlačítko "Uložit" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "uživatele")

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte testovacího uživatele Britta Simon toouse Azure jednotného přihlašování k udělení přístupu tooCezanne HR softwaru.

![Test přístupu uživatele][200] 

1. V hello portálu Azure otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení. Vyberte **podnikové aplikace, které** > **všechny aplikace**.

    ![Hello "Všechny aplikace" odkaz][201] 

2. V seznamu aplikace hello vyberte **Cezanne HR softwaru**.

    ![seznam "Aplikací" Hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png) 

3. V nabídce hello na levé straně hello vyberte **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Vyberte **Přidat**. Potom v hello **přidat přiřazení** dialogové okno, vyberte **uživatelů a skupin**.

    !["Uživatelé a skupiny" odkaz][203]

5. V hello **uživatelů a skupin** dialogové okno, v hello **uživatelé** seznamu, vyberte **Britta Simon**.

6. V hello **uživatelů a skupin** dialogové okno, vyberte **vyberte**.

7. V hello **přidat přiřazení** dialogové okno, vyberte **přiřadit**.
    
### <a name="test-sso"></a>Test jednotného přihlašování

V této části otestovat konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.

Když vyberete hello Cezanne HR softwaru dlaždice v hello přístupového panelu, přihlásit automaticky tooyour Cezanne HR softwarová aplikace.

## <a name="next-steps"></a>Další kroky

* [Seznam kurzů toointegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png

