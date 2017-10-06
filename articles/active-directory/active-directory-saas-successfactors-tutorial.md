---
title: 'Kurz: Azure Active Directory integrace s SuccessFactors | Microsoft Docs'
description: "Zjistěte, jak toouse SuccessFactors s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 3f7895d7d5e26fda27f555ae2f14a1645b50dcba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Kurz: Azure Active Directory integrace s SuccessFactors
cílem Hello tohoto kurzu je tooshow můžete jak toointegrate SuccessFactors s Azure Active Directory (Azure AD).

Integrace SuccessFactors s Azure AD poskytuje hello následující výhody:

* Můžete řídit ve službě Azure AD, který má přístup tooSuccessFactors
* Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSuccessFactors (jednotné přihlášení) s jejich účty Azure AD
* Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
Integrace služby Azure AD s SuccessFactors tooconfigure, je třeba hello následující položky:

* Platné předplatné Azure
* Klienta v SuccessFactors

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.
> 
> 

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

* Provozním prostředí byste neměli používat, pokud je to nutné.
* Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
cílem Hello tohoto kurzu je tooenable tootest Azure AD jednotné přihlašování v testovacím prostředí.

Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SuccessFactors z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-successfactors-from-hello-gallery"></a>Přidání SuccessFactors z Galerie hello
tooconfigure hello integrace SuccessFactors do Azure AD, je nutné tooadd SuccessFactors hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd SuccessFactors z Galerie hello, proveďte následující kroky hello:**

1. V hello portál Azure classic, na levém navigačním panelu hello, klikněte na **služby Active Directory**.
   
    ![Konfigurace jednotného přihlašování][1]
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Konfigurace jednotného přihlašování][2]
4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
    ![Aplikace][3]
5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
    ![Konfigurace jednotného přihlašování][4]
6. V hello **vyhledávacího pole**, typ **SuccessFactors**.
   
    ![Konfigurace jednotného přihlašování][5]
7. Na panelu výsledků hello vyberte **SuccessFactors**a potom klikněte na **Complete** tooadd hello aplikace.
   
    ![Konfigurace jednotného přihlašování][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
Hello cílem této části je tooshow vám jak tooconfigure a testování Azure AD jednotné přihlašování s SuccessFactors na základě testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel hello protějškem v SuccessFactors tooan uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SuccessFactors musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v SuccessFactors.

tooconfigure a testu Azure AD jednotné přihlašování s SuccessFactors, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele SuccessFactors](#creating-a-successfactors-test-user)**  -toohave protějšek Britta Simon v SuccessFactors, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování
V této části můžete povolit Azure AD jednotné přihlašování v portálu classic hello a nakonfigurovat jednotné přihlašování v aplikaci SuccessFactors.

**tooconfigure Azure AD jednotné přihlašování s SuccessFactors, proveďte následující kroky hello:**

1. V portálu Azure classic, na hello hello **SuccessFactors** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** Dialogové okno.
   
    ![Konfigurace jednotného přihlašování][7]
2. Na hello **jak jste by například uživatelé toosign na tooSuccessFactors** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
    ![Konfigurace jednotného přihlašování][8]
3. Na hello **konfigurace adresy URL aplikace** stránky, proveďte hello následující kroky a pak klikněte na tlačítko **Další**.
   
    ![Konfigurace jednotného přihlašování][9]
   
    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí jedné z následujících vzory hello: 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
   
    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí jedné z následujících vzory hello: 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
    | `https://<company name>.sapsf.eu/<company name>` |
   
    c. Klikněte na **Další**. 

    > [!NOTE]
    > Upozorňujeme, že tyto nejsou hello skutečné hodnoty. Máte tooupdate tyto hodnoty pomocí hello skutečné přihlašovací adresa URL a odpovědi adresy URL. Obraťte se na tooget tyto hodnoty [tým podpory SuccessFactors](https://www.successfactors.com/en_us/support.html).

1. Na hello **nakonfigurovat jednotné přihlašování v SuccessFactors** klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu hello místně na vašem počítači.
   
    ![Konfigurace jednotného přihlašování][10]

2. V okně prohlížeče jiný web, přihlaste se k vaší **portál pro správu SuccessFactors** jako správce.

3. Navštivte **zabezpečení aplikací** a nativní příliš**jeden znak na funkci**. 

4. Umístění všechny hodnoty v hello **resetovat tokenu** a klikněte na tlačítko **uložit tokenu** tooenable jednotné přihlašování SAML.
   
    ![Konfigurace jednotného přihlašování na straně aplikace][11]

    > [!NOTE] 
    > Tato hodnota se právě používá jako hello zapnout nebo vypnout přepínače. Pokud je uloženo žádnou hodnotu, je hello jednotné přihlašování SAML ON. Pokud je uloženo na prázdnou hodnotu hello jednotné přihlašování SAML je VYPNUTÝ.

1. Snímek obrazovky nativní toobelow a proveďte následující akce hello.
   
    ![Konfigurace jednotného přihlašování na straně aplikace][12]
   
    a. Vyberte hello **jednotné přihlašování SAML v2** přepínač
   
    b. Nastavte hello Name(e.g. SAml issuer + company name) strany potvrzující SAML.
   
    c. V hello **SAML vystavitele** textbox put hello hodnotu **URL vystavitele** z Průvodce konfigurací aplikace Azure AD.
   
    d. Vyberte **odpovědi (zákazníka generované nebo IdP/přístupový bod)** jako **vyžadují povinné podpis**.
   
    e. Vyberte **povoleno** jako **povolit SAML příznak**.
   
    f. Vyberte **ne** jako **podpisu požadavku přihlášení (SF generované/SP/RP)**.
   
    g. Vyberte **profil prohlížeče nebo Pozálohovacího** jako **SAML profil**.
   
    h. Vyberte **ne** jako **vynutit období platný certifikát**.
   
    i. Zkopírujte obsah hello hello stažený certifikát souboru a pak ji vložit do hello **ověření certifikátu SAML** textové pole.

    > [!NOTE] 
    > obsahu certifikátu Hello musí začínat certifikátu a koncovou značkou certifikátu.

1. Přejděte tooSAML V2 a potom proveďte hello následující kroky:
   
    ![Konfigurace jednotného přihlašování na straně aplikace][13]
   
    a. Vyberte **Ano** jako **podporu spouštěná SP globální odhlášení**.
   
    b. V hello **globální odhlášení adresa URL služby (LogoutRequest cíl)** textbox put hello hodnotu **vzdálené adresy URL odhlašovací** z Průvodce konfigurací aplikace Azure AD.
   
    c. Vyberte **ne** jako **vyžadují sp musí zašifrovat všechny element NameID**.
   
    d. Vyberte **neurčené** jako **NameID formátu**.
   
    e. Vyberte **Ano** jako **povolit sp iniciované přihlášení (AuthnRequest)**.
   
    f. V hello **odeslán požadavek na jako vydavatel společnosti** textbox put hello hodnotu **vzdálené adresy URL pro přihlášení** z Průvodce konfigurací aplikace Azure AD.
2. Proveďte tyto kroky, pokud chcete, aby uživatelská jména přihlášení hello toomake malá a velká písmena,.
   
    a. Navštivte **nastavení společnosti**(téměř hello dole).
   
    b. Zaškrtněte políčko téměř **povolit uživatelské jméno bez rozlišování**.
   
    c.Click **Uložit**.
   
    ![Konfigurovat jednotné přihlašování][29]

    > [!NOTE] 
    > Pokud tooenable zkusíte to, zkontroluje hello systému, pokud vytvoří duplicitní SAML přihlašovací jméno. Například pokud hello zákazník má uživatelských jmen uživatel1 a user1. Pořízení rychle rozlišování umožňuje tyto duplikáty. systém Hello získáte chybovou zprávu a neumožní hello funkce. Hello zákazníka bude nutné toochange mezi hello uživatelských jmen, je ve skutečnosti napsán jiný. 

1. Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Aplikace][14]
2. Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.
   
    ![Aplikace][15]

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele na portálu classic hello názvem Britta Simon.

![Vytvořit uživatele Azure AD][16]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Vytváření testovacího uživatele Azure AD][17]
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.
   
    ![Vytváření testovacího uživatele Azure AD][18]
4. tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.
   
    ![Vytváření testovacího uživatele Azure AD][19]
5. Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD][20]
   
    a. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.
   
    b. V hello uživatelské jméno **textbox**, typ **BrittaSimon**.
   
    c. Klikněte na **Další**.
6. Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD][21]
   
    a. V hello **křestní jméno** textovému poli, typ **Britta**.  
   
    b. V hello **příjmení** textovému poli, typ, **Simon**.
   
    c. V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.
   
    d. V hello **Role** seznamu, vyberte **uživatele**.
   
    e. Klikněte na **Další**.
7. Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.
   
    ![Vytváření testovacího uživatele Azure AD][22]
8. Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD][23]
   
    a. Poznamenejte si hodnotu hello hello **nové heslo**.
   
    b. Klikněte na **Dokončit**.  

### <a name="creating-a-successfactors-test-user"></a>Vytvoření zkušebního uživatele SuccessFactors
V pořadí tooenable Azure AD Uživatelé toolog do SuccessFactors musí být zřízená do SuccessFactors.  
V případě hello SuccessFactors zřizování je ruční úloha.

tooget uživatelé vytvoření ve SuccessFactors, je třeba toocontact hello [tým podpory SuccessFactors](https://www.successfactors.com/en_us/support.html).

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele
Hello cílem této části je tooenabling toouse Britta Simon Azure jednotné přihlašování, poskytněte tooSuccessFactors svůj přístup.

![Přiřadit uživatele][24]

**tooassign Britta Simon tooSuccessFactors, proveďte následující kroky hello:**

1. Na portálu classic hello, klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Přiřadit uživatele][25]
2. V seznamu aplikace hello vyberte **SuccessFactors**.
   
    ![Konfigurovat jednotné přihlašování][26]
3. V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.
   
    ![Přiřadit uživatele][27]
4. V seznamu uživatelé hello vyberte **Britta Simon**.
5. V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.
   
    ![Přiřadit uživatele][28]

### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování
Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.

Po kliknutí na tlačítko hello SuccessFactors dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour SuccessFactors aplikace.

## <a name="additional-resources"></a>Další zdroje
* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_00.png
[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_01.png
[6]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_02.png
[7]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_03.png
[8]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_04.png
[9]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_05.png
[10]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_06.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png

[16]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_00.png
[17]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png
[18]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png
[19]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png
[20]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png
[21]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_05.png
[22]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_06.png
[23]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_07.png

[24]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_07.png
[25]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_08.png
[26]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_11.png
[27]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_09.png
[28]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_10.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png
