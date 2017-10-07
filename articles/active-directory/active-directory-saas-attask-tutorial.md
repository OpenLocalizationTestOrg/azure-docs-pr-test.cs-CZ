---
title: 'Kurz: Azure Active Directory integrace s @Task| Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a @Task."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 0840763622086a02a27cfafff3b741bc66cec498
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-task"></a>Kurz: Azure Active Directory integrace s@Task
cílem Hello tohoto kurzu je tooshow můžete jak toointegrate @Task s Azure Active Directory (Azure AD).  
Integrace @Task s Azure AD poskytuje hello následující výhody: 

* Můžete řídit ve službě Azure AD, který má přístuptoo@Task
* Můžete povolit uživatelům tooautomatically získat přihlášeného too@Task (jednotné přihlášení) s jejich účty Azure AD
* Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
integrace tooconfigure Azure AD s @Task, budete potřebovat hello následující položky:

* Předplatné služby Azure AD
* @Task Jednotného přihlašování povolené předplatné

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

1. Přidání @Task z Galerie hello 
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-task-from-hello-gallery"></a>Přidání @Task z Galerie hello
integrace hello tooconfigure @Task do služby Azure AD, je nutné tooadd @Task hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd @Task z Galerie hello provést hello následující kroky:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**. 
   
    ![Active Directory][1] 
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Aplikace][2] 
4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
    ![Aplikace][3] 
5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
    ![Aplikace][4] 
6. Hello vyhledávacího pole zadejte  **@Task** .
   
    ![Aplikace][5] 
7. V podokně výsledků hello, vyberte  **@Task** a potom klikněte na **Complete** tooadd hello aplikace.
   
    ![Aplikace][30] 

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
Hello cílem této části je tooshow můžete jak tooconfigure a testování Azure AD jednotného přihlašování pomocí @Task podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování, musí Azure AD tooknow hello příslušného uživatele v @Task tooan uživatel ve službě Azure AD. Jinými slovy, odkaz vztah mezi uživatele Azure AD a související uživatelské hello v @Task musí toobe navázat.   
Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v @Task.

tooconfigure a testování Azure AD jednotné přihlašování s @Task, budete potřebovat následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření @Tasktest uživatele](#creating-a-halogen-software-test-user)**  -toohave a protějšku Britta Simon v @Taskthat je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení
Hello cílem této části je tooenable Azure AD jednotné přihlašování v hello portál Azure classic a tooconfigure jednotné přihlašování v vaší @Task aplikace.

**tooconfigure Azure AD jednotné přihlašování s @Task, proveďte následující kroky hello:**

1. V portálu Azure classic, na hello hello  **@Task**  stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování**  Dialogové okno.
   
    ![Konfigurovat jednotné přihlašování][6] 
2. Na hello **jak můžete jako toosign uživatelé by na too@Task**  vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.
   
    ![Azure AD jednotné přihlášení][7] 
3. Na hello **nakonfigurovat nastavení aplikace** dialogové okno proveďte hello následující kroky:
   
    ![Konfigurovat nastavení aplikace][8] 
   
     a. V hello **přihlašovací adresa URL** textovému poli, adresa URL typu hello používá vaši uživatelé toosign na tooyour @Task aplikace (např:*https://<Tenant name>.attask ondemand.com*).
   
     b. Klikněte na **Další**.
4. Na hello **nakonfigurovat jednotné přihlašování v @Task**  klikněte na tlačítko **stáhnout metadata**, uložit soubor metadat hello místně na vašem počítači a pak klikněte na tlačítko **Další**.
   
    ![Co je služba Azure AD Connect][9] 
5. Přihlášení tooyour @Task společnosti lokality jako správce.
6. Přejděte příliš**jedna přihlašovací na konfigurační**.
7. Na hello **jednotné přihlašování** dialogové okno, proveďte následující kroky hello
   
    ![Konfigurovat jednotné přihlašování][23]
   
    a. Jako **typ**, vyberte **SAML 2.0**.
   
    b. Vyberte **služby ID zprostředkovatele**.
   
    c. Na portálu Azure classic hello, zkopírujte hello **vzdálené adresy URL pro přihlášení**a pak ji vložit do hello **adresu URL pro přihlášení portálu** textové pole.
   
    d. Na portálu Azure classic hello, zkopírujte hello **jednu adresu URL služby Sign-Out**a pak ji vložit do hello **Sign-Out URL** textové pole.
   
    e. Na portálu Azure classic hello, zkopírujte hello **heslo změnit adresu URL**a pak ji vložit do hello **heslo změnit adresu URL** textové pole.
   
    f. Klikněte na **Uložit**.
8. Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Další**. 
   
    ![Co je služba Azure AD Connect][10]
9. Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.  
   
    ![Co je služba Azure AD Connect][11]

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v hello názvem Britta Simon portál Azure classic.  

![Vytvořit uživatele Azure AD][20]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
4. tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**. 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 
5. Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky: 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 
   
    a. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.
   
    b. V hello uživatelské jméno **textbox**, typ **BrittaSimon**.
   
    c. Klikněte na **Další**.
6. Na hello **profil uživatele** dialogové okno proveďte hello následující kroky: 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
   
    a. V hello **křestní jméno** textovému poli, typ **Britta**.  
   
    b. V hello **příjmení** textovému poli, typ, **Simon**.
   
    c. V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.
   
    d. V hello **Role** seznamu, vyberte **uživatele**.

    e. Klikněte na **Další**.

7. Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
8. Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
   
    a. Poznamenejte si hodnotu hello hello **nové heslo**.
   
    b. Klikněte na **Dokončit**.   

### <a name="creating-an-task-test-user"></a>Vytvoření @Task testovacího uživatele
Hello cílem této části je toocreate uživatel volal Britta Simon v @Task.

**toocreate uživatel volal Britta Simon v @Task, proveďte následující kroky hello:**

1. Přihlaste se tooyour @Task společnosti lokality jako správce.
2. V nabídce hello hello nahoře, klikněte na tlačítko **osoby**.
3. Klikněte na tlačítko **nové osobě**. 
4. V dialogovém okně hello nové osobě proveďte následující kroky hello:
   
    ![Vytvoření @Task testovacího uživatele][21] 
   
    a. V hello **křestní jméno** textovému poli, zadejte "Britta".
   
    b. V hello **příjmení** textovému poli, zadejte "Simon".
   
    c. V hello **e-mailovou adresu** textovému poli, zadejte e-mailovou adresu Britta Simon v Azure Active Directory.
   
    d. Klikněte na tlačítko **přidat osobu**.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele
Hello cílem této části je tooenabling toouse Britta Simon Azure jednotné přihlašování tak, že udělíte přístup too@Task.

![Přiřadit uživatele][200] 

**tooassign Britta Simon too@Task, proveďte následující kroky hello:**

1. Na hello Azure klikněte na portálu classic, zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Přiřadit uživatele][201] 
2. V seznamu aplikace hello vyberte  **@Task** .
   
    ![Přiřadit uživatele][202] 
3. V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.
   
    ![Přiřadit uživatele][203] 
4. V seznamu uživatelé hello vyberte **Britta Simon**.
5. V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.
   
    ![Přiřadit uživatele][205]

### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování
Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.  
Když kliknete na tlačítko hello @Task dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour @Task aplikace.

## <a name="additional-resources"></a>Další zdroje
* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






