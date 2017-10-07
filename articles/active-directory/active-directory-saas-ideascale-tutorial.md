---
title: 'Kurz: Azure Active Directory integrace s IdeaScale | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a IdeaScale."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 10722b137e7565ee165e73994fd5a60b994719bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Kurz: Azure Active Directory integrace s IdeaScale

V tomto kurzu zjistíte, jak toointegrate IdeaScale s Azure Active Directory (Azure AD).

Integrace IdeaScale s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooIdeaScale
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooIdeaScale (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s IdeaScale tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- IdeaScale jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání IdeaScale z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-ideascale-from-hello-gallery"></a>Přidání IdeaScale z Galerie hello
tooconfigure hello integrace IdeaScale do Azure AD, je nutné tooadd IdeaScale hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd IdeaScale z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **IdeaScale**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_search.png)

5. Na panelu výsledků hello vyberte **IdeaScale**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s IdeaScale podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v IdeaScale je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v IdeaScale musí toobe navázat.

V IdeaScale, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s IdeaScale, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele IdeaScale](#creating-an-ideascale-test-user)**  -toohave protějšek Britta Simon v IdeaScale, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci IdeaScale.

**tooconfigure Azure AD jednotné přihlašování s IdeaScale, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **IdeaScale** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_samlbase.png)

3. Na hello **IdeaScale domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.ideascale.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory IdeaScale klienta](http://support.ideascale.com/) tooget tyto hodnoty. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_general_400.png)

6. Na hello **IdeaScale konfigurace** klikněte na tlačítko **konfigurace IdeaScale** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresy URL a SAML Entity ID** z hello **Stručná referenční příručka části**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti IdeaScale tooyour.

8. Přejděte příliš**komunity nastavení**.
   
    ![Nastavení komunity](./media/active-directory-saas-ideascale-tutorial/ic790847.png "nastavení komunity")

9. Přejděte příliš**zabezpečení \> nastavení jednotného Sign-on**.
   
    ![Jednoduchého nastavení Sign-on](./media/active-directory-saas-ideascale-tutorial/ic790848.png "jednoduchého nastavení Sign-on")

10. Jako **typu Single-Sign-on**, vyberte **SAML 2.0**.
   
    ![Jeden typ Sign-on](./media/active-directory-saas-ideascale-tutorial/ic790849.png "jeden typ Sign-on")

11. Na hello **nastavení jednotného Sign-on** dialogové okno, proveďte následující kroky hello:
   
    ![Jednoduchého nastavení Sign-on](./media/active-directory-saas-ideascale-tutorial/ic790850.png "jednoduchého nastavení Sign-on")
   
    a. V **SAML IdP Entity ID** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.

    b. Kopírovat obsah hello souboru metadat stažený z portálu Azure a vložte jej do hello **SAML IdP Metadata** textové pole.

    c. V **adresy URL odhlašovací úspěch** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.

    d. Klikněte na tlačítko **uložit změny**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-ideascale-test-user"></a>Vytváření testovacího uživatele IdeaScale

Uživatelé toolog tooenable Azure AD do IdeaScale, se musí být zřízená v tooIdeaScale. V případě hello IdeaScale zřizování je ruční úloha.

**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**

1. Přihlaste se tooyour **IdeaScale** společnosti lokality jako správce.

2. Přejděte příliš**komunity nastavení**.
   
    ![Nastavení komunity](./media/active-directory-saas-ideascale-tutorial/ic790847.png "nastavení komunity")

3. Přejděte příliš**základní nastavení \> člen správu**.

4. Klikněte na tlačítko **přidat člena**.
   
    ![Člen správu](./media/active-directory-saas-ideascale-tutorial/ic790852.png "člen správy")

5. V části Přidání nového člena hello proveďte následující kroky hello:
   
    ![Přidání nového člena](./media/active-directory-saas-ideascale-tutorial/ic790853.png "přidání nového člena")
   
    a. V hello **e-mailové adresy** textovému poli, typ hello e-mailovou adresu chcete tooprovision platného účtu AAD.
   
    b. Klikněte na tlačítko **uložit změny**. 
   
    >[!NOTE]
    >Držitel účtu Azure Active Directory Hello získá e-mail s účtem odkaz tooconfirm hello předtím, než se stane aktivní.
      
>[!NOTE]
>Můžete použít všechny ostatní IdeaScale uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované IdeaScale tooprovision AAD uživatelské účty.
 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooIdeaScale toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooIdeaScale, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **IdeaScale**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování


Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.

Když kliknete na dlaždici IdeaScale hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour IdeaScale aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_203.png

