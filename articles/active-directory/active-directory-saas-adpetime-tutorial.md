---
title: 'Kurz: Azure Active Directory integrace s ADP eTime | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ADP eTime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a3e9f0be-19ed-4b99-a180-619e7624fc26
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 9a5c7d14a18220f8c7a5b14055c30662ecd8f14d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a>Kurz: Azure Active Directory integrace s ADP eTime

V tomto kurzu zjistíte, jak toointegrate ADP eTime službou Azure Active Directory (Azure AD).

Integrace ADP eTime s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooADP eTime
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooADP eTime (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s ADP eTime tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- ADP eTime jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání ADP eTime z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-adp-etime-from-hello-gallery"></a>Přidání ADP eTime z Galerie hello
tooconfigure hello integrace ADP eTime do Azure AD, je nutné tooadd ADP eTime hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**eTime tooadd ADP z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **ADP eTime**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_search.png)

5. Na panelu výsledků hello vyberte **ADP eTime**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s ADP eTime podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v ADP eTime je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v ADP eTime musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v ADP eTime.

tooconfigure a testu Azure AD jednotné přihlašování s ADP eTime, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele ADP eTime](#creating-an-adp-etime-test-user)**  -toohave protějšek Britta Simon v eTime ADP, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci eTime ADP.

**tooconfigure Azure AD jednotné přihlašování s ADP eTime, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **ADP eTime** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_samlbase.png)

3. Na hello **ADP eTime domény a adresy URL** část, proveďte následující krok hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_url.png)

    a. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<servername>.adp.com`

    b. V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<servername>.adp.com/affwebservices/public/saml2assertionconsumer` 
 
    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné hello. Tyto hodnoty aktualizujte hello skutečná adresa URL odpovědi a identifikátor. Obraťte se na [tým podpory eTime ADP](https://www.adp.com/contact-us/overview.aspx) tooget tyto hodnoty.

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_certificate.png) 

5. Hello ADP eTime aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování tooyour tokenu atributy konfigurace SAML. Hello následující snímek obrazovky ukazuje příklad pro tento. Hello názvu deklarací identit budou vždy **"PersonImmutableID"** a které jsme jste namapovali tooExtensionAttribute2, který obsahuje hodnotu hello hello EmployeeID hello uživatele. 

    Zde hello mapování uživatelů ze služby Azure AD tooADP eTime bude provedeno v hello EmployeeID ale můžete namapovat jinou hodnotu této tooa také podle nastavení aplikace. Proto prosím práci s [tým podpory eTime ADP](https://www.adp.com/contact-us/overview.aspx) první toouse hello správný identifikátor uživatele a mapovat danou hodnotu s hello **"PersonImmutableID"** deklarací identity.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_attribute.png)

6. V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:
    
    | Název atributu | Hodnota atributu |
    | ------------------- | -------------------- |    
    | PersonImmutableID | User.extensionattribute2 |
    
    a. Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_05.png)

    b. V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.

    c. Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.
    
    d. Klikněte na tlačítko **OK**.

    > [!NOTE] 
    > Před konfigurací hello kontrolního výrazu SAML, je nutné toocontact vaše [tým podpory eTime ADP](https://www.adp.com/contact-us/overview.aspx) a požadovat hello hodnotu atributu hello jedinečný identifikátor pro vašeho klienta. Je nutné tuto hodnotu tooconfigure hello vlastních deklarací identity pro vaši aplikaci. 

7. Na hello **ADP eTime konfigurace** klikněte na tlačítko **konfigurace ADP eTime** tooopen **konfigurovat přihlášení** okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_configure.png) 

8. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_general_400.png)

9. tooconfigure jednotného přihlašování na **ADP eTime** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory eTime ADP](https://www.adp.com/contact-us/overview.aspx). 

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-adp-etime-test-user"></a>Vytvoření ADP eTime testovacího uživatele

Hello cílem této části je toocreate volal Britta Simon v ADP eTime uživatele. Práce s [tým podpory eTime ADP](https://www.adp.com/contact-us/overview.aspx) tooadd hello uživatelé v hello ADP eTime účtu. 
   
### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooADP eTime toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign eTime tooADP Britta Simon, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **ADP eTime**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello ADP eTime dlaždice v hello přístupového panelu, měli byste obdržet tooyour automaticky přihlášeného ADP eTime aplikace.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png

