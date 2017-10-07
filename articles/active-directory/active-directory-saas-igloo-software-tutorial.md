---
title: 'Kurz: Azure Active Directory integrace s Igloo softwarem | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Igloo softwarem a Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 406405d4faa6e56f1005a61e69a29ef2ef2eb34b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Kurz: Azure Active Directory integrace s Igloo softwaru

V tomto kurzu zjistíte, jak toointegrate Igloo Software s Azure Active Directory (Azure AD).

Integrace Igloo softwaru s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooIgloo softwaru
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooIgloo softwaru (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Igloo softwaru, je třeba hello následující položky:

- Předplatné služby Azure AD
- Softwaru Igloo jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání softwaru Igloo z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-igloo-software-from-hello-gallery"></a>Přidání softwaru Igloo z Galerie hello
tooconfigure hello integrace Igloo softwaru do služby Azure AD, je nutné tooadd Igloo softwaru hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Igloo softwaru z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Igloo softwaru**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_search.png)

5. Na panelu výsledků hello vyberte **Igloo softwaru**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Igloo Software založený na testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Igloo softwaru je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v softwaru Igloo musí toobe navázat.

V softwaru Igloo přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Igloo softwaru, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele softwaru Igloo](#creating-an-igloo-software-test-user)**  -toohave protějšek Britta Simon v Igloo Software, který je propojený toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Igloo softwaru.

**tooconfigure Azure AD jednotné přihlašování s Igloo softwarem, provést hello následující kroky:**

1. V portálu Azure, na hello hello **Igloo softwaru** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_samlbase.png)

3. Na hello **Igloo softwaru domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_url.png)
    
    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.igloocommmunities.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.igloocommmunities.com/saml.digest`

    c. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.igloocommmunities.com/saml.digest`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL. Obraťte se na [tým podpory klientský Software Igloo](https://www.igloosoftware.com/services/support) tooget tyto hodnoty. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_general_400.png)
    
6. Na hello **Igloo softwarové konfigurace** klikněte na tlačítko **konfigurace softwaru Igloo** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti tooyour Igloo softwaru.

8. Přejděte toohello **ovládací panely**.
   
     ![Ovládací panely](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "ovládací panely")

9. V hello **členství** , klikněte na **přihlášení v nastavení**.
   
    ![Přihlaste se nastavení](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "přihlášení nastavení")

10. V hello SAML konfigurační oddíl, klikněte na **konfigurace ověřování SAML**.
   
    ![Konfigurace SAML](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "konfigurace SAML")
   
11. V hello **obecné konfigurace** část, proveďte následující kroky hello:
   
    ![Obecná konfigurace](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "obecné konfigurace")

    a. V hello **název připojení** textovému poli, napište vlastní název pro svou konfiguraci.
   
    b. V hello **IdP přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.
   
    c. V hello **adresy URL odhlašovací IdP** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.
    
    d. Vyberte **odhlášení odpovědi a typ požadavku HTTP** jako **POST**.
   
    e. Otevřete váš **kódování base-64** kódování certifikátu v poznámkovém bloku stáhli z portálu Azure, hello kopírování obsahu ho do schránky a pak ji vložit toohello **veřejný certifikát** textové pole.
    
12. V hello **odpovědi a konfiguraci ověřování**, proveďte následující kroky hello:
    
    ![Odpověď a konfiguraci ověřování](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "odpovědi a konfiguraci ověřování")
  
      a. Jako **zprostředkovatele Identity**, vyberte **Microsoft ADFS**.
      
      b. Jako **typ identifikátoru**, vyberte **e-mailovou adresu**. 

      c. V hello **atribut e-mailu** textovému poli, typ **emailaddress**.

      d. V hello **křestní jméno atribut** textovému poli, typ **givenname**.

      e. V hello **poslední atribut Name** textovému poli, typ **Přezdívka**.

13. Proveďte následující kroky toocomplete hello konfigurace hello:
    
    ![Vytvoření uživatele na přihlašovací](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "vytvoření uživatele na přihlášení") 

     a. Jako **vytvoření uživatele na přihlašovací**, vyberte **vytvořte nového uživatele ve vaší lokalitě při přihlášení**.

     b. Jako **přihlášení nastavení**, vyberte **SAML použijte tlačítko na obrazovce "Přihlásit"**.

     c. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-igloo-software-test-user"></a>Vytváření testovacího uživatele Igloo softwaru

Neexistuje žádná položka akce, pro které uživatele tooconfigure zřizování tooIgloo softwaru.  

Když se uživatel s přiřazenou pokusí toolog v tooIgloo Software pomocí hello přístupový panel Igloo softwaru kontroly, zda text hello uživatele existuje.  Pokud neexistuje žádný účet k dispozici dosud, se automaticky vytvoří Igloo softwarem.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooIgloo softwaru Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooIgloo softwaru, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Igloo softwaru**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Igloo softwaru hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Igloo softwarová aplikace.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_203.png

