---
title: 'Kurz: Azure Active Directory integrace s Clarizen | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Clarizen."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: f24ccda3b90e5df9a203a444dfda905043b30276
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Kurz: Azure Active Directory integrace s Clarizen

V tomto kurzu zjistíte, jak Azure Active Directory (Azure AD) s Clarizen toointegrate. Tato integrace nabízí hello následující výhody:

- Můžete ovládat, ve službě Azure AD, který má přístup tooClarizen.
- Můžete povolit vaši uživatelé toobe automaticky přihlášeni v tooClarizen (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě, hello portálu Azure.

Hello scénář v tomto kurzu se skládá z dvě hlavní úlohy:

1. Přidejte Clarizen z Galerie hello.
2. Konfigurace a otestování Azure AD jednotné přihlašování.

Pokud chcete další informace o softwaru jako integraci aplikace služby (SaaS) s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
Integrace služby Azure AD s Clarizen tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Clarizen odběr, který je povolený pro jednotné přihlašování

tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:

- Testování Azure AD jednotné přihlašování v testovacím prostředí. Nepoužívejte produkční prostředí, pokud je to nutné.
- Pokud nemáte testovacím prostředí s Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-clarizen-from-hello-gallery"></a>Přidat Clarizen z Galerie hello
tooconfigure hello integrace Clarizen do Azure AD, přidejte Clarizen hello Galerie tooyour seznamu spravovaných aplikací SaaS.

1. V hello [portál Azure](https://portal.azure.com), v levém podokně text hello, klikněte na hello **Azure Active Directory** ikonu.

    ![Azure Active Directory – ikona][1]

2. Klikněte na tlačítko **podnikové aplikace, které**. Pak klikněte na tlačítko **všechny aplikace**.

    ![Kliknutím na tlačítko "Podnikové aplikace" a "Všechny aplikace"][2]

3. Klikněte na tlačítko hello **přidat** tlačítka v horní části hello dialogového okna hello.

    ![tlačítko "Přidat" Hello][3]

4. Hello vyhledávacího pole zadejte **Clarizen**.

    ![Zadejte "Clarizen" hello vyhledávacího pole](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_000.png)

5. V podokně výsledků hello, vyberte **Clarizen**a potom klikněte na **přidat** tooadd hello aplikace.

    ![Výběr Clarizen v podokně výsledků hello](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V následujících částech hello nakonfigurovat a otestovat Azure AD jednotné přihlašování s Clarizen na základě hello testovací uživatele Britta Simon.

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Clarizen je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Clarizen musí toobe navázat. Vytvořit tento odkaz vztah přiřazením hello hodnotu **uživatelské jméno** ve službě Azure AD jako hodnota hello **uživatelské jméno** v Clarizen.

tooconfigure a testování Azure AD jednotné přihlašování s Clarizen, dokončení hello stavební bloky následující:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Clarizen](#create-a-clarizen-test-user)**  toohave protějšek Britta Simon v Clarizen, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování
Povolení služby Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Clarizen.

1. V portálu Azure, na hello hello **Clarizen** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Kliknutím na tlačítko "Single sign-on"][4]

2. V hello **jednotného přihlašování** dialogové okno, pro **režimu**, vyberte **na základě SAML přihlašování** tooenable jednotné přihlašování.

    ![Výběr "na základě SAML přihlášení"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_01.png)

3. V hello **Clarizen domény a adresy URL** část, proveďte následující kroky hello:

    ![Pole pro adresu URL identifikátor dotazů a odpovědí](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_02.png)

    a. V hello **identifikátor** pole Hodnota typu hello jako: **Clarizen**

    b. V hello **adresa URL odpovědi** zadejte adresu URL pomocí hello následující vzor: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**

    > [!NOTE]
    > Tyto nejsou hello skutečné hodnoty. Máte toouse hello skutečné identifikátor a adresa URL odpovědi. Tady doporučujeme použít hello jedinečnou hodnotu řetězce, jak hello identifikátor. tooget hello skutečnými hodnotami, kontaktujte hello [tým podpory Clarizen](https://success.clarizen.com/hc/en-us/requests/new).

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.

    ![Kliknutím na tlačítko "Vytvořit nový certifikát"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_03.png)  

5. V hello **vytvořit nový certifikát** dialogové okno pole, klikněte na ikonu hello kalendáře a vyberte datum vypršení platnosti. Potom klikněte na **Uložit**.

    ![Výběr a ukládání datum vypršení platnosti](./media/active-directory-saas-clarizen-tutorial/tutorial_general_300.png)

6. V hello **SAML podpisový certifikát** vyberte **aktivujte nový certifikát**a potom klikněte na **Uložit**.

    ![Výběrem zaškrtávacího políčka hello pro vytvoření nového certifikátu hello active](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_04.png)

7. V hello **certifikát výměny** dialogové okno, klikněte na tlačítko **OK**.

    ![Kliknutím na tlačítko "OK" tooconfirm, které chcete certifikát hello toomake active](./media/active-directory-saas-clarizen-tutorial/tutorial_general_400.png)

8. V hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Kliknutím na tlačítko Stáhnout hello toostart "Certifikát (Base64)"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_05.png)

9. V hello **Clarizen konfigurace** klikněte na tlačítko **konfigurace Clarizen** tooopen hello **konfigurovat přihlášení** okno.

    ![Kliknutím na tlačítko "Konfigurace Clarizen"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_06.png)

    ![Okno "Konfigurace přihlášení", včetně souborů a adresy URL](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_07.png)

10. V okně prohlížeče jiný web přihlaste jako správce tooyour Clarizen společnosti lokality.

11. Klikněte na své uživatelské jméno a potom klikněte na **nastavení**.

    ![Kliknutím na "Nastavení" v části vaše uživatelské jméno](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "nastavení")

12. Klikněte na tlačítko hello **globální nastavení** kartě. Pak další příliš**federovaného ověřování**, klikněte na tlačítko **upravit**.

    ![Karta "Globální nastavení"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "globální nastavení")

13. V hello **federovaného ověřování** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno "Federovaného ověřování"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "federovaného ověřování")

    a. Vyberte **povolit federovaného ověřování**.

    b. Klikněte na tlačítko **nahrát** tooupload stažený certifikát.

    c. V hello **přihlašovací adresa URL** zadejte hodnotu hello **SAML jeden přihlašování adresa URL služby** z okna konfigurace aplikace hello Azure AD.

    d. V hello **Sign-out URL** zadejte hodnotu hello **Sign-Out URL** z okna konfigurace aplikace hello Azure AD.

    e. Vyberte **použít POST**.

    f. Klikněte na **Uložit**.

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
V hello portálu Azure vytvoření zkušebního uživatele volat Britta Simon.

![Jméno a e-mailovou adresu uživatele testovací hello Azure AD][100]

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** ikonu.

    ![Azure Active Directory – ikona](./media/active-directory-saas-clarizen-tutorial/create_aaduser_01.png)

2. Klikněte na tlačítko **uživatelů a skupin**a potom klikněte na **všichni uživatelé** toodisplay hello seznam uživatelů.

    ![Kliknutím na tlačítko "Uživatelé a skupiny" a "Všichni uživatelé"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_02.png)

3. V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.

    ![tlačítko "Přidat" Hello](./media/active-directory-saas-clarizen-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno "User" s název, e-mailovou adresu a heslo vyplněno](./media/active-directory-saas-clarizen-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu hello Britta Simon účtu.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.

### <a name="create-a-clarizen-test-user"></a>Vytvoření zkušebního uživatele Clarizen
tooenable toosign uživatele Azure AD v tooClarizen, je třeba zřídit uživatelské účty. V případě hello Clarizen zřizování je ruční úloha.

1. Přihlaste se tooyour Clarizen společnosti lokality jako správce.

2. Klikněte na tlačítko **osoby**.

    ![Kliknutím na tlačítko "Uživatelé"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "osoby")

3. Klikněte na tlačítko **pozvat uživatele**.

    ![Tlačítko "Pozvat uživatele"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "pozvat uživatele")

4. V hello **pozvat osoby** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno "Pozvat osoby"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "pozvat osoby")

    a. V hello **e-mailu** pole typu hello e-mailovou adresu hello Britta Simon účtu.

    b. Klikněte na tlačítko **pozvat**.

    > [!NOTE]
    > Držitel účtu Azure Active Directory Hello bude dostávat e-mailu a postupujte podle tooconfirm odkaz svého účtu před stane aktivní.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele
Povolte Britta Simon toouse Azure jednotné přihlašování udělení jeho tooClarizen přístup.

![Přiřazené testovacího uživatele][200]

1. V hello portálu Azure, otevřete zobrazení aplikace hello, procházet toohello directory zobrazení, klikněte na **podnikové aplikace, které**a potom klikněte na **všechny aplikace**.

    ![Kliknutím na tlačítko "Podnikové aplikace" a "Všechny aplikace"][201]

2. V seznamu aplikace hello vyberte **Clarizen**.

    ![Výběr Clarizen v seznamu hello](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_50.png)

3. V levém podokně hello, klikněte na **uživatelů a skupin**.

    ![Kliknutím na tlačítko "Uživatelé a skupiny"][202]

4. Klikněte na tlačítko hello **přidat** tlačítko. Potom v hello **přidat přiřazení** dialogové okno, vyberte **uživatelů a skupin**.

    ![tlačítko "Přidat" Hello a dialogové okno "Přidat přiřazení" hello][203]

5. V hello **uživatelů a skupin** dialogové okno, vyberte **Britta Simon** v hello seznam uživatelů.

6. V hello **uživatelů a skupin** dialogovém okně klikněte na hello **vyberte** tlačítko.

7. V hello **přidat přiřazení** dialogovém okně klikněte na hello **přiřadit** tlačítko.

### <a name="test-single-sign-on"></a>Test jednotného přihlašování
Otestujte vaše konfigurace Azure AD jeden přihlašování pomocí hello přístupového panelu.

Když kliknete na dlaždici Clarizen hello v hello přístupového panelu, by měl být automaticky přihlášeni tooyour Clarizen aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů toointegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_203.png
