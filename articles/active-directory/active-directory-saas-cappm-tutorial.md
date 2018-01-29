---
title: "Kurz: Azure Active Directory integrace s správy Portfolia certifikační Autority | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a správy Portfolia certifikační Autority."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: ca9d5e71-e429-4891-8d10-3498e7210e89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: c00b054619b6dbfffd66bfd74b1a8567a83a910b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ca-ppm"></a>Kurz: Azure Active Directory integrace s správy Portfolia certifikační Autority

V tomto kurzu zjistěte, jak integrovat správy Portfolia certifikační Autority s Azure Active Directory (Azure AD).

Integrace správy Portfolia certifikační Autority s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k certifikační Autority stran za MINUTU
- Můžete povolit uživatelům, aby automaticky získat přihlášení k správy Portfolia certifikační Autority (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s správy Portfolia certifikační Autority, potřebujete následující položky:

- Předplatné služby Azure AD
- Certifikační Autoritu správy Portfolia jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání správy Portfolia certifikační Autority z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-ca-ppm-from-the-gallery"></a>Přidání správy Portfolia certifikační Autority z Galerie
Při konfiguraci integrace správy Portfolia certifikační Autority do služby Azure AD potřebujete přidat certifikační Autoritu správy Portfolia z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat certifikační Autoritu správy Portfolia z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **správy Portfolia certifikační Autority**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_search.png)

5. Na panelu výsledků vyberte **správy Portfolia certifikační Autority**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s správy Portfolia certifikační Autority podle testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v správy Portfolia certifikační Autority je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v správy Portfolia certifikační Autority je potřeba vytvořit.

V CA správy Portfolia, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s správy Portfolia certifikační Autority, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele správy Portfolia certifikační Autority](#creating-a-ca-ppm-test-user)**  – Pokud chcete mít protějšek Britta Simon v CA správy Portfolia, propojené služby Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci správy Portfolia certifikační Autority.

**Ke konfiguraci Azure AD jednotné přihlašování s správy Portfolia certifikační Autority, proveďte následující kroky:**

1. Na portálu Azure na **správy Portfolia certifikační Autority** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_samlbase.png)

3. Na **certifikační Autority správy Portfolia domény a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_url.png)

    a. V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://ca.ondemand.saml.20.post.<companyname>`
    
    b. V **adresa URL odpovědi** textovému poli, typ jako:`https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer`

    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte tato hodnota se skutečným identifikátorem. Obraťte se na [tým podpory správy Portfolia certifikační Autority](mailto:catechnicalsupport@ca.com) získat tuto hodnotu.
 
4. Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_general_400.png)

6. Na **konfiguraci certifikační Autority správy Portfolia** klikněte na tlačítko **konfigurace certifikační Autority správy Portfolia** otevřete **konfigurovat přihlášení** okno. Kopírování **SAML Entity ID** z **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_configure.png) 

7. Konfigurace jednotného přihlašování na **správy Portfolia certifikační Autority** straně, budete muset odeslat stažené **Certificate(Base64)** a **SAML Entity ID** k [tým podpory správy Portfolia certifikační Autority](mailto:catechnicalsupport@ca.com).

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_02.png) 

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-ca-ppm-test-user"></a>Vytvoření zkušebního uživatele správy Portfolia certifikační Autority

V této části vytvoříte uživatele volal Britta Simon v správy Portfolia certifikační Autority. Práce s [tým podpory správy Portfolia certifikační Autority](mailto:catechnicalsupport@ca.com) přidat uživatele do platformy správy Portfolia certifikační Autority.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu správy Portfolia certifikační Autority.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon správy Portfolia certifikační Autority, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **správy Portfolia certifikační Autority**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování k použití na přístupovém panelu.

Když kliknete na dlaždici správy Portfolia certifikační Autority na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci správy Portfolia certifikační Autority.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_203.png

