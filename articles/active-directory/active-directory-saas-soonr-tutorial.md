---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti Soonr | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Soonr síti na pracovišti."
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
ms.openlocfilehash: 76946e4af624d70f2202601ee935523ca3db4314
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a>Kurz: Azure Active Directory integrace s Soonr síti na pracovišti

Cílem tohoto kurzu je ukazují, jak k síti na pracovišti Soonr integraci s Azure Active Directory (Azure AD).  
Integrace Soonr síti na pracovišti s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k síti na pracovišti Soonr
- Můžete povolit uživatelům, aby automaticky získat přihlášení k síti na pracovišti Soonr (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Soonr síti na pracovišti, potřebujete následující položky:

- Předplatné služby Azure AD
- Síti na pracovišti Soonr jednotného přihlašování povolené předplatné


> [!NOTE] 
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Popis scénáře
Cílem tohoto kurzu je vám umožní testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Soonr síti na pracovišti z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování


## <a name="adding-soonr-workplace-from-the-gallery"></a>Přidání Soonr síti na pracovišti z Galerie
Při konfiguraci integrace síti na pracovišti Soonr do služby Azure AD potřebujete přidat Soonr síti na pracovišti z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat Soonr síti na pracovišti z galerie, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**. 

    ![Active Directory][1]

2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.

3. Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.

    ![Aplikace][2]

4. Klikněte na tlačítko **přidat** v dolní části stránky.

    ![Aplikace][3]

5. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.
 
    ![Aplikace][4]

6. Do vyhledávacího pole zadejte **Soonr síti na pracovišti**.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_01.png)

7. V podokně výsledků vyberte **síti na pracovišti Soonr**a potom klikněte na **Complete** tuto aplikaci přidat.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
Cílem této části je ukazují, jak nakonfigurovat a otestovat Azure AD jednotné přihlašování se Soonr pracovišti na základě testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co je příslušného uživatele v síti na pracovišti Soonr pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživateli v síti na pracovišti Soonr musí navázat.  

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v síti na pracovišti Soonr.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Soonr síti na pracovišti, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele síti na pracovišti Soonr](#creating-a-soonr-workplace-test-user)**  – Pokud chcete mít protějšek Britta Simon v síti na pracovišti Soonr propojeném s Azure AD reprezentace jí.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu classic a nakonfigurovat jednotné přihlašování v aplikaci Soonr síti na pracovišti.


**Ke konfiguraci Azure AD jednotné přihlašování s Soonr síti na pracovišti, proveďte následující kroky:**

1. Na portálu Azure classic na **síti na pracovišti Soonr** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.

    ![Konfigurovat jednotné přihlašování][6] 

2. Na **jak chcete uživatelům přihlásit se k síti na pracovišti Soonr** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_03.png) 

3. Na **nakonfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_04.png) 

    a. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.

    b. Klikněte na **Další**.

    > [!NOTE] 
    > Upozorňujeme, že se nejedná skutečné hodnoty. Budete muset aktualizovat tuto hodnotu s skutečné přihlašovací na adresy URL. Kontaktujte tým podpory Soonr síti na pracovišti, chcete-li získat tuto hodnotu.

4. Na **nakonfigurovat jednotné přihlašování v síti na pracovišti Soonr** klikněte na tlačítko **stáhnout metadata** a uložte soubor v počítači:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_05.png) 

5. Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, kontaktujte tým podpory Soonr síti na pracovišti a poskytněte s následujícími službami: 

    • Stažené **Metadata** souboru

    • **URL vystavitele**

    • **URL jednotné přihlašování SAML**

    • **Jedna adresa URL služby odhlášení**

    >[!NOTE]
    >Tato aplikace je nahrazena <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">síti na pracovišti Autotask</a> a najdete <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">to</a> kurzu konfigurace aplikace s Azure AD.
   
6. Portál Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na **Další**.

    ![Azure AD jednotné přihlášení][10]

7. Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.  
  
    ![Azure AD jednotné přihlášení][11]



### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure classic názvem Britta Simon.  

![Vytvořit uživatele Azure AD][20]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_09.png) 

2. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.

3. Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 

4. Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 

5. Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky:

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_05.png) 

    a. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. V uživatelské jméno **textbox**, typ **BrittaSimon**.

    c. Klikněte na **Další**.

6.  Na **profil uživatele** dialogové okno proveďte následující kroky:

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_06.png) 

    a. V **křestní jméno** textovému poli, typ **Britta**.  

    b. V **příjmení** textovému poli, typ, **Simon**.

    c. V **zobrazovaný název** textovému poli, typ **Britta Simon**.

    d. V **Role** seznamu, vyberte **uživatele**.

    e. Klikněte na **Další**.

7. Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_07.png) 

8. Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_08.png) 

    a. Poznamenejte si hodnotu **nové heslo**.

    b. Klikněte na **Dokončit**.   



### <a name="creating-a-soonr-workplace-test-user"></a>Vytvoření zkušebního uživatele Soonr síti na pracovišti

Cílem této části je vytvoření uživatele v síti na pracovišti Soonr nazývá Britta Simon. Spojte se s tým podpory Soonr síti na pracovišti a vytvořte uživatele v platformu. Můžete zvýšit lístku podpory s Soonr z <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">zde</a>.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

Cílem této části je povolení Britta Simon používat tak, že udělíte přístup k síti na pracovišti Soonr Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**Britta Simon přiřadit k síti na pracovišti Soonr, proveďte následující kroky:**

1. Na portálu Azure classic, otevřete zobrazení aplikací, v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Soonr síti na pracovišti**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_50.png) 

1. V nabídce v horní části, klikněte na tlačítko **uživatelé**.

    ![Přiřadit uživatele][203] 

1. V seznamu uživatelů vyberte **Britta Simon**.

2. Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.

    ![Přiřadit uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.  
Když kliknete na dlaždici Soonr síti na pracovišti na přístupovém panelu, můžete by měl získat automaticky přihlášení k síti na pracovišti Soonr aplikace.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
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
