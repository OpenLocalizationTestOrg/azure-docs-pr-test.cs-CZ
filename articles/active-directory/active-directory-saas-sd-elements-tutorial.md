---
title: 'Kurz: Azure Active Directory integrace s SD elementy | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SD elementy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 796f4d1b679c06be8677cd07f52ce305a7bc1ef8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a>Kurz: Azure Active Directory integrace s SD elementy

V tomto kurzu zjistěte, jak integrovat SD elementů s Azure Active Directory (Azure AD).

Integrace s Azure AD SD elementy poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k elementům SD
- Můžete povolit uživatelům, aby automaticky získat přihlášeného k elementům SD (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD SD elementy, potřebujete následující položky:

- Předplatné služby Azure AD
- Elementy SD jednotné přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidávání elementů SD z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-sd-elements-from-the-gallery"></a>Přidávání elementů SD z Galerie
Při konfiguraci integrace SD elementů do služby Azure AD, potřebujete přidat SD elementy z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat SD elementy z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **SD elementy**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_search.png)

5. Na panelu výsledků vyberte **SD elementy**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s SD elementy podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v elementech SD je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v elementech SD musí navázat.

V elementech SD přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SD elementy, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele SD elementy](#creating-a-sd-elements-test-user)**  – Pokud chcete mít protějšek Britta Simon v elementech SD, propojené služby Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci SD elementy.

**Ke konfiguraci Azure AD jednotné přihlašování s prvky SD, proveďte následující kroky:**

1. Na portálu Azure na **SD elementy** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_samlbase.png)

3. Na **SD elementy domény a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_url.png)

    a. V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenantname>.sdelements.com/sso/saml2/metadata`

    b. V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenantname>.sdelements.com/sso/saml2/acs/`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi. Obraťte se na [tým podpory SD elementy](mailto:support@sdelements.com) k získání těchto hodnot.

4. Elementy SD aplikace očekává SAML kontrolní výrazy ve specifickém formátu. Nakonfigurujte následující deklarace identity pro tuto aplikaci. Můžete spravovat hodnoty těchto atributů z **"Atribut uživatele"** aplikace. Následující snímek obrazovky ukazuje příklad pro tento.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_attribute.png)

5. V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku a proveďte následující kroky: 

    | Název atributu | Hodnota atributu |
    | --- | --- |
    | E-mailu |User.Mail |
    | FirstName |User.givenName |
    | Příjmení |User.Surname |

    a. Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_05.png)

    b. V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.

    c. Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.

    d. Klikněte na tlačítko **OK**.
 
6. Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_certificate.png) 

7. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_general_400.png)

8. Na **SD prvky konfigurace** klikněte na tlačítko **konfigurace elementy SD** otevřete **konfigurovat přihlášení** okno. Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_configure.png)

9. Pokud chcete získat jednotné přihlašování povolené, obraťte se na vaše [tým podpory SD elementy](mailto:support@sdelements.com) a poskytnout soubor stažený certifikát. 

10. V okně jiný prohlížeč přihlášení jako správce klienta SD elementy.

11. V nabídce v horní části, klikněte na tlačítko **systému**a potom **jednotné přihlašování**. 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_09.png) 

12. Na **nastavení jednotného přihlašování** dialogové okno, proveďte následující kroky:
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    a. Jako **typ jednotného přihlašování k**, vyberte **SAML**.
   
    b. V **ID Entity zprostředkovatele Identity** textovému poli, vložte hodnotu **SAML Entity ID**, který jste zkopírovali z portálu Azure. 
   
    c. V **Identity zprostředkovatele-služby přihlášení** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure. 
   
    d. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_02.png) 

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-sd-elements-test-user"></a>Vytvoření zkušebního uživatele SD elementy

Cílem této části je vytvoření uživatele volal Britta Simon v SD elementy. V případě elementů SD vytváření SD elementy uživatelů je ruční úloha.

**Pokud chcete vytvořit Britta Simon v SD elementy, proveďte následující kroky:**

1. V okně webového prohlížeče přihlašování k webu společnosti SD elementy jako správce.

2. V nabídce v horní části, klikněte na tlačítko **Správa uživatelů**a potom **uživatelé**.
   
    ![Vytvoření zkušebního uživatele SD elementy](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_11.png) 

3. Klikněte na tlačítko **přidat nového uživatele**.
   
    ![Vytvoření zkušebního uživatele SD elementy](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_12.png)
 
4. Na **přidat nové uživatele** dialogové okno, proveďte následující kroky:
   
    ![Vytvoření zkušebního uživatele SD elementy](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
    a. V **e-mailu** textovému poli, zadejte e-mailu uživatele jako  **brittasimon@contoso.com** .
   
    b. V **křestní jméno** textovému poli, zadejte jméno uživatele jako **Britta**.
   
    c. V **příjmení** textovému poli, zadejte příjmení uživatele jako **Simon**.
   
    d. Jako **Role**, vyberte **uživatele**. 
   
    e. Klikněte na tlačítko **vytvořit uživatele**.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu SD elementy.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon SD elementy, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **SD elementy**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.
  
Když kliknete na dlaždici SD elementy na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci SD elementy.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_203.png

