---
title: "Kurz: Azure Active Directory integrace s Wizergos produkční Software | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi softwarem Wizergos produktivitu a Azure Active Directory."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: acc04396-13c5-4c24-ab9a-30fbc9234ebd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: jeedes
ms.openlocfilehash: cdd186c38c426dde404ed8bb84700faf9c8c1a46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a>Kurz: Azure Active Directory integrace s Wizergos produktivitu softwaru
cílem Hello tohoto kurzu je tooshow můžete jak toointegrate Wizergos produktivitu Software s Azure Active Directory (Azure AD).

Integrace Wizergos produkční Software s Azure AD poskytuje hello následující výhody:

* Můžete řídit ve službě Azure AD, který má přístup tooWizergos produktivitu softwaru
* Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWizergos produktivitu softwaru jednotné přihlašování (SSO) s jejich účty Azure AD
* Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky
tooconfigure integrace Azure AD s Wizergos produktivitu softwaru, je třeba hello následující položky:

* Předplatné služby Azure AD
* Povolené předplatné produktivity Wizergos, jednotného přihlašování k softwaru

>[!NOTE]
>tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí. 
> 

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

* Provozním prostředí byste neměli používat, pokud je to nutné.
* Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
cílem Hello tohoto kurzu je tooenable jste tootest Azure AD jednotné přihlašování v testovacím prostředí.

Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání softwaru produktivitu Wizergos z Galerie hello
2. Konfigurace a testování Azure AD SSO

## <a name="adding-wizergos-productivity-software-from-hello-gallery"></a>Přidání softwaru produktivitu Wizergos z Galerie hello
tooconfigure hello integrace Wizergos produktivitu softwaru do služby Azure AD, je nutné tooadd Wizergos produktivitu softwaru hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Wizergos produktivitu softwaru z Galerie hello, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**. 
   
    ![Active Directory][1]
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Aplikace][2]
4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
    ![Aplikace][3]
5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
   
    ![Aplikace][4]
6. Hello vyhledávacího pole zadejte **Wizergos produktivitu softwaru**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)
7. Na panelu výsledků hello vyberte **Wizergos produktivitu softwaru**a potom klikněte na **Complete** tooadd hello aplikace.
   
    ![Výběr aplikace hello v galerii hello](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

## <a name="configure-and-test-azure-ad-sso"></a>Konfigurace a otestování Azure AD SSO
Hello cílem této části je tooshow vám jak tooconfigure a testování Azure AD přihlášení SSO se Wizergos produktivitu softwaru na základě testovacího uživatele názvem "Britta Simon".

Pro jednotné přihlašování toowork Azure AD musí tooknow, co uživatel hello protějškem v softwaru produktivitu Wizergos tooan uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v softwaru produktivitu Wizergos musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Wizergos produktivitu softwaru.

tooconfigure a testu Azure AD jednotné přihlašování s Softwareder BynWizergos produktivitu, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele softwaru produktivitu Wizergos](#creating-a-wizergos-productivity-software-test-user)**  -toohave protějšek Britta Simon v Wizergos produktivitu softwaru, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-sso"></a>Konfigurace Azure AD jednotného přihlašování
V této části můžete povolit Azure AD jednotné přihlašování v portálu classic hello a nakonfigurovat jednotné přihlašování v aplikaci Wizergos produktivitu softwaru.

**tooconfigure Azure AD jednotné přihlašování s Wizergos produktivitu softwarem, provést hello následující kroky:**

1. Na portálu classic hello na hello **Wizergos produktivitu softwaru** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování**dialogové okno.
   
    ![Konfigurovat jednotné přihlašování][6] 
2. Na hello **jak jste by například uživatelé toosign na tooWizergos produktivitu softwaru** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)
3. Na hello **nakonfigurovat nastavení aplikace** dialogové okno stránky, klikněte na tlačítko **Další**:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)
4. Na hello **nakonfigurovat jednotné přihlašování v softwaru produktivitu Wizergos** klikněte na tlačítko **stažení certifikátu**a potom uložte soubor hello ve vašem počítači:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)
5. V okně prohlížeče jiný web, Wizergos produkční Software klienta tooyour přihlášení jako správce.
6. Hello hamburger nabídce vyberte **správce**.
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)
7. Správce stránce v nabídce vlevo vyberte **ověřování** a klikněte na **Azure AD**.
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)
8. Proveďte následující kroky hello **ověřování** části.
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)
  1. Klikněte na tlačítko **NAHRÁT** hello tooupload tlačítko Stáhnout certifikát z Azure AD. 
  2. V hello **URL vystavitele** textbox put hello hodnotu **URL vystavitele** z Průvodce konfigurací aplikace Azure AD.
  3. V hello **adresy jednotného přihlašování** textbox put hello hodnotu **jeden přihlašování adresa URL služby** z Průvodce konfigurací aplikace Azure AD.
  4. V hello **jednu adresu URL Sign-Out** textbox put hello hodnotu **jednu adresu URL služby Sign-out** z Průvodce konfigurací aplikace Azure AD.
  5. Klikněte na tlačítko **Uložit** tlačítko.
9. Hello portálu classic, vyberte hello potvrzení konfigurace přihlášení a pak klikněte na **Další**.
   
    ![Azure AD jednotné přihlášení][10]
10. Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.  
    
    ![Azure AD jednotné přihlášení][11]

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele na portálu classic hello názvem Britta Simon.

![Vytvořit uživatele Azure AD][20]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)
2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
3. Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)
4. tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)
5. Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png) 
  1. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.
  2. V hello uživatelské jméno **textbox**, typ **BrittaSimon**.
  3. Klikněte na **Další**.
6. Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)
  1. V hello **křestní jméno** textovému poli, typ **Britta**.  
  2. V hello **příjmení** textovému poli, typ, **Simon**.
  3. V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.
  4. V hello **Role** seznamu, vyberte **uživatele**.
  5. Klikněte na **Další**.
7. Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)
8. Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)
  1. Poznamenejte si hodnotu hello hello **nové heslo**.
  2. Klikněte na **Dokončit**.   

### <a name="create-a-wizergos-productivity-software-test-user"></a>Vytvoření zkušebního uživatele Wizergos produktivitu softwaru
V této části vytvoříte volal Britta Simon v softwaru Wizergos produktivitu uživatele. Spojte se s tým podpory Wizergos produkční Software prostřednictvím [ support@wizergos.com ](emailTo:support@wizergos.com) tooadd hello uživatelé v platformě Wizergos produktivitu softwaru hello.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele
Hello cílem této části je tooenabling toouse Britta Simon jednotného přihlašování k Azure tak, že udělíte tooWizergos svůj přístup k produkční Software.

  ![Přiřadit uživatele][200]

**tooassign Britta Simon tooWizergos Software produktivitu, proveďte hello následující kroky:**

1. Na portálu classic hello, klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.
   
    ![Přiřadit uživatele][201]
2. V seznamu aplikace hello vyberte **Wizergos produktivitu softwaru**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)
3. V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.
   
    ![Přiřadit uživatele][203]
4. V seznamu uživatelé hello vyberte **Britta Simon**.
5. V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.
   
    ![Přiřadit uživatele][205]

### <a name="test-single-sign-on"></a>Test jednotného přihlašování
Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.

Když kliknete na dlaždici Wizergos produktivitu softwaru hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Wizergos produktivitu softwarová aplikace.

## <a name="additional-resources"></a>Další zdroje
* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
