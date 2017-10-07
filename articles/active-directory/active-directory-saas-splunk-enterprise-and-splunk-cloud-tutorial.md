---
title: 'Kurz: Azure Active Directory integrace s Splunk Enterprise a Splunk cloudu | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Splunk Enterprise a Splunk cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/09/2017
ms.author: jeedes
ms.openlocfilehash: 9bb6817cb31dce684cd9cc1c567fa3efc8906ad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a>Kurz: Azure Active Directory integrace s Splunk Enterprise a Splunk cloudu

V tomto kurzu zjistíte, jak toointegrate Splunk Enterprise a Splunk cloudu s Azure Active Directory (Azure AD).

Integrace Splunk Enterprise a Splunk cloudu s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooSplunk Enterprise a Splunk cloudu
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSplunk Enterprise a Splunk cloudu jednotné přihlašování (SSO) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Splunk Enterprise a Splunk cloudu, je třeba hello následující položky:

- Předplatné služby Azure AD
- Splunk Enterprise nebo jednotného přihlašování k cloudu Splunk povolené předplatné


>[!NOTE]
>tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.
>

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.

Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Splunk Enterprise a Splunk cloudu z Galerie hello
2. Konfigurace a testování Azure AD SSO


## <a name="add-splunk-enterprise-and-splunk-cloud-from-hello-gallery"></a>Přidat Splunk Enterprise a Splunk cloudu z Galerie hello
tooconfigure hello integrace Splunk Enterprise a Splunk cloudu do Azure AD, budete muset tooadd Splunk Enterprise a Splunk cloudu hello Galerie tooyour seznam spravovaných aplikací SaaS.

**tooadd Splunk Enterprise a Splunk cloudu z Galerie hello, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.

    ![Active Directory][1]

2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.

3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.

    ![Aplikace][2]

4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.

    ![Aplikace][3]

5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.

    ![Aplikace][4]

6. Hello vyhledávacího pole zadejte **Splunk Enterprise nebo Splunk Cloud**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_01.png)

7. V podokně výsledků hello, vyberte **Splunk Enterprise a cloudu Splunk**a potom klikněte na **Complete** tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_02.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotného přihlašování k Splunk organizace a Splunk cloudu podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Splunk Enterprise a Splunk cloudu je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello Splunk Enterprise a Splunk cloudu musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** Splunk Enterprise a Splunk cloudu.

tooconfigure a testu Azure AD jednotné přihlašování s Splunk Enterprise a Splunk cloudu, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Splunk Enterprise a cloudu Splunk](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  -toohave protějšku Britta Simon ve Splunk podniku a Splunk cloudu, který je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování k portálu classic hello a nakonfigurovat jednotné přihlašování v aplikaci Splunk Enterprise a Splunk cloudu.


**tooconfigure Azure AD jednotné přihlašování s Splunk Enterprise a Splunk cloudu, proveďte následující kroky hello:**

1. Na portálu classic hello na hello **Splunk Enterprise a cloudu Splunk** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno.
     
    ![Konfigurovat jednotné přihlašování][6] 

2. Na hello **jakým způsobem uživatelé toosign na tooSplunk Enterprise a Splunk Cloud** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_03.png) 

3. Na hello **nakonfigurovat nastavení aplikace** dialogové okno proveďte hello následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_04.png) 
  1. V hello **přihlašovací adresa URL** textovému poli, adresa URL typu hello používá vaše uživatele toosign na tooyour Splunk Enterprise a Splunk cloudových aplikací pomocí následujících vzor hello:`https://<splunkserverUrl>/en-US/app/launcher/home`
  2. V hello **identifikátor** textovému poli, zadat adresu URL hello Splunk serveru.
  3. V hello **adresa URL odpovědi** textovému poli, zadat adresu URL hello s hello následující vzoru:`https://<splunkserver>/saml/acs`
  4. Klikněte na **Další**.
 
4. Na hello **nakonfigurovat jednotné přihlašování v Splunk Enterprise a Splunk cloudu** proveďte hello následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_05.png)
  1. Klikněte na tlačítko **stáhnout metadata**a potom uložte soubor hello ve vašem počítači.
  2. Klikněte na **Další**.

5. tooget jednotné přihlašování, které jsou nakonfigurované pro aplikace, obraťte se na Splunk Enterprise a tým podpory Splunk cloudu a poskytnout hello následující:

    * Hello Stáhnout **federaton metadat**
6. Hello portálu classic, vyberte hello potvrzení konfigurace přihlášení a pak klikněte na **Další**.
    
    ![Azure AD jednotné přihlášení][10]

7. Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.  
 
    ![Azure AD jednotné přihlášení][11]

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
V této části vytvoříte na portálu classic hello názvem Britta Simon testovacího uživatele.

![Vytvořit uživatele Azure AD][20]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_09.png) 

2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.

3. Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_03.png) 

4. tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_04.png) 

5. Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_05.png) 
  1. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.
  2. V hello uživatelské jméno **textbox**, typ **BrittaSimon**.
  3. Klikněte na **Další**.

6.  Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:
  
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_06.png) 
  1. V hello **křestní jméno** textovému poli, typ **Britta**.  
  2. V hello **příjmení** textovému poli, typ, **Simon**.
  3. V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.
  4. V hello **Role** seznamu, vyberte **uživatele**.
  5. Klikněte na **Další**.

7. Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_07.png) 

8. Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_08.png) 
  1. Poznamenejte si hodnotu hello hello **nové heslo**.
  2. Klikněte na **Dokončit**.   

### <a name="create-a-splunk-enterprise-and-splunk-cloud-test-user"></a>Vytvoření Splunk Enterprise a Splunk cloudu testovacího uživatele

V této části vytvoříte uživatele volat Britta Simon ve Splunk podniku a Splunk cloudu. Spojte se s Splunk Enterprise a Splunk cloudu podporu team tooadd hello uživatele v hello Splunk Enterprise a Splunk Cloudová platforma.


### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse SSOy Azure udělení svůj přístup tooSplunk Enterprise a Splunk cloudu.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooSplunk Enterprise a Splunk cloudu, proveďte hello následující kroky:**

1. Na portálu classic hello, klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Splunk Enterprise a Splunk cloudu**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_50.png) 

3. V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.

    ![Přiřadit uživatele][203]

4. V seznamu uživatelé hello vyberte **Britta Simon**.

5. V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.

    ![Přiřadit uživatele][205]

### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části je otestovat vaše Azure AD SSOonfiguration pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello Splunk Enterprise Splunk Cloud dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Splunk Enterprise a Splunk cloudové aplikace.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_205.png
