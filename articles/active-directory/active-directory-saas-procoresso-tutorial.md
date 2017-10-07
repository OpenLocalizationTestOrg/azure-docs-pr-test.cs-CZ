---
title: "Kurz: Azure Active Directory integration Procore jednotného přihlašování k | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Procore jednotné přihlašování."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: bb918617f18ba3f44ddde469e6fc411977752f15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a>Kurz: Azure Active Directory integrace s Procore jednotného přihlašování

V tomto kurzu zjistíte, jak toointegrate Procore jednotné přihlašování s Azure Active Directory (Azure AD).

Integrace Procore jednotné přihlašování s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooProcore jednotného přihlašování
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooProcore jednotné přihlašování (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

tooenable single sign-on with Procore SSO, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Procore SSO.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD pomocí Procore jednotného přihlašování, je třeba hello následující položky:

- Předplatné služby Azure AD
- Procore SSO jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Procore jednotného přihlašování z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-procore-sso-from-hello-gallery"></a>Přidání Procore jednotného přihlašování z Galerie hello
tooconfigure hello integrace Procore přihlašování do služby Azure AD, je nutné tooadd Procore SSO hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Procore jednotného přihlašování z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Procore SSO**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_search.png)

5. Na panelu výsledků hello vyberte **Procore jednotného přihlašování k**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Procore jednotného přihlašování na základě testovací uživatele, nazývá "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello Procore SSO je tooa uživatelem ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello Procore SSO musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** Procore sso.

tooconfigure a testování Azure AD jednotné přihlašování pomocí jednotného přihlašování služby Procore, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Procore SSO](#creating-a-procore-sso-test-user)**  -toohave protějšek Britta Simon v Procore jednotné přihlašování, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci Procore jednotné přihlašování.

**tooconfigure Azure AD jednotné přihlašování s Procore jednotné přihlašování, proveďte následující kroky hello:**

1. V hello Azure Management portal na hello **Procore SSO** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** jednotného přihlašování k tooenable.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_samlbase.png)

3. Na hello **Procore jednotného přihlašování k doméně a adresy URL** část, hello uživatel nemá tooperform žádné kroky jako aplikace hello je už předem integrováno s Azure.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_url.png)

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_general_400.png)

6. Na hello **Procore Konfigurace jednotného přihlašování k** klikněte na tlačítko **Konfigurace jednotného přihlašování k Procore** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_configure.png) 

7. tooconfigure jednotného přihlašování na **Procore SSO** straně, lokality procore společnosti tooyour přihlášení jako správce.

8. Sada nástrojů rozevíracím hello dolů, klikněte na **správce** tooopen hello jednotného přihlašování k nastavení stránky.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/procore_tool_admin.png)

9. Vložit hello hodnoty v polích hello jako popsané níže-

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/procore_setting_admin.png)    

    a. V hello **jedné URL přihlašování na vystavitele** pole, vložte hello SAML Entity ID zkopírovaných z hello portálu Azure.

    b. V hello **SAML přihlašovací na cílová adresa URL** pole, vložte hello SAML jeden přihlašování adresa URL služby zkopírovaných z hello portálu Azure.

    c. Nyní otevřete hello **soubor XML s metadaty** výše stažený z hello portál Azure a kopírovat hello certifikátu hello značky s názvem **certifikátu x 509**. Vložení hello zkopírují hello hodnotu **jednotné přihlašování x509 certifikát** pole.

10. Klikněte na **uložit změny**.

11. Po tato nastavení, musí toosend hello **název domény** (např **contoso.com**) prostřednictvím které přihlašujete Procore toohello [tým podpory Procore](https://support.procore.com/) a ty budou Aktivujte federované jednotné přihlašování pro tuto doménu.

<!--### Next steps

tooensure users can sign-in tooProcore SSO after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooProcore SSO in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-procore-sso-test-user"></a>Vytvoření zkušebního uživatele Procore jednotného přihlašování

Postupujte podle hello níže kroky toocreate Procore testovacího uživatele na jejich straně.

1. Lokalita procore společnosti tooyour přihlášení jako správce.  

2. Sada nástrojů rozevíracím hello dolů, klikněte na **Directory** stránky adresáře společnosti tooopen hello.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/Procore_sso_directory.png)

3. Klikněte na **přidat osobu** možnost tooopen hello formuláře a zadejte provést následující možnosti -

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/Procore_user_add.png)

    a. V hello **křestní jméno** textovému poli, křestní jméno uživatele typu jako **Britta**.

    b. V hello **příjmení** textovému poli, příjmení uživatele typu jako **Simon**.

    c. V hello **e-mailovou adresu** textovému poli, typu uživatele e-mailovou adresu jako  **BrittaSimon@contoso.com** .

    d. Vyberte **šablona oprávnění** jako **později použít šablonu oprávnění**.

    e. Klikněte na možnost **Vytvořit**.

4. Zkontrolujte a aktualizujte hello podrobnosti pro hello nově přidali kontakt.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/Procore_user_check.png)

5. Klikněte na **uložit a odeslat Invitiation** (Pokud pozvání prostřednictvím e-mailu je vyžadována) nebo **Uložit** (uložit přímo) toocomplete hello uživatele registrace.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/Procore_user_save.png)    

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte jeho přístup tooProcore jednotné přihlašování, poskytněte Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign tooProcore Britta Simon jednotné přihlašování, proveďte následující kroky hello:**

1. Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Procore SSO**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu. Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586). Když kliknete na dlaždici hello Procore jednotné přihlašování v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Procore jednotného přihlašování k aplikaci.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_203.png

