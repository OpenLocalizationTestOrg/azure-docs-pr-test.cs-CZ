---
title: 'Kurz: Azure Active Directory integrace s Enterprise smysl Qlik | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Qlik smysl Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 153edb3f8cd41790d4e9a74f15cfb806e5e9e3b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Kurz: Azure Active Directory integrace s Qlik smysl Enterprise

V tomto kurzu zjistíte, jak toointegrate Qlik smysl Enterprise s Azure Active Directory (Azure AD).

Integrace Qlik smysl Enterprise s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooQlik smysl Enterprise.
- Vaši uživatelé tooautomatically get přihlášeného tooQlik smysl Enterprise (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Enterprise Qlik smysl, je třeba hello následující položky:

- Předplatné služby Azure AD
- Qlik smysl Enterprise jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Qlik smysl Enterprise z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-qlik-sense-enterprise-from-hello-gallery"></a>Přidání Qlik smysl Enterprise z Galerie hello
tooconfigure hello integrace Qlik smysl Enterprise do Azure AD, je nutné tooadd Qlik smysl Enterprise hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Qlik smysl Enterprise z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **Qlik smysl Enterprise**, vyberte **Qlik smysl Enterprise** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Enterprise Qlik smysl v seznamu výsledků hello](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Enterprise smysl Qlik podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello protějšku uživatele v podniku smysl Qlik je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Qlik smysl Enterprise musí toobe navázat.

V Qlik smysl Enterprise, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Enterprise Qlik smysl, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Qlik smysl Enterprise](#create-a-qlik-sense-enterprise-test-user)**  -toohave protějšek Britta Simon v Qlik smysl organizace, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Qlik smysl Enterprise.

**tooconfigure Azure AD jednotné přihlašování s Qlik smysl Enterprise, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Qlik smysl Enterprise** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_samlbase.png)

3. Na hello **Qlik smysl podnikové domény a adresy URL** část, proveďte následující kroky hello:

    ![Qlik smysl podnikové domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`
    
    > [!NOTE]
    > Všimněte si hello koncové lomítko na konci hello tento identifikátor URI. Je vyžadována.
    
    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizace tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor, který je popsán později v tomto kurzu nebo kontaktujte [tým podpory klient systému Enterprise smysl Qlik](https://www.qlik.com/us/services/support) tooget tyto hodnoty. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_400.png)

6. Připravte si soubor XML federační Metadata hello tak, aby tento server smysl tooQlik můžete nahrát.
   
    > [!NOTE]
    > Před nahráním hello IdP metadata toohello Qlik smysl serveru, třeba soubor hello toobe upravit tooremove informace tooensure správné operace mezi službou Azure AD a server Qlik smysl.
    
    ![QlikSense][qs24]
   
    a. Otevřete soubor FederationMetaData.xml hello, který jste si stáhli z portálu Azure v textovém editoru.
   
    b. Vyhledejte hodnotu hello **RoleDescriptor**.  Existují čtyři záznamy (dvě dvojice otevření a zavření značky elementu).
   
    c. Odstraníte hello RoleDescriptor značky a všechny informace v rozmezí ze souboru hello.
   
    d. Uložte soubor hello a udržovat nedaleko pro použití později v tomto dokumentu.

7. Jako uživatel, který můžete vytvořit virtuální proxy konfigurace přejděte toohello Qlik smysl Qlik Management Console (QMC).

8. V hello QMC, klikněte na hello **virtuální proxy** položku nabídky.
   
    ![QlikSense][qs6] 

9. V hello dolní části úvodní obrazovka, klikněte na položku hello **vytvořit nový** tlačítko.
   
    ![QlikSense][qs7]

10. Zobrazí se obrazovka úpravy proxy virtuálním Hello.  Na hello je pravé straně úvodní obrazovka nabídky pro viditelnosti možnosti konfigurace.
   
    ![QlikSense][qs9]

11. Možnost nabídky identifikace hello zaškrtnuto zadejte hello identifikační informace pro konfiguraci Azure virtuální proxy hello.
    
    ![QlikSense][qs8]  
    
    a. Hello **popis** pole je popisný název pro konfiguraci proxy serveru virtuálním hello.  Zadejte hodnotu pro popis.
    
    b. Hello **předpony** pole identifikuje koncový bod hello virtuální proxy pro připojení tooQlik smysl s Azure AD jednotné přihlašování.  Zadejte název jedinečnou předponu pro tento virtuální server proxy.
    
    c. **Časový limit nečinnosti relace (minuty)** hello vypršel časový limit pro připojení přes tuto virtuální proxy serveru.
    
    d. Hello **název hlavičky souboru cookie relace** je ukládání název souboru cookie hello hello identifikátor relace pro hello Qlik smysl relace a uživatel obdrží po úspěšném ověření.  Tento název musí být jedinečný.

12. Klikněte na hello ověřování nabídky možnost toomake je viditelné.  Zobrazí se obrazovka Hello ověřování.
    
    ![QlikSense][qs10]
    
    a. Hello **anonymní přístup režimu** rozevíracího seznamu určuje Pokud anonymní uživatelé mohou přistupovat k Qlik smysl prostřednictvím proxy serveru virtuálním hello.  Hello výchozí možnost je žádné anonymního uživatele.
    
    b. Hello **metodu ověřování** rozevíracího seznamu určuje hello ověřování proxy schéma virtuální server hello použije.  Vyberte z rozevíracího seznamu hello SAML.  Proto se zobrazí další možnosti.
    
    c. V hello **pole URI hostitel SAML**, vstupní hello hostname uživatelé zadat tooaccess Qlik smysl prostřednictvím tento proxy server virtuální SAML.  název hostitele Hello je identifikátor uri hello hello Qlik smysl serveru.
    
    d. V hello **SAML entity ID**, zadejte hello stejnou hodnotu zadaná pro pole hello SAML hostitel identifikátoru URI.
    
    e. Hello **SAML IdP metadata** je soubor hello upravit dříve v hello **upravit federačních metadat z konfigurace služby Azure AD** části.  **Před nahráním hello IdP metadata, třeba soubor hello toobe upravit** tooremove informace tooensure správné operace mezi službou Azure AD a server Qlik smysl.  **Pokud má soubor hello ještě upravit toobe najdete výše uvedené pokyny toohello.**  Pokud bylo upraveno hello souboru klikněte na hello tlačítko Procházet a vyberte hello upravená metadata souboru tooupload ho toohello konfigurace virtuálního serveru proxy.
    
    f. Zadejte název nebo schématu odkaz atribut hello atributu SAML hello představující hello **UserID** toohello Qlik smysl server odešle Azure AD.  Informace o schématu odkaz je k dispozici v konfiguraci post obrazovky aplikace Azure hello.  atribut name hello toouse, zadejte `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    g. Zadejte hodnotu hello hello **adresář uživatelského** , budou připojené toousers při ověření tooQlik smysl serveru prostřednictvím služby Azure AD.  Pevně zakódované hodnoty musí být uzavřena do **hranaté závorky []**.  toouse atribut odeslaných za hello kontrolního výrazu Azure AD SAML, zadejte do tohoto textového pole hello název atributu hello **bez** hranaté závorky.
    
    h. Hello **SAML podpisový algoritmus** nastaví hello podepsání pro konfiguraci proxy serveru virtuálním hello certifikátu zprostředkovatele (v tomto případu Qlik smysl serveru).  Pokud Qlik smysl serveru využívá důvěryhodné certifikát generovaný pomocí Microsoft Enhanced RSA a AES Cryptographic Provider, změňte hello SAML podpisový algoritmus příliš**SHA-256**.
    
    i. Hello části mapování atributů SAML umožňuje další atributy, jako jsou skupiny toobe odeslané tooQlik smysl pro použití v pravidla zabezpečení.

13. Klikněte na hello **Vyrovnávání zatížení** nabídky možnost toomake je viditelné.  Zobrazí se obrazovka Hello Vyrovnávání zatížení.
    
    ![QlikSense][qs11]

14. Klikněte na hello **přidat nový uzel serveru** tlačítko, vyberte modul uzlu nebo uzly Qlik smysl bude odeslat relací toofor zátěže účely a klikněte na hello **přidat** tlačítko.
    
    ![QlikSense][qs12]

15. Klikněte na hello pokročilé nabídky možnost toomake je viditelné. Zobrazí se obrazovka pokročilé Hello.
    
    ![QlikSense][qs13]
    
    seznamu povolených Hello hostitele identifikuje názvy hostitelů, které jsou přijaty při připojování toohello Qlik smysl serveru.  **Zadejte název hostitele hello, které budou uživatelé zadávat při připojování serveru tooQlik smysl.** název hostitele Hello je hello stejnou hodnotu jako text hello uri SAML hostitele bez hello https://.

16. Klikněte na tlačítko hello **použít** tlačítko.
    
    ![QlikSense][qs14]

17. Klikněte na tlačítko OK tooaccept hello upozornění, které stavy proxy propojené toohello virtuální proxy se restartuje.
    
    ![QlikSense][qs15]

18. Na pravé straně hello úvodní obrazovka zobrazí se hello přidružené položky nabídky.  Klikněte na hello **proxy** možnost nabídky.
    
    ![QlikSense][qs16]

19. Zobrazí se obrazovka proxy Hello.  Klikněte na tlačítko hello **odkaz** tlačítko na hello dolní toolink toohello virtuální proxy server.
    
    ![QlikSense][qs17]

20. Vyberte hello proxy uzlu, který bude podporovat toto připojení virtuální proxy serveru a klikněte na tlačítko hello **odkaz** tlačítko.  Po propojení, se objeví pod přidružené proxy hello proxy.
    
    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

21. Po přibližně pět sekund tooten, hello aktualizovat QMC zpráva se zobrazí.  Klikněte na tlačítko hello **aktualizovat QMC** tlačítko.
    
    ![QlikSense][qs20]

22. Když hello QMC aktualizuje, klikněte na hello **virtuální proxy** položku nabídky. Hello nové SAML virtuální proxy položku je uveden v tabulce hello na úvodní obrazovka.  Jedním kliknutím na položku virtuální proxy hello.
    
    ![QlikSense][qs51]

23. V hello dolní části obrazovky hello se budou aktivovat hello stáhnout SP metadata tlačítko.  Klikněte na tlačítko hello **stáhnout SP metadata** soubor tooa tlačítko toosave hello metadat.
    
    ![QlikSense][qs52]

24. Soubor metadat sp otevřete hello.  Sledovat hello **entityID** položku a hello **AssertionConsumerService** položku.  Tyto hodnoty jsou ekvivalentní toohello **identifikátor** a hello **přihlásit na adrese URL** v konfiguraci aplikace hello Azure AD. Vložte tyto hodnoty hello **Qlik smysl podnikové domény a adresy URL** část v konfiguraci aplikace hello Azure AD, pokud jejich nejsou párování, pak by měl nahradíte je v Průvodci konfigurací hello aplikace Azure AD.
    
    ![QlikSense][qs53]

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

   ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

   ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

   ![tlačítko Přidat Hello](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

   ![Dialogové okno uživatelského Hello](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png)

   a. V hello **název** zadejte **BrittaSimon**.

   b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

   c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

   d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-qlik-sense-enterprise-test-user"></a>Vytvoření zkušebního uživatele Qlik smysl Enterprise

V této části vytvoříte uživatele volal Britta Simon v Qlik smysl Enterprise. Práce s [tým podpory klient systému Enterprise smysl Qlik](https://www.qlik.com/us/services/support) pro přidání uživatelů hello hello Qlik smysl Enterprise platformy. Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování. 

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooQlik smysl Enterprise toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign tooQlik Britta Simon smysl Enterprise, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Qlik smysl Enterprise**.

    ![Hello Qlik smysl Enterprise odkaz v seznamu aplikace hello](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Qlik smysl Enterprise hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Qlik smysl podniková aplikace. 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

