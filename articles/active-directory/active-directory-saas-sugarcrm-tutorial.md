---
title: 'Kurz: Azure Active Directory integrace s Sugar CRM | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Sugar CRM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3331b9fc-ebc0-4a3a-9f7b-bf20ee35d180
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 108d2f8125e410743ee7bc48883a1d0b00602615
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sugar-crm"></a>Kurz: Azure Active Directory integrace s Sugar CRM

V tomto kurzu zjistíte, jak toointegrate Sugar CRM s Azure Active Directory (Azure AD).

Integrace s Azure AD Sugar CRM poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooSugar CRM
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSugar CRM (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Sugar CRM, budete potřebovat hello následující položky:

- Předplatné služby Azure AD
- Sugar CRM jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Sugar CRM z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-sugar-crm-from-hello-gallery"></a>Přidání Sugar CRM z Galerie hello
tooconfigure hello integrace Sugar CRM do Azure AD, je nutné tooadd Sugar CRM hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Sugar CRM z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Sugar CRM**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_search.png)

5. Na panelu výsledků hello vyberte **Sugar CRM**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Sugar CRM podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Sugar CRM je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Sugar CRM musí toobe navázat.

V Sugar CRM přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Sugar CRM, budete potřebovat následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Sugar CRM](#creating-a-sugar-crm-test-user)**  -toohave protějšek Britta Simon v CRM Sugar, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Sugar CRM.

**tooconfigure Azure AD jednotné přihlašování s Sugar CRM, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Sugar CRM** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_samlbase.png)

3. Na hello **Sugar CRM domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_url.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:
    | |
    |--|
    | `https://<companyname>.sugarondemand.com` |
    | `https://<companyname>.trial.sugarcrm` |

    > [!NOTE] 
    > Hodnota Hello není skutečné. Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory klienta CRM Sugar](https://support.sugarcrm.com/) tooget hello hodnotu. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_400.png)

6. Na hello **Sugar CRM konfigurace** klikněte na tlačítko **konfigurace CRM Sugar** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti tooyour Sugar CRM.

8. Přejděte příliš**správce**.
   
    ![Správce](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "správce")

9. V hello **správy** klikněte na tlačítko **Správa hesel**.
   
    ![Správa](./media/active-directory-saas-sugarcrm-tutorial/ic795889.png "správy")

10. Vyberte **povolit ověřování SAML**.
   
    ![Správa](./media/active-directory-saas-sugarcrm-tutorial/ic795890.png "správy")

11. V hello **ověřování SAML** část, proveďte následující kroky hello:
   
    ![Ověřování SAML](./media/active-directory-saas-sugarcrm-tutorial/ic795891.png "ověřování SAML")  
 
    a. V hello **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.
  
    b. V hello **SLO URL** textovému poli, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.
  
    c. Otevření kódovaného certifikátu kódování base-64 v poznámkovém bloku hello kopírování obsahu ho do schránky a potom vložte hello celý certifikát do **certifikát X.509** textové pole.
  
    d. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-sugar-crm-test-user"></a>Vytvoření zkušebního uživatele Sugar CRM

V pořadí tooenable Azure AD Uživatelé toolog v tooSugar CRM musí být zřízená tooSugar CRM.

V případě hello aplikace Sugar CRM je zřizování ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour **Sugar CRM** společnosti lokality jako správce.

2. Přejděte příliš**správce**.
   
    ![Správce](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "správce")

3. V hello **správy** klikněte na tlačítko **Správa uživatelů**.
   
    ![Správa](./media/active-directory-saas-sugarcrm-tutorial/ic795893.png "správy")

4. Přejděte příliš**uživatelé \> vytvořit nového uživatele**.
   
    ![Vytvoření nového uživatele](./media/active-directory-saas-sugarcrm-tutorial/ic795894.png "vytvořit nového uživatele")

5. Na hello **profil uživatele** kartu, proveďte následující kroky hello:
   
    ![Nový uživatel](./media/active-directory-saas-sugarcrm-tutorial/ic795895.png "nového uživatele")

    a. Typ hello **uživatelské jméno**, **příjmení**, a **e-mailová adresa** platný uživatele Azure Active Directory do hello související textových polí.
  
6. Jako **stav**, vyberte **Active**.

7. Na kartě heslo hello proveďte následující kroky hello:
   
    ![Nový uživatel](./media/active-directory-saas-sugarcrm-tutorial/ic795896.png "nového uživatele")

    a. Zadejte heslo hello do hello související textové pole.

    b. Klikněte na **Uložit**.

>[!NOTE]
>Můžete použít všechny ostatní Sugar CRM uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Sugar CRM tooprovision AAD uživatelské účty. 
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooSugar CRM Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign tooSugar Britta Simon CRM, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Sugar CRM**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.

Když kliknete na dlaždici Sugar CRM hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Sugar CRM aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_203.png

