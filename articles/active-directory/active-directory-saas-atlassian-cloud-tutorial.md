---
title: 'Kurz: Azure Active Directory integrace s Atlassian cloudu | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Atlassian cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: f679e8b3306bf0efb9373d8baa0cfe095b760aaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Kurz: Azure Active Directory integrace s Atlassian cloudu

V tomto kurzu zjistíte, jak toointegrate Atlassian cloudu s Azure Active Directory (Azure AD).

Integrace Atlassian cloudu s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooAtlassian cloudu
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAtlassian cloudu (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Atlassian cloudu, je třeba hello následující položky:

- Předplatné služby Azure AD
- Cloudu Atlassian jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Atlassian cloudu z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-atlassian-cloud-from-hello-gallery"></a>Přidání Atlassian cloudu z Galerie hello
tooconfigure hello integrace Atlassian cloudu do Azure AD, je nutné tooadd Atlassian cloudu hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Atlassian cloudu z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Atlassian cloudu**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_search.png)

5. Na panelu výsledků hello vyberte **Atlassian cloudu**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurujete a testu Azure AD jednotné přihlašování s Atlassian cloudu podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v cloudu Atlassian je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v cloudu Atlassian musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Atlassian cloudu.

tooconfigure a testu Azure AD jednotné přihlašování s Atlassian cloudu, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele cloudu Atlassian](#creating-an-atlassian-cloud-test-user)**  -toohave protějšek Britta Simon Atlassian cloudu, který je propojený toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Atlassian cloudu.

**tooconfigure Azure AD jednotné přihlašování s Atlassian cloudu, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Atlassian cloudu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. Na hello **Atlassian cloudové domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **IDP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)

    a. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instancename>.atlassian.net/admin/saml/edit`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL jako:`https://id.atlassian.com/login/saml/acs`

4. Zkontrolujte **zobrazit upřesňující nastavení adresy URL** a proveďte následující krok, pokud chcete aplikace hello tooconfigure hello **SP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instancename>.atlassian.net`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné identifikátor a přihlašovací adresa URL. Přesné hodnoty hello můžete získat z obrazovky konfigurace SAML Atlassian cloudu.
 
5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. Na hello **konfigurace cloudu Atlassian** klikněte na tlačítko **konfigurace cloudu Atlassian** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

7. tooget jednotné přihlašování nakonfigurovat pro vaši aplikaci, přihlášení toohello Atlassian portálu pomocí hello práva správce.

8. V části ověřování hello hello levé navigační na **domény**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

    a. Hello textovému poli, zadejte název domény a pak klikněte na **přidáním domény**.
        
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_07.png)

    b. tooverify hello domény, klikněte na tlačítko **ověřte**. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_08.png)

    c. Stáhnout soubor html ověření domény hello, nahrajte ho toohello kořenové složky vaší doméně webu a pak klikněte na tlačítko **ověřit doménu**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_09.png)

    d. Po ověření domény hello hello hodnotu hello **stav** pole je **ověřeno**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_10.png)

9. V levém navigačním panelu hello, klikněte na tlačítko **SAML**.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

10. Vytvoření konfigurace SAML a přidejte hello konfigurace zprostředkovatele Identity.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. V hello **zprostředkovatele Identity Entity ID** textového pole, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.

    b. V hello **zprostředkovatele Identity jednotného přihlašování k adrese URL** textového pole, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.

    c. Otevřete certifikát hello stáhli z Azure portal a zkopírujte hello hodnoty bez hello Begin a End řádky a vložte ji do hello **X509 veřejný certifikát** pole.
    
    d. Klikněte na tlačítko **uložit konfiguraci** tooSave hello nastavení.
     
11. Aktualizujte hello Azure AD nastavení toomake že máte hello instalaci opravte identifikátoru adresy URL.
  
    a. Kopírování hello **SP Identity ID** z hello SAML obrazovky a vložte ji ve službě Azure AD jako hello **identifikátor** hodnotu.

    b. Přihlašovací adresa URL je adresa URL klienta hello ve vašem cloudu Atlassian.     

     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)
    
12. V hello portálu Azure, klikněte na **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-atlassian-cloud-test-user"></a>Vytváření testovacího uživatele Atlassian cloudu

Uživatelé toolog tooenable Azure AD v cloudu tooAtlassian, se musí být zřízená do Atlassian cloudu.  
V případě cloudu Atlassian zřizování je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. V části Správa lokality hello, klikněte na tlačítko hello **uživatelé** tlačítko

    ![Vytvoření uživatele Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. Klikněte na tlačítko hello **vytvořit uživatele** tlačítko toocreate uživatele v hello Atlassian cloudu

    ![Vytvoření uživatele Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. Zadejte uživatele hello **e-mailová adresa**, **uživatelské jméno**, a **úplný název** a přiřaďte hello přístup k aplikaci. 

    ![Vytvoření uživatele Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. Klikněte na tlačítko **vytvořit uživateli** tlačítko, kterou bude odesílat hello e-mailová pozvánka toohello uživatele a po přijetí hello pozvánku hello uživatel se bude v hello systému aktivní. 

>[!NOTE] 
>Můžete také vytvořit hello hromadné uživatele kliknutím hello **vytvořit hromadné** tlačítka na hello část uživatelé.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooAtlassian cloudu Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign tooAtlassian Britta Simon cloudu, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Atlassian cloudu**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello Atlassian cloudu dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Atlassian cloudových aplikací. Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

