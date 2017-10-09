---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti Soonr | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Soonr síti na pracovišti."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na
ms.assetid: b75f5f00-ea8b-4850-ae2e-134e5d678d97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: f950b45d0beceab2fa17b7690c9de81ec6603089
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a>Kurz: Azure Active Directory integrace s Soonr síti na pracovišti

cílem Hello tohoto kurzu je tooshow můžete jak toointegrate Soonr síti na pracovišti s Azure Active Directory (Azure AD).  
Integrace s Azure AD Soonr síti na pracovišti vám poskytne hello následující výhody:

- Můžete řídit ve službě Azure AD, který má tooSoonr přístup k síti na pracovišti
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSoonr síti na pracovišti (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Soonr síti na pracovišti, je třeba hello následující položky:

- Předplatné služby Azure AD
- Síti na pracovišti Soonr jednotného přihlašování povolené předplatné


> [!NOTE] 
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.


tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Popis scénáře
cílem Hello tohoto kurzu je tooenable tootest Azure AD jednotné přihlašování v testovacím prostředí.  
Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Soonr síti na pracovišti z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování


## <a name="adding-soonr-workplace-from-hello-gallery"></a>Přidání Soonr síti na pracovišti z Galerie hello
tooconfigure hello integrace síti na pracovišti Soonr do Azure AD, je nutné tooadd síti na pracovišti Soonr hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Soonr síti na pracovišti z Galerie hello, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**. 

    ![Active Directory][1]

2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.

3. Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.

    ![Aplikace][2]

4. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.

    ![Aplikace][3]

5. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.
 
    ![Aplikace][4]

6. Hello vyhledávacího pole zadejte **Soonr síti na pracovišti**.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_01.png)

7. V podokně výsledků hello, vyberte **síti na pracovišti Soonr**a potom klikněte na **Complete** tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
Hello cílem této části je tooshow vám jak tooconfigure a testování Azure AD jednotné přihlašování se Soonr pracovišti na základě testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel hello protějškem v síti na pracovišti Soonr tooan uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v síti na pracovišti Soonr musí toobe navázat.  

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v síti na pracovišti Soonr.

tooconfigure a testu Azure AD jednotné přihlašování s Soonr síti na pracovišti, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele síti na pracovišti Soonr](#creating-a-soonr-workplace-test-user)**  -toohave protějšek Britta Simon v síti na pracovišti Soonr, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v portálu classic hello a nakonfigurovat jednotné přihlašování v aplikaci Soonr síti na pracovišti.


**tooconfigure Azure AD jednotné přihlašování s Soonr síti na pracovišti, proveďte následující kroky hello:**

1. V portálu Azure classic, na hello hello **síti na pracovišti Soonr** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování**  Dialogové okno.

    ![Konfigurovat jednotné přihlašování][6] 

2. Na hello **jak jste by jako toosign uživatelů v síti na pracovišti tooSoonr** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_03.png) 

3. Na hello **nakonfigurovat nastavení aplikace** dialogové okno proveďte následující kroky hello:.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_04.png) 

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzor: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.

    b. Klikněte na **Další**.

    > [!NOTE] 
    > Upozorňujeme, že se nejedná hello skutečné hodnoty. Máte tooupdate tuto hodnotu s hello skutečné přihlásit na adrese URL. Obraťte se na tuto hodnotu tooget tým podpory Soonr síti na pracovišti.

4. Na hello **nakonfigurovat jednotné přihlašování v síti na pracovišti Soonr** klikněte na tlačítko **stáhnout metadata** a uložte soubor hello ve vašem počítači:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_05.png) 

5. tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, kontaktujte tým podpory Soonr síti na pracovišti a poskytnout hello následující: 

    • hello Stáhnout **Metadata** souboru

    • hello **URL vystavitele**

    • hello **URL jednotné přihlašování SAML**

    • hello **jednu adresu URL služby Sign-Out**

    >[!NOTE]
    >Tato aplikace je nahrazena <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">síti na pracovišti Autotask</a> a najdete <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">to</a> kurz pro konfiguraci aplikace hello s Azure AD.
   
6. Hello portál Azure classic, vyberte hello potvrzení konfigurace přihlášení a pak klikněte na **Další**.

    ![Azure AD jednotné přihlášení][10]

7. Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.  
  
    ![Azure AD jednotné přihlášení][11]



### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v hello názvem Britta Simon portál Azure classic.  

![Vytvořit uživatele Azure AD][20]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_09.png) 

2. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.

3. Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 

4. tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 

5. Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_05.png) 

    a. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. V hello uživatelské jméno **textbox**, typ **BrittaSimon**.

    c. Klikněte na **Další**.

6.  Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_06.png) 

    a. V hello **křestní jméno** textovému poli, typ **Britta**.  

    b. V hello **příjmení** textovému poli, typ, **Simon**.

    c. V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.

    d. V hello **Role** seznamu, vyberte **uživatele**.

    e. Klikněte na **Další**.

7. Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_07.png) 

8. Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_08.png) 

    a. Poznamenejte si hodnotu hello hello **nové heslo**.

    b. Klikněte na **Dokončit**.   



### <a name="creating-a-soonr-workplace-test-user"></a>Vytvoření zkušebního uživatele Soonr síti na pracovišti

Hello cílem této části je toocreate volal Britta Simon v síti na pracovišti Soonr uživatele. Spojte se s toocreate tým podpory síti na pracovišti Soonr uživatele v platformě hello. Můžete zvýšit lístku podpory hello s Soonr z <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">zde</a>.


### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

Hello cílem této části je tooenabling toouse Britta Simon Azure jednotné přihlašování, poskytněte tooSoonr svůj přístup k síti na pracovišti.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooSoonr síti na pracovišti, proveďte následující kroky hello:**

1. Na hello Azure klikněte na portálu classic, zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Soonr síti na pracovišti**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_50.png) 

1. V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.

    ![Přiřadit uživatele][203] 

1. V seznamu uživatelé hello vyberte **Britta Simon**.

2. V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.

    ![Přiřadit uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.  
Po kliknutí na tlačítko hello síti na pracovišti Soonr dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour aplikace Soonr síti na pracovišti.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_205.png
