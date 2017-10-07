---
title: "Kurz: Azure Active Directory integrace s SilkRoad životnosti Suite | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SilkRoad životnosti Suite."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
ms.openlocfilehash: 07367282ab42b7332f166d64743b4b447aec4935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Kurz: Azure Active Directory integrace s SilkRoad životnosti Suite
cílem Hello tohoto kurzu je tooshow můžete jak toointegrate SilkRoad životnosti Suite a Azure Active Directory (Azure AD). 

Integrace SilkRoad životnosti Suite s Azure AD poskytuje hello následující výhody: 

* Můžete řídit ve službě Azure AD, který má přístup tooSilkRoad Suite životnosti 
* Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSilkRoad Suite života jednotného přihlašování (SSO) s jejich účty Azure AD

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
tooconfigure integrace Azure AD s SilkRoad životnosti Suite, je třeba hello následující položky:

* Předplatné služby Azure AD
* Předplatné SilkRoad životnosti Suite jednotné přihlašování povoleno

>[!NOTE]
>tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí. 
> 

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

* Provozním prostředí byste neměli používat, pokud je to nutné.
* Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Popis scénáře
cílem Hello tohoto kurzu je tooenable jste tootest Azure AD jednotné přihlašování v testovacím prostředí.

Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SilkRoad životnosti Suite z Galerie hello 
2. Konfigurace a testování Azure AD SSO

## <a name="add-silkroad-life-suite-from-hello-gallery"></a>Přidat Suite životnosti SilkRoad z Galerie hello
tooconfigure hello integraci sady životnosti SilkRoad do služby Azure AD, je nutné tooadd SilkRoad životnosti Suite hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Suite životnosti SilkRoad z Galerie hello, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**. 
   
    ![Active Directory][1]

2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.

3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Aplikace][2]

4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
    ![Aplikace][3]

5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
    ![Aplikace][4]

6. Hello vyhledávacího pole zadejte **SilkRoad životnosti Suite**.
   
    ![Aplikace][5]

7. V podokně výsledků hello, vyberte **Suite životnosti SilkRoad**a potom klikněte na **Complete** tooadd hello aplikace.
   
    ![Aplikace][50]

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
Hello cílem této části je tooshow můžete jak tooconfigure a testování Azure AD přihlášení SSO se SilkRoad životnosti Suite podle testovacího uživatele názvem "Britta Simon".

Pro jednotné přihlašování toowork Azure AD musí tooknow, co uživatel hello protějškem v SilkRoad životnosti Suite tooan uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v sadě životnosti SilkRoad musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v sadě SilkRoad životnosti.

tooconfigure a testu Azure AD jednotné přihlašování s SilkRoad životnosti Suite, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Suite životnosti SilkRoad](#creating-a-silkroad-life-suite-test-user)**  -toohave protějšek Britta Simon životní sady SilkRoad, který je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování
Hello cílem této části je tooenable Azure AD jednotné přihlašování v hello portál Azure classic a tooconfigure jednotné přihlašování v aplikaci SilkRoad životnosti Suite.

**tooconfigure Azure AD jednotné přihlašování s SilkRoad životnosti Suite, proveďte následující kroky hello:**

1. Web společnosti SilkRoad tooyour přihlášení jako správce. 

  >[!NOTE] 
  > tooobtain přístup toohello SilkRoad životnosti Suite ověřování aplikace pro konfiguraci federační služby Microsoft Azure AD, kontaktujte prosím podporu SilkRoad nebo vaším zástupcem SilkRoad služby.
  > 

2. Přejděte příliš**poskytovatele služeb**a potom klikněte na **Federation podrobnosti**. 
   
    ![Azure AD jednotné přihlášení][10] 

3. Klikněte na tlačítko **stáhnout federačních metadat**a potom uložte soubor metadat hello ve vašem počítači.
   
    ![Azure AD jednotné přihlášení][11] 

4. V portálu Azure classic, na hello hello **Suite životnosti SilkRoad** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování**  Dialogové okno.
   
    ![Konfigurovat jednotné přihlašování][6] 

5. Na hello **jak jste by například uživatelé toosign na tooSilkRoad životnosti Suite** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.
   
    ![Azure AD jednotné přihlášení][7] 

6. Na hello **nakonfigurovat nastavení aplikace** dialogové okno proveďte hello následující kroky:
   
    ![Azure AD jednotné přihlášení][8]   
 1. V hello **přihlašovací adresa URL** textovému poli, adresa URL typu hello používá váš web Suite životnosti SilkRoad tooyour toosign na uživatele (např: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).  
 2. Otevřete hello Stáhnout **Silkroad** soubor metadat. 
 3. Vyhledejte hello **AssertionConsumerService** značku a potom kopie hello **umístění** atribut.         
   
    ![Azure AD jednotné přihlášení][21] 
 4. Vložte hodnotu hello do hello **adresa URL odpovědi** textové pole.  
 5. Klikněte na **Další**.

6. Na hello **nakonfigurovat jednotné přihlašování v SilkRoad životnosti Suite** proveďte hello následující kroky:
   
    ![Azure AD jednotné přihlášení][9]  
 1. Klikněte na tlačítko Stáhnout certifikát a uložte soubor hello ve vašem počítači.  
 2. Klikněte na **Další**.

7. Ve vaší **SilkRoad** aplikace, klikněte na tlačítko **ověřování zdrojů**.
   
    ![Azure AD jednotné přihlášení][12] 

8. Klikněte na tlačítko **přidat zdroj ověřování**. 
   
    ![Azure AD jednotné přihlášení][13] 

9. V hello **přidat zdroj ověřování** část, proveďte následující kroky hello: 
   
    ![Azure AD jednotné přihlášení][14]  
 1. V části **možnost 2 – soubor metadat**, klikněte na tlačítko **Procházet** tooupload hello stáhnout soubor metadat.  
 2. Klikněte na tlačítko **vytvořit zprostředkovatelů Identity pomocí dat souboru**.

10. V hello **ověřování zdrojů** klikněte na tlačítko **upravit**. 
    
     ![Azure AD jednotné přihlášení][15] 

11. Na hello **upravit zdroj ověřování** dialogové okno, proveďte následující kroky hello: 
    
     ![Azure AD jednotné přihlášení][16] 
 1. Jako **povoleno**, vyberte **Ano**.   
 2. V hello **IdP popis** textovému poli, zadejte popis pro konfiguraci (například: *Azure AD jednotného přihlašování k*).  
 3. V hello **IdP název** textovému poli, zadejte název, který je konkrétní tooyour konfigurace (např: *Azure SP*).  
 4. Klikněte na **Uložit**.

12. Zakažte všechny ostatní zdroje ověřování. 
    
     ![Azure AD jednotné přihlášení][17]

13. V portálu Azure classic, na hello hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Další**.  
    
     ![Azure AD jednotné přihlášení][18]

14. Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.
    
     ![Azure AD jednotné přihlášení][19]

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v hello názvem Britta Simon portál Azure classic.

![Vytvořit uživatele Azure AD][20]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)  

2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.

3. Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) 

4. tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**. 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) 

5. Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky: 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)  
 1. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.  
 2. V hello uživatelské jméno **textbox**, typ **BrittaSimon**. 
 3. Klikněte na **Další**.

6. Na hello **profil uživatele** dialogové okno proveďte hello následující kroky: 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png)  
 1. V hello **křestní jméno** textovému poli, typ **Britta**.    
 2. V hello **příjmení** textovému poli, typ, **Simon**. 
 3. V hello **zobrazovaný název** textovému poli, typ **Britta Simon**. 
 4. V hello **Role** seznamu, vyberte **uživatele**.
 5. Klikněte na **Další**.

7. Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) 

8. Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png)  
 1. Poznamenejte si hodnotu hello hello **nové heslo**. 
 2. Klikněte na **Dokončit**.   

### <a name="create-a-silkroad-life-suite-test-user"></a>Vytvoření zkušebního uživatele SilkRoad životnosti Suite
Hello cílem této části je toocreate uživatele názvem Britta Simon v sadě SilkRoad životnosti. Na Britta musí mít ID jednotné přihlašování (někdy se také označuje tooas *AuthParam*) odpovídající na Britta **emailaddress** ve službě Azure AD.

**toocreate uživatele volat Britta Simon v sadě životnosti SilkRoad, proveďte následující kroky hello:**

- Požádejte uživatele, který má jako vaše Suite SilkRoad životnosti podpory team toocreate **jednotného přihlašování k ID** atribut hello stejnou hodnotu jako hello **emailaddress** z Britta Simon ve službě Azure AD.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele
Hello cílem této části je tooenable toouse Britta Simon jednotného přihlašování k Azure tak, že udělíte svůj přístup tooSilkRoad Suite životnosti.

![Přiřadit uživatele][200] 

**tooassign tooSilkRoad Britta Simon životnosti Suite, proveďte hello následující kroky:**

1. Na hello Azure klikněte na portálu classic, zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **SilkRoad životnosti Suite**.
   
    ![Přiřadit uživatele][202] 

3. V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.
   
    ![Přiřadit uživatele][203] 

4. V seznamu uživatelé hello vyberte **Britta Simon**.

5. V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.
   
    ![Přiřadit uživatele][205]

### <a name="test-single-sign-on"></a>Test jednotného přihlašování
Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.  

Když kliknete na dlaždici SilkRoad životnosti Suite hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour SilkRoad životnosti sada aplikací.

## <a name="additional-resources"></a>Další zdroje
* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png





