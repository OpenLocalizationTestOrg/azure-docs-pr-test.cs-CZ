---
title: 'Kurz: Azure Active Directory integrace s Teamphoria | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Teamphoria."
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
ms.openlocfilehash: f32be9742b76f7fe464036dadc108c62e4a787a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a>Kurz: Azure Active Directory integrace s Teamphoria

V tomto kurzu zjistíte, jak toointegrate Teamphoria s Azure Active Directory (Azure AD).

Integrace Teamphoria s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooTeamphoria
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTeamphoria (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

tooenable single sign-on with Teamphoria, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Teamphoria.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Teamphoria tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Teamphoria jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Teamphoria z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-teamphoria-from-hello-gallery"></a>Přidání Teamphoria z Galerie hello
tooconfigure hello integrace Teamphoria do Azure AD, je nutné tooadd Teamphoria hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Teamphoria z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Teamphoria**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_search.png)

5. Na panelu výsledků hello vyberte **Teamphoria**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Teamphoria podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Teamphoria je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Teamphoria musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Teamphoria.

tooconfigure a testu Azure AD jednotné přihlašování s Teamphoria, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Teamphoria](#creating-a-teamphoria-test-user)**  -toohave protějšek Britta Simon v Teamphoria, která je jí reprezentace toohello propojené služby Azure AD.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci Teamphoria.

**tooconfigure Azure AD jednotné přihlašování s Teamphoria, proveďte následující kroky hello:**

1. V hello Azure Management portal na hello **Teamphoria** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. Na hello **Teamphoria domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, typ hello adresu URL pomocí hello následující vzoru:`https://<sub-domain>.teamphoria.com/login`  

    > [!NOTE] 
    > Upozorňujeme, že tyto nejsou hello skutečné hodnoty. Máte tooupdate tyto hodnoty s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory Teamphoria klienta](https://www.teamphoria.com/) tooget hello přihlašování URL. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte certifikát hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_general_400.png)

6. Na hello **Teamphoria konfigurace** klikněte na tlačítko **konfigurace Teamphoria** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_configure.png) 

7. tooconfigure jednotného přihlašování na **Teamphoria** straně, aplikace Teamphoria tooyour přihlášení jako správce.

8. Přejděte příliš**nastavení správce** v levém panelu nástrojů hello a v části hello hello karta Konfigurace klikněte na možnost **jeden přihlašování** okno Konfigurace jednotného přihlašování k tooopen hello.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/admin_sso_configure.png)

9. Klikněte na **přidat nového poskytovatele IDENTITY** možnost v hello pravém horním rohu tooopen hello formuláře pro přidání hello nastavení pro jednotné přihlašování.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/add_new_identity_provider.png)

10. Zadejte podrobnosti hello v polích text hello, jak je popsáno níže-

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/Teamphoria_sso_save.png)

    a. **Zobrazit název** : Zadejte zobrazovaný název modulu plug-in hello hello na stránky pro správu hello.

    b. **Název TLAČÍTKA** : název hello hello kartu, která se zobrazí na přihlašovací stránku hello pro přihlašování pomocí jednotného přihlašování.

    c. **CERTIFIKÁT** : Otevřete hello certifikát předtím stáhli z portálu Azure v poznámkovém bloku, kopírovat obsah hello hello hello stejné a vložte ji sem hello pole.

    d. **VSTUPNÍ bod** : vložení hello **SAML jeden přihlašování adresa URL služby** zkopírovat starší hello formulář portálu Azure.

    e. Přepněte možnost hello příliš**ON** a klikněte na **Uložit**. 

<!--### Next steps

tooensure users can sign-in tooTeamphoria after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Teamphoria prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooTeamphoria in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Teamphoria users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-teamphoria-test-user"></a>Vytvoření zkušebního uživatele Teamphoria

V pořadí tooenable Azure AD Uživatelé toolog do Teamphoria musí být zřízená do Teamphoria. V případě hello Teamphoria zřizování je ruční úloha.

**tooprovision uživatelské účty, provádět hello následující kroky:**

1. Přihlaste se tooyour Teamphoria společnosti lokality jako správce.

2. Klikněte na **správce** nastavení na levém panelu nástrojů hello a v části hello **SPRAVOVAT** kartě kliknutím na **uživatelé** stránky pro správu hello tooopen pro uživatele.

    ![Můžete přidat zaměstnance](./media/active-directory-saas-teamphoria-tutorial/admin_manage_users.png)

3. Klikněte na hello **RUČNÍ POZVAT** možnost.

    ![Pozvat uživatele](./media/active-directory-saas-teamphoria-tutorial/admin_manage_add_users.png)  

4. Na této stránce proveďte následující akce. 
    
    ![Pozvat uživatele](./media/active-directory-saas-teamphoria-tutorial/manual_user_invite.png)  

    a. V hello **e-MAILOVOU adresu** textovému poli, hello **e-mailová adresa** z BrittaSimon.

    b. V hello **KŘESTNÍ jméno** textovému poli, typ **Britta**.

    c. V hello **příjmení** textovému poli, typ **Simon**.

    d. Klikněte na tlačítko **pozvání 1 uživatele**. Uživatel potřebuje tooaccept hello pozvání tooget vytvořené v systému hello.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooTeamphoria svůj přístup.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooTeamphoria, proveďte následující kroky hello:**

1. Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Teamphoria**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu. Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
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

