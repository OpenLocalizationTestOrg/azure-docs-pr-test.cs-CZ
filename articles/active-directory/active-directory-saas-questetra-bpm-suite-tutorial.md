---
title: 'Kurz: Azure Active Directory integrace se sadou Questetra BPM Suite | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Questetra BPM Suite."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: fb6d5b73-e491-4dd2-92d6-94e5aba21465
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2017
ms.author: jeedes
ms.openlocfilehash: 4907e3b5751cd79f994fbd2ebcb7faec4eac34e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Kurz: Azure Active Directory integrace se sadou Questetra BPM Suite
cílem Hello tohoto kurzu je tooshow můžete jak toointegrate Questetra BPM Suite a Azure Active Directory (Azure AD).  
Integrace sady BPM Questetra s Azure AD poskytuje hello následující výhody: 

* Můžete řídit ve službě Azure AD, který má přístup tooQuestetra BPM Suite 
* Vaši uživatelé tooautomatically get přihlášeného tooQuestetra Suite BPM (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD
* Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
tooconfigure integrace Azure AD s Questetra BPM Suite, je třeba hello následující položky:

* Předplatné služby Azure AD
* [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.
> 
> 

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

* Provozním prostředí byste neměli používat, pokud je to nutné.
* Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Popis scénáře
cílem Hello tohoto kurzu je tooenable tootest Azure AD jednotné přihlašování v testovacím prostředí.  
scénář Hello uvedených v tomto kurzu se skládá ze tří hlavních stavebních bloků:

1. Přidání Questetra BPM Suite z Galerie hello 
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-questetra-bpm-suite-from-hello-gallery"></a>Přidání Questetra BPM Suite z Galerie hello
tooconfigure hello integraci sady BPM Questetra do služby Azure AD, je nutné tooadd Questetra BPM Suite hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Suite BPM Questetra z Galerie hello, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**. 
   
    ![Active Directory][1]

2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.

3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Aplikace][2]

4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
    ![Aplikace][3]

5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
    ![Aplikace][4]

6. Hello vyhledávacího pole zadejte **Questetra BPM Suite**.
   
    ![Aplikace][5]

7. V podokně výsledků hello, vyberte **Questetra BPM Suite**a potom klikněte na **Complete** tooadd hello aplikace.

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
Hello cílem této části je tooshow vám jak tooconfigure a testování Azure AD jednotné přihlašování se sadou Questetra BPM Suite na základě testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel hello protějškem v Questetra BPM Suite tooan uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v sadě BPM Questetra musí toobe navázat.  
Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v sadě BPM Questetra.

tooconfigure a testu Azure AD jednotné přihlašování s Questetra BPM Suite, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Questetra BPM Suite](#creating-a-questetra-bpm-suite-test-user)**  -toohave protějšek Britta Simon v sadě BPM Questetra, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení
Hello cílem této části je tooenable Azure AD jednotné přihlašování v hello portál Azure classic a tooconfigure jednotné přihlašování v aplikaci Questetra BPM Suite.

**tooconfigure Azure AD jednotné přihlašování s Questetra BPM Suite, proveďte následující kroky hello:**

1. V portálu Azure classic, na hello hello **Questetra BPM Suite** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování**  Dialogové okno.
   
    ![Konfigurovat jednotné přihlašování][8]

2. Na hello **jak jste by například uživatelé toosign na tooQuestetra BPM Suite** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.
   
    ![Azure AD jednotné přihlášení][9]

3. V okně prohlížeče jiný web, přihlaste se k vaší **Questetra BPM Suite** společnosti lokality jako správce.

4. V nabídce hello hello nahoře, klikněte na tlačítko **nastavení systému**. 
   
    ![Azure AD jednotné přihlášení][10]

5. tooopen hello **SingleSignOnSAML** klikněte na tlačítko **jednotné přihlašování (SAML)**. 
   
    ![Azure AD jednotné přihlášení][11]

6. V portálu Azure classic, na hello hello **nakonfigurovat nastavení aplikace** dialogové okno proveďte hello následující kroky: 
   
    ![Konfigurovat nastavení aplikace][13]
   
    a. Na jste **Questetra BPM Suite** společnosti lokality v hello SP informace oddílu, kopie hello **adresa URL služby ACS**a pak ji vložit do hello **přihlašovací adresa URL** textové pole.
   
    b. Na jste **Questetra BPM Suite** společnosti lokality v hello SP informace oddílu, kopie hello **Entity ID**a pak ji vložit do hello **URL vystavitele** textové pole.
   
    c. Na jste **Questetra BPM Suite** společnosti lokality v hello SP informace oddílu, kopie hello **adresa URL služby ACS**a pak ji vložit do hello **adresa URL odpovědi** textové pole.
   
    d. Klikněte na **Další**.

7. Na hello **nakonfigurovat jednotné přihlašování v Questetra BPM Suite** klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu hello místně na vašem počítači.
   
    ![Konfigurovat jednotné přihlašování][14]

8. Na jste **Questetra BPM Suite** společnosti lokality, provedení hello následující kroky: 
   
    ![Konfigurovat jednotné přihlašování][15]
   
    a. Vyberte **povolit jednotné přihlašování**.
   
    b. Na portálu Azure classic hello, zkopírujte hello **URL vystavitele** hodnotu a pak ji vložit do hello **Entity ID** textové pole.
   
    c. Na portálu Azure classic hello, zkopírujte hello **jeden přihlašování adresa URL služby** hodnotu a pak ji vložit do hello **přihlašovací adresa URL stránky** textové pole.
   
    d. Na portálu Azure classic hello, zkopírujte hello **jednu adresu URL služby Sign-Out** hodnotu a pak ji vložit do hello **adresy URL odhlašovací stránky** textové pole.
   
    e. V hello **NameID formátu** textovému poli, typ **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: emailAddress**.

    f. Vytvořte soubor kódováním base-64 z stažený certifikát. 

    >[!TIP] 
    >Další podrobnosti najdete v tématu [jak tooconvert binární certifikátů do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

    g. Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit do hello **ověření certifikátu** textové pole. 

    h. Klikněte na **Uložit**.

1. Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Další**. 
   
    ![Co je služba Azure AD Connect][17]

2. Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.  
   
    ![Co je služba Azure AD Connect][18]


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v hello názvem Britta Simon portál Azure classic.

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Vytvoření zkušebního uživatele Azure AD][100] 

2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.

3. Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.
   
    ![Vytvoření zkušebního uživatele Azure AD][101] 

4. tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**. 
   
    ![Vytvoření zkušebního uživatele Azure AD][102] 

5. Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:
   
    ![Vytvoření zkušebního uživatele Azure AD][103]
   
    a. Jako **typ uživatele**, vyberte **nového uživatele ve vaší organizaci**.
   
    b. V hello uživatelské jméno **textbox**, typ **BrittaSimon**.
   
    c. Klikněte na Další.

6. Na hello **profil uživatele** dialogové okno proveďte hello následující kroky: 
   
    ![Vytvoření zkušebního uživatele Azure AD][104] 
   
    a. V hello **křestní jméno** textovému poli, typ **Britta**. 
   
    b. V hello **příjmení** textovému poli, typ, **Simon**.
   
    c. V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.
   
    d. V hello **Role** seznamu, vyberte **uživatele**.
   
    e. Klikněte na **Další**.

7. Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.
   
    ![Vytvoření zkušebního uživatele Azure AD][105]  

8. Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:
   
    ![Vytvoření zkušebního uživatele Azure AD][106]   
   
    a. Poznamenejte si hodnotu hello hello **nové heslo**.
   
    b. Klikněte na **Dokončit**.   

### <a name="creating-a-questetra-bpm-suite-test-user"></a>Vytvoření zkušebního uživatele Questetra BPM Suite
Hello cílem této části je toocreate názvem Britta Simon v sadě BPM Questetra uživatele.

**toocreate uživatele volat Britta Simon v sadě BPM Questetra, proveďte následující kroky hello:**

1. Web společnosti Questetra BPM Suite tooyour přihlášení jako správce.
2. Přejděte příliš**nastavení systému > seznam uživatelů > Nový uživatel**. 
3. V dialogovém okně Nový uživatel hello proveďte následující kroky hello: 
   
    ![Vytvoření zkušebního uživatele][300] 
   
    a. V hello **název** textovému poli, zadejte uživatelské jméno je Britta ve službě Azure AD.
   
    b. V hello **e-mailu** textovému poli, zadejte uživatelské jméno je Britta ve službě Azure AD.
   
    c. V hello **heslo** textovému poli, zadejte heslo.

4. Klikněte na tlačítko **přidat nové uživatele**.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele
Hello cílem této části je tooenabling toouse Britta Simon Azure jednotné přihlašování, poskytněte svůj přístup tooQuestetra BPM Suite.

![Co je služba Azure AD Connect][200]

**tooassign tooQuestetra Britta Simon BPM Suite, proveďte hello následující kroky:**

1. Na hello Azure klikněte na portálu classic, zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Co je služba Azure AD Connect][201]
2. V seznamu aplikace hello vyberte **Questetra BPM Suite**.
   
    ![Co je služba Azure AD Connect][205]
3. V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.
   
    ![Co je služba Azure AD Connect][202]
4. V seznamu uživatelé hello vyberte **Britta Simon**.
   
    ![Co je služba Azure AD Connect][203]
5. V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.
   
    ![Co je služba Azure AD Connect][204]

### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování
Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.  
Když kliknete na dlaždici Questetra BPM Suite hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Questetra BPM sada aplikací.

## <a name="additional-resources"></a>Další zdroje
* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_11.png 
