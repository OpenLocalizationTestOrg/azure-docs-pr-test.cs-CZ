---
title: "Kurz: Integrace Azure Active Directory pomocí konzoly pro správu Mimecast | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Mimecast konzoly pro správu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 5a04a5abd9ff30d484bce0a5c97a1d3e48b69e4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Kurz: Azure Active Directory integrace s Mimecast konzoly pro správu

V tomto kurzu zjistíte, jak toointegrate Mimecast konzoly pro správu s Azure Active Directory (Azure AD).

Integrace konzoly pro správu Mimecast s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooMimecast konzoly pro správu.
- Vaši uživatelé tooautomatically get přihlášeného tooMimecast konzoly pro správu (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Mimecast konzoly pro správu, je třeba hello následující položky:

- Předplatné služby Azure AD
- Konzole pro správu Mimecast jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Mimecast konzoly pro správu z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-mimecast-admin-console-from-hello-gallery"></a>Přidání Mimecast konzoly pro správu z Galerie hello
tooconfigure hello integrace Mimecast konzoly pro správu do služby Azure AD, je nutné tooadd konzoly pro správu Mimecast hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Mimecast konzoly pro správu z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **konzoly pro správu Mimecast**, vyberte **konzoly pro správu Mimecast** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Mimecast konzoly pro správu v seznamu výsledků hello](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí konzoly pro správu Mimecast podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v konzole pro správu Mimecast je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v konzole pro správu Mimecast musí toobe navázat.

V konzole pro správu Mimecast přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Mimecast konzoly pro správu, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele konzoly pro správu Mimecast](#create-a-mimecast-admin-console-test-user)**  -toohave protějšek Britta Simon v konzole pro správu Mimecast, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Mimecast konzoly pro správu.

**tooconfigure Azure AD jednotné přihlašování s Mimecast konzoly pro správu, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **konzoly pro správu Mimecast** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_samlbase.png)

3. Na hello **Mimecast správce konzoly domény a adresy URL** část, proveďte následující kroky hello:

    ![Mimecast správce konzoly domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_url.png)

    V hello **přihlašovací adresa URL** textovému poli, hello zadat adresu URL:
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > Adresa URL přihlašování Hello je konkrétní oblasti.

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_400.png)

6. Na hello **konfigurace konzoly správy Mimecast** klikněte na tlačítko **konfigurace konzoly pro správu Mimecast** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurace konzoly správy Mimecast](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se jako správce do konzoly Správce Mimecast.

8. Přejděte příliš**služby \> aplikace**.

    ![Služby](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794998.png "služby")

9. Klikněte na tlačítko **ověřování profily**.

    ![Profily ověřování](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794999.png "profily ověřování")
    
10. Klikněte na tlačítko **nový profil ověřování**.

    ![Nové profily ověřování](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795000.png "nové profily ověřování")

11. V hello **profil ověření** část, proveďte následující kroky hello:

    ![Profil ověření](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795015.png "profil ověření")
    
    a. V hello **popis** textovému poli, zadejte název pro svou konfiguraci.
    
    b. Vyberte **vynutit ověřování SAML pro konzolu Správce Mimecast**.
    
    c. Jako **zprostředkovatele**, vyberte **Azure Active Directory**.
    
    d. Vložení **SAML Entity ID**, který jste zkopírovali z hello portálu Azure do hello **URL vystavitele** textové pole.
    
    e. Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z hello portálu Azure do hello **přihlašovací adresa URL** textové pole.

    f. Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z hello portálu Azure do hello **adresy URL odhlašovací** textové pole.
    
    >[!NOTE]
    >Hello přihlašovací adresa URL hodnota a hodnota adresy URL odhlašovací hello jsou pro hello konzoly pro správu Mimecast hello stejné.
    
    g. Kódování base-64 certifikát stažený z portálu Azure v poznámkovém bloku otevřete odebrat hello prvního řádku ("*--*") a poslední řádek hello ("*--*"), hello kopie zbývající obsahu je do schránky a pak ji vložit toohello **certifikát zprostředkovatele Identity (Metadata)** textové pole.
    
    h. Vyberte **povolit jednotné přihlašování na**.
    
    i. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985) 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-mimecast-admin-console-test-user"></a>Vytvoření zkušebního uživatele Mimecast konzoly pro správu

V pořadí tooenable Azure AD Uživatelé toolog do konzoly pro správu Mimecast musí být zřízená do konzoly pro správu Mimecast. V případě hello z konzoly pro správu Mimecast zřizování je ruční úloha.

* Než můžete vytvořit uživatele, musíte tooregister domény.

**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**

1. Přihlaste se tooyour **konzoly pro správu Mimecast** jako správce.
2. Přejděte příliš**adresáře \> interní**.
   
   ![Adresáře](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795003.png "adresáře")
3. Klikněte na tlačítko **zaregistrovat nové domény**.
   
   ![Zaregistrujte novou doménu](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795004.png "zaregistrovat nové domény")
4. Po vytvoření nové domény, klikněte na tlačítko **novou adresu**.
   
   ![Nové adresy](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795005.png "novou adresu")
5. V hello adresu dialogové okno Nový proveďte hello následující kroky:
   
   ![Uložit](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795006.png "uložit")
   
   a. Typ hello **e-mailovou adresu**, **globální název**, **heslo**, a **Potvrdit heslo** atributy platný Azure AD účet má tooprovision do hello související textových polí.

   b. Klikněte na **Uložit**.

>[!NOTE]
>Můžete použít jiné nástroje pro tvorbu účet uživatele konzoly pro správu Mimecast nebo rozhraní API poskytovaných konzoly pro správu Mimecast tooprovision Azure AD uživatelské účty. 

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooMimecast konzoly pro správu Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign Britta Simon tooMimecast konzoly pro správu, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **konzoly pro správu Mimecast**.

    ![Hello konzoly pro správu Mimecast odkaz v seznamu aplikace hello](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici konzoly pro správu Mimecast hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Mimecast konzoly pro správu aplikací.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_203.png

