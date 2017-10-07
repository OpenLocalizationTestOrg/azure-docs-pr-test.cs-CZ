---
title: "Kurz: Azure Active Directory integrace s SciQuest tráví ředitel | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SciQuest tráví ředitel."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 9fab641b-292e-4bef-91d1-8ccc4f3a0c1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 47c46f1297054fd96b86c1d8c66e1a55ec151497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a>Kurz: Azure Active Directory integrace s SciQuest tráví ředitel
cílem Hello tohoto kurzu je tooshow můžete jak toointegrate SciQuest tráví nacházející se službou Azure Active Directory (Azure AD).  
Integrace SciQuest tráví ředitel s Azure AD poskytuje hello následující výhody: 

* Můžete řídit ve službě Azure AD, který má přístup tooSciQuest výdaji ředitel 
* Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSciQuest výdaji ředitel (jednotné přihlášení) s jejich účty Azure AD
* Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
tooconfigure integrace Azure AD s SciQuest tráví ředitel, je třeba hello následující položky:

* Předplatné služby Azure AD
* SciQuest tráví ředitel jednotného přihlašování povolené předplatné

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

1. Přidání SciQuest tráví ředitel z Galerie hello 
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-sciquest-spend-director-from-hello-gallery"></a>Přidání SciQuest tráví ředitel z Galerie hello
tooconfigure hello integrace SciQuest tráví ředitel do Azure AD, je nutné tooadd SciQuest tráví ředitel hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd SciQuest tráví ředitel z Galerie hello, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**. 
   
    ![Active Directory][1]

2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.

3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Aplikace][2]

4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
    ![Aplikace][3]

5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
    ![Aplikace][4]

6. Hello vyhledávacího pole zadejte **sciQuest tráví ředitel**.
   
    ![Aplikace][5]

7. V podokně výsledků hello, vyberte **SciQuest tráví ředitel**a potom klikněte na **Complete** tooadd hello aplikace.
   
    ![Aplikace][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
Hello cílem této části je tooshow můžete jak tooconfigure a testování Azure AD jednotné přihlašování s SciQuest tráví ředitel podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel hello protějškem v SciQuest tráví ředitel tooan uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SciQuest tráví ředitel musí toobe navázat.  
Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v SciQuest tráví ředitel.

tooconfigure a testu Azure AD jednotné přihlašování s SciQuest tráví ředitel, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jeden jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele ředitel tráví SciQuest](#creating-a-halogen-software-test-user)**  -toohave protějšek Britta Simon v tráví ředitel SciQuest, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfigurace Azure AD jednoho jednotného přihlašování
Hello cílem této části je tooenable Azure AD jednotné přihlašování v hello portál Azure classic a tooconfigure jednotné přihlašování v aplikaci SciQuest tráví ředitel.

**tooconfigure Azure AD jednotné přihlašování s SciQuest tráví ředitel, proveďte hello následující kroky:**

1. V portálu Azure classic, na hello hello **SciQuest tráví ředitel** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování**dialogové okno.
   
    ![Konfigurovat jednotné přihlašování][8]

2. Na hello **jak jste by například uživatelé toosign na tooSciQuest výdaji ředitel** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.
   
    ![Azure AD jednotné přihlášení][9]

3. Na hello **nakonfigurovat nastavení aplikace** dialogové okno proveďte hello následující kroky: 
   
    ![Konfigurovat nastavení aplikace][10]
   
     a. V hello **přihlašovací adresa URL** textové pole, zadejte adresu URL používá vaši uživatelé toosign na tooyour SciQuest tráví ředitel aplikace pomocí hello následující vzor: *https://.* SciQuest.com/.**
   
     b. V hello **adresa URL odpovědi** textové pole, typ hello stejnou hodnotou, kterou jste zadali do hello **přihlašovací adresa URL** textové pole. 
   
     c. Klikněte na **Další**.

4. Na hello **nakonfigurovat jednotné přihlašování v ředitel tráví SciQuest** klikněte na tlačítko **stáhnout metadata**a potom uložte soubor metadat hello místně na vašem počítači.
   
    ![Co je služba Azure AD Connect][11]

5. Obraťte se na podporu tooenable SciQuest tuto metodu ověřování pomocí metadat hello výše stáhli.

6. Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno. 
   
    ![Co je služba Azure AD Connect][15]

7. Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.  

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v hello názvem Britta Simon portál Azure classic.

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Co je služba Azure AD Connect][100] 

2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.

3. Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.
   
    ![Co je služba Azure AD Connect][101] 

4. tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**. 
   
    ![Co je služba Azure AD Connect][102] 

5. Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:
   
    ![Co je služba Azure AD Connect][103] 
   
    a. Jako **typ uživatele**, vyberte **nového uživatele ve vaší organizaci**.
   
    b. V hello uživatelské jméno **textbox**, typ **BrittaSimon**.
   
    c. Klikněte na **Další**.

6. Na hello **profil uživatele** dialogové okno proveďte hello následující kroky: 
   
    ![Co je služba Azure AD Connect][104] 
   
    a. V hello **křestní jméno** textovému poli, typ **Britta**.  
   
    b. V hello **příjmení** txtbox, typ, **Simon**.
   
    c. V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.
   
    d. V hello **Role** seznamu, vyberte **uživatele**.
   
    e. Klikněte na **Další**.

7. Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.
   
    ![Co je služba Azure AD Connect][105]  

8. Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:
   
    ![Co je služba Azure AD Connect][106]   
   
    a. Poznamenejte si hodnotu hello hello **nové heslo**.
   
    b. Klikněte na **Dokončit**.   

### <a name="creating-a-sciquest-spend-director-test-user"></a>Vytvoření zkušebního uživatele SciQuest tráví ředitel
Hello cílem této části je toocreate volal Britta Simon v SciQuest tráví ředitel uživatele.

Potřebujete toocontact váš tým podpory SciQuest tráví ředitele a jim poskytnout hello podrobnosti o váš účet tooget test, vytvořen.

Alternativně můžete využít i za běhu zřizování, jeden přihlašování funkce, která podporuje SciQuest tráví ředitel.  
Pokud za běhu zřizování je povolená, uživatelé jsou automaticky vytváří SciQuest tráví ředitel během jednoho pokusu o přihlášení Pokud ještě neexistují. Tato funkce eliminuje nutnost hello toomanually vytváření protějšku přihlašování uživatelů.

zřizování tooget za běhu povolena, je nutné toocontact jste váš tým podpory SciQuest tráví ředitel.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele
Hello cílem této části je tooenabling toouse Britta Simon Azure jednotné přihlašování, poskytněte svůj přístup tooSciQuest výdaji ředitel.

![Co je služba Azure AD Connect][200]

**tooassign Britta Simon tooSciQuest výdaji ředitel, proveďte hello následující kroky:**

1. Na hello Azure klikněte na portálu classic, zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Co je služba Azure AD Connect][201]

2. V seznamu aplikace hello vyberte **SciQuest tráví ředitel**.
   
    ![Co je služba Azure AD Connect][202]

3. V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.
   
    ![Co je služba Azure AD Connect][203]

4. V seznamu uživatelé hello vyberte **Britta Simon**.
   
    ![Co je služba Azure AD Connect][204]

5. V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.
   
    ![Co je služba Azure AD Connect][205]

### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování
Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.  
Když kliknete na dlaždici SciQuest tráví ředitel hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour SciQuest tráví ředitel aplikace.

## <a name="additional-resources"></a>Další zdroje
* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_20.png

