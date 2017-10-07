---
title: "Kurz: Azure Active Directory integrace s Five9 Plus adaptér (CTI, obraťte se na centrum agenty) | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Five9 Plus adaptér (CTI, obraťte se na centrum agenty)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: 2e3ea8b5f2a6eaa8ad17d39e03fa490038b14561
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Kurz: Azure Active Directory integrace s Five9 Plus adaptér (CTI, obraťte se na centrum agentů)

V tomto kurzu zjistíte, jak toointegrate Five9 Plus adaptér (CTI, obraťte se na centrum agenty) s Azure Active Directory (Azure AD).

Integrace Five9 Plus adaptér (CTI, obraťte se na centrum agenty) s Azure AD poskytuje hello následující výhody:

- Můžete řídit v Azure AD, který má přístup tooFive9 Plus adaptér (CTI, obraťte se na centrum agentů)
- Můžete povolit uživatelům tooautomatically get přihlášeného tooFive9 Plus adaptér (CTI, obraťte se na centrum agenty) (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

integrace tooconfigure Azure AD s Five9 Plus adaptérem (CTI, obraťte se na centrum agenty), musíte hello následující položky:

- Předplatné služby Azure AD
- Five9 Plus adaptér (CTI, obraťte se na centrum agenty) jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Five9 Plus adaptér (CTI, obraťte se na centrum agenty) z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-hello-gallery"></a>Přidání Five9 Plus adaptér (CTI, obraťte se na centrum agenty) z Galerie hello
tooconfigure hello integrace Five9 Plus adaptéru (CTI, obraťte se na centrum agenty) do Azure AD, musíte tooadd Five9 Plus adaptér (CTI, obraťte se na centrum agenty) hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Five9 Plus adaptér (CTI, obraťte se na centrum agenty) z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Five9 Plus adaptér (CTI, obraťte se na centrum agenty)**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-five9-tutorial/tutorial_five9_search.png)

5. Na panelu výsledků hello vyberte **Five9 Plus adaptér (CTI, obraťte se na centrum agenty)**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části konfiguraci a testování Azure AD jednotné přihlašování s Five9 Plus adaptér (CTI, obraťte se na centrum agenty) na základě testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Five9 Plus adaptéru (CTI, obraťte se na centrum agenty) je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Five9 Plus adaptéru (CTI, obraťte se na centrum agenty) musí toobe navázat.

V Five9 Plus adaptér (CTI, obraťte se na centrum agenty), přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testování Azure AD jednotné přihlašování s Five9 Plus adaptérem (CTI, obraťte se na centrum agenty), musíte toocomplete hello následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Five9 Plus adaptér (CTI, obraťte se na centrum agenty)](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)**  -toohave protějšek Britta Simon v Five9 Plus adaptéru (CTI, obraťte se na centrum agenty), je toohello propojené služby Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Five9 Plus adaptér (CTI, obraťte se na centrum agenty).

**tooconfigure Azure AD jednotné přihlašování s Five9 Plus adaptérem (CTI, obraťte se na centrum agenty), proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Five9 Plus adaptér (CTI, obraťte se na centrum agenty)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-five9-tutorial/tutorial_five9_samlbase.png)

3. Na hello **Five9 Plus adaptér (CTI, obraťte se na centrum agenty) domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-five9-tutorial/tutorial_five9_url.png)
    
    a. V hello **identifikátor** textové pole, zadejte adresu URL pomocí hello následující vzory:

    |    Prostředí      |       ADRESA URL      |
    | :-- | :-- |
    | Pro "Five9 Plus adaptér pro aplikaci Microsoft Dynamics CRM" | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | Pro "Five9 Plus adaptér Zendesk" | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | Pro "Five9 Plus adaptér plochy Toolkit agenta" | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:

    |      Prostředí     |      ADRESA URL      |
    | :--                  | :--           |
    | Pro "Five9 Plus adaptér pro aplikaci Microsoft Dynamics CRM" | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | Pro "Five9 Plus adaptér Zendesk" | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | Pro "Five9 Plus adaptér plochy Toolkit agenta" | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-five9-tutorial/tutorial_five9_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-five9-tutorial/tutorial_general_400.png)

6. Na hello **konfigurace Five9 Plus adaptéru (CTI, obraťte se na centrum agenty)** klikněte na tlačítko **konfigurace Five9 Plus adaptér (CTI, obraťte se na centrum agenty)** tooopen **konfigurovatpřihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-five9-tutorial/tutorial_five9_configure.png) 

7. tooconfigure jednotného přihlašování na **Five9 Plus adaptér (CTI, obraťte se na centrum agenty)** straně, je nutné stáhnout hello toosend **Certificate(Base64), Sign-Out URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** příliš[tým podpory Five9 Plus adaptér (CTI, obraťte se na centrum agenty)](https://www.five9.com/about/contact). Také navíc další konfigurace jednotného přihlašování k postupujte hello níže uvedených pokynů podle toohello adaptéru:

    a. "Five9 Plus adaptér plochy Toolkit agenta" Příručka správce: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. "Five9 Plus adaptér pro aplikaci Microsoft Dynamics CRM" Příručka správce: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. "Five9 Plus adaptér Zendesk" Příručka správce: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)


> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Vytvoření zkušebního uživatele Five9 Plus adaptér (CTI, obraťte se na centrum agentů)

V této části vytvoříte uživatele názvem Britta Simon v Five9 Plus adaptéru (CTI, obraťte se na centrum agenty). Práce s [tým podpory Five9 Plus adaptér (CTI, obraťte se na centrum agenty)](https://www.five9.com/about/contact) pro přidání uživatelů hello platformy hello Five9 Plus adaptér (CTI, obraťte se na centrum agenty). Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooFive9 Plus adaptér (CTI, obraťte se na centrum agenty) toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooFive9 Plus adaptér (CTI, obraťte se na centrum agenty), proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Five9 Plus adaptér (CTI, obraťte se na centrum agenty)**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-five9-tutorial/tutorial_five9_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici hello Five9 Plus adaptér (CTI, obraťte se na centrum agenty) v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Five9 Plus adaptér (CTI, obraťte se na centrum agenty) aplikace.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-five9-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-five9-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-five9-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-five9-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-five9-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-five9-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-five9-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-five9-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-five9-tutorial/tutorial_general_203.png

