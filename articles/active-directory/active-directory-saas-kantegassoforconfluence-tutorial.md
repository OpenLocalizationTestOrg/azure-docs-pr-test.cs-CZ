---
title: "Kurz: Azure Active Directory integrace s Kantega jednotné přihlašování pro soutoku | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Kantega jednotné přihlašování pro soutoku."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d0d99c14-a6ca-45f2-bb84-633126095e7a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: b35eb8757e41e86228a3a9ee10869086cc801c9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-confluence"></a>Kurz: Azure Active Directory integrace s Kantega jednotné přihlašování pro soutoku

V tomto kurzu zjistíte, jak toointegrate Kantega jednotné přihlašování pro soutoku s Azure Active Directory (Azure AD).

Integrace Kantega jednotné přihlašování pro soutoku s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooKantega jednotné přihlašování pro soutoku
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooKantega jednotné přihlašování pro soutoku (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD pomocí jednotného přihlašování Kantega pro soutoku tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Předplatné povolené Kantega SSO pro soutoku jednotné přihlašování

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Kantega jednotné přihlašování pro soutoku z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-kantega-sso-for-confluence-from-hello-gallery"></a>Přidání Kantega jednotné přihlašování pro soutoku z Galerie hello
integrace hello tooconfigure Kantega přihlašování pro soutoku do služby Azure AD, potřebujete pro soutoku hello Galerie tooyour seznamu spravovaných aplikací SaaS tooadd Kantega jednotné přihlašování.

**tooadd Kantega jednotné přihlašování pro soutoku z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Kantega jednotné přihlašování pro soutoku**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_search.png)

5. Na panelu výsledků hello vyberte **Kantega jednotné přihlašování pro soutoku**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí jednotného přihlašování Kantega pro soutoku podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello Kantega SSO pro soutoku je tooa uživatelem ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello Kantega SSO pro soutoku musí toobe navázat.

V Kantega jednotné přihlašování pro soutoku, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testování Azure AD jednotné přihlašování pomocí jednotného přihlašování Kantega soutoku, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření Kantega SSO pro testovací uživatele soutoku](#creating-a-kantega-sso-for-confluence-test-user)**  -toohave protějšek Britta Simon Kantega SSO pro soutoku, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší Kantega jednotné přihlašování pro aplikace soutoku.

**tooconfigure Azure AD jednotné přihlašování pomocí jednotného přihlašování Kantega pro soutoku, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Kantega jednotné přihlašování pro soutoku** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_samlbase.png)

3. V **IDP** iniciované režimu na hello **Kantega SSO soutoku domény a adresy URL** části provést následující krok hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url1.png)

    a. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

4. V **SP** initiated režimu, zkontrolujte **zobrazit upřesňující nastavení adresy URL** a proveďte následující krok hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url2.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL. Tyto hodnoty jsou přijímány během konfigurace hello soutoku modul plug-in, který je vysvětlen později v kurzu hello.

5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_400.png)
    
7. V okně prohlížeče jiných webových přihlásit tooyour **portál pro správu soutoku** jako správce.

8. Pozastavte ukazatel myši na ikonu a klikněte na tlačítko hello **doplňky**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon1.png)

9. V části **ATLASSIAN MARKETPLACE** , klikněte na **najít nové rozšíření**. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon.png)

10. Hledání **Kantega jednotné přihlašování pro protokol Kerberos SAML soutoku** a klikněte na tlačítko **nainstalovat** tooinstall tlačítko hello nové zásuvný modul SAML.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon2.png)

11. spuštění instalace modulu plug-in Hello.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon3.png)

12. Po dokončení instalace hello. Klikněte na **Zavřít**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon33.png)

13. Klikněte na **Manage** (Spravovat).

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon34.png)
    
14. Klikněte na tlačítko **konfigurace** tooconfigure hello nového modulu plug-in.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon35.png)

15. Tento nový modul plug-in můžete také najít v části **uživatelů a zabezpečení** kartě.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon36.png)
    
16. V hello **SAML** části. Vyberte **Azure Active Directory (Azure AD)** z hello **zprostředkovatele identity přidat** rozevíracího seznamu.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon4.png)

17. Vyberte úroveň předplatné jako **základní**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon5.png)       

18. Na hello **vlastností aplikace** část, proveďte následující kroky: 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon6.png)

    a. Kopírování hello **identifikátor ID URI aplikace** hodnotu a použít ho jako **identifikátor, adresa URL odpovědi a přihlašovací adresa URL** na hello **Kantega SSO soutoku domény a adresy URL** části na portálu Azure.

    b. Klikněte na **Další**.

19. Na hello **import metadat** část, proveďte následující kroky: 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon7.png)

    a. Vyberte **soubor metadat v mém počítači**a metadata souboru k odeslání, který jste si stáhli z portálu Azure.

    b. Klikněte na **Další**.

20. Na hello **název a jednotného přihlašování k umístění** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon8.png)
    
    a. Přidat název hello zprostředkovatele Identity v **název zprostředkovatele Identity** textbox (např. Azure AD).

    b. Klikněte na **Další**.

21. Ověřte hello podpisového certifikátu a klikněte na **Další**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon9.png)

22. Na hello **soutoku uživatelské účty** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon10.png)

    a. Vyberte **v případě potřeby vytvořte uživatele v adresáři interní na soutoku** a zadejte vhodný název hello hello skupiny pro uživatele (může být více ne. skupin oddělené čárkou).

    b. Klikněte na **Další**.

23. Klikněte na **Dokončit**.   

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon11.png)

24. Na hello **známé domén pro Azure AD** část, proveďte následující kroky: 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon12.png)

    a. Vyberte **známé domén** z hello levém panelu hello stránky.

    b. Zadejte název domény v hello **známé domén** textové pole.

    c. Klikněte na **Uložit**. 

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-kantega-sso-for-confluence-test-user"></a>Vytváření Kantega SSO pro soutoku testovacího uživatele

Uživatelé toolog tooenable Azure AD v tooConfluence, se musí být zřízená do soutoku. V případě hello Kantega přihlašování pro soutoku zřizování je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Tooyour Kantega jednotné přihlašování pro web společnosti soutoku se přihlaste jako správce.

2. Pozastavte ukazatel myši na ikonu a klikněte na tlačítko hello **Správa uživatelů**.

    ![Můžete přidat zaměstnance](./media/active-directory-saas-kantegassoforconfluence-tutorial/user1.png) 

3. V části uživatelé klikněte na tlačítko **přidat uživatele** kartě. Na hello **"Přidat uživatele"** dialogové okno proveďte hello následující kroky:

    ![Můžete přidat zaměstnance](./media/active-directory-saas-kantegassoforconfluence-tutorial/user2.png) 

    a. V hello **uživatelské jméno** jako typ hello e-mailu uživatele k textovému poli, Brittasimon@contoso.com.

    b. V hello **úplný název** textovému poli, úplný název typu hello uživatele jako Britta Simon.

    c. V hello **e-mailu** jako typ hello e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.

    d. V hello **heslo** textovému poli, zadejte hello heslo pro uživatele.

    e. Klikněte na tlačítko **Potvrdit heslo** zadejte znovu heslo hello.
    
    f. Klikněte na tlačítko **přidat** tlačítko.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooKantega jednotné přihlašování pro soutoku Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign tooKantega Britta Simon jednotného přihlašování pro soutoku, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Kantega jednotné přihlašování pro soutoku**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello Kantega jednotné přihlašování pro dlaždici soutoku v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Kantega jednotné přihlašování pro aplikace soutoku.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_203.png

