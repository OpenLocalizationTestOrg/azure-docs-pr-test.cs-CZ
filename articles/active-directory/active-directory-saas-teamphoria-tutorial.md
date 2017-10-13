---
title: 'Kurz: Azure Active Directory integrace s Teamphoria | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Teamphoria."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: 2a35efb04d7fe22abc6894c149caf090666ce016
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a>Kurz: Azure Active Directory integrace s Teamphoria

V tomto kurzu zjistěte, jak integrovat Teamphoria s Azure Active Directory (Azure AD).

Integrace Teamphoria s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k Teamphoria
- Můžete povolit uživatelům, aby automaticky získat přihlášení k Teamphoria (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

To enable single sign-on with Teamphoria, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Teamphoria.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Teamphoria, potřebujete následující položky:

- Předplatné služby Azure AD
- Teamphoria jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Teamphoria z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-teamphoria-from-the-gallery"></a>Přidání Teamphoria z Galerie
Při konfiguraci integrace Teamphoria do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Teamphoria z galerie.

**Pokud chcete přidat Teamphoria z galerie, proveďte následující kroky:**

1. V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **Teamphoria**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_search.png)

5. Na panelu výsledků vyberte **Teamphoria**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Teamphoria podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Teamphoria je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Teamphoria musí navázat.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Teamphoria.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Teamphoria, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Teamphoria](#creating-a-teamphoria-test-user)**  – Pokud chcete mít protějšek Britta Simon v Teamphoria propojeném s Azure AD reprezentace jí.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci Teamphoria.

**Ke konfiguraci Azure AD jednotné přihlašování s Teamphoria, proveďte následující kroky:**

1. Na portálu Azure Management portal na **Teamphoria** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. Na **Teamphoria domény a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_url.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL, pomocí následujícího vzorce:`https://<sub-domain>.teamphoria.com/login`    

    > [!NOTE] 
    > Upozorňujeme, že tyto nejsou skutečné hodnoty. Budete muset aktualizovat tyto hodnoty s skutečná adresa URL přihlašování. Obraťte se na [tým podpory Teamphoria klienta](https://www.teamphoria.com/) získat adresu URL přihlašování. 

4. Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte certifikát v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_general_400.png)

6. Na **Teamphoria konfigurace** klikněte na tlačítko **konfigurace Teamphoria** otevřete **konfigurovat přihlášení** okno. Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_configure.png) 

7. Konfigurace jednotného přihlašování na **Teamphoria** straně, přihlášení do aplikace Teamphoria jako správce.

8. Přejděte na **nastavení správce** možnost v levém panelu nástrojů a v části kartě Konfigurace klikněte na **jeden přihlašování** a otevřete okno Konfigurace jednotného přihlašování.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/admin_sso_configure.png)

9. Klikněte na **přidat nového poskytovatele IDENTITY** možnost v pravém horním rohu otevřete formulář pro přidání nastavení pro jednotné přihlašování.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/add_new_identity_provider.png)

10. Zadejte podrobnosti v polích, jak je popsáno níže-

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/Teamphoria_sso_save.png)

    a. **Zobrazit název** : Zadejte zobrazovaný název modulu plug-in na stránky pro správu.

    b. **Název TLAČÍTKA** : název kartu, která se zobrazí na přihlašovací stránce pro přihlašování pomocí jednotného přihlašování.

    c. **CERTIFIKÁT** : certifikát předtím stáhli z portálu Azure v poznámkovém bloku otevřete zkopírujte obsah stejné a vložte jej zde v poli.

    d. **VSTUPNÍ bod** : vložení **SAML jeden přihlašování adresa URL služby** zkopírovat starší formulář portálu Azure.

    e. Přepněte možnost **ON** a klikněte na **Uložit**.   

<!--### Next steps

To ensure users can sign-in to Teamphoria after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Teamphoria prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Teamphoria in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Teamphoria users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_01.png) 

2. Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_02.png) 

3. V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-teamphoria-test-user"></a>Vytvoření zkušebního uživatele Teamphoria

Pokud chcete povolit uživatelům Azure AD přihlášení do Teamphoria, musí být zřízená do Teamphoria. V případě Teamphoria zřizování je ruční úloha.

**Ke zřízení uživatelských účtů, proveďte následující kroky:**

1. Přihlaste se k serveru vaší společnosti Teamphoria jako správce.

2. Klikněte na **správce** nastavení na levém panelu nástrojů a v části **SPRAVOVAT** kartě kliknutím na **uživatelé** chcete otevřít stránku Správce pro uživatele.

    ![Můžete přidat zaměstnance](./media/active-directory-saas-teamphoria-tutorial/admin_manage_users.png)

3. Klikněte na **RUČNÍ POZVAT** možnost.

    ![Pozvat uživatele](./media/active-directory-saas-teamphoria-tutorial/admin_manage_add_users.png)  

4. Na této stránce proveďte následující akce. 
    
    ![Pozvat uživatele](./media/active-directory-saas-teamphoria-tutorial/manual_user_invite.png)  

    a. V **e-MAILOVOU adresu** textovému poli, **e-mailová adresa** z BrittaSimon.

    b. V **KŘESTNÍ jméno** textovému poli, typ **Britta**.

    c. V **příjmení** textovému poli, typ **Simon**.

    d. Klikněte na tlačítko **pozvání 1 uživatele**. Uživatel musí přijmout pozvání k získání vytvořené v systému.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat tak, že udělíte přístup k Teamphoria Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon Teamphoria, proveďte následující kroky:**

1. V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Teamphoria**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu. Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_203.png

