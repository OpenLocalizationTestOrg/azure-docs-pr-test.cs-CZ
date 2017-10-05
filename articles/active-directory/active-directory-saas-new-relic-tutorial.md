---
title: 'Kurz: Azure Active Directory integrace s New Relic. | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a New Relic."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: 605e85c23a849f70fcc0237361d7a891f716ca3a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Kurz: Azure Active Directory integrace s New Relic.

V tomto kurzu zjistěte, jak integrovat New Relic s Azure Active Directory (Azure AD).

Integrace New Relic s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k New Relic.
- Můžete povolit uživatelům, aby automaticky získat přihlášení k New Relic (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s New Relic, potřebujete následující položky:

- Předplatné služby Azure AD
- New Relic jednotné přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání New Relic z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-new-relic-from-the-gallery"></a>Přidání New Relic z Galerie
Při konfiguraci integrace New Relic do služby Azure AD potřebujete přidat New Relic z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat New Relic z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **New Relic**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_search.png)

5. Na panelu výsledků vyberte **New Relic**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s New Relic podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v New Relic je pro uživatele ve službě Azure AD. Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v New Relic.

V New Relic, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s New Relic, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele New Relic](#creating-a-new-relic-test-user)**  – Pokud chcete mít protějšek Britta Simon v New Relic, propojené služby Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci New Relic.

**Ke konfiguraci Azure AD jednotné přihlašování s New Relic, proveďte následující kroky:**

1. Na portálu Azure na **New Relic** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_samlbase.png)

3. Na **nové domény Relic a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_url.png)

    V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.newrelic.com`

    > [!NOTE] 
    > Hodnota není skutečné. Aktualizujte hodnotu s skutečná adresa URL přihlašování. Obraťte se na [tým podpory pro nového klienta Relic](https://support.newrelic.com/) k získání hodnoty. 
 
4. Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_general_400.png)

6. Na **novou konfiguraci Relic** klikněte na tlačítko **konfigurace New Relic** otevřete **konfigurovat přihlášení** okno. Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_configure.png) 

7. V okně prohlížeče jiný web, přihlaste se k vaší **New Relic** společnosti lokality jako správce.

8. V nabídce v horní části, klikněte na tlačítko **nastavení účtu**.
   
    ![Nastavení účtu](./media/active-directory-saas-new-relic-tutorial/ic797036.png "nastavení účtu")

9. Klikněte na tlačítko **zabezpečení a ověřování** a pak klikněte **jednotné přihlašování** kartě.
   
    ![Jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/ic797037.png "jednotného přihlašování")

10. Na stránce dialogové okno SAML proveďte následující kroky:
   
    ![SAML](./media/active-directory-saas-new-relic-tutorial/ic797038.png "SAML")
   
   a. Klikněte na tlačítko **zvolit soubor** odeslání vašeho certifikátu stažené Azure Active Directory.

   b. V **adresu URL pro vzdálené přihlášení** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.
   
   c. V **cílová stránka adresy URL odhlašovací** textovému poli, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.

   d. Klikněte na tlačítko **uložit změny provedené v**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_02.png) 

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-new-relic-test-user"></a>Vytvoření zkušebního uživatele New Relic.

Pokud chcete povolit uživatelů Azure Active Directory k přihlášení do New Relic, musí být zřízená do New Relic. V případě New Relic zřizování je ruční úloha.

**Pokud chcete zřídit uživatelský účet New Relic, proveďte následující kroky:**

1. Přihlaste se k vaší **New Relic** společnosti lokality jako správce.

2. V nabídce v horní části, klikněte na tlačítko **nastavení účtu**.
   
    ![Nastavení účtu](./media/active-directory-saas-new-relic-tutorial/ic797040.png "nastavení účtu")

3. V **účet** podokna na levé straně klikněte na tlačítko **Souhrn**a potom klikněte na **přidat uživatele**.
   
    ![Nastavení účtu](./media/active-directory-saas-new-relic-tutorial/ic797041.png "nastavení účtu")

4. Na **aktivní uživatelé** dialogové okno, proveďte následující kroky:
   
    ![Aktivní uživatelé](./media/active-directory-saas-new-relic-tutorial/ic797042.png "aktivní uživatelé")
   
    a. V **e-mailu** textovému poli, zadejte e-mailovou adresu chcete zřídit platného uživatele Azure Active Directory.

    b. Jako **Role** vyberte **uživatele**.

    c. Klikněte na tlačítko **přidejte tohoto uživatele**.

>[!NOTE]
>Můžete použít všechny ostatní New Relic uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované New Relic zřídit AAD uživatelské účty.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu New Relic.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon New Relic, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **New Relic**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.

Když kliknete na dlaždici New Relic na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci New Relic.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_203.png

