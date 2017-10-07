---
title: 'Kurz: Azure Active Directory integrace s Envoy | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Envoy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 71f7afcc-1033-4098-9b7e-4f9f2b26f734
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 93add7c1f3cf1fc163acc505f11e34bd696c571c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-envoy"></a>Kurz: Azure Active Directory integrace s Envoy

V tomto kurzu zjistíte, jak toointegrate Envoy službou Azure Active Directory (Azure AD).

Integrace Envoy s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooEnvoy.
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooEnvoy (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Envoy tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Envoy jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Envoy z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-envoy-from-hello-gallery"></a>Přidání Envoy z Galerie hello
tooconfigure hello integrace Envoy do Azure AD, je nutné tooadd Envoy hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Envoy z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **Envoy**, vyberte **Envoy** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Envoy v seznamu výsledků hello](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Envoy podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Envoy je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Envoy musí toobe navázat.

V Envoy, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Envoy, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvořit testovací uživatele s Envoy](#create-an-envoy-test-user)**  -toohave protějšek Britta Simon v Envoy, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Envoy.

**tooconfigure Azure AD jednotné přihlašování s Envoy, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Envoy** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_samlbase.png)

3. Na hello **Envoy domény a adresy URL** část, proveďte následující kroky hello:

    ![Envoy domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_url.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.Envoy.com`
    
    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory Envoy klienta](https://envoy.com/contact/) tooget tuto hodnotu.

4. Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnota certifikátu...

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-envoy-tutorial/tutorial_general_400.png)

6. Na hello **Envoy konfigurace** klikněte na tlačítko **konfigurace Envoy** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurace Envoy](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_configure.png)

7. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Envoy.

8. V panelu nástrojů hello hello nahoře, klikněte na **nastavení**.

    ![Envoy](./media/active-directory-saas-envoy-tutorial/ic776782.png "Envoy")

9. Klikněte na tlačítko **společnosti**.

    ![Společnosti](./media/active-directory-saas-envoy-tutorial/ic776783.png "společnosti")

10. Klikněte na tlačítko **SAML**.

    ![SAML](./media/active-directory-saas-envoy-tutorial/ic776784.png "SAML")

11. V hello **ověřování SAML** konfigurace části, proveďte následující kroky hello:

    ![Ověřování SAML](./media/active-directory-saas-envoy-tutorial/ic776785.png "ověřování SAML")
    
    >[!NOTE]
    >Hello hodnotou pro ID umístění Ústředí hello je automaticky generovány hello aplikací.
    
    a. V **otisk prstu** textovému poli, vložte hello **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.
    
    b. Vložení **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali formuláři hello Azure portálu do hello **adresa URL IDENTITY zprostředkovatele protokolu HTTP SAML** textové pole.
    
    c. Klikněte na tlačítko **uložit změny**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-envoy-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-envoy-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-envoy-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-envoy-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-an-envoy-test-user"></a>Vytvořit uživatele s Envoy testu

Neexistuje žádná položka akce, pro které uživatele tooconfigure zřizování tooEnvoy. Když se uživatel s přiřazenou pokusí toolog do Envoy pomocí hello přístupového panelu, Envoy ověří, zda existuje hello uživatele. Pokud neexistuje žádný účet k dispozici dosud, se automaticky vytvoří pomocí Envoy.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooEnvoy toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign Britta Simon tooEnvoy, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Envoy**.

    ![Hello Envoy odkaz v seznamu aplikace hello](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Envoy hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Envoy aplikace.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_203.png

