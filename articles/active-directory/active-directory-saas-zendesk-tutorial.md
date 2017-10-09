---
title: 'Kurz: Azure Active Directory integrace s Zendesk | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a služby Zendesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 46ccd57a4adeb810af459caaa1e592cf2b62cb8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Kurz: Azure Active Directory integrace s Zendesk

V tomto kurzu zjistíte, jak toointegrate Zendesk s Azure Active Directory (Azure AD).

Integrace služby Zendesk s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooZendesk
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooZendesk (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Zendesk, je třeba hello následující položky:

- Předplatné služby Azure AD
- Této služby jednotného přihlašování povolené předplatné


> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.


tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Zendesk z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování


## <a name="adding-zendesk-from-hello-gallery"></a>Přidání Zendesk z Galerie hello
tooconfigure hello integrace Zendesk do Azure AD, je nutné tooadd Zendesk hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Zendesk z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portálu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Zendesk**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_search.png)

5. Na panelu výsledků hello vyberte **Zendesk**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí služby Zendesk podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v této služby je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Zendesku musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v této služby.

tooconfigure a testu Azure AD jednotné přihlašování s Zendesk, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Zendesk](#creating-a-zendesk-test-user)**  -toohave protějšek Britta Simon v této služby, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci služby Zendesk.

**tooconfigure Azure AD jednotné přihlašování s Zendesk, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Zendesk** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. Na hello **Zendesk domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://<subdomain>.zendesk.com`

    b. V hello **identifikátor** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://<subdomain>.zendesk.com`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte hello skutečné přihlašovací adresa URL a identifikátoru adresy URL. Obraťte se na [tým podpory služby Zendesk](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) tooget tyto hodnoty. 

4. Zendesk očekává hello SAML kontrolní výrazy ve specifickém formátu. Neexistují žádné povinné atributy SAML, ale můžete volitelně přidat atribut z **uživatelské atributy** části podle následující hello následujících kroků: 

     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    a. Klikněte na tlačítko hello **prohlížení a úpravy všechny ostatní atributy hello** zaškrtávací políčko.
     
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes2.png)
   
    b. Klikněte na tlačítko hello **přidat atribut** tooopen **přidat atribut** dialogové okno.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    c. V hello **název** textovému poli, zadejte název atributu hello (například **emailaddress**).
    
    d. Z hello **hodnotu** seznam, vyberte hello hodnota atributu (jako **user.mail**).
    
    e. Klikněte na tlačítko **Ok**
 
    > [!NOTE] 
    > Můžete použít rozšíření atributy tooadd atributy, které nejsou ve službě Azure AD ve výchozím nastavení. Klikněte na tlačítko [atributy uživatele, které lze nastavit v SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) tooget hello úplný seznam SAML atributy, které **Zendesk** přijímá. 

5. Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnota certifikátu.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png) 

6. Na hello **Zendesk konfigurace** klikněte na tlačítko **konfigurace Zendesk** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Zendesk.

8. Klikněte na tlačítko **správce**.

9. V levém navigačním podokně hello, klikněte na **nastavení**a potom klikněte na **zabezpečení**.

10. Na hello **zabezpečení** proveďte hello následující kroky: 
   
     ![Zabezpečení](./media/active-directory-saas-zendesk-tutorial/ic773089.png "zabezpečení")

    ![Jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/ic773090.png "jednotného přihlašování")

     a. Klikněte na tlačítko hello **správce & agenty** kartě.

     b. Vyberte **jednotné přihlašování (SSO) a SAML**a potom vyberte **SAML**.

     c. V **URL jednotné přihlašování SAML** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure. 

     d. V **vzdálené adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.
        
     e. V **otisků prstů certifikátů** textovému poli, vložte hello **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.
     
     f. Klikněte na **Uložit**.

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**. 

### <a name="creating-a-zendesk-test-user"></a>Vytvoření zkušebního uživatele Zendesk

Uživatelé toolog tooenable Azure AD do **Zendesk**, musí být zřízená do **Zendesk**.  
V závislosti na roli hello přiřazené v aplikacích hello jeho hello očekávané chování:

 1. **Koncový uživatel** účty jsou zřízené automaticky při přihlášení.
 2. **Agent** a **správce** nutné účty toobe ručně zřízené v **Zendesk** před přihlášením.
 
**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour **Zendesk** klienta.

2. Vyberte hello **seznamu zákazníků** kartě.

3. Vyberte hello **uživatele** a klikněte na **přidat**.
   
    ![Přidat uživatele](./media/active-directory-saas-zendesk-tutorial/ic773632.png "přidat uživatele")
4. Zadejte e-mailovou adresu hello existující účet služby Azure AD tooprovision a pak klikněte na **Uložit**.
   
    ![Nový uživatel](./media/active-directory-saas-zendesk-tutorial/ic773633.png "nového uživatele")

> [!NOTE]
> Můžete použít všechny ostatní Zendesk uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Zendesk tooprovision AAD uživatelské účty.


### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooZendesk toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooZendesk, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Zendesk**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici služby Zendesk hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Zendesk aplikace.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png
