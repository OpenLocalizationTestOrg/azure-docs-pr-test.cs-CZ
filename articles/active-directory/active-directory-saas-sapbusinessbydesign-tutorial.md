---
title: 'Kurz: Azure Active Directory integrace s SAP Business ByDesign | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SAP Business ByDesign."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: c14714fd27f8d7fc555f25c7be83fad2b0d7f333
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Kurz: Integrace Azure Active Directory se službou SAP Business ByDesign

V tomto kurzu zjistíte, jak toointegrate SAP Business ByDesign službou Azure Active Directory (Azure AD).

Integrace SAP Business ByDesign s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooSAP ByDesign firmy.
- Vaši uživatelé tooautomatically get přihlášeného tooSAP obchodní ByDesign (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s SAP Business ByDesign tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- SAP Business ByDesign jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SAP Business ByDesign z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-sap-business-bydesign-from-hello-gallery"></a>Přidání SAP Business ByDesign z Galerie hello
tooconfigure hello integrace ByDesign obchodní SAP do Azure AD, je nutné tooadd SAP Business ByDesign hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd SAP Business ByDesign z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **SAP Business ByDesign**, vyberte **SAP Business ByDesign** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![SAP Business ByDesign v seznamu výsledků hello](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s SAP Business ByDesign podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v SAP Business ByDesign je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SAP Business ByDesign musí toobe navázat.

V SAP Business ByDesign, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s SAP Business ByDesign, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvořit testovací uživatele s SAP Business ByDesign](#create-an-sap-business-bydesign-test-user)**  -toohave protějšek Britta Simon v ByDesign SAP Business, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci SAP Business ByDesign.

**tooconfigure Azure AD jednotné přihlašování s SAP Business ByDesign, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **SAP Business ByDesign** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_samlbase.png)

3. Na hello **SAP Business ByDesign domény a adresy URL** část, proveďte následující kroky hello:

    ![SAP Business ByDesign domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<servername>.sapbydesign.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<servername>.sapbydesign.com`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory SAP Business ByDesign klienta](https://www.sap.com/products/cloud-analytics.support.html) tooget tyto hodnoty.

4. Na hello **uživatelské atributy** část, proveďte následující kroky hello:

    ![SAP Business ByDesign atribut části](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_attribute.png)
    
    a. V **uživatelský identifikátor** seznamu, vyberte hello **ExtractMailPrefix()** funkce.
    
    b. Z hello **e-mailu** seznamu, vyberte hello atribut uživatele chcete toouse týkající se vaší implementace. Například pokud chcete, aby toouse hello EmployeeID jako jedinečný identifikátor uživatele a hodnota atributu hello jsou uloženy v hello ExtensionAttribute2, pak vyberte user.extensionattribute2.   

5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_400.png)

7. Na hello **SAP Business ByDesign konfigurace** klikněte na tlačítko **konfigurace SAP Business ByDesign** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![SAP Business ByDesign konfigurace](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_configure.png) 

8. tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, proveďte následující kroky hello:
   
    a. Přihlaste na portál SAP Business ByDesign tooyour s právy správce.
   
    b. Přejděte příliš**aplikace a běžné úlohy správy uživatele** a klikněte na tlačítko hello **zprostředkovatele Identity** kartě.
   
    c. Klikněte na tlačítko **nového poskytovatele Identity** a vyberte hello metadata XML soubor, který jste si stáhli z portálu Azure hello. Importováním hello metadata hello systému automaticky nahrává hello dodejka certifikát a certifikát pro šifrování.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)
   
    d. tooinclude hello **adresa URL služby příjemce Assertion** do hello žádost SAML, vyberte **zahrnují Assertion příjemce adresa URL služby**.
   
    e. Klikněte na tlačítko **aktivovat jednotné přihlašování**.
   
    f. Uložte provedené změny.
   
    g. Klikněte na tlačítko hello **Moje systému** kartě.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)
   
    h. Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z hello portál Azure ho do hello **Azure AD adresa URL přihlašování** textové pole.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)
   
    i. Zadejte, zda zaměstnanec hello ručně vybrat mezi přihlásit pomocí ID uživatele a heslo nebo jednotného přihlašování k výběrem **výběr zprostředkovatele Identity ruční**.
   
    j. V hello **jednotného přihlašování k adrese URL** části, zadejte adresu URL hello, který má být používána hello zaměstnanec toologon toohello systému. 
    Hello odeslané na adresu URL tooEmployee rozevíracího seznamu můžete zvolit hello následující možnosti:
   
    **Adresa URL jednotného přihlašování**
   
    systém Hello odešle jenom hello normální systému URL toohello zaměstnanců. Zaměstnanec Hello nelze přihlášení pomocí jednotného přihlašování a musí používat heslo nebo místo toho certifikátu.
   
    **ADRESA URL JEDNOTNÉHO PŘIHLAŠOVÁNÍ** 
   
    systém Hello odešle jenom hello zaměstnanec toohello jednotného přihlašování k adrese URL. Hello zaměstnanců může přihlásit pomocí jednotného přihlašování. Prostřednictvím hello IdP přesměruje požadavek na ověření.
   
    **Automatický výběr**
   
    Pokud jednotného přihlašování není aktivní, odešle hello systému hello normální systému URL toohello zaměstnanců. Pokud je aktivní jednotné přihlašování, hello systému zkontroluje, zda hello zaměstnanec má heslo. Pokud heslo je k dispozici, jednotného přihlašování k adrese URL a adresy URL bez jednotného přihlašování k odešlou toohello zaměstnanců. Ale pokud zaměstnanec hello nemá žádné heslo, pouze hello URL jednotného přihlašování k odešle toohello zaměstnanců.
   
    kB. Uložte provedené změny.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-an-sap-business-bydesign-test-user"></a>Vytvořit uživatele s SAP Business ByDesign testu

V této části vytvoříte uživatele volal Britta Simon v SAP Business ByDesign. Spojte se s [tým podpory SAP Business ByDesign klienta](https://www.sap.com/products/cloud-analytics.support.html) tooadd hello uživatelé v platformě SAP Business ByDesign hello. 

> [!NOTE]
> Zkontrolujte, zda hodnota NameID by měl odpovídat hello pole pro uživatelské jméno v platformě SAP Business ByDesign hello.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooSAP obchodní ByDesign toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign tooSAP Britta Simon obchodní ByDesign, proveďte hello následující kroky:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **SAP Business ByDesign**.

    ![Hello SAP Business ByDesign odkaz v seznamu aplikace hello](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici SAP Business ByDesign hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour ByDesign SAP obchodní aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png

