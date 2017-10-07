---
title: 'Kurz: Azure Active Directory integrace s Wdesk | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Wdesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 2c278a5e4f50cc362663efc0f826ff0c0fcf6e7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wdesk"></a>Kurz: Azure Active Directory integrace s Wdesk

V tomto kurzu zjistíte, jak toointegrate Wdesk s Azure Active Directory (Azure AD).

Integrace Wdesk s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooWdesk
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWdesk (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v článku. [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Wdesk tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Wdesk jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Wdesk z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-wdesk-from-hello-gallery"></a>Přidání Wdesk z Galerie hello
tooconfigure hello integrace Wdesk do Azure AD, je nutné tooadd Wdesk hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Wdesk z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Wdesk**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_search.png)

5. Na panelu výsledků hello vyberte **Wdesk**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Wdesk podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Wdesk je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Wdesk musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Wdesk.

tooconfigure a testu Azure AD jednotné přihlašování s Wdesk, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Wdesk](#creating-a-wdesk-test-user)**  -toohave protějšek Britta Simon v Wdesk, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Wdesk.

**tooconfigure Azure AD jednotné přihlašování s Wdesk, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Wdesk** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_samlbase.png)

3. Na hello **Wdesk domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** initiated režim provedení hello následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url.png)

    a. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`

4. Zkontrolujte **zobrazit upřesňující nastavení adresy URL**. Pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu, proveďte následující krok hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url1.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`
     
    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL. Tyto hodnoty z portálu WDesk získat konfiguraci hello jednotné přihlašování. 
  
5. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_general_400.png)
    
7. V okně prohlížeče jiný web, tooWdesk přihlášení jako správce zabezpečení.

8. Hello dole vlevo, klikněte na tlačítko **správce** a zvolte **správce účtu**:
 
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

9. V Wdesk správce, přejděte příliš**zabezpečení**, pak **SAML** > **SAML nastavení**:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

10. V části **obecné nastavení**, zkontrolujte hello **povolit SAML jednotné přihlašování**:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

11. V části **podrobné informace o poskytovateli služby**, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      a. Kopírování hello **přihlašovací adresa URL** a vložte jej do **přihlašovací adresa Url** textové pole na portálu Azure.
   
      b. Kopírování hello **adresu Url metadat** a vložte jej do **identifikátor** textové pole na portálu Azure.
       
      c. Kopírování hello **příjemce url** a vložte jej do **adresa Url odpovědi** textové pole na portálu Azure.
   
      d. Klikněte na tlačítko **Uložit** na portálu Azure toosave hello změny.      

12. Klikněte na tlačítko **konfigurovat nastavení IdP** tooopen **upravit nastavení IdP** dialogové okno. Klikněte na tlačítko **zvolit soubor** toolocate hello **Metadata.xml** soubor uložit z portálu Azure, pak nahrajte ho.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
13. Klikněte na tlačítko **uložit změny**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-wdesk-test-user"></a>Vytvoření zkušebního uživatele Wdesk

Uživatelé toolog tooenable Azure AD v tooWdesk, se musí být zřízená do Wdesk. V Wdesk zřizování je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooWdesk jako správce zabezpečení.
2. Přejděte příliš**správce** > **správce účtu**.

     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. Klikněte na tlačítko **členy** pod **osoby**.

4. Klikněte na tlačítko **přidat člena** tooopen **přidat člena** dialogové okno. 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser1.png)  

5. V **uživatele** textové pole, zadejte uživatelské jméno hello uživatele jako  **brittasimon@contoso.com**  a klikněte na tlačítko **pokračovat** tlačítko.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser3.png)

6.  Zadejte podrobnosti hello, jak je uvedeno níže:
  
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser4.png)
 
    a. V **e-mailu** textové pole, zadejte e-mailu hello uživatele jako  **brittasimon@contoso.com** .

    b. V **křestní jméno** textové pole, zadejte hello křestní jméno uživatele jako **Britta**.

    c. V **příjmení** text zadejte příjmení uživatele jako hello **Simon**.

7. Klikněte na tlačítko **uložit člen** tlačítko.  

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser5.png)

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooWdesk toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooWdesk, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Wdesk**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Wdesk hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Wdesk aplikace.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_203.png

