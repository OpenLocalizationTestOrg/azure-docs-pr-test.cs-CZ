---
title: "Kurz: Azure Active Directory integrace s Symantec webové zabezpečení služby (WSS) | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Symantec webové zabezpečení služby (WSS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 9f02b3d4ce2073110c55af4b567b0e3b5a88404f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a>Kurz: Azure Active Directory integrace s Symantec webové zabezpečení služby (WSS)

V tomto kurzu se dozvíte, jak toointegrate vaší společnosti Symantec webové zabezpečení služby (WSS) účet pomocí účtu Azure Active Directory (Azure AD), aby se WSS koncový uživatel zřízené v hello Azure AD ověřit pomocí ověřování SAML a vynutit uživatele nebo pravidla úrovni zásady skupiny.

Integrace Symantec webové zabezpečení služby (WSS) s Azure AD poskytuje hello následující výhody:

- Spravujte všechny hello koncoví uživatelé a skupiny používané účtu WSS z portálu Azure AD. 

- Povolí hello koncoví uživatelé tooauthenticate sami ve WSS pomocí svých přihlašovacích údajů Azure AD.

- Povolte hello vynucení uživatele a skupiny zásadou na úrovni pravidla definovaná v účtu WSS.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Symantec webové zabezpečení služby (WSS), je třeba hello následující položky:

- Předplatné služby Azure AD
- Účet služby pro zabezpečení webové Symantec (WSS)

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme pomocí WSS účtu, který je aktuálně používá pro produkční účely.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte účtu WSS, který je aktuálně používá pro produkční účely pro tento test Pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu nakonfigurujete vaší služby Azure AD tooenable jeden přihlašování tooWSS pomocí přihlašovacích údajů koncového uživatele hello definované v účtu Azure AD.
Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání aplikace hello Symantec webové zabezpečení služby (WSS) z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-symantec-web-security-service-wss-from-hello-gallery"></a>Přidání Symantec webové zabezpečení služby (WSS) z Galerie hello
tooconfigure hello integrace Symantec webové zabezpečení služby (WSS) do Azure AD, je nutné tooadd Symantec webové zabezpečení služby (WSS) hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Symantec webové zabezpečení služby (WSS) z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **Symantec webové zabezpečení služby (WSS)**, vyberte **Symantec webové zabezpečení služby (WSS)** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Symantec webové zabezpečení služby (WSS) v seznamu výsledků hello](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Symantec webové zabezpečení služby (WSS) podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Symantec webové zabezpečení služby (WSS) je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Symantec webové zabezpečení služby (WSS) musí toobe navázat.

V Symantec webové zabezpečení služby (WSS) přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testování Azure AD jednotné přihlašování s Symantec webové zabezpečení služby (WSS), je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Symantec webové zabezpečení služby (WSS)](#create-a-symantec-web-security-service-wss-test-user)**  -toohave protějšek Britta Simon v zabezpečení webové Symantec služby (WSS), je toohello propojené služby Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Symantec webové zabezpečení služby (WSS).

**tooconfigure Azure AD jednotné přihlašování s Symantec webové zabezpečení služby (WSS) proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Symantec webové zabezpečení služby (WSS)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. Na hello **Symantec webové zabezpečení služby (WSS) domény a adresy URL** část, proveďte následující kroky hello:

    ![Symantec webové zabezpečení služby (WSS) domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    a. V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://saml.threatpulse.net:8443/saml/saml_realm`

    b. V hello **adresa URL odpovědi** textovému poli, hello zadat adresu URL:`https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`

    > [!NOTE]
    > Obraťte se na hello [tým podpory Symantec webové zabezpečení služby (WSS) klienta](https://www.symantec.com/contact-us) Pokud hello hodnoty pro hello **identifikátor** a **adresa URL odpovědi** nefungují z nějakého důvodu.

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-symantec-tutorial/tutorial_general_400.png)
    
6. tooconfigure jednotného přihlašování na hello straně Symantec webové zabezpečení služby (WSS), naleznete v online dokumentaci WSS toohello. Hello Stáhnout **soubor XML s metadaty** soubor bude nutné importovat do portálu WSS hello toobe. Kontaktujte hello [tým podpory Symantec webové zabezpečení služby (WSS)](https://www.symantec.com/contact-us) Pokud potřebujete pomoc s konfigurací hello na portálu WSS hello.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-symantec-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-symantec-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-symantec-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-symantec-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-symantec-web-security-service-wss-test-user"></a>Vytvoření zkušebního uživatele Symantec webové zabezpečení služby (WSS)

V této části vytvoříte uživatele volat Britta Simon v Symantec webové zabezpečení služby (WSS). Hello odpovídající end uživatelské jméno je možné ručně vytvořit hello WSS portálu nebo můžete počkat, hello uživatele nebo skupiny zřízené v portálu WSS hello Azure AD synchronizovány toobe toohello po několika minutách (~ 15 minut). Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování. Hello veřejnou IP adresu počítače hello koncového uživatele, který bude použité toobrowse weby také potřebovat toobe zřízené v portálu společnosti Symantec webové zabezpečení služby (WSS) hello.

> [!NOTE]
> Prosím [kliknutím sem](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) tooget váš počítač je veřejná IP adresa.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooSymantec zabezpečení webové služby (WSS) toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign tooSymantec Britta Simon webové zabezpečení služby (WSS), proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Symantec webové zabezpečení služby (WSS)**.

    ![odkaz Hello Symantec webové zabezpečení služby (WSS) v seznamu aplikace hello](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části budete testovat hello jeden přihlašování funkce nyní, když jste nakonfigurovali vaší toouse WSS účet služby Azure AD pro ověřování SAML.

Po konfiguraci vaší webové prohlížeče tooproxy provoz tooWSS, když jste otevřete ve webovém prohlížeči a zkuste to toobrowse tooa lokality, pak budete stránku přesměrovaného toohello Azure přihlášení. Zadejte přihlašovací údaje hello hello testovací koncového uživatele, která byla zajištěna v hello Azure AD (BrittaSimon) a přidružené heslo. Po ověření, budete moct toobrowse toohello web, který jste zvolili. Měli jste vytvoření pravidla zásad na hello WSS straně tooblock BrittaSimon z procházení tooa určité lokalitě, potom hello WSS bloku stránky měli vidět, pokusíte-li jako uživatel BrittaSimon toobrowse toothat lokality.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_203.png

