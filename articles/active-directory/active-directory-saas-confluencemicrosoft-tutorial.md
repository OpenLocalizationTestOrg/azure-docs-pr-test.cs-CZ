---
title: "Kurz: Integrace Azure Active Directory pomocí jednotného přihlašování SAML soutoku microsoftem | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a jednotné přihlašování SAML soutoku společností Microsoft."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1ad1cf90-52bc-4b71-ab2b-9a5a1280fb2d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: ace23800e3908c8125052b4a2edcaae71f19935d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-confluence-saml-sso-by-microsoft"></a>Kurz: Integrace Azure Active Directory pomocí jednotného přihlašování SAML soutoku společností Microsoft

V tomto kurzu zjistíte, jak toointegrate jednotné přihlašování SAML soutoku společností Microsoft s Azure Active Directory (Azure AD).

Integrace jednotné přihlašování SAML soutoku společností Microsoft s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooConfluence jednotné přihlašování SAML společností Microsoft
- Vaši uživatelé tooautomatically get přihlášeného tooConfluence jednotné přihlašování SAML společností Microsoft (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD pomocí jednotného přihlašování SAML soutoku společností Microsoft, je třeba hello následující položky:

- Předplatné služby Azure AD
- Aplikace serveru soutoku nainstalovaný na 64bitovou verzi Windows serveru (místní nebo v cloudu hello infrastruktury IaaS)
- Soutoku server je povolen protokol HTTPS
- Poznámka hello podporované verze pro modul plug-in soutoku jsou uvedené v následující části.
- Soutoku server je dostupný na Internetu zvlášť tooAzure AD přihlašovací stránka pro ověřování a musí mít tooreceive hello tokenu z Azure AD
- Přihlašovací údaje správce se nastavují v soutoku
- V soutoku je zakázáno WebSudo
- Testovací uživatel vytvořené v hello soutoku serverové aplikace

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme pomocí soutoku provozním prostředí. Integrace hello se nejdřív otestujte v vývoj nebo pracovní prostředí hello aplikace a pak použijte hello produkčního prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-confluence"></a>Podporované verze systému soutoku 

Od nyní jsou podporovány následující verze soutoku:

- Soutoku: 5.0 too5.10

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání jednotné přihlašování SAML soutoku společností Microsoft z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-confluence-saml-sso-by-microsoft-from-hello-gallery"></a>Přidání jednotné přihlašování SAML soutoku společností Microsoft z Galerie hello
tooconfigure hello integrace přihlašování SAML soutoku společností Microsoft do služby Azure AD, je nutné tooadd jednotné přihlašování SAML soutoku Microsoft hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd jednotné přihlašování SAML soutoku společností Microsoft z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **jednotné přihlašování SAML soutoku Microsoft**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_search.png)

5. Na panelu výsledků hello vyberte **jednotné přihlašování SAML soutoku Microsoft**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML soutoku společnosti Microsoft podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v jednotné přihlašování SAML soutoku společností Microsoft je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello soutoku SAML SSO společností Microsoft musí toobe navázat.

V soutoku jednotné přihlašování SAML společností Microsoft, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testování Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML soutoku společností Microsoft, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření jednotné přihlašování SAML soutoku uživatelem testovací Microsoft](#creating-a-confluence-saml-sso-by-microsoft-test-user)**  -toohave protějšek Britta Simon soutoku SAML SSO společnosti Microsoft, který je propojený toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší jednotné přihlašování SAML soutoku aplikací společnosti Microsoft.

**tooconfigure Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML soutoku společností Microsoft, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **jednotné přihlašování SAML soutoku Microsoft** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_samlbase.png)

3. Na hello **jednotné přihlašování SAML soutoku Microsoft Domain a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<domain:port>/plugins/servlet/saml/auth`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<domain:port>/`

    c. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL. Port je volitelný, v případě, že je adresa URL s názvem. Tyto hodnoty jsou přijímány během konfigurace hello soutoku modul plug-in, který je vysvětlen později v kurzu hello.
 

4. toogenerate hello **Metadata** adresu url, proveďte následující kroky hello:

    a. Klikněte na tlačítko **registrace aplikace**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Confluencemicrosoft-tutorial/appregistrations.png)
   
    b. Klikněte na tlačítko **koncové body** tooopen **koncové body** dialogové okno.  
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpointicon.png)

    c. Klikněte na tlačítko hello kopie tlačítko toocopy **dokument FEDERAČNÍCH METADAT** adresy url a vložte do poznámkového bloku.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpoint.png)
     
    d. Nyní přejděte na stránce vlastností toohello **jednotné přihlašování SAML soutoku Microsoft** a kopírování hello **Id aplikace** pomocí **kopie** tlačítko a vložte do poznámkového bloku.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Confluencemicrosoft-tutorial/appid.png)

    e. Generovat hello **adresu URL metadat** pomocí hello následující vzor: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` a zkopírujte tuto hodnotu v poznámkovém bloku, jak se později používá pro konfiguraci hello hello modulu plug-in.

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Confluencemicrosoft-tutorial/tutorial_general_400.png)

6. Obraťte se na [Microsoft](mailto:waadpartners@microsoft.com) s hello následující informace pro modul plug-in soutoku hello.
    
    *   Jméno zákazníka:
    *   Název domény:
    *   Azure AD Premium: Ano/Ne (modul plug-in bude k dispozici tooall hello zákazníka volné, Basic a skladová položka Premium)
    *   Počet uživatelů, kteří budou používat Tato integrace:
    *   Soutoku verze:
    *   Poznámka:

7. V okně prohlížeče jiný web Přihlaste se jako správce v instanci soutoku tooyour.

8. Pozastavte ukazatel myši na ikonu a klikněte na tlačítko hello **doplňky**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon1.png)

9. Karta části doplňky, klikněte na tlačítko **spravovat doplňky**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon72.png)

10. Ručně odešlete modulu plug-in hello od společnosti Microsoft. Po instalaci modulu plug-in hello se zobrazí v **uživatel nainstaloval** doplňky části **spravovat rozšíření** části.

11. Klikněte na tlačítko **konfigurace** tooconfigure hello nového modulu plug-in.

12. Proveďte následující kroky na stránce konfigurace:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon5.png)
 
    a. V **adresu URL metadat** vložit hello **adresu URL metadat** generované z Azure AD a klikněte na tlačítko hello **vyřešit** tlačítko. Přečte adresu URL metadat hello IdP a naplní všechny informace pole hello.

    > [!Note]
    > Výchozí umístění SAML ID uživatele je identifikátor jména. Můžete změnit tato tooan atribut možnost a zadejte název odpovídajícího atributu hello.

    > [!TIP]
    > Zajistěte, aby pouze jeden certifikát namapované proti hello aplikace tak, aby se nezobrazí žádná chyba při řešení hello metadat. Pokud máte víc certifikátů, při řešení hello metadata, získá správce k chybě.
    
    b. Kopírování hello **identifikátor, adresa URL odpovědi a adresa URL přihlašování** hodnoty a vložte je do **identifikátor, adresa URL odpovědi a adresa URL přihlašování** textových polí v uvedeném pořadí v **jednotné přihlašování SAML soutoku Microsoft Domain a adresy URL**  části na portálu Azure.

    c. V **přihlašovací jméno tlačítko** hello název typu tlačítka vaše organizace hodlá hello toosee uživatele na přihlašovací obrazovce.

    d. V **SAML uživatele ID umístění** vyberte buď **ID uživatele je v elementu NameIdentifier hello hello subjektu příkaz** nebo **ID uživatele je v elementu atribut**.  Toto ID má id uživatele soutoku toobe hello. Pokud id uživatele hello neodpovídá, systém nedovolí toolog uživatelé v. 
    
    e. Pokud vyberete **ID uživatele je v elementu atribut** možnost, pak v **název atributu** textového pole Název hello typu atributu hello, kde je očekávána Id uživatele. 

    f. Pokud používáte hello federované domény (například služby AD FS atd.) s Azure AD, potom klikněte na hello **povolit zjišťování domovské sféry** řádku a nakonfigurovat hello **název domény**.
    
    g. V **název domény** hello domény sem napište název v případě hello přihlášení na základě služby AD FS.

    h. Zkontrolujte **povolit jednotné přihlašování se** z Azure AD, když se uživatel neodhlásí z soutoku toolog si chcete. 

    i. Klikněte na tlačítko **Uložit** tlačítko toosave hello nastavení.


> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-confluence-saml-sso-by-microsoft-test-user"></a>Vytváření jednotné přihlašování SAML soutoku Microsoft testovací uživatel

Uživatelé toolog tooenable Azure AD v tooConfluence na místním serveru, se musí být zřízená do jednotné přihlašování SAML soutoku společností Microsoft. Pro jednotné přihlašování SAML soutoku společností Microsoft zřizování je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Tooyour soutoku na místním serveru se přihlaste jako správce.

2. Pozastavte ukazatel myši na ikonu a klikněte na tlačítko hello **Správa uživatelů**.

    ![Můžete přidat zaměstnance](./media/active-directory-saas-confluencemicrosoft-tutorial/user1.png) 

3. V části uživatelé klikněte na tlačítko **přidat uživatele** kartě. Na hello **"Přidat uživatele"** dialogové okno proveďte hello následující kroky:

    ![Můžete přidat zaměstnance](./media/active-directory-saas-confluencemicrosoft-tutorial/user2.png) 

    a. V hello **uživatelské jméno** textovému poli, typ hello e-mailu uživatele jako Britta Simon.

    b. V hello **úplný název** textovému poli, úplný název typu hello uživatele jako Britta Simon.

    c. V hello **e-mailu** jako typ hello e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.

    d. V hello **heslo** textovému poli, zadejte hello heslo pro Britta Simon.

    e. Klikněte na tlačítko **Potvrdit heslo** zadejte znovu heslo hello.
    
    f. Klikněte na tlačítko **přidat** tlačítko.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části je povolit Britta Simon toouse Azure jednotné přihlašování tak, že udělíte přístup tooConfluence jednotné přihlašování SAML společností Microsoft.

![Přiřadit uživatele][200] 

**tooassign tooConfluence Britta Simon jednotné přihlašování SAML společností Microsoft, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **jednotné přihlašování SAML soutoku Microsoft**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello jednotné přihlašování SAML soutoku podle Microsoft dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour jednotné přihlašování SAML soutoku aplikací společnosti Microsoft.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_203.png
