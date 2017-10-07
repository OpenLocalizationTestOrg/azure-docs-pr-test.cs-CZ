---
title: 'Kurz: Azure Active Directory integrace s SAP HANA | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SAP HANA."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 5ff6bfde0b1d7ab14025a4bc45199f9f826affd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Kurz: Azure Active Directory integrace s SAP HANA

V tomto kurzu zjistíte, jak toointegrate SAP HANA službou Azure Active Directory (Azure AD).

Integrace SAP HANA s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooSAP HANA
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSAP HANA (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s SAP HANA tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- SAP HANA jednotné přihlašování povolené předplatné
- Spuštěné HANA Instance buď na všechny veřejné IaaS, místní, virtuální počítače Azure nebo velké instance SAP v Azure
- Hello XSA správy webové rozhraní, jakož i HANA Studio nainstalována na hello HANA instance.

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí SAP HANA. Integrace hello se nejdřív otestujte v vývoj nebo pracovní prostředí hello aplikace a pak použijte hello produkčního prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SAP HANA z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-sap-hana-from-hello-gallery"></a>Přidání SAP HANA z Galerie hello
tooconfigure hello integrace SAP HANA do Azure AD, je nutné tooadd SAP HANA hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd SAP HANA z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **SAP HANA**, vyberte **SAP HANA** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace. 

    ![Hello nové aplikace](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s SAP HANA podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v SAP HANA je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SAP HANA musí toobe navázat.

V SAP HANA přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s SAP HANA, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele SAP HANA](#creating-a-sap-hana-test-user)**  -toohave protějšek Britta Simon v SAP HANA, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci SAP HANA.

**tooconfigure Azure AD jednotné přihlašování s SAP HANA, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **SAP HANA** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_samlbase.png)

3. Na hello **SAP HANA domény a adresy URL** část, proveďte následující kroky hello:

    ![Domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_url.png)

    a. V hello **identifikátor** textovému poli, typ jako:`HA100` 

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL. Obraťte se na [tým podpory SAP HANA klienta](https://cloudplatform.sap.com/contact.html) tooget tyto hodnoty. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    >Pokud certifikát není aktivní pak nastavte ji jako aktivní kliknutím hello "Zkontrolujte nový certifikát active" políčko v hello Azure AD. 

5. Aplikace SAP HANA očekává hello SAML kontrolní výrazy ve specifickém formátu. Hello následující snímek obrazovky ukazuje příklad pro tento. Zde jsme jste namapovali hello **uživatelský identifikátor** s **ExtractMailPrefix()** funkce **user.mail**. Díky tomu hodnotu předpony hello e-mailů hello uživatele, který je hello jedinečné ID uživatele. Ta se odešle toohello SAP HANA aplikace v každé úspěšné odpovědi.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-saphana-tutorial/attribute.png)

6. V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno:

    a. V hello **uživatelský identifikátor** rozevíracího seznamu vyberte **ExtractMailPrefix**.
    
    b. V hello **e-mailu** rozevíracího seznamu vyberte **user.mail**.

7. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-saphana-tutorial/tutorial_general_400.png)
    
8. tooconfigure jednotného přihlašování na **SAP HANA** straně, přihlášení tooyour **HANA XSA webové konzole** procházením toohello příslušných koncový bod HTTPS.

    > [!Note]
    > V konfiguraci hello výchozí adresa URL hello přesměruje hello požadavek tooa přihlašovací obrazovka, která vyžaduje hello přihlašovací údaje ověření SAP HANA databáze toocomplete hello procesem přihlášení uživatele. úlohy správy hello oprávnění požadované tooperform SAML musí mít Hello přihlášeného uživatele.

9. Hello XSA webové rozhraní, přejděte v příliš**poskytovatele Identity SAML** a z ní, klikněte na tlačítko hello **"+"** – tlačítko hello dolní části hello obrazovky toodisplay hello přidat informace o poskytovateli Identity podokna a provádění Hello následující kroky:

    ![Přidejte zprostředkovatele Identity](./media/active-directory-saas-saphana-tutorial/sap1.png)

    a. V hello **přidat informace o poskytovateli Identity** podokně, vložte obsah hello hello Metadata XML, který jste si stáhli z portálu Azure do hello **Metadata** textové pole.

    ![Přidejte nastavení zprostředkovatele Identity](./media/active-directory-saas-saphana-tutorial/sap2.png)

    b. Pokud hello obsah dokumentu XML hello platné, hello Analýza procesu extrahuje tooinsert hello požadované informace do hello **předmět, Entity ID a vystavitele** polí v hello obecné datové oblasti obrazovky a hello pole adresy URL v hello Cílová oblast obrazovky, například  **základní adresu URL a adresy URL SingleSignOn (*)**.

    ![Přidejte nastavení zprostředkovatele Identity](./media/active-directory-saas-saphana-tutorial/sap3.png)

    c. V poli Název hello hello obecné Data obrazovky oblasti, zadejte název hello nového poskytovatele identity SAML jednotné přihlašování.

    > [!Note]
    > Název Hello hello SAML IDP je povinná a musí být jedinečné; Zobrazí se v seznamu hello k dispozici IDPs SAML, který se zobrazí, pokud vyberete SAML jako metodu ověřování hello SAP HANA XS toouse aplikace, například v oblasti obrazovky ověřování hello nástroje správy Křížky artefaktů hello.

10. Uložte hello podrobnosti o hello nového poskytovatele identity SAML. Zvolte **Uložit** toosave hello podrobnosti poskytovatele identity SAML hello a přidejte hello nový SAML IDP toohello seznam známých IDPs SAML.

    ![Tlačítko Uložit](./media/active-directory-saas-saphana-tutorial/sap4.png)

11. V sadě HANA Studio v rámci vlastnosti systému hello hello **konfigurace** kartě, právě filtrovat nastavení **saml** a upravte hello **assertion_timeout** z **10 sekund** příliš**120 sekundu**.

    ![nastavení assertion_timeout](./media/active-directory-saas-saphana-tutorial/sap7.png)

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-saphana-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-saphana-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-saphana-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-saphana-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-sap-hana-test-user"></a>Vytvoření zkušebního uživatele SAP HANA

Uživatelé toolog tooenable Azure AD v tooSAP HANA, se musí být zřízená do SAP HANA.
SAP HANA podporuje za běhu zřizování, který je ve výchozím nastavení povolené.

Pokud potřebujete toocreate uživatel ručně, proveďte následující kroky hello:

>[!Note]
>Externí ověřování hello hello uživatel používá, můžete změnit.
Externí uživatelé se ověřují pomocí externího systému, například systém protokolu Kerberos. Podrobné informace o externí identity, kontaktujte vašeho [správce domény](https://cloudplatform.sap.com/contact.html).

1. Otevřete hello [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) jako správce a povolení hello uživatele databáze pro jednotné přihlašování SAML.

    ![Vytvoření uživatele](./media/active-directory-saas-saphana-tutorial/sap5.png)

2. Levé značek hello neviditelná políčko toohello **SAML** a postupujte podle pokynů hello konfigurovat odkaz.

3. Klikněte na tlačítko **přidat** tooadd hello SAML IDP a klikněte na tlačítko **OK** výběr hello odpovídající IDP SAML.

4. Přidat hello **externí Identity** (např. Zde BrittaSimon), nebo zvolte **"Žádné"** a klikněte na tlačítko **OK**.

    >[!Note]
    >Pokud není zaškrtnuto políčko "Žádné" zaškrtávací políčko, pak hello uživatelské jméno v HANA musí tooexactly shodu hello jméno uživatele hello v hello UPN před příponu domény hello (tj. BrittaSimon@contoso.com by se stala BrittaSimon v HANA).

5. Pro účely testování, přiřaďte všechny **"XS"** toohello uživatelské role.

    ![přiřazení rolí](./media/active-directory-saas-saphana-tutorial/sap6.png)

    > [!TIP]
    > Tato oprávnění, které jsou vhodné pro vaše případy použití, jenom musíte získat.

6. Uložte hello uživatele.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooSAP HANA toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign Britta Simon tooSAP HANA, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **SAP HANA**.

    ![Přiřadit uživatele](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici SAP HANA hello v hello přístupového panelu, měli byste obdržet aplikace automaticky přihlášeného tooyour SAP HANA.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_203.png

