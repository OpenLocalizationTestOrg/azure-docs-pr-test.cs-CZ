---
title: "Kurz: Azure Active Directory integrace s Image předávání | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a předávací bitové kopie."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: baf39e4437b85c2de5b524984ad5ca39badbab63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a>Kurz: Azure Active Directory integrace s předávání bitové kopie

V tomto kurzu zjistíte, jak toointegrate předávání bitové kopie s Azure Active Directory (Azure AD).

Integrace předávání bitové kopie s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooImage předávání
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooImage předávání (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s předávání bitové kopie, je třeba hello následující položky:

- Předplatné služby Azure AD
- Předávání Image jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání bitové kopie předávání z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-image-relay-from-hello-gallery"></a>Přidání bitové kopie předávání z Galerie hello
tooconfigure hello integrace předávání bitové kopie do Azure AD, je nutné tooadd předávání bitové kopie z hello Galerie tooyour seznam spravovaných aplikací SaaS.

**tooadd předávání bitové kopie z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Image předávání**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_search.png)

5. Na panelu výsledků hello vyberte **Image předávání**a pak klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s předávání bitové kopie založené na testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello Relay bitové kopie je tooa uživatelem ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello Relay Image musí toobe navázat.

V předávání bitové kopie, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s předávání bitové kopie, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele předávání Image](#creating-an-image-relay-test-user)**  -toohave protějšek Britta Simon Relay bitovou kopii, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci předávání bitové kopie.

**tooconfigure Azure AD jednotné přihlašování s předávání bitové kopie, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Image předávání** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_samlbase.png)

3. Na hello **Image předávání domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.imagerelay.com/`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.imagerelay.com/sso/metadata`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory Image předávání klienta](http://support.imagerelay.com/) tooget tyto hodnoty. 
 


4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_general_400.png)

6. Na hello **konfigurace přenosového Image** klikněte na tlačítko **konfigurace přenosového bitové kopie** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresa URL služby a SAML jeden přihlašování adresu URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_configure.png) 

7. V jiném okně prohlížeče Přihlaste se jako správce na webu společnosti tooyour předávání bitové kopie.

8. V panelu nástrojů hello hello nahoře, klikněte na tlačítko hello **uživatelé a oprávnění** zatížení.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) 

9. Klikněte na tlačítko **vytvořit nová položka oprávnění**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png)

10. V hello **jeden znak v nastavení** úlohy, vyberte hello **tato skupina může pouze přihlášení přes jednotné přihlašování** zaškrtněte políčko a potom klikněte na **Uložit**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) 

11. Přejděte příliš**nastavení účtu**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) 

12. Přejděte toohello **jeden znak v nastavení** zatížení.
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

13. Na hello **SAML nastavení** dialogové okno, proveďte následující kroky hello:
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)
    
    a. V **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.

    b. V **adresy URL odhlašovací** textovému poli, vložte hodnotu hello **jednu adresu URL služby Sign-Out** který jste zkopírovali z portálu Azure.

    c. Jako **formát Id názvu**, vyberte **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: emailAddress**.

    d. Jako **vazby možnosti pro žádosti od hello poskytovatele služeb (Image relé)**, vyberte **POST vazby**.

    e. V části **x.509 Certificate**, klikněte na tlačítko **aktualizace certifikátu**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. V poznámkovém bloku otevřete hello stáhnout certifikát, zkopírujte hello obsah a pak ji vložit do textového pole certifikátu x.509 hello.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. V **zřizování uživatelů JIT** části, vyberte hello **povolit zřizování uživatelů JIT**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. Vyberte hello skupině oprávnění (například **jednotného přihlašování k základní**) povolený toosign v pouze prostřednictvím jednotného přihlašování.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    i. Klikněte na **Uložit**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-image-relay-test-user"></a>Vytváření testovacího uživatele předávání bitové kopie

Hello cílem této části je toocreate uživatel volal Britta Simon v předávání bitové kopie.

**toocreate uživatel volal Britta Simon v předávání bitové kopie, proveďte následující kroky hello:**

1. Web společnosti předávání bitové kopie tooyour přihlášení jako správce.

2. Přejděte příliš**uživatelé a oprávnění** a vyberte **vytvořit jednotné přihlašování uživatele**.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) 

3. Zadejte hello **e-mailu**, **křestní jméno**, **příjmení**, a **společnosti** hello uživatele chcete tooprovision a vyberte hello oprávnění skupiny (například jednotného přihlašování k základní) tedy hello skupinu, která může přihlásit pouze pomocí jednotného přihlašování.
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) 

4. Klikněte na možnost **Vytvořit**.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooImage předávání Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign tooImage Britta Simon předávání, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Image předávání**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.    

Když kliknete na dlaždici hello předávání bitové kopie v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour předávání Image aplikace.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png


[100]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png

