---
title: "Kurz: Azure Active Directory integrace s LinkedIn vyhledávání | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a vyhledávání LinkedIn."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2757a39-1ead-4a3e-91e4-270be3055683
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: e296431866a8611b30e72f286884890adf0f7e50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-lookup"></a>Kurz: Azure Active Directory integrace s LinkedIn vyhledávání

V tomto kurzu zjistěte, jak integrovat LinkedIn vyhledávání s Azure Active Directory (Azure AD).

Integrace vyhledávání LinkedIn s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k vyhledávání LinkedIn
- Můžete povolit uživatelům, aby automaticky získat přihlášení k vyhledávání LinkedIn (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s LinkedIn vyhledávání, potřebujete následující položky:

- Předplatné služby Azure AD
- Vyhledávání LinkedIn jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání LinkedIn vyhledávání z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-linkedin-lookup-from-the-gallery"></a>Přidání LinkedIn vyhledávání z Galerie
Konfigurace integrace vyhledávání LinkedIn do Azure AD, musíte přidat do seznamu spravovaných aplikací SaaS LinkedIn vyhledávání z galerie.

**Pokud chcete přidat LinkedIn vyhledávání z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno pro přidání nové aplikace.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **LinkedIn vyhledávání**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_search.png)

5. Na panelu výsledků vyberte **LinkedIn vyhledávání**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s LinkedIn vyhledávání podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v LinkedIn vyhledávání je pro uživatele ve službě Azure AD. Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské LinkedIn vyhledávání.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** LinkedIn vyhledávání.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s LinkedIn vyhledávání, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele vyhledávání LinkedIn](#creating-an-linkedin-lookup-test-user)**  – Pokud chcete mít protějšek Britta Simon v LinkedIn vyhledávání, které je propojena k reprezentaci Azure AD.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci LinkedIn vyhledávání.

**Ke konfiguraci Azure AD jednotné přihlašování s LinkedIn vyhledávání, proveďte následující kroky:**

1. Na portálu Azure na **LinkedIn vyhledávání** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogové okno, v **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_samlbase.png)

3. V okně prohlížeče jiný web, přihlašování k vaší **LinkedIn vyhledávání** webu jako správce.

4. V **centra účtů**, klikněte na tlačítko **globální nastavení** pod **nastavení**. Kromě toho **vyhledávání** z rozevíracího seznamu.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_011.png)

5. Klikněte na tlačítko **nebo klikněte na tlačítko sem můžete načíst a zkopírujte jednotlivých polí z formuláře** a zkopírujte **Entity Id** a **adresu Url pro přístup k příjemce Assertion (ACS)**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_032.png)

6. Na portálu Azure v části **LinkedIn vyhledávání domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url.png)

    a. V **identifikátor** textovému poli, zadejte **Entity ID** zkopírovat z portálu LinkedIn 

    b. V **adresa URL odpovědi** textovému poli, zadejte **adresu Url pro přístup k příjemce Assertion (ACS)** zkopírovat z portálu LinkedIn

7. Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url1.png)

    V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`
     
    > [!NOTE] 
    > Toto není skutečné hodnoty. Uživatel má tyto hodnoty aktualizovat s skutečná adresa URL přihlašování. Obraťte se na [tým podpory klienta sítě LinkedIn vyhledávání](https://business.LinkedIn.com/lookup) získat tuto hodnotu.

8. Vaše **LinkedIn vyhledávání** aplikace očekává SAML kontrolní výrazy ve specifickém formátu. Uživatel má můžete přidat mapování vlastních atributů ke konfiguraci atributy tokenu SAML. Následující snímek obrazovky ukazuje příklad. Výchozí hodnota **uživatelský identifikátor** je **user.userprincipalname** ale LinkedIn vyhledávání očekává to nejde mapovat pomocí e-mailovou adresu uživatele. Můžete použít **user.mail** atribut ze seznamu, nebo použijte hodnotu odpovídajícího atributu na základě konfigurace vaší organizace. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/updateusermail.png)
    
9. V **uživatelské atributy** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** a nastavit atributy. Uživatel musí přidat čtyři deklarace identity s názvem **e-mailu**, **oddělení**, **firstname**, a **lastname** a hodnota má být namapována na **user.mail**, **user.department**, **user.givenname**, a **user.surname** v uvedeném pořadí

    | Název atributu | Hodnota atributu |
    | --- | --- |
    | E-mailu| User.Mail |    
    | Oddělení| User.Department |
    | FirstName| User.givenName |
    | Příjmení| User.Surname |

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/userattribute.png)

    a. Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/4.png)
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/5.png)
   
    b. V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.
    
    c. Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.
    
    d. Klikněte na tlačítko **Ok**

10. Proveďte následující kroky na **název** atribut -

    a. Klikněte na atribut, který se otevře **Upravit atribut** okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/url_update.png)

    b. Odstranit hodnotu adresy URL **obor názvů**.
    
    c. Klikněte na tlačítko **Ok** uložte nastavení.

10. Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_certificate.png) 

11. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_400.png)

12. Přejděte na **nastavení správce LinkedIn** části. Nahrát soubor XML, který jste si stáhli z portálu Azure kliknutím **soubor nahrát XML** možnost.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_metadata_03.png)

13. Klikněte na tlačítko **na** umožňující jednotného přihlašování. Jednotné přihlašování stav se změní z **Nepřipojeno** k **připojeno**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_admin_05.png)

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_01.png) 

2. Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_02.png) 

3. Klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **Britta Simon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z Britta Simon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-linkedin-lookup-test-user"></a>Vytváření testovacího uživatele vyhledávání LinkedIn

Propojené vyhledávání aplikace podporuje pouze v zřizování uživatelů čas (JIT) a po ověření uživatelé jsou automaticky vytvořené v aplikaci. Aktivovat **automaticky přiřadit licence** přiřadit licence pro uživatele.
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedin_admin_license.png)

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k vyhledávání LinkedIn.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon LinkedIn vyhledávání, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **LinkedIn vyhledávání**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.

    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici LinkedIn vyhledávání na přístupovém panelu, přesměrovat na organizační stránku, kde je nutné zadat osobní podrobnosti o účtu LinkedIn. Ho propojí s vaším účtem obchodní LinkedIn svůj osobní účet. 

Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_203.png

