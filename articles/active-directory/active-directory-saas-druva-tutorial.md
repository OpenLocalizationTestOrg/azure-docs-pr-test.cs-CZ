---
title: 'Kurz: Azure Active Directory integrace s Druva | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Druva."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: a1c36c06d6d005e0aa363fbf34efe630e4cca1ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-druva"></a>Kurz: Azure Active Directory integrace s Druva

V tomto kurzu zjistíte, jak toointegrate Druva s Azure Active Directory (Azure AD).

Integrace Druva s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooDruva.
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooDruva (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Druva tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Druva jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Druva z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-druva-from-hello-gallery"></a>Přidání Druva z Galerie hello
tooconfigure hello integrace Druva do Azure AD, je nutné tooadd Druva hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Druva z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **Druva**, vyberte **Druva** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Druva v seznamu výsledků hello](./media/active-directory-saas-druva-tutorial/tutorial_druva_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Druva podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Druva je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Druva musí toobe navázat.

V Druva, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Druva, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Druva](#create-a-druva-test-user)**  -toohave protějšek Britta Simon v Druva, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Druva.

**tooconfigure Azure AD jednotné přihlašování s Druva, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Druva** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-druva-tutorial/tutorial_druva_samlbase.png)

3. Na hello **Druva domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-druva-tutorial/tutorial_druva_url.png)

    V hello **přihlašovací adresa URL** textovému poli, hello zadat adresu URL:`https://cloud.druva.com/home`

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-druva-tutorial/tutorial_druva_certificate.png) 

5. Aplikace Druva očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, abyste tooadd vlastních atributů mapování tooyour **atributy tokenu SAML** konfigurace. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-druva-tutorial/tutorial_druva_attribute.png)

6. V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v hello předcházející bitové kopie a provést hello následující kroky:

    | Název atributu      | Hodnota atributu      |
    | ------------------- | -------------------- |
    | synchronizována\_auth\_tokenu |Zadejte hodnotu tokenu generovaného hello |
    
    a. Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-druva-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-druva-tutorial/tutorial_attribute_05.png)
    
    b. V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.

    c. Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek. Hodnota tokenu generovaného Hello se vysvětluje dále v kurzu.
    
    d. Klikněte na tlačítko **OK**.    

7. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-druva-tutorial/tutorial_general_400.png)

8. Na hello **Druva konfigurace** klikněte na tlačítko **konfigurace Druva** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-druva-tutorial/tutorial_druva_configure.png) 

9. V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti Druva tooyour.

10. Přejděte příliš**spravovat \> nastavení**.

    ![Nastavení](./media/active-directory-saas-druva-tutorial/ic795091.png "nastavení")

11. V dialogovém okně Nastavení jednotného přihlašování hello proveďte následující kroky hello:

    ![Jednotné přihlašování v nastavení](./media/active-directory-saas-druva-tutorial/ic795092.png "jednotné přihlašování v nastavení")
    
    a. Vložení **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z hello portálu Azure do hello **ID zprostředkovatele přihlašovací adresa URL** textové pole.
    
    b. Vložení **Sign-Out URL** hodnotu, kterou jste zkopírovali z hello portálu Azure do hello **adresy URL odhlašovací ID zprostředkovatele** textové pole.
    
     c. Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát poskytovatele ID** textbox
     
     d. tooopen hello **nastavení** klikněte na tlačítko **Uložit**.

12. Na hello **nastavení** klikněte na tlačítko **vygenerovat Token jednotného přihlašování k**.

    ![Nastavení](./media/active-directory-saas-druva-tutorial/ic795093.png "nastavení")

13. Na hello **jeden přihlašování ověřování tokenu** dialogové okno, proveďte následující kroky hello:

    ![Token jednotného přihlašování k](./media/active-directory-saas-druva-tutorial/ic795094.png "tokenu jednotného přihlašování")
    
    a. Klikněte na tlačítko **kopie**, vložte zkopírovaný hodnotu v hello **hodnotu** textového pole v hello **přidat atribut** části.
    
    b. Klikněte na **Zavřít**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-druva-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-druva-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-druva-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-druva-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-druva-test-user"></a>Vytvoření zkušebního uživatele Druva

V pořadí tooenable Azure AD Uživatelé toolog v tooDruva musí být zřízená do Druva. V případě hello Druva zřizování je ruční úloha.

**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**

1. Přihlaste se tooyour **Druva** společnosti lokality jako správce.

2. Přejděte příliš**spravovat \> uživatelé**.
   
   ![Správa uživatelů](./media/active-directory-saas-druva-tutorial/ic795097.png "Správa uživatelů")

3. Klikněte na tlačítko **vytvořit nový**.
   
   ![Správa uživatelů](./media/active-directory-saas-druva-tutorial/ic795098.png "Správa uživatelů")

4. V dialogovém okně Vytvořit nového uživatele hello proveďte následující kroky hello:
   
   ![Vytvoření NewUser](./media/active-directory-saas-druva-tutorial/ic795099.png "vytvořit NewUser")
   
   a. V hello **e-mailová adresa** textovému poli, zadejte e-mailu hello uživatele jako  **brittasimon@contoso.com** .
   
   b. V hello **název** textovému poli, zadejte název hello uživatele jako **BrittaSimon**.
   
   c. Klikněte na tlačítko **vytvořit uživatele**.

>[!NOTE]
>Můžete použít všechny ostatní Druva uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Druva tooprovision uživatelské účty Azure AD.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooDruva toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign Britta Simon tooDruva, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Druva**.

    ![v seznamu aplikace hello Hello Druva odkaz](./media/active-directory-saas-druva-tutorial/tutorial_druva_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Druva hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Druva aplikace.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-druva-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-druva-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-druva-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-druva-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-druva-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-druva-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-druva-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-druva-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-druva-tutorial/tutorial_general_203.png

