---
title: 'Kurz: Azure Active Directory integrace s Adobe Creative cloudu | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Adobe Creative cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ba1171e-56b1-4475-b308-58637d35e5a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 5e66255e9785465974a23cd3ef79c24e28c0250f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a>Kurz: Azure Active Directory integrace s Adobe Creative cloudu

V tomto kurzu zjistíte, jak toointegrate Adobe Creative cloudu s Azure Active Directory (Azure AD).

Integrace Adobe Creative cloudu s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooAdobe tvůrčí cloudu
- Vaši uživatelé tooautomatically get přihlášeného tooAdobe tvůrčí cloudu (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Adobe Creative cloudu, je třeba hello následující položky:

- Předplatné služby Azure AD
- Tvůrčí cloudu Adobe jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Adobe Creative cloudu z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-adobe-creative-cloud-from-hello-gallery"></a>Přidání Adobe Creative cloudu z Galerie hello
tooconfigure hello Integrace Adobe Creative cloudu do Azure AD, je nutné tooadd Adobe Creative cloudu hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Adobe Creative cloudu z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Adobe Creative cloudu**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_000.png)

5. Na panelu výsledků hello vyberte **Adobe Creative cloudu**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Adobe Creative cloudu podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v cloudu tvůrčí Adobe je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v cloudu tvůrčí Adobe musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Adobe Creative cloudu.

tooconfigure a testu Azure AD jednotné přihlašování s Adobe Creative cloudu, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele Adobe Creative cloudu](#creating-an-adobe-creative-cloud-test-user)**  -toohave protějšek Britta Simon Adobe Creative cloudu, který je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci Adobe Creative cloudu.

**tooconfigure Azure AD jednotné přihlašování s Adobe Creative cloudu, proveďte následující kroky hello:**

1. V hello Azure Management portal na hello **Adobe Creative cloudu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_01.png)

3. Na hello **Adobe Creative cloudové domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **IDP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url1.png)

    a. V hello **identifikátor** textovému poli, hodnota typu hello jako:`https://www.okta.com/saml2/service-provider/<token>`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.okta.com/auth/saml20/accauthlinktest`

    > [!NOTE] 
    > Upozorňujeme, že tyto nejsou hello skutečné hodnoty. Máte tooupdate tyto hodnoty pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL. Zde, doporučujeme vám toouse hello jedinečnou hodnotu řetězce v hello identifikátor. Pokud potřebujete toocreate uživatelé ručně, je nutné tým podpory Adobe Creative cloudu toocontact hello.

4. Na hello **Adobe Creative cloudové domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **SP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url2.png)

    a. Klikněte na hello **zobrazit upřesňující nastavení adresy URL** možnost

    b. V hello **přihlašovací adresa URL** textovému poli, hodnota typu hello jako:`https://adobe.com`

5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_05.png) 

6. Na hello **Adobe Creative konfigurace cloudu** klikněte na tlačítko **konfigurace cloudu tvůrčí Adobe** tooopen **konfigurovat přihlášení** okno. Zkopírujte hello **SAML Entity Id** a **adresa URL služby Jednotné přihlašování SAML** z Stručná referenční příručka oddílu.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_06.png) 

7. V okně prohlížeče jiný web, klienta Adobe Creative cloudových tooyour přihlášení jako správce.

8.  Přejděte příliš**Identity** hello levém navigačním podokně a klikněte na doménu. Potom proveďte následující kroky hello **jeden znak na konfigurace požadované** části.

    ![Nastavení](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "nastavení")

9. Klikněte na tlačítko **Procházet** tooupload hello stáhnout certifikát z Azure AD příliš**IDP certifikát**.

10. V hello **IDP vystavitele** textovému poli, vložte hodnotu hello z **SAML Entity Id** který jste zkopírovali ze **konfigurovat přihlášení** části na portálu Azure.

11. V hello **IDP přihlašovací adresa URL** textovému poli, vložte hodnotu hello z **adresa URL služby Jednotné přihlašování SAML** který jste zkopírovali ze **konfigurovat přihlášení** části na portálu Azure.

12. Vyberte **HTTP - přesměrování** jako **IDP vazby**.

13. Vyberte **e-mailová adresa** jako **nastavení přihlášení uživatele**.
 
14. Klikněte na tlačítko **Uložit** tlačítko.

15. řídicí panel Hello teď nabídne hello XML **"Stáhnout Metadata"** souboru. Obsahuje EntityDescriptor adresy URL a adresy URL AssertionConsumerService na Adobe. Otevřete soubor hello a nakonfigurujte je hello aplikaci Azure AD.

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_002.png)

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. Použití hello EntityDescriptor hodnota Adobe zadaný pro **identifikátor** na hello **nakonfigurovat nastavení aplikace** dialogové okno.

    b. Použití hello AssertionConsumerService hodnota Adobe zadaný pro **adresa URL odpovědi** na hello **nakonfigurovat nastavení aplikace** dialogové okno.
 
### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**. 

### <a name="creating-an-adobe-creative-cloud-test-user"></a>Vytváření testovacího uživatele Adobe Creative cloudu

V pořadí tooenable Azure AD Uživatelé toolog do Adobe Creative cloudu musí být zřízená do Adobe Creative cloudu.  
V případě hello Adobe Creative cloudu zřizování je ruční úloha.

**tooprovision uživatelské účty, provádět hello následující kroky:**

1. Přihlaste se tooyour Adobe Creative cloudu na webu společnosti jako správce.

2. Klikněte na tlačítko **osoby**.

    ![Lidé](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "osoby")

3. Klikněte na tlačítko **pozvat uživatele**.

    ![Uživatele pozvat](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "pozvat uživatele")

4. Na hello **pozvat osoby** dialogové okno proveďte hello následující kroky:

    ![Pozvěte lidi, kteří](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "pozvat uživatele")

    a. V hello **e-mailu** textovému poli, typ hello e-mailovou adresu účtu Britta Simon.
    
    b. Klikněte na tlačítko **pozvat**.

    > [!NOTE]
    > Držitel účtu Azure Active Directory Hello bude dostávat e-mailu a postupujte podle tooconfirm odkaz svého účtu před stane aktivní.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte svůj přístup tooAdobe tvůrčí cloudu.

![Přiřadit uživatele][200] 

**tooassign tooAdobe Britta Simon tvůrčí cloudu, proveďte následující kroky hello:**

1. Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Adobe Creative cloudu**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_50.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Adobe Creative cloudu hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Adobe Creative cloudových aplikací.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_203.png