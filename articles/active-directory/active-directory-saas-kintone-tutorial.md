---
title: 'Kurz: Azure Active Directory integrace s Kintone | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Kintone."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2b947dc-e1a8-4f5f-b40e-2c5180648e4f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 4f61fb95c643743504699b175beeff06e01c95c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kintone"></a>Kurz: Azure Active Directory integrace s Kintone

V tomto kurzu zjistíte, jak toointegrate Kintone s Azure Active Directory (Azure AD).

Integrace Kintone s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooKintone
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooKintone (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Kintone tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Kintone jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Kintone z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-kintone-from-hello-gallery"></a>Přidání Kintone z Galerie hello
tooconfigure hello integrace Kintone do Azure AD, je nutné tooadd Kintone hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Kintone z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Kintone**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_search.png)

5. Na panelu výsledků hello vyberte **Kintone**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kintone podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Kintone je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Kintone musí toobe navázat.

V Kintone, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Kintone, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Kintone](#creating-a-kintone-test-user)**  -toohave protějšek Britta Simon v Kintone, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Kintone.

**tooconfigure Azure AD jednotné přihlašování s Kintone, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Kintone** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_samlbase.png)

3. Na hello **Kintone domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.kintone.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:
    | |
    |--|
    | `https://<companyname>.cybozu.com`|
    | `https://<companyname>.kintone.com`|

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory Kintone klienta](https://www.kintone.com/contact/) tooget tyto hodnoty. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_general_400.png)

6. Na hello **Kintone konfigurace** klikněte na tlačítko **konfigurace Kintone** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_configure.png) 

7. V okně prohlížeče jiný web, přihlaste se k vaší **Kintone** společnosti lokality jako správce.

8. Klikněte na tlačítko **nastavení**.
   
    ![Nastavení](./media/active-directory-saas-kintone-tutorial/ic785879.png "nastavení")

9. Klikněte na tlačítko **uživatelů a správu systému**.
   
    ![Uživatelé a správu systému](./media/active-directory-saas-kintone-tutorial/ic785880.png "uživatelů a správu systému")

10. V části **systému správy \> zabezpečení** klikněte na tlačítko **přihlášení**.
   
    ![Přihlášení](./media/active-directory-saas-kintone-tutorial/ic785881.png "přihlášení")

11. Klikněte na tlačítko **ověřování povolit SAML**.
   
    ![Ověřování SAML](./media/active-directory-saas-kintone-tutorial/ic785882.png "ověřování SAML")

12. V části ověřování SAML hello proveďte hello následující kroky:
    
    ![Ověřování SAML](./media/active-directory-saas-kintone-tutorial/ic785883.png "ověřování SAML")
    
    a. V hello **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.
   
    b. V hello **adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.
    
    c. Klikněte na tlačítko **Procházet** tooupload stažený certifikát.
    
    d. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-kintone-test-user"></a>Vytvoření zkušebního uživatele Kintone

Uživatelé toolog tooenable Azure AD v tooKintone, se musí být zřízená do Kintone.  
V případě hello Kintone zřizování je ruční úloha.

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a>tooprovision uživatelský účet, proveďte následující kroky hello:

1. Přihlaste se tooyour **Kintone** společnosti lokality jako správce.

2. Klikněte na tlačítko **nastavení**.
   
    ![Nastavení](./media/active-directory-saas-kintone-tutorial/ic785879.png "nastavení")

3. Klikněte na tlačítko **uživatelů a správu systému**.
   
    ![& Systém správy uživatelského](./media/active-directory-saas-kintone-tutorial/ic785880.png "uživatele & správu systému")

4. V části **Správa uživatele**, klikněte na tlačítko **oddělení a uživatelé**.
   
    ![Oddělení a uživatelé](./media/active-directory-saas-kintone-tutorial/ic785888.png "oddělení a uživatelé")

5. Klikněte na tlačítko **nového uživatele**.
   
    ![Noví uživatelé](./media/active-directory-saas-kintone-tutorial/ic785889.png "noví uživatelé")

6. V hello **nového uživatele** část, proveďte následující kroky hello:
   
    ![Noví uživatelé](./media/active-directory-saas-kintone-tutorial/ic785890.png "noví uživatelé")
   
    a. Zadejte **zobrazovaný název**, **přihlašovací jméno**, **nové heslo**, **potvrzení hesla**, **e-mailová adresa**, a další podrobnosti o platný účet AAD, že chcete tooprovision do hello související textových polí.
 
    b. Klikněte na **Uložit**.

> [!NOTE]
> Můžete použít všechny ostatní Kintone uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Kintone tooprovision AAD uživatelské účty.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooKintone toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooKintone, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Kintone**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.

Když kliknete na dlaždici Kintone hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Kintone aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_203.png

