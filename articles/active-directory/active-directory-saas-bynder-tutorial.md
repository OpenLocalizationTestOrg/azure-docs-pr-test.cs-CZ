---
title: 'Kurz: Azure Active Directory integrace s Bynder | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Bynder."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 4fb0ab26-b3b9-420a-8072-a0be80ea021e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: a2a8477580d28fe422f2836f483dff286bc71c93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bynder"></a>Kurz: Azure Active Directory integrace s Bynder
cílem Hello tohoto kurzu je tooshow můžete jak toointegrate Bynder s Azure Active Directory (Azure AD).

Integrace Bynder s Azure AD poskytuje hello následující výhody:

* Můžete řídit ve službě Azure AD, který má přístup tooBynder
* Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooBynder jednotné přihlašování (SSO) s jejich účty Azure AD
* Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
Integrace služby Azure AD s Bynder tooconfigure, je třeba hello následující položky:

* Předplatné služby Azure AD
* Předplatné povolené Bynder jednotného přihlašování (SSO)

>[!NOTE]
>tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí. 
> 

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

* Provozním prostředí byste neměli používat, pokud je to nutné.
* Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
cílem Hello tohoto kurzu je tooenable jste tootest Microsoft Azure AD jednotné přihlašování v testovacím prostředí.

Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Bynder z Galerie hello
2. Konfigurace a testování jednotného přihlašování pro aplikaci Microsoft Azure AD

## <a name="add-bynder-from-hello-gallery"></a>Přidat Bynder z Galerie hello
tooconfigure hello integrace Bynder do Azure AD, je nutné tooadd Bynder hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Bynder z Galerie hello, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**. 
   
    ![Active Directory][1]
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Aplikace][2]
4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
    ![Aplikace][3]
5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
    ![Aplikace][4]
6. Hello vyhledávacího pole zadejte **Bynder**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_01.png)
7. Na panelu výsledků hello vyberte **Bynder**a potom klikněte na **Complete** tooadd hello aplikace.
   
    ![Výběr aplikace hello v galerii hello](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_001.png)

## <a name="configure-and-test-microsoft-azure-ad-sso"></a>Konfigurace a otestování jednotného přihlašování pro aplikaci Microsoft Azure AD
Hello cílem této části je tooshow vám jak tooconfigure a testovací Microsoft Azure AD přihlášení SSO se Bynder na základě testovacího uživatele názvem "Britta Simon".

Pro jednotné přihlašování toowork Azure AD musí tooknow, co uživatel hello protějškem v Bynder tooan uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Bynder musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Bynder.

tooconfigure a testovací Microsoft Azure AD přihlášení SSO se Bynder, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Microsoft Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Microsoft Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Bynder](#creating-a-bynder-test-user)**  -toohave protějšek Britta Simon v Bynder, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable toouse Britta Simon Microsoft Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-microsoft-azure-ad-sso"></a>Konfigurace jednotného přihlašování Microsoft Azure AD
V této části můžete povolit jednotné přihlašování pro aplikaci Microsoft Azure AD na portálu classic hello a nakonfigurovat jednotné přihlašování v aplikaci Bynder.

**tooconfigure Microsoft Azure AD přihlášení SSO se Bynder, proveďte následující kroky hello:**

1. Na portálu classic hello na hello **Bynder** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno.
   
    ![Konfigurovat jednotné přihlašování][6] 
2. Na hello **jak jste by například uživatelé toosign na tooBynder** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_03.png)
3. Na hello **nakonfigurovat nastavení aplikace** dialogové okno stránky, pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu**, proveďte následující kroky hello a klikněte na tlačítko **Další**:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_04.png)
  1. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.getbynder.com/sso/SAML/authenticate/`
  2. Klikněte na **Další**.
4. Pokud chcete aplikace hello tooconfigure v **SP iniciované režimu** na hello **nakonfigurovat nastavení aplikace** stránku dialogové okno a potom klikněte na hello **"Zobrazit rozšířená nastavení (volitelné)"**a pak zadejte hello **přihlašovací adresa URL** a klikněte na tlačítko **Další**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_10.png)
  1. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.getbynder.com/login/`
  2. Klikněte na **Další**.
  
   >[!NOTE]
   >Hodnota Hello hello přihlašovací adresa URL v tomto kurzu je právě placeholfer. tooget hello skutečné vlaue pro vaše prostředí, obraťte se na Bynder.
   >

5. Na hello **nakonfigurovat jednotné přihlašování v Bynder** , proveďte následující kroky hello a klikněte na tlačítko **Další**:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_05.png)  
  1. Klikněte na tlačítko **stáhnout metadata**a potom uložte soubor hello ve vašem počítači.
  2. Klikněte na **Další**.
6. tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, kontaktujte tým podpory Bynder. Připojte soubor stažený metadat hello a sdílet s Bynder team tooset až jednotného přihlašování na jejich straně.
7. Hello portálu classic, vyberte hello potvrzení konfigurace přihlášení a pak klikněte na **Další**.
   
    ![Azure AD jednotné přihlášení][10]
8. Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.  
   
    ![Azure AD jednotné přihlášení][11]

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele na portálu classic hello názvem Britta Simon.

![Vytvořit uživatele Azure AD][20]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_09.png)
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_03.png)
4. tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_04.png)
5. Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_05.png)
  1. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.
  2. V hello uživatelské jméno **textbox**, typ **BrittaSimon**.
  3. Klikněte na **Další**.
6. Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_06.png)
  1. V hello **křestní jméno** textovému poli, typ **Britta**.  
  2. V hello **příjmení** textovému poli, typ, **Simon**. 
  3. V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.
  4. V hello **Role** seznamu, vyberte **uživatele**.
  5. Klikněte na **Další**.
7. Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_07.png)
8. Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_08.png)
   1. Poznamenejte si hodnotu hello hello **nové heslo**.
   2. Klikněte na **Dokončit**.   

### <a name="create-a-bynder-test-user"></a>Vytvoření zkušebního uživatele Bynder
Hello cílem této části je toocreate volal Britta Simon v Bynder uživatele. Bynder podporuje za běhu zřizování, který je ve výchozím nastavení povolené.

Neexistuje žádná položka akce pro vás v této části. Pokud ještě neexistuje, se během pokusu o tooaccess Bynder vytvoří nového uživatele.

>[!NOTE]
>Pokud potřebujete toocreate uživatelé ručně, je nutné tým podpory Bynder toocontact hello. 
> 

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele
Hello cílem této části je tooenabling toouse Britta Simon jednotného přihlašování k Azure tak, že udělíte jeho tooBynder přístup.

   ![Přiřadit uživatele][200]

**tooassign Britta Simon tooBynder, proveďte následující kroky hello:**

1. Na portálu classic hello, klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Přiřadit uživatele][201]
2. V seznamu aplikace hello vyberte **Bynder**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_50.png)
3. V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.
   
    ![Přiřadit uživatele][203]
4. V seznamu uživatelé hello vyberte **Britta Simon**.
5. V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.
   
    ![Přiřadit uživatele][205]

### <a name="test-single-sign-on"></a>Test jednotného přihlašování
Hello cílem této části je tootest pomocí konfigurace Microsoft Azure AD SSO hello přístupového panelu.

Když kliknete na dlaždici Bynder hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Bynder aplikace.

## <a name="additional-resources"></a>Další zdroje
* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_205.png
