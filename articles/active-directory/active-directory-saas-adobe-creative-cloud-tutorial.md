---
title: 'Kurz: Azure Active Directory integrace s Adobe Creative cloudu | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Adobe Creative cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ba1171e-56b1-4475-b308-58637d35e5a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 3d13608612c77236346b0e98551d7fc427d602e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a>Kurz: Azure Active Directory integrace s Adobe Creative cloudu

V tomto kurzu zjistěte, jak integrovat Adobe Creative cloudu s Azure Active Directory (Azure AD).

Integrace Adobe Creative cloudu s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup do cloudu tvůrčí Adobe
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Adobe Creative cloudu (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Adobe Creative cloudu, potřebujete následující položky:

- Předplatné služby Azure AD
- Tvůrčí cloudu Adobe jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Adobe Creative cloudu z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-adobe-creative-cloud-from-the-gallery"></a>Přidání Adobe Creative cloudu z Galerie
Při konfiguraci Integrace Adobe Creative cloudu do služby Azure AD, potřebujete přidat Adobe Creative cloudu z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat Adobe Creative cloudu z galerie, postupujte takto:**

1. V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **Adobe Creative cloudu**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_000.png)

5. Na panelu výsledků vyberte **Adobe Creative cloudu**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Adobe Creative cloudu podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v cloudu tvůrčí Adobe je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v cloudu tvůrčí Adobe musí navázat.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Adobe Creative cloudu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Adobe Creative cloudu, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele Adobe Creative cloudu](#creating-an-adobe-creative-cloud-test-user)**  – Pokud chcete mít protějšek Britta Simon Adobe Creative cloudu, který je propojený s Azure AD reprezentace jí.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci Adobe Creative cloudu.

**Ke konfiguraci Azure AD jednotné přihlašování s Adobe Creative cloudu, proveďte následující kroky:**

1. Na portálu Azure Management portal na **Adobe Creative cloudu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_01.png)

3. Na **Adobe Creative cloudové domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url1.png)

    a. V **identifikátor** textovému poli, zadejte hodnotu jako:`https://www.okta.com/saml2/service-provider/<token>`

    b. V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.okta.com/auth/saml20/accauthlinktest`

    > [!NOTE] 
    > Upozorňujeme, že tyto nejsou skutečné hodnoty. Budete muset aktualizovat tyto hodnoty se skutečným identifikátorem a adresa URL odpovědi. Zde, doporučujeme vám použít jedinečnou hodnotu řetězce v identifikátoru. Pokud potřebujete vytvořit uživatele s ručně, budete muset kontaktujte tým podpory Adobe Creative cloudu.

4. Na **Adobe Creative cloudové domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url2.png)

    a. Klikněte na **zobrazit upřesňující nastavení adresy URL** možnost

    b. V **přihlašovací adresa URL** textovému poli, zadejte hodnotu jako:`https://adobe.com`

5. Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_05.png) 

6. Na **Adobe Creative konfigurace cloudu** klikněte na tlačítko **konfigurace cloudu tvůrčí Adobe** otevřete **konfigurovat přihlášení** okno. Zkopírujte **SAML Entity Id** a **adresa URL služby Jednotné přihlašování SAML** z Stručná referenční příručka oddílu.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_06.png) 

7. V okně prohlížeče jiný web přihlášení jako správce klienta Adobe Creative cloudu.

8.  Přejděte na **Identity** na levém navigačním podokně a klikněte na doménu. Potom proveďte následující kroky na **jeden znak na konfigurace požadované** části.

    ![Nastavení](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "nastavení")

9. Klikněte na tlačítko **Procházet** na kterou odešlete certifikát stažený z Azure AD **IDP certifikát**.

10. V **IDP vystavitele** textovému poli, vložte hodnotu **SAML Entity Id** který jste zkopírovali ze **konfigurovat přihlášení** části na portálu Azure.

11. V **IDP přihlašovací adresa URL** textovému poli, vložte hodnotu **adresa URL služby Jednotné přihlašování SAML** který jste zkopírovali ze **konfigurovat přihlášení** části na portálu Azure.

12. Vyberte **HTTP - přesměrování** jako **IDP vazby**.

13. Vyberte **e-mailová adresa** jako **nastavení přihlášení uživatele**.
 
14. Klikněte na tlačítko **Uložit** tlačítko.

15. Řídicí panel bude nyní k dispozici soubor XML **"Stáhnout Metadata"** souboru. Obsahuje EntityDescriptor adresy URL a adresy URL AssertionConsumerService na Adobe. Otevřete soubor a nakonfigurujete je v aplikaci Azure AD.

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_002.png)

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. Použijte hodnotu EntityDescriptor Adobe zadaný pro **identifikátor** na **nakonfigurovat nastavení aplikace** dialogové okno.

    b. Použijte hodnotu AssertionConsumerService Adobe zadaný pro **adresa URL odpovědi** na **nakonfigurovat nastavení aplikace** dialogové okno.
 
### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_01.png) 

2. Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_02.png) 

3. V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**. 

### <a name="creating-an-adobe-creative-cloud-test-user"></a>Vytváření testovacího uživatele Adobe Creative cloudu

Pokud chcete povolit uživatelům Azure AD přihlášení do cloudu tvůrčí Adobe, musí být zřízená do Adobe Creative cloudu.  
V případě Adobe Creative cloudu zřizování je ruční úloha.

**Ke zřízení uživatelských účtů, proveďte následující kroky:**

1. Přihlaste se na váš web společnosti Adobe Creative Cloud jako správce.

2. Klikněte na tlačítko **osoby**.

    ![Lidé](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "osoby")

3. Klikněte na tlačítko **pozvat uživatele**.

    ![Uživatele pozvat](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "pozvat uživatele")

4. Na **pozvat osoby** dialogové okno proveďte následující kroky:

    ![Pozvěte lidi, kteří](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "pozvat uživatele")

    a. V **e-mailu** textovému poli, zadejte e-mailovou adresu účtu Britta Simon.
    
    b. Klikněte na tlačítko **pozvat**.

    > [!NOTE]
    > Držitel účtu Azure Active Directory bude dostávat e-mailu a postupujte podle odkaz potvrďte svůj účet, pak se změní na aktivní.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat tak, že udělíte přístup do cloudu tvůrčí Adobe Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**Přiřadit Britta Simon Adobe Creative cloudu, proveďte následující kroky:**

1. V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Adobe Creative cloudu**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_50.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici Adobe Creative cloudu na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Adobe Creative cloudu.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_203.png