---
title: 'Kurz: Azure Active Directory integrace s Google Apps v Azure | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Google Apps."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 2093b5ab605ec0d7bbefe7a78e1eede34d756f53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-google-apps"></a>Kurz: Azure Active Directory integrace s Google Apps

V tomto kurzu zjistíte, jak toointegrate Google Apps v Azure Active Directory (Azure AD).

Integrace s Azure AD Google Apps poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooGoogle aplikace
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooGoogle aplikace (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Google Apps, je třeba hello následující položky:

- Předplatné služby Azure AD
- Google Apps jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

## <a name="video-tutorial"></a>Videokurz
Jak tooEnable jednotné přihlašování tooGoogle aplikace během 2 minut:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enable-single-sign-on-to-Google-Apps-in-2-minutes-with-Azure-AD/player]

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
1. **Otázka: je Chromebooks a dalších zařízení Chrome kompatibilní s Azure AD jednotné přihlašování?**
   
    Odpověď: Ano, jsou uživatelé moct toosign do jejich zařízení Chromebook pomocí svých přihlašovacích údajů Azure AD. Toto [Google Apps podporují článku](https://support.google.com/chrome/a/answer/6060880) informace o důvod, proč může získat uživatelé vyzváni k zadání pověření dvakrát.

2. **Otázka:-li povolit jednotné přihlašování, uživatelé budou mít možnost toouse jejich toosign přihlašovací údaje Azure AD do jakékoli Google produkt, například Google učebny, GMail, Google Drive, YouTube a tak dále?**
   
    Odpověď: Ano, v závislosti na [které Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) zvolte tooenable nebo zakázat pro vaši organizaci.

3. **Otázka: je možné povolit jednotné přihlašování pro pouze podmnožinu Moji uživatelé Google Apps?**
   
    Odpověď: Ne, zapnout jednotné přihlašování okamžitě vyžaduje všechny tooauthenticate uživatelé vaší Google Apps pomocí svých přihlašovacích údajů Azure AD. Protože Google Apps nepodporuje, s více poskytovatelů identit, hello zprostředkovatele identity pro vaše prostředí Google Apps může být buď Azure AD nebo Google – ale ne oba v hello stejnou dobu.

4. **Otázka: Pokud je uživatel přihlášený prostřednictvím systému Windows, jsou že automaticky ověřují tooGoogle aplikace bez získávání výzva k zadání hesla?**
   
    Odpověď: existují dvě možnosti pro povolení tohoto scénáře. Nejdřív by uživatelé přihlašovat do zařízení s Windows 10 prostřednictvím [Azure Active Directory Join](active-directory-azureadjoin-overview.md). Alternativně může uživatelé přihlašovat do zařízení s Windows, které jsou připojené k doméně tooan místní služby Active Directory, která byla povolená pro jeden přihlašování tooAzure AD prostřednictvím [Active Directory Federation Services (AD FS)](active-directory-aadconnect-user-signin.md) nasazení. Obě možnosti vyžadovat tooperform hello kroky v následujícím kurzu tooenable jednotné přihlašování mezi službou Azure AD hello a Google Apps.

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Google Apps z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-google-apps-from-hello-gallery"></a>Přidání Google Apps z Galerie hello
tooconfigure hello integrace Google Apps do Azure AD, je nutné tooadd Google Apps hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Google Apps z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Google Apps**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_search.png)

5. Na panelu výsledků hello vyberte **Google Apps**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Google Apps podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Google Apps je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Google Apps musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Google Apps.

tooconfigure a testu Azure AD jednotné přihlašování s Google Apps, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Google Apps](#creating-a-google-apps-test-user)**  -toohave protějšek Britta Simon v Google Apps, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Google Apps.

**tooconfigure Azure AD jednotné přihlašování s Google Apps, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Google Apps** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_samlbase.png)

3. Na hello **Google Apps domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_url.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://mail.google.com/a/<yourdomain>`

    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte hodnotu hello s hello skutečná adresa URL přihlašování. Obraťte se na hello [tým podpory Google](https://www.google.com/contact/).
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte certifikát hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_general_400.png)

6. Na hello **Google Apps konfigurace** klikněte na tlačítko **konfigurace Google Apps** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML jeden přihlašování adresa URL služby a změnit heslo URL** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_configure.png) 

7. V prohlížeči otevřete novou kartu a přihlaste se k hello [konzoly pro správu aplikace Google](http://admin.google.com/) pomocí účtu správce.

8. Klikněte na tlačítko **zabezpečení**. Pokud nevidíte odkaz hello, mohou být skryty pod hello **více ovládacích prvků** nabídky v hello dolní části obrazovky hello.
   
    ![Klikněte na Zabezpečení.][10]

9. Na hello **zabezpečení** klikněte na tlačítko **nastavit jednotné přihlašování (SSO).**
   
    ![Klikněte na tlačítko jednotné přihlašování.][11]

10. Proveďte následující změny konfigurace hello:
   
    ![Konfigurace jednotného přihlašování][12]
   
    a. Vyberte **nastavení jednotného přihlašování pomocí zprostředkovatele identity jiných výrobců**.

    b. V **přihlašovací adresa URL stránky** pole v Google Apps, vložte hodnotu hello **jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.

    c. V hello **adresy URL odhlašovací stránky** pole v Google Apps, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure. 

    d. V hello **změnit heslo URL** pole v Google Apps, vložte hodnotu hello **změnit heslo URL**, který jste zkopírovali z portálu Azure. 

    e. V Google Apps pro hello **ověřovací certifikát**, nahrávání hello certifikátu, kterou jste si stáhli z portálu Azure.

    f. Klikněte na tlačítko **uložit změny**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-google-apps-test-user"></a>Vytvoření zkušebního uživatele Google Apps

Hello cílem této části je toocreate uživatel volal Britta Simon v Google Apps softwaru. Google Apps podporuje automatické zřizování, který je ve výchozím nastavení povolené. Neexistuje žádná akce pro vás v této části. Pokud uživatel ještě neexistuje v Google Apps softwaru, novou vytvoří při pokusíte tooaccess Google Apps softwaru.

>[!NOTE] 
>Pokud potřebujete toocreate uživatel ručně, obraťte se na hello [tým podpory Google](https://www.google.com/contact/).

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte Britta Simon toouse Azure jednotného přihlašování k udělení přístupu tooGoogle aplikace.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooGoogle aplikace, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Google Apps**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části tootest jeden přihlašování nastavení, otevřete hello přístupovému panelu na adrese [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md), pak se přihlaste do hello testovací účet a klikněte na tlačítko **Google Apps** dlaždici v hello přístupového panelu.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfiguraci zřizování uživatelů](active-directory-saas-google-apps-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_203.png

[0]: ./media/active-directory-saas-google-apps-tutorial/azure-active-directory.png

[5]: ./media/active-directory-saas-google-apps-tutorial/gapps-added.png
[6]: ./media/active-directory-saas-google-apps-tutorial/config-sso.png
[7]: ./media/active-directory-saas-google-apps-tutorial/sso-gapps.png
[8]: ./media/active-directory-saas-google-apps-tutorial/sso-url.png
[9]: ./media/active-directory-saas-google-apps-tutorial/download-cert.png
[10]: ./media/active-directory-saas-google-apps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-google-apps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-config.png
[13]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-confirm.png
[14]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-email.png
[15]: ./media/active-directory-saas-google-apps-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-tutorial/gapps-api-enabled.png
[17]: ./media/active-directory-saas-google-apps-tutorial/add-custom-domain.png
[18]: ./media/active-directory-saas-google-apps-tutorial/specify-domain.png
[19]: ./media/active-directory-saas-google-apps-tutorial/verify-domain.png
[20]: ./media/active-directory-saas-google-apps-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-another.png
[23]: ./media/active-directory-saas-google-apps-tutorial/apps-gapps.png
[24]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-tutorial/gapps-auth.png
[29]: ./media/active-directory-saas-google-apps-tutorial/assign-users.png
[30]: ./media/active-directory-saas-google-apps-tutorial/assign-confirm.png