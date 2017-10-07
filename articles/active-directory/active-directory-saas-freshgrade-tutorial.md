---
title: 'Kurz: Azure Active Directory integrace s FreshGrade | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a FreshGrade."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1055bba6-f4df-462e-bc9b-1ad5ada0f638
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: e2fe7cedd45290945ec5624453a9675abdd7726d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshgrade"></a>Kurz: Azure Active Directory integrace s FreshGrade

V tomto kurzu zjistíte, jak toointegrate FreshGrade s Azure Active Directory (Azure AD).

Integrace FreshGrade s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooFreshGrade
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooFreshGrade (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s FreshGrade tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- FreshGrade jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání FreshGrade z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-freshgrade-from-hello-gallery"></a>Přidání FreshGrade z Galerie hello
tooconfigure hello integrace FreshGrade do Azure AD, je nutné tooadd FreshGrade hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd FreshGrade z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **FreshGrade**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_search.png)

5. Na panelu výsledků hello vyberte **FreshGrade**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s FreshGrade podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v FreshGrade je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v FreshGrade musí toobe navázat.

V FreshGrade, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s FreshGrade, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele FreshGrade](#creating-a-freshgrade-test-user)**  -toohave protějšek Britta Simon v FreshGrade, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci FreshGrade.

**tooconfigure Azure AD jednotné přihlašování s FreshGrade, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **FreshGrade** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_samlbase.png)

3. Na hello **FreshGrade domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_url.png)

    a. V hello **přihlašovací adresa URL** textové pole, zadejte adresu URL pomocí hello následující vzory: 
      | |
      |--|
      | `https://<subdomain>.freshgrade.com/login` |    
      | `https://<subdomain>.onboarding.freshgrade.com/login` |

    b. V hello **identifikátor** textové pole, zadejte adresu URL pomocí hello následující vzory: 
      | |
      |--|
      | `https://login.onboarding.freshgrade.com:443/saml/metadata/alias/<instancename>` |      
      | `https://login.freshgrade.com:443/saml/metadata/alias/<instancename>` |

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory FreshGrade klienta](mailTo:support@freshgrade.com) tooget tyto hodnoty. 
 


4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_general_400.png)

6. Na hello **FreshGrade konfigurace** klikněte na tlačítko **konfigurace FreshGrade** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_configure.png) 

7. toogenerate hello **Metadata** adresu url, proveďte následující kroky hello:

    a. Klikněte na tlačítko **registrace aplikace**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_appregistrations.png)
   
    b. Klikněte na tlačítko **koncové body** tooopen **koncové body** dialogové okno.  
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_endpointicon.png)

    c. Klikněte na tlačítko hello kopie tlačítko toocopy **dokument FEDERAČNÍCH METADAT** adresy url a vložte do poznámkového bloku.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_endpoint.png)
     
    d. Nyní přejděte na stránce vlastností toohello **FreshGrade** a kopírování hello **Id aplikace** pomocí **kopie** tlačítko a vložte do poznámkového bloku.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_appid.png)

    e. Generovat hello **adresu URL metadat** pomocí hello následující vzoru:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`

8. tooconfigure jednotného přihlašování na **FreshGrade** postranní, budete potřebovat toosend hello **adresu URL metadat** a **SAML jeden přihlašování adresa URL služby** příliš[FreshGrade tým podpory](mailTo:support@freshgrade.com). Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-freshgrade-test-user"></a>Vytvoření zkušebního uživatele FreshGrade

V této části vytvoříte volal Britta Simon v FreshGrade uživatele. Spojte se s [tým podpory FreshGrade](mailTo:support@freshgrade.com) tooadd hello uživatelé v platformě FreshGrade hello.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooFreshGrade toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooFreshGrade, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **FreshGrade**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici FreshGrade hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour FreshGrade aplikace.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_203.png

