---
title: 'Kurz: Integrovat Azure Active Directory vxMaintain | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a vxMaintain."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 841a1066-593c-4603-9abe-f48496d73d10
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 937ea276d898986fc5a953c96fddabdc8940309f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-vxmaintain"></a>Kurz: Integrovat vxMaintain Azure Active Directory

V tomto kurzu zjistíte, jak toointegrate vxMaintain s Azure Active Directory (Azure AD).

Tato integrace poskytuje několik výhod důležité. Můžete:

- Ovládací prvek ve službě Azure AD, který má přístup k toovxMaintain.
- Povolte přihlášení uživatelů tooautomatically toovxMaintain s jednotné přihlašování (SSO) prostřednictvím jejich účty Azure AD.
- Spravovat účty v jednom centrálním místě: hello portálu Azure.

toolearn Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s vxMaintain tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- VxMaintain předplatné povolené jednotné přihlašování

> [!NOTE]
> Při testování hello kroky v tomto kurzu, doporučujeme vám, nepoužívejte provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. 

Hello scénář, který tento kurz popisuje se skládá ze dvou hlavních stavebních bloků:

* Přidání vxMaintain z Galerie hello
* Konfigurace a testování Azure AD jednotného přihlašování

## <a name="add-vxmaintain-from-hello-gallery"></a>Přidat vxMaintain z Galerie hello
integrace hello tooconfigure vxMaintain s Azure AD, je nutné tooadd vxMaintain hello Galerie tooyour seznamu spravovaných aplikací SaaS.

tooadd vxMaintain z Galerie hello hello následující:

1. V hello [portál Azure](https://portal.azure.com), v levém podokně text hello, vyberte hello **Azure Active Directory** tlačítko. 

    ![tlačítko Azure Active Directory Hello][1]

2. Vyberte **podnikové aplikace, které** > **všechny aplikace**.

    ![podokno "Podnikové aplikace" Hello][2]
    
3. tooadd aplikace, v hello **všechny aplikace** dialogové okno, vyberte **novou aplikaci**.

    ![Hello "Nové aplikace" tlačítko][3]

4. Hello vyhledávacího pole zadejte **vxMaintain**.

    ![Hello "Jednoho přihlašování v režimu" rozevíracího seznamu](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_search.png)

5. V seznamu výsledků hello vyberte **vxMaintain**a potom vyberte **přidat**.

    ![odkaz vxMaintain Hello](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD SSO pomocí vxMaintain, podle testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování toowork musí tooknow hello vxMaintain protějšku toohello uživatele Azure AD. To znamená je potřeba vytvořit vztah propojení mezi hello uživatele Azure AD a hello odpovídající vxMaintain uživatele.

tooestablish hello odkaz vztah, přiřaďte hello vxMaintain **uživatelské jméno** hodnotu jako hello Azure AD **uživatelské jméno** hodnotu.

tooconfigure a testování Azure AD jednotného přihlašování pomocí vxMaintain, dokončení hello následující stavební bloky.

### <a name="configure-azure-ad-sso"></a>Konfigurovat Azure AD jednotného přihlašování

V této části můžete povolit jednotné přihlašování Azure AD v hello portálu Azure i nakonfigurovat jednotné přihlašování v aplikaci vxMaintain díky hello následující:

1. V portálu Azure, na hello hello **vxMaintain** stránky integrace aplikací, vyberte **jednotného přihlašování**.

    ![příkaz "Jednotného přihlašování" Hello][4]

2. tooenable jednotné přihlašování, v hello **režimu přihlašování** rozevíracího seznamu vyberte **na základě SAML přihlašování**.
 
    ![Hello příkaz "na základě SAML přihlášení"](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

3. V části **vxMaintain domény a adresy URL**, hello následující:

    ![Hello vxMaintain oddílu domény a adresy URL](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    a. V hello **identifikátor** pole, zadejte adresu URL, která má hello následující syntaxi:`https://<company name>.verisae.com`

    b. V hello **adresa URL odpovědi** pole, zadejte adresu URL, která má hello následující syntaxi:`https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`

    > [!NOTE] 
    > Hello předchozí hodnoty nejsou skutečné. Je aktualizovat skutečným identifikátorem hello a adresa URL odpovědi. tooobtain hello hodnoty, kontaktujte hello [tým podpory vxMaintain](http://www.verisae.com/contact-us).
 
4. V části **SAML podpisový certifikát**, vyberte **soubor XML s metadaty**a potom uložte hello metadata souboru tooyour počítače.

    ![Hello část "SAML podpisový certifikát"](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

5. Vyberte **Uložit**.

    ![tlačítko Uložit Hello](./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_400.png)

6. tooconfigure **vxMaintain** jednotné přihlašování, odeslání hello Stáhnout **soubor XML s metadaty** souboru toohello [tým podpory vxMaintain](http://www.verisae.com/contact-us).

> [!TIP]
> Při nastavování aplikace hello, si můžete přečíst stručným verzi hello předchozích pokynů v hello [portál Azure](https://portal.azure.com). Po přidání aplikace hello z hello **služby Active Directory** > **podnikové aplikace, které** části, vyberte hello **jednotné přihlašování** kartě a potom hello přístup vložené dokumentace z hello **konfigurace** části. 
>
>toolearn Další informace o funkci embedded dokumentace hello, najdete v části [Správa jednotného přihlašování pro podnikové aplikace](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
V této části vytvoříte testovacího uživatele Britta Simon v hello portálu Azure pomocí tohoto postupu hello následující:

![Hello Azure AD testovacího uživatele][100]

1. V hello **portál Azure**, v levém podokně text hello, vyberte hello **Azure Active Directory** tlačítko.

    ![tlačítko "Azure Active Directory" Hello](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_01.png) 

2. toodisplay seznam uživatelů, přejděte příliš**uživatelů a skupin** > **všichni uživatelé**.
    
    ![Hello "Všichni uživatelé" odkaz](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)  
    Hello **všichni uživatelé** otevře se dialogové okno. 

3. tooopen hello **uživatele** dialogové okno, vyberte **přidat**.
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_03.png) 

4. V hello **uživatele** dialogové okno pole, hello následující:
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_04.png) 

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu testovacího uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtávací políčko a potom Poznámka hello hodnotu, která byla vygenerována v hello **heslo** pole.

    d. Vyberte **Vytvořit**.
 
### <a name="create-a-vxmaintain-test-user"></a>Vytvoření zkušebního uživatele vxMaintain

V této části vytvoříte testovacího uživatele Britta Simon v vxMaintain. Uživatelé tooadd hello vxMaintain platformy, pracovat s [tým podpory vxMaintain](http://www.verisae.com/contact-us). Před použitím jednotného přihlašování k vytvoření a aktivace uživatele hello.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup toovxMaintain testovacího uživatele Britta Simon toouse jednotného přihlašování k Azure. toodo tedy hello následující:

![Testovací uživatel v seznamu zobrazovaný název hello][200] 

1. V portálu Azure hello **aplikace** zobrazit, přejděte příliš**Directory** zobrazení > **podnikové aplikace, které** > **všechny aplikace**.

    ![Hello "Všechny aplikace" odkaz][201] 

2. V hello **aplikace** seznamu, vyberte **vxMaintain**.

    ![odkaz vxMaintain Hello](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

3. V levém podokně hello vyberte **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202] 

4. Vyberte **přidat** a pak na hello **přidat přiřazení** podokně, vyberte **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][203]

5. V hello **uživatelů a skupin** dialogové okno, v hello **uživatelé** seznamu, vyberte **Britta Simon**a potom vyberte hello **vyberte** tlačítko.

7. V hello **přidat přiřazení** dialogové okno, vyberte **přiřadit**.
    
### <a name="test-your-azure-ad-single-sign-on"></a>Testování vaší služby Azure AD jednotné přihlašování

V této části otestovat konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.

Výběr hello **vxMaintain** dlaždice v hello Panel přístupu by se měl přihlásit můžete tooyour vxMaintain aplikace automaticky.

Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="next-steps"></a>Další kroky

* [Seznam kurzů k integraci aplikací SaaS v Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_203.png

