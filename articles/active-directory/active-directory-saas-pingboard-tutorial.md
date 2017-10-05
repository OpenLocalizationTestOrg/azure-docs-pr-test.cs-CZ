---
title: 'Kurz: Azure Active Directory integrace s Pingboard | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Pingboard."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 008c670a8043da0c67ccefde48d5ef721c75d97c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a>Kurz: Azure Active Directory integrace s Pingboard

V tomto kurzu zjistěte, jak integrovat Pingboard s Azure Active Directory (Azure AD).

Integrace Pingboard s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k Pingboard
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Pingboard (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Pingboard, potřebujete následující položky:

- Předplatné služby Azure AD
- Pingboard jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Pingboard z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-pingboard-from-the-gallery"></a>Přidání Pingboard z Galerie
Při konfiguraci integrace Pingboard do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Pingboard z galerie.

**Pokud chcete přidat Pingboard z galerie, proveďte následující kroky:**

1. V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **Pingboard**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_search.png)

5. Na panelu výsledků vyberte **Pingboard**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Pingboard podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Pingboard je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Pingboard musí navázat.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Pingboard.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Pingboard, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Pingboard](#creating-a-pingboard-test-user)**  – Pokud chcete mít protějšek Britta Simon v Pingboard propojeném s Azure AD reprezentace jí.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci Pingboard.

**Ke konfiguraci Azure AD jednotné přihlašování s Pingboard, proveďte následující kroky:**

1. Na portálu Azure Management portal na **Pingboard** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_samlbase.png)

3. Na **Pingboard domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_url.png)

    a. V **identifikátor** textovému poli, zadejte hodnotu jako:`http://<entity-id>.pingboard.com/sp`

    b. V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<entity-id>.pingboard.com/auth/saml/consume`

    > [!NOTE] 
    > Upozorňujeme, že tyto nejsou skutečné hodnoty. Budete muset aktualizovat tyto hodnoty se skutečným identifikátorem a adresa URL odpovědi. Zde, doporučujeme vám použít jedinečnou hodnotu řetězce v identifikátoru. Obraťte se na [tým podpory Pingboard klienta](https://support.pingboard.com/) k získání těchto hodnot. 

4. Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte hodnotu jako:`http://<sub-domain>.pingboard.com/sign_in`
     
5. Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_general_400.png)

7. Na straně Pingboard nakonfigurovat jednotné přihlašování, otevřete nové okno prohlížeče a přihlaste se k účtu Pingboard. Musí být správcem Pingboard nastavit jednotné přihlašování.

8. V hlavní nabídce vyberte **aplikace > integrace**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_integration.png)

9.  Na **integrace** stránky, vyhledejte **"Azure Active Directory"** dlaždici a klikněte na něj.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_aad.png)

10. V modální, který následuje dále klikněte na tlačítko **"Konfigurace"**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_configure.png)

11. Na následující stránce si všimnete, že "Integrace se službou Azure jednotného přihlašování k dispozici.". Otevřete soubor stažený soubor XML s metadaty v programu Poznámkový blok a vložte obsah **IDP Metadata**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_sso_configure.png)

12. Soubor ověří a pokud se vše, co je správný, jednotného přihlašování na bude nyní povoleno

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_01.png) 

2. Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_02.png) 

3. V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-pingboard-test-user"></a>Vytvoření zkušebního uživatele Pingboard

Pokud chcete povolit uživatelům Azure AD přihlášení do Pingboard, musí být zřízená do Pingboard.  
V případě Pingboard zřizování je ruční úloha.

**Ke zřízení uživatelských účtů, proveďte následující kroky:**

1. Přihlaste se k serveru vaší společnosti Pingboard jako správce.

2. Klikněte na tlačítko **"Přidat zaměstnancem"** tlačítko **Directory** stránky.

    ![Můžete přidat zaměstnance](./media/active-directory-saas-pingboard-tutorial/create_testuser_add.png)

3. Na **"Přidat zaměstnancem"** dialogové okno proveďte následující kroky.

    ![Pozvat uživatele](./media/active-directory-saas-pingboard-tutorial/create_testuser_name.png)

    a. V **úplný název** textovému poli, zadejte úplný název Britta Simon.

    b. V **e-mailu** textovému poli, zadejte e-mailovou adresu účtu Britta Simon.

    c. V **funkce** textovému poli, zadejte název úlohy Britta Simon.

    d. V **umístění** rozevíracího seznamu, vyberte umístění Britta Simon.
    
    e. Klikněte na tlačítko **Přidat**.   

4. Obrazovka s potvrzením bude spuštěna potvrďte přidání uživatele.
    
    ![Potvrďte](./media/active-directory-saas-pingboard-tutorial/create_testuser_confirm.png)
        
    > [!NOTE]
    > Držitel účtu Azure Active Directory bude dostávat e-mailu a postupujte podle odkaz potvrďte svůj účet, pak se změní na aktivní.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat tak, že udělíte přístup k Pingboard Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon Pingboard, proveďte následující kroky:**

1. V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Pingboard**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici Pingboard na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Pingboard.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_203.png
