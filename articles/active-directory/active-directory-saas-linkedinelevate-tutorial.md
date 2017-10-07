---
title: "Kurz: Azure Active Directory integrace s zvýšení oprávnění LinkedIn | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a zvýšení oprávnění LinkedIn."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: 189bd72c230be7dc0c0b934f94ea01e84af9ad23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a>Kurz: Azure Active Directory integrace s LinkedIn zvýšení oprávnění

V tomto kurzu zjistíte, jak toointegrate LinkedIn zvýšení oprávnění v Azure Active Directory (Azure AD).

Integrace zvýšení oprávnění LinkedIn s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooLinkedIn zvýšení
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLinkedIn zvýšení (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s LinkedIn zvýšení oprávnění, je třeba hello následující položky:

- Předplatné služby Azure AD
- Zvýšení oprávnění LinkedIn jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání LinkedIn zvýšení oprávnění z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-linkedin-elevate-from-hello-gallery"></a>Přidání LinkedIn zvýšení oprávnění z Galerie hello
tooconfigure hello integrace zvýšení oprávnění LinkedIn do Azure AD, je nutné tooadd zvýšení oprávnění LinkedIn hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd LinkedIn zvýšení oprávnění z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **zvýšení oprávnění LinkedIn**. Na panelu výsledků klikněte na **zvýšení oprávnění LinkedIn** tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s LinkedIn zvýšení oprávnění na základě testovací uživatele, nazývá "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v LinkedIn zvýšení oprávnění je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v zvýšení oprávnění LinkedIn musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v LinkedIn zvýšení oprávnění.

tooconfigure a testu Azure AD jednotné přihlašování s LinkedIn zvýšení oprávnění, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele zvýšení oprávnění LinkedIn](#creating-a-linkedin-elevate-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci LinkedIn zvýšení oprávnění.

**tooconfigure Azure AD jednotné přihlašování s LinkedIn zvýšení oprávnění, proveďte následující kroky hello:**

1. V hello Azure Management portal na hello **zvýšení oprávnění LinkedIn** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedin_01.png)

3. V okně prohlížeče jiný web, klienta LinkedIn zvýšení tooyour přihlášení jako správce.

4. V **centra účtů**, klikněte na tlačítko **globální nastavení** pod **nastavení**. Kromě toho **zvýšení - zvýšení oprávnění Test AAD** z rozevíracího seznamu hello.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_01.png)

5. Klikněte na **nebo klepněte sem tooload a zkopírujte jednotlivých polí z formuláře hello** a zkopírujte **Entity Id** a **adresu Url pro přístup k příjemce Assertion (ACS)**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_03.png)

6. Na portálu Azure v části **LinkedIn zvýšení oprávnění domény a adresy URL**, proveďte následující kroky, pokud chcete, aby tooconfigure jednotné přihlašování hello v **IdP iniciované** režimu

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_01.png)

    a. V hello **identifikátor** textovému poli, zadejte hello **Entity ID** zkopírovat z portálu LinkedIn 

    b. V hello **adresa URL odpovědi** textovému poli, zadejte hello **adresu Url pro přístup k příjemce Assertion (ACS)** zkopírovat z portálu LinkedIn

7. Pokud chcete, aby tooconfigure jednotné přihlašování v **iniciované SP**, klikněte na možnost zobrazit Advanced adresy URL v části Konfigurace hello a konfigurace přihlášení hello na adresu URL s hello následující vzoru:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>` 
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_02.png) 
    
8. Zvýšení oprávnění LinkedIn aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování tooyour tokenu atributy konfigurace SAML. Hello následující snímek obrazovky ukazuje příklad pro tento. Výchozí hodnota Hello **uživatelský identifikátor** je **user.userprincipalname** ale zvýšení oprávnění LinkedIn očekává tento toobe namapována na hello uživatele e-mailovou adresu. K tomu můžete použít **user.mail** atribut ze seznamu hello nebo použijte hodnotu hello odpovídajícího atributu na základě konfigurace vaší organizace. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/updateusermail.png)

9. V **uživatelské atributy** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** a nastavte atributy hello. Budete potřebovat další deklarace identity s názvem tooadd **oddělení** a hello hodnota se musí mapovat příliš toobe**user.department**.

    | Název atributu | Hodnota atributu |
    | --- | --- |    
    | Oddělení| User.Department |

      ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/userattribute.png)

      a. Kliknutím na Přidat atribut tooopen hello atribut stránku Podrobnosti o přidání atributu hello oddělení, jak je znázorněno níže-

      ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/adduserattribute.png)

      b. Klikněte na **Ok** toosave hello atribut.

      c. Název hello změnu atributu hello **emailaddress** příliš**e-mailu**.


10. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_certificate.png) 

11. Klikněte na **Uložit**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_400.png)

12. Přejděte příliš**nastavení správce LinkedIn** části. Nahrajte soubor hello XML, který jste právě stáhli z portálu Azure hello kliknutím na možnost soubor nahrát XML hello.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_metadata_03.png)

13. Klikněte na tlačítko **na** tooenable jednotné přihlašování. Jednotné přihlašování stav se změní z **Nepřipojeno** příliš**připojeno**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**. 

### <a name="creating-a-linkedin-elevate-test-user"></a>Vytvoření zkušebního uživatele LinkedIn zvýšení oprávnění

Propojené zvýšení oprávnění aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatele budou vytvořeny v hello aplikace automaticky. Na stránce Nastavení správce hello na přepínači portálu flip hello zvýšení oprávnění LinkedIn hello **automaticky přiřadit licence** tooactive tooenable pouze v době zřizování a to také přiřadit licence toohello uživatele.

   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte svůj přístup tooLinkedIn zvýšení Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign tooLinkedIn Britta Simon zvýšení, proveďte následující kroky hello:**

1. Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **zvýšení oprávnění LinkedIn**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_0001.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici hello LinkedIn zvýšení oprávnění v hello přístupového panelu, měli byste obdržet hello Azure přihlašovací stránku a na po úspěšném přihlášení, měli byste obdržet do své aplikace LinkedIn zvýšení oprávnění.

## <a name="additional-resources"></a>Další zdroje

* [Kurz: Konfigurace LinkedIn zvýšení oprávnění pro automatické zřizování s Azure Active Directory uživatelů](active-directory-saas-linkedinelevate-provisioning-tutorial.md)
* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_203.png
