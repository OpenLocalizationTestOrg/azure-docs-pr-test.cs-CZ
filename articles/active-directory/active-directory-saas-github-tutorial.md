---
title: 'Kurz: Azure Active Directory integrace s Githubu | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Githubu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4395bd95-05de-4deb-87a5-dc3bc8ac4d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 688779de4e6627e49c0e3e8a7576f2f8c7a81ab1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Kurz: Azure Active Directory integrace s Githubu

V tomto kurzu zjistíte, jak toointegrate Githubu s Azure Active Directory (Azure AD).

Integrace Githubu s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooGitHub
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooGitHub (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Githubu, je třeba hello následující položky:

- Předplatné služby Azure AD
- Githubu jednotného přihlašování povolené předplatné


> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.


tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Githubu z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování


## <a name="adding-github-from-hello-gallery"></a>Přidání Githubu z Galerie hello
tooconfigure hello integrace Githubu do Azure AD, je nutné tooadd Githubu hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Githubu z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **webu GitHub.com**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/tutorial_github_search02.png)

5. Na panelu výsledků hello vyberte **Githubu**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/tutorial_github_search_result02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Githubu podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Githubu je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Githubu musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Githubu.

tooconfigure a testu Azure AD jednotné přihlašování s Githubu, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Githubu](#creating-a-GitHub-test-user)**  -toohave protějšek Britta Simon v Githubu, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci Githubu.

**tooconfigure Azure AD jednotné přihlašování s Githubu, proveďte následující kroky hello:**

1. V hello Azure Management portal na hello **Githubu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_01.png)

3. Na hello **Githubu domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_saml011.png)

    a. V hello **přihlašovací adresa URL** textovému poli, hodnota typu hello jako:`https://github.com/orgs/<entity-id>/sso`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://github.com/orgs/<entity-id>`

    > [!NOTE] 
    > Upozorňujeme, že tyto nejsou hello skutečné hodnoty. Máte tooupdate tyto hodnoty s hello skutečné Sing na adresu URL a identifikátor. Zde, doporučujeme vám toouse hello jedinečnou hodnotu řetězce v hello identifikátor. Přejděte tooGitHub správce části tooretrieve tyto hodnoty. 

4. Na hello **uživatelské atributy** vyberte **uživatelský identifikátor** jako user.mail.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_attribute_new01.png)
    
5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_03.png)

6. Na hello **vytvořit nový certifikát** dialogové okno, klikněte na ikonu hello kalendáře a vyberte **datum vypršení platnosti**. Pak klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_general_300.png)

7. Na hello **SAML podpisový certifikát** vyberte **aktivujte nový certifikát** a klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_04.png)

8. V místní nabídce hello **certifikát výměny** okně klikněte na tlačítko **OK**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_general_400.png)

9. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_05.png) 

10. Na hello **Githubu konfigurace** klikněte na tlačítko **konfigurace Githubu** tooopen **konfigurovat přihlášení** okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_06.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_07.png)

11. V okně prohlížeče jiný web Přihlaste se jako správce k organizace webu GitHub.

12. Přejděte příliš**nastavení** a klikněte na tlačítko **zabezpečení**

    ![Nastavení](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_03.png)

13. Zkontrolujte hello **ověřování povolit SAML** pole odhalil hello jednotné přihlašování konfigurační pole. Pak použijte hello jednoho přihlášení adresy URL hodnotu tooupdate hello jeden přihlašování adresu URL v konfiguraci služby Azure AD.

    ![Nastavení](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_13.png)

14. Nakonfigurujte hello následující pole:

    a. **Přihlaste na adrese URL**: Zadejte **SAML jednotné přihlašování adresa URL služby** z hello **konfigurace Githubu** části na Azure AD

    b. **Vystavitel**: Zadejte **SAML Entity ID** z hello **konfigurace Githubu** části na Azure AD

    c. **Veřejný certifikát**: Otevřete hello stáhnout certifikát z Azure AD v programu Poznámkový blok a zkopírujte hello obsah, včetně "Začít certifikát" a "END CERTIFICATE"

    ![Nastavení](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_051.png)

15. Klikněte na **Konfigurace testu SAML** tooconfirm, žádná chyb při ověřování nebo chyby během jednotného přihlašování.

    ![Nastavení](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_06.png)

16. Klikněte na tlačítko **uložit**

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**. 


### <a name="creating-a-github-test-user"></a>Vytvoření zkušebního uživatele Githubu

V pořadí tooenable Azure AD Uživatelé toolog do Githubu musí být zřízená na Githubu.  
V případě hello z Githubu zřizování je ruční úloha.

**tooprovision uživatelské účty, provádět hello následující kroky:**

1. Přihlaste se tooyour Githubu společnosti lokality jako správce.

2. Klikněte na tlačítko **osoby**.

    ![Lidé](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "osoby")

3. Klikněte na tlačítko **pozvání člen**.

    ![Uživatele pozvat](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "pozvat uživatele")

4. Na hello **pozvání člen** dialogové okno proveďte hello následující kroky:

    a. V hello **e-mailu** textovému poli, typ hello e-mailovou adresu účtu Britta Simon.

    ![Pozvěte lidi, kteří](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "pozvat uživatele")
    
    b. Klikněte na tlačítko **odeslat pozvánky**.

    ![Pozvěte lidi, kteří](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "pozvat uživatele")

    > [!NOTE]
    > Držitel účtu Azure Active Directory Hello bude dostávat e-mailu a postupujte podle tooconfirm odkaz svého účtu před stane aktivní.


### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooGitHub svůj přístup.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooGitHub, proveďte následující kroky hello:**

1. Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **webu GitHub.com**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-github-tutorial/tutorial_github_search_result021.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Githubu hello v hello přístupového panelu, měli byste obdržet přihlášeného tooyour Githubu aplikace. Pomocí svého osobního účtu, budete mít přihlášení jako účtu organizace, ale pak toolog potřeba.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-github-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-github-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-github-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-github-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-github-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-github-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-github-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-github-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-github-tutorial/tutorial_general_203.png
