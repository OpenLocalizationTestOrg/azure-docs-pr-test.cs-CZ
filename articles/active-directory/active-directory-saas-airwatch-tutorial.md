---
title: 'Kurz: Azure Active Directory integrace s AirWatch | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a AirWatch."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: e5230d5a36824778a4d9804dadf9f13a0d11a68d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Kurz: Azure Active Directory integrace s AirWatch

V tomto kurzu zjistíte, jak toointegrate AirWatch s Azure Active Directory (Azure AD).

Integrace AirWatch s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooAirWatch
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAirWatch (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s AirWatch tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- AirWatch jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání AirWatch z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-airwatch-from-hello-gallery"></a>Přidání AirWatch z Galerie hello
tooconfigure hello integrace AirWatch do Azure AD, je nutné tooadd AirWatch hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd AirWatch z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **AirWatch**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_search.png)

5. Na panelu výsledků hello vyberte **AirWatch**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s AirWatch podle testovacího uživatele názvem "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v AirWatch je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v AirWatch musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v AirWatch.

tooconfigure a testu Azure AD jednotné přihlašování s AirWatch, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele AirWatch](#creating-a-airwatch-test-user)**  -toohave protějšek Britta Simon v AirWatch, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci AirWatch.

**tooconfigure Azure AD jednotné přihlašování s AirWatch, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **AirWatch** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. Na hello **AirWatch domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`

    b. V hello **identifikátor** textovému poli, hodnota typu hello jako`AirWatch`

    > [!NOTE] 
    > Tato hodnota není skutečné hello. Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory AirWatch klienta](http://www.air-watch.com/company/contact-us/) tooget tuto hodnotu. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. Na hello **AirWatch konfigurace** klikněte na tlačítko **konfigurace AirWatch** tooopen **konfigurovat přihlášení** okno. Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_configure.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS>
7. V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti AirWatch tooyour.

8. V levém navigačním podokně hello, klikněte na **účty**a potom klikněte na **správci**.
   
   ![Správci](./media/active-directory-saas-airwatch-tutorial/ic791920.png "správci")

9. Rozbalte hello **nastavení** nabídce a pak klikněte na tlačítko **Directory Services**.
   
   ![Nastavení](./media/active-directory-saas-airwatch-tutorial/ic791921.png "nastavení")

10. Klikněte na tlačítko hello **uživatele** na kartě hello **Base DN** textovému poli, zadejte název domény a pak klikněte na tlačítko **Uložit**.
   
   ![Uživatel](./media/active-directory-saas-airwatch-tutorial/ic791922.png "uživatele")

11. Klikněte na tlačítko hello **Server** kartě.
   
   ![Server](./media/active-directory-saas-airwatch-tutorial/ic791923.png "serveru")

12. Proveďte hello následující kroky:
    
    ![Nahrát](./media/active-directory-saas-airwatch-tutorial/ic791924.png "nahrát")   
    
    a. Jako **typu adresáře**, vyberte **žádné**.

    b. Vyberte **pomocí SAML pro ověřování**.

    c. tooupload hello stažený certifikát, klikněte na tlačítko **nahrát**.

13. V hello **požadavku** část, proveďte následující kroky hello:
    
    ![Žádosti o](./media/active-directory-saas-airwatch-tutorial/ic791925.png "požadavku")  

    a. Jako **typ vazby žádosti**, vyberte **POST**.

    b. V portálu Azure, na hello hello **nakonfigurovat jednotné přihlašování v Airwatch** dialogové okno stránky, kopie hello **SAML jeden přihlašování adresa URL služby** hodnotu a pak ji vložit do hello **zprostředkovatele Identity Jednotné přihlašování na adresu URL** textové pole.

    c. Jako **NameID formátu**, vyberte **e-mailovou adresu**.

    d. Klikněte na **Uložit**.

14. Klikněte na tlačítko hello **uživatele** kartě znovu.
    
    ![Uživatel](./media/active-directory-saas-airwatch-tutorial/ic791926.png "uživatele")

15. V hello **atribut** část, proveďte následující kroky hello:
    
    ![Atribut](./media/active-directory-saas-airwatch-tutorial/ic791927.png "atribut")

    a. V hello **identifikátor objektu** textovému poli, typ **http://schemas.microsoft.com/identity/claims/objectidentifier**.

    b. V hello **uživatelské jméno** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    c. V hello **zobrazovaný název** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    d. V hello **křestní jméno** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    e. V hello **příjmení** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    f. V hello **e-mailu** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    g. Klikněte na **Uložit**.

<CE>

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z Britta Simon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-airwatch-test-user"></a>Vytvoření zkušebního uživatele AirWatch

Uživatelé toolog tooenable Azure AD v tooAirWatch, se musí být zřízená v tooAirWatch.

* Při zřizování AirWatch, je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour **AirWatch** společnosti lokality jako správce.
2. V navigačním podokně hello na levé straně hello, klikněte na tlačítko **účty**a potom klikněte na **uživatelé**.
   
   ![Uživatelé](./media/active-directory-saas-airwatch-tutorial/ic791929.png "uživatelů")
3. V hello **uživatelé** nabídky, klikněte na tlačítko **zobrazení seznamu**a potom klikněte na **přidat \> přidat uživatele**.
   
   ![Přidat uživatele](./media/active-directory-saas-airwatch-tutorial/ic791930.png "přidat uživatele")
4. Na hello **přidat nebo upravit uživatele** dialogové okno, proveďte následující kroky hello:

   ![Přidat uživatele](./media/active-directory-saas-airwatch-tutorial/ic791931.png "přidat uživatele")   
   1. Typ hello **uživatelské jméno**, **heslo**, **Potvrdit heslo**, **křestní jméno**, **příjmení**,  **E-mailová adresa** platný Azure souvisejícím s účtem služby Active Directory do hello chcete tooprovision textových polí.
   2. Klikněte na **Uložit**.

>[!NOTE]
>Můžete použít všechny ostatní AirWatch uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované AirWatch tooprovision AAD uživatelské účty.
>  

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooAirWatch toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooAirWatch, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **AirWatch**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu. Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_203.png

