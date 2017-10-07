---
title: 'Kurz: Azure Active Directory integrace s TimeOffManager | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a TimeOffManager."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: c871257bfb49883e31b1c4860a9d7faa70e9ab48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Kurz: Azure Active Directory integrace s TimeOffManager

V tomto kurzu zjistíte, jak toointegrate TimeOffManager s Azure Active Directory (Azure AD).

Integrace TimeOffManager s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooTimeOffManager
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTimeOffManager (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s TimeOffManager tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- TimeOffManager jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidat TimeOffManager z Galerie hello
2. Konfigurace a otestování Azure AD jednotné přihlašování

## <a name="add-timeoffmanager-from-hello-gallery"></a>Přidat TimeOffManager z Galerie hello
tooconfigure hello integrace TimeOffManager do Azure AD, je nutné tooadd TimeOffManager hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd TimeOffManager z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **TimeOffManager**, vyberte **TimeOffManager** z panelu výsledek a pak klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.

    ![Přidat z Galerie](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s TimeOffManager podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v TimeOffManager je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v TimeOffManager musí toobe navázat.

V TimeOffManager, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s TimeOffManager, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele TimeOffManager](#create-a-timeoffmanager-test-user)**  -toohave protějšek Britta Simon v TimeOffManager, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci TimeOffManager.

**tooconfigure Azure AD jednotné přihlašování s TimeOffManager, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **TimeOffManager** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Přihlášení na základě SAML](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_samlbase.png)

3. Na hello **TimeOffManager domény a adresy URL** část, proveďte následující hello:

     ![Část TimeOffManager domény a adresy URL](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_url.png)

    V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`

    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte tuto hodnotu s hello skutečná adresa URL odpovědi. Můžete získat tuto hodnotu z **jednotného přihlašování na stránce nastavení** kterého se vysvětluje dále v kurzu hello nebo kontaktujte [tým podpory TimeOffManager](http://www.timeoffmanager.com/contact-us.aspx).
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Certifikát pro podpis SAML části](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_certificate.png) 

5. Hello cílem této části je toooutline jak tooTimeOffManger tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.
    
    Aplikace TimeOffManger očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování tooyour tokenu atributy konfigurace SAML. Hello následující snímek obrazovky ukazuje příklad pro tento.

    ![atributy tokenu SAML](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "atributy tokenu saml")
    
    | Název atributu | Hodnota atributu |
    | --- | --- |
    | FirstName |User.givenName |
    | Příjmení |User.Surname |
    | E-mail |User.Mail |
    
    a.  Pro každý řádek dat v tabulce hello výše, klikněte na tlačítko **přidat atribut uživatele**.
    
    ![atributy tokenu SAML](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "atributy tokenu saml")
    
    ![atributy tokenu SAML](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "atributy tokenu saml")
    
    b.  V hello **název atributu** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.
    
    c.  V hello **hodnota atributu** textovému poli, hodnota atributu vyberte hello zobrazený pro tento řádek.
    
    d.  Klikněte na tlačítko **OK**.
    
6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_400.png)

7. Na hello **TimeOffManager konfigurace** klikněte na tlačítko **konfigurace TimeOffManager** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![TimeOffManager konfigurační oddíl](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_configure.png) 

8. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti TimeOffManager.

9. Přejděte příliš**účet \> možnosti účtu \> nastavení jednotného přihlašování**.
   
   ![Jednotné přihlašování v nastavení](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "jednotné přihlašování v nastavení")
7. V hello **nastavení jednotného přihlašování** část, proveďte následující kroky hello:
   
   ![Jednotné přihlašování v nastavení](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "jednotné přihlašování v nastavení")
   
   a. Otevření kódovaného certifikátu kódování base-64 v poznámkovém bloku hello kopírování obsahu ho do schránky a potom vložte hello celý certifikát do **certifikát X.509** textové pole.
   
   b. V **Idp vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.
   
   c. V **adresu URL koncového bodu IdP** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.
   
   d. Jako **vynutit SAML**, vyberte **ne**.
   
   e. Jako **automaticky vytvářet uživatelé**, vyberte **Ano**.
   
   f. V **adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.
   
   g. Klikněte na tlačítko **uložit změny**.

11. V **jednotného přihlašování nastavení** stránky, hodnota hello kopie **adresa URL služby příjemce Assertion** a vložte ji do hello **adresa URL odpovědi** textového pole pod **TimeOffManager Domény a adresy URL** části na portálu Azure. 

      ![Jednotné přihlašování v nastavení](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "jednotné přihlašování v nastavení")

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Uživatelé a skupiny--> všechny uživatele](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Tlačítko Přidat](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Dialogové okno stránka uživatel](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-timeoffmanager-test-user"></a>Vytvoření zkušebního uživatele TimeOffManager

V pořadí tooenable Azure AD Uživatelé toolog do TimeOffManager musí být zřízená tooTimeOffManager.  

TimeOffManager podporuje jenom při zřizování uživatelů čas. Neexistuje žádná položka akce za vás.  

Hello uživatelé se přidávají automaticky během hello první přihlášení pomocí jednotného přihlašování na.

>[!NOTE]
>Můžete použít všechny ostatní TimeOffManager uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované TimeOffManager tooprovision uživatelské účty Azure AD.
> 

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooTimeOffManager toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooTimeOffManager, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **TimeOffManager**.

    ![TimeOffManager v seznamu aplikací](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici TimeOffManager hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour TimeOffManager aplikace. Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_203.png

