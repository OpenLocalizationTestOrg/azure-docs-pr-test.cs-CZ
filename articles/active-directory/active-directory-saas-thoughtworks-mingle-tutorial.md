---
title: 'Kurz: Azure Active Directory integrace s Thoughtworks Mingle | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Thoughtworks Mingle."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: c17f8e13d2db3de7d228d9b27128d134f98d6cdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Kurz: Azure Active Directory integrace s Thoughtworks Mingle

V tomto kurzu zjistíte, jak toointegrate Thoughtworks Mingle s Azure Active Directory (Azure AD).

Integrace Thoughtworks Mingle s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooThoughtworks Mingle
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooThoughtworks Mingle (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Thoughtworks Mingle tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Thoughtworks Mingle jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Thoughtworks Mingle z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-thoughtworks-mingle-from-hello-gallery"></a>Přidání Thoughtworks Mingle z Galerie hello
tooconfigure hello integrace Thoughtworks Mingle do Azure AD, je nutné tooadd Thoughtworks Mingle hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Thoughtworks Mingle z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **Thoughtworks Mingle**, vyberte **Thoughtworks Mingle** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Thoughtworks Mingle v seznamu výsledků hello](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Thoughtworks Mingle podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Thoughtworks Mingle je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Thoughtworks Mingle musí toobe navázat.

V Thoughtworks Mingle, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Thoughtworks Mingle, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Thoughtworks Mingle](#create-a-thoughtworks-mingle-test-user)**  -toohave protějšek Britta Simon v Thoughtworks Mingle, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Thoughtworks Mingle.

**tooconfigure Azure AD jednotné přihlašování s Thoughtworks Mingle, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Thoughtworks Mingle** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

3. Na hello **Thoughtworks Mingle domény a adresy URL** část, proveďte následující kroky hello:

    ![Thoughtworks Mingle domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.mingle.thoughtworks.com`

    > [!NOTE] 
    > Hodnota Hello není skutečné. Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory klienta Mingle Thoughtworks](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) tooget hello hodnotu. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_400.png)

6. Přihlaste se tooyour **Thoughtworks Mingle** společnosti lokality jako správce.

7. Klikněte na tlačítko hello **správce** kartě a potom klikněte na **Konfigurace jednotného přihlašování k**.
   
    ![Karta správce](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785157.png "Konfigurace jednotného přihlašování")

8. V hello **Konfigurace jednotného přihlašování k** část, proveďte následující kroky hello:
   
    ![Konfigurace jednotného přihlašování k](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785158.png "Konfigurace jednotného přihlašování")
    
    a. Soubor metadat hello tooupload, klikněte na tlačítko **zvolte soubor**. 

    b. Klikněte na tlačítko **uložit změny**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-thoughtworks-mingle-test-user"></a>Vytvoření zkušebního uživatele Thoughtworks Mingle

Pro Azure AD Uživatelé toobe možné toosign v musí být zřízená toohello Thoughtworks Mingle aplikace pomocí jejich názvy uživatele Azure Active Directory. V případě hello Thoughtworks Mingle zřizování je ruční úloha.

**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**

1. Přihlaste se tooyour Thoughtworks Mingle společnosti lokality jako správce.

2. Klikněte na tlačítko **profil**.
   
    ![Svůj první projekt](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785160.png "prvního projektu")

3. Klikněte na tlačítko hello **správce** a pak klikněte **uživatelé**.
   
    ![Uživatelé](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785161.png "uživatelů")

4. Klikněte na tlačítko **nového uživatele**.
   
    ![Nový uživatel](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785162.png "nového uživatele")

5. Na hello **nového uživatele** dialogové okno proveďte hello následující kroky:
   
    ![Dialogové okno Nový uživatel](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785163.png "nového uživatele")  
 
    a. Typ hello **přihlašovací jméno**, **zobrazovaný název**, **zvolte heslo**, **potvrzení hesla** platný Azure AD účet chcete tooprovision do hello související textových polí. 

    b. Jako **typ uživatele**, vyberte **úplné uživatelské**.

    c. Klikněte na tlačítko **vytvořit tento profil**.

>[!NOTE]
>Můžete použít všechny ostatní Thoughtworks Mingle uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Thoughtworks Mingle tooprovision AAD uživatelské účty.
> 

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooThoughtworks Mingle toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign Britta Simon tooThoughtworks Mingle, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Thoughtworks Mingle**.

    ![v seznamu aplikace hello odkaz Thoughtworks Mingle Hello](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.

Po kliknutí na tlačítko hello Thoughtworks Mingle dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Thoughtworks Mingle aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_203.png

