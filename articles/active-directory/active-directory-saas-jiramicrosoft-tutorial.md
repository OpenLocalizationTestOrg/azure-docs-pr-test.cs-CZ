---
title: "Kurz: Integrace Azure Active Directory pomocí jednotného přihlašování SAML JIRA microsoftem | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a jednotné přihlašování SAML JIRA společností Microsoft."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0dc847b9-eec4-4c31-845e-0144ddedc4a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: b5f7813c8244d2964b6894ae49cd64e0ee71b704
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jira-saml-sso-by-microsoft"></a>Kurz: Integrace Azure Active Directory pomocí jednotného přihlašování SAML JIRA společností Microsoft

V tomto kurzu zjistěte, jak integrovat jednotné přihlašování SAML JIRA společností Microsoft s Azure Active Directory (Azure AD).

Integrace jednotné přihlašování SAML JIRA společností Microsoft s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k jednotné přihlašování SAML JIRA společností Microsoft
- Můžete povolit uživatelům, aby automaticky získat přihlášení k jednotné přihlašování SAML JIRA společností Microsoft (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD pomocí jednotného přihlašování SAML JIRA společností Microsoft, potřebujete následující položky:

- Předplatné služby Azure AD
- Aplikace serveru JIRA nainstalovaný na 64bitovou verzi Windows serveru (místní nebo v cloudu IaaS infrastrukturu)
- JIRA server je povolen protokol HTTPS
- Všimněte si, že podporované verze pro modul plug-in JIRA jsou uvedené v následující části.
- JIRA server je dostupný na Internetu, zvláště na Azure AD přihlašovací stránku pro ověřování a měli schopný přijímat tokenu z Azure AD
- Přihlašovací údaje správce se nastavují v JIRA
- V JIRA je zakázáno WebSudo
- Testovací uživatel vytvořené v aplikaci JIRA server

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí JIRA. Nejdřív otestovat integrace v vývoj nebo pracovní prostředí aplikace a pak pomocí produkčního prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-jira"></a>Podporované verze systému JIRA 

Od nyní jsou podporovány následující verze JIRA:

- Základní JIRA a Software: 6.0 na 7.2.0
- JIRA technickou podporu: 3.2 k 3.0

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání jednotné přihlašování SAML JIRA společností Microsoft z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-jira-saml-sso-by-microsoft-from-the-gallery"></a>Přidání jednotné přihlašování SAML JIRA společností Microsoft z Galerie
Při konfiguraci integrace přihlašování SAML JIRA společností Microsoft do služby Azure AD, potřebujete přidat jednotné přihlašování SAML JIRA společností Microsoft z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat jednotné přihlašování SAML JIRA společností Microsoft z galerie, postupujte takto:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **jednotné přihlašování SAML JIRA Microsoft**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_search.png)

5. Na panelu výsledků vyberte **jednotné přihlašování SAML JIRA Microsoft**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML JIRA společnosti Microsoft podle testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v jednotné přihlašování SAML JIRA společností Microsoft je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské JIRA SAML SSO společností Microsoft musí navázat.

V JIRA jednotné přihlašování SAML společností Microsoft, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML JIRA společností Microsoft, budete muset provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření jednotné přihlašování SAML JIRA uživatelem testovací Microsoft](#creating-a-jira-saml-sso-by-microsoft-test-user)**  – Pokud chcete mít protějšek Britta Simon v jednotné přihlašování SAML JIRA společnosti Microsoft, který je propojený s Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaší jednotné přihlašování SAML JIRA aplikací společnosti Microsoft.

**Ke konfiguraci Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML JIRA společností Microsoft, proveďte následující kroky:**

1. Na portálu Azure na **jednotné přihlašování SAML JIRA Microsoft** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_samlbase.png)

3. Na **jednotné přihlašování SAML JIRA Microsoft Domain a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_url.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<domain:port>/plugins/servlet/saml/auth`

    b. V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<domain:port>/`

    c. V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL. Port je volitelný, v případě, že je adresa URL s názvem. Tyto hodnoty jsou přijímány během konfigurace modulu plug-in Jira, který je vysvětlen později v tomto kurzu.
 
4. Ke generování **Metadata** adresu url, proveďte následující kroky:

    a. Klikněte na tlačítko **registrace aplikace**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/appregistrations.png)
   
    b. Klikněte na tlačítko **koncové body** otevřete **koncové body** dialogové okno.  
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/endpointicon.png)

    c. Klikněte na tlačítko Kopírovat kopírování **dokument FEDERAČNÍCH METADAT** adresy url a vložte do poznámkového bloku.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/endpoint.png)
     
    d. Nyní přejděte na stránku vlastností **jednotné přihlašování SAML JIRA Microsoft** a zkopírujte **Id aplikace** pomocí **kopie** tlačítko a vložte do poznámkového bloku.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/appid.png)

    e. Vygenerovat **adresu URL metadat** pomocí následujícího vzorce: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` a zkopírujte tuto hodnotu v poznámkovém bloku, jak se později používá pro konfiguraci modulu plug-in.

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_400.png)

6. Obraťte se na [Microsoft](mailto:waadpartners@microsoft.com) s následující informace pro modul plug-in JIRA.
    
    *   Jméno zákazníka:
    *   Název domény:
    *   Azure AD Premium: Ano/Ne (modul plug-in bude k dispozici pro zákazníka Free, Basic a skladová položka Premium)
    *   Počet uživatelů, kteří budou používat Tato integrace:
    *   JIRA verze:
    *   Poznámka:

7. V okně prohlížeče jiný web Přihlaste se k instanci JIRA jako správce.

8. Pozastavte ukazatel myši na ikonu a klikněte na **doplňky**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/addon1.png)

9. Karta části doplňky, klikněte na tlačítko **spravovat doplňky**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/addon7.png)

10. Ručně odešlete modulu plug-in od společnosti Microsoft. Po instalaci modulu plug-in se zobrazí v **uživatel nainstaloval** doplňky části **spravovat rozšíření** části.

11. Klikněte na tlačítko **konfigurace** konfigurace nového modulu plug-in.

12. Proveďte následující kroky na stránce konfigurace:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/addon5.png)
 
    a. V **adresu URL metadat** vložit **adresu URL metadat** generované z Azure AD a klikněte na tlačítko **vyřešit** tlačítko. Přečte adresu URL metadat IdP a naplní všechny informace o pole.

    > [!Note]
    > Výchozí umístění SAML ID uživatele je identifikátor jména. Můžete to změnit na možnost atribut a zadejte odpovídající název.

    > [!TIP]
    > Zkontrolujte, zda je namapovaný na aplikaci tak, aby se nezobrazí žádná chyba při řešení metadata jen jeden certifikát. Pokud máte víc certifikátů, při řešení metadata, získá správce k chybě.
    
    b. Kopírování **identifikátor, adresa URL odpovědi a adresa URL přihlašování** hodnoty a vložte je do **identifikátor, adresa URL odpovědi a adresa URL přihlašování** textových polí v uvedeném pořadí v **jednotné přihlašování SAML JIRA Microsoft Domain a adresy URL** části na portálu Azure.

    c. V **přihlašovací jméno tlačítko** zadejte název vaší organizace chce, aby se uživatelům zobrazit na přihlašovací obrazovku tlačítka.

    d. V **SAML uživatele ID umístění** vyberte buď **ID uživatele je v elementu NameIdentifier příkaz subjektu** nebo **ID uživatele je v elementu atribut**.  Toto ID musí být JIRA id uživatele. Pokud neodpovídá id uživatele, systém nedovolí přihlášení uživatelů. 
    
    e. Pokud vyberete **ID uživatele je v elementu atribut** možnost, pak v **název atributu** textového pole zadejte název atributu, kde je očekávána Id uživatele. 

    f. Pokud používáte federované domény (například služby AD FS atd.) s Azure AD, potom klikněte na **povolit zjišťování domovské sféry** řádku a nakonfigurovat **název domény**.
    
    g. V **název domény** zadejte název domény zde v případě přihlášení na základě služby AD FS.

    h. Zkontrolujte **povolit jednotné přihlašování se** Pokud se chcete odhlásit z Azure AD, když se uživatel neodhlásí z JIRA. 

    i. Klikněte na tlačítko **Uložit** tlačítko pro uložení nastavení.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_02.png) 

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-jira-saml-sso-by-microsoft-test-user"></a>Vytváření jednotné přihlašování SAML JIRA Microsoft testovací uživatel

Pokud chcete povolit uživatelům Azure AD přihlášení do JIRA na místním serveru, se musí být zřízená do jednotné přihlašování SAML JIRA společností Microsoft. Pro jednotné přihlašování SAML JIRA společností Microsoft zřizování je ruční úloha.

**K poskytnutí uživatelského účtu, proveďte následující kroky:**

1. Přihlaste se k serveru na místní JIRA jako správce.

2. Pozastavte ukazatel myši na ikonu a klikněte na **Správa uživatelů**.

    ![Můžete přidat zaměstnance](./media/active-directory-saas-jiramicrosoft-tutorial/user1.png) 

3. Budete přesměrováni na stránku přístup správce k zadání **heslo** a klikněte na tlačítko **potvrdit** tlačítko.

    ![Můžete přidat zaměstnance](./media/active-directory-saas-jiramicrosoft-tutorial/user2.png) 

4. V části **Správa uživatelů** oddíl, klikněte na **vytvořit uživatele**.

    ![Můžete přidat zaměstnance](./media/active-directory-saas-jiramicrosoft-tutorial/user3.png) 

5. Na **"Vytvořit nový uživatel"** dialogové okno proveďte následující kroky:

    ![Můžete přidat zaměstnance](./media/active-directory-saas-jiramicrosoft-tutorial/user4.png) 

    a. V **e-mailová adresa** jako typ e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.

    b. V **úplný název** textovému poli, úplný název typu uživatele jako Britta Simon.

    c. V **uživatelské jméno** jako typ e-mailu uživatele k textovému poli, Brittasimon@contoso.com.

    d. V **heslo** textovému poli, zadejte heslo uživatele.

    e. Klikněte na tlačítko **vytvořit uživateli**.   

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu jednotné přihlašování SAML JIRA společností Microsoft.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon jednotné přihlašování SAML JIRA společností Microsoft, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **jednotné přihlašování SAML JIRA Microsoft**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Po kliknutí na tlačítko jednotné přihlašování SAML JIRA podle Microsoft dlaždice na přístupovém panelu jste měli získat automaticky přihlášení k vaší jednotné přihlašování SAML JIRA aplikací Microsoft.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_203.png

