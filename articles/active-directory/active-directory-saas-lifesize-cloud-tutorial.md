---
title: 'Kurz: Azure Active Directory integrace s Lifesize cloudu | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Lifesize cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ae599907e872571b3220de7122006c7db8db4a2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Kurz: Azure Active Directory integrace s Lifesize cloudu

V tomto kurzu zjistíte, jak toointegrate Lifesize cloudu s Azure Active Directory (Azure AD).

Integrace Lifesize cloudu s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooLifesize cloudu
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLifesize cloudu (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Lifesize cloudu, je třeba hello následující položky:

- Předplatné služby Azure AD
- Cloudu Lifesize jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Lifesize cloudu z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-lifesize-cloud-from-hello-gallery"></a>Přidání Lifesize cloudu z Galerie hello
tooconfigure hello integrace Lifesize cloudu do Azure AD, je nutné tooadd Lifesize cloudu hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Lifesize cloudu z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Lifesize cloudu**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_search.png)

5. Na panelu výsledků hello vyberte **Lifesize cloudu**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s cloudem Lifesize podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v cloudu Lifesize je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v cloudu Lifesize musí toobe navázat.

V cloudu Lifesize přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Lifesize cloudu, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Lifesize Cloud](#creating-a-lifesize-cloud-test-user)**  -toohave protějšek Britta Simon Lifesize cloudu, který je propojený toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Lifesize cloudu.

**tooconfigure Azure AD jednotné přihlašování s Lifesize cloudu, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Lifesize cloudu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_samlbase.png)

3. Na hello **Lifesize cloudové domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://login.lifesizecloud.com/ls/?acs`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://login.lifesizecloud.com/<companyname>`

     
4. Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, proveďte následující krok hello:  
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url1.png)

    V hello **předávání stavu** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://webapp.lifesizecloud.com/?ent=<identifier>`
   
   > [!NOTE] 
   >Upozorňujeme, že tyto nejsou hello skutečné hodnoty. Máte tooupdate tyto hodnoty s hello skutečná adresa URL přihlašování, stav směrování a identifikátor. Obraťte se na [tým podpory klient Cloud Lifesize](https://www.lifesize.com/support) tooget přihlašovací adresa URL a identifikátor hodnoty a můžete získat hodnotu předávání stavu z konfigurace jednotného přihlašování, která je vysvětlená později v kurzu hello.

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_400.png)

6. Na hello **konfigurace cloudu Lifesize** klikněte na tlačítko **konfigurace cloudu Lifesize** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_configure.png) 

7. tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, přihlásit se k hello Lifesize cloudových aplikací s oprávněními správce.

8. V hello pravém horním rohu klikněte na název a potom klikněte na hello **rozšířená nastavení**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

9. V hello rozšířená nastavení klikněte na na hello **Konfigurace jednotného přihlašování k** odkaz. Otevře stránku hello Konfigurace jednotného přihlašování pro vaše instance.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

10. Teď nakonfigurujte hello následující hodnoty v konfiguraci jednotného přihlašování k hello uživatelského rozhraní.    
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
    
    a. V **vystavitele zprostředkovatele Identity** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.

    b.  V **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.

    c. Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku stáhli z portálu Azure, hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát X.509** textové pole.
  
    d. Hello SAML atribut mapování pro hello křestní jméno textového pole zadejte hodnotu hello jako **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    
    e. V mapování hello atribut SAML pro hello **příjmení** textového pole zadejte hodnotu hello jako **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    
    f. V mapování hello atribut SAML pro hello **e-mailu** textového pole zadejte hodnotu hello jako **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

11. Konfigurace hello toocheck můžete kliknutím na hello **Test** tlačítko.
   
    >[!NOTE]
    >Pro úspěšné testování potřebovat Průvodce konfigurací hello toocomplete ve službě Azure AD a také poskytnout přístup toousers nebo skupin, které můžete provést hello test.

12. Povolit hello jednotné přihlašování kontrolou na hello **povolit jednotné přihlašování** tlačítko.

13. Nyní klikněte na hello **aktualizace** tlačítko tak, aby se ukládají všechna nastavení hello. Tím se vygeneruje hello RelayState hodnotu. Kopírování hello RelayState hodnotu, která se generují hello textového pole, vložte jej v hello **předávání stavu** textového pole pod **Lifesize cloudové domény a adresy URL** části. 

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-lifesize-cloud-test-user"></a>Vytvoření zkušebního uživatele Lifesize cloudu

V této části vytvoříte uživatele názvem Britta Simon v Lifesize cloudu. Lifesize cloudu podporovat zřizování automatické uživatelů. Po úspěšném ověření v Azure AD se automaticky zřídí hello uživatele v aplikaci hello. 

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooLifesize cloudu Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign tooLifesize Britta Simon cloudu, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Lifesize cloudu**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello Lifesize cloudu dlaždici v hello přístupového panelu, měli byste obdržet přihlašovací stránku Lifesize cloudových aplikací.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png

