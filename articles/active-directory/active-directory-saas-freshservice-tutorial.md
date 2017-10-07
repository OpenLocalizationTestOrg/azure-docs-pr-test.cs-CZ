---
title: 'Kurz: Azure Active Directory integrace s Freshservice | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Freshservice."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: d73624b87d058f66885ae72fda69a0aacc89c1ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Kurz: Azure Active Directory integrace s Freshservice

V tomto kurzu zjistíte, jak toointegrate Freshservice s Azure Active Directory (Azure AD).

Integrace Freshservice s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooFreshservice
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooFreshservice (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Freshservice tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Freshservice jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Freshservice z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-freshservice-from-hello-gallery"></a>Přidání Freshservice z Galerie hello
tooconfigure hello integrace Freshservice do Azure AD, je nutné tooadd Freshservice hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Freshservice z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Freshservice**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_search.png)

5. Na panelu výsledků hello vyberte **Freshservice**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Freshservice podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Freshservice je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Freshservice musí toobe navázat.

V Freshservice, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Freshservice, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Freshservice](#creating-a-freshservice-test-user)**  -toohave protějšek Britta Simon v Freshservice, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Freshservice.

**tooconfigure Azure AD jednotné přihlašování s Freshservice, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Freshservice** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_samlbase.png)

3. Na hello **Freshservice domény a adresy URL** část, proveďte následující kroky hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<democompany>.freshservice.com`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<democompany>.freshservice.com`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory Freshservice klienta](https://support.freshservice.com/) tooget tyto hodnoty. 
 
4. Na hello **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnota certifikátu.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_general_400.png)

6. Na hello **Freshservice konfigurace** klikněte na tlačítko **konfigurace Freshservice** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_configure.png) 

7. V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti Freshservice tooyour.

8. V nabídce hello hello nahoře, klikněte na tlačítko **správce**.
   
    ![Správce](./media/active-directory-saas-freshservice-tutorial/ic790814.png "správce")

9. V hello **zákaznický portál**, klikněte na tlačítko **zabezpečení**.
   
    ![Zabezpečení](./media/active-directory-saas-freshservice-tutorial/ic790815.png "zabezpečení")

10. V hello **zabezpečení** část, proveďte následující kroky hello:
   
    ![Jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/ic790816.png "jednotné přihlašování")
   
    a. Přepínač **jednotné přihlašování**.

    b. Vyberte **jednotné přihlašování SAML**.

    c. V hello **SAML přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.

    d. V hello **adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.

    e. V **otisků certifikátu zabezpečení** textovému poli, vložte hello **kryptografický OTISK** hodnota certifikát, který jste zkopírovali z portálu Azure.

    f. Klikněte na tlačítko **uložit**
   
> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-freshservice-test-user"></a>Vytvoření zkušebního uživatele Freshservice

Uživatelé toolog tooenable Azure AD v tooFreshService, se musí být zřízená do FreshService. V případě hello FreshService zřizování je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour **FreshService** společnosti lokality jako správce.

2. V nabídce hello hello nahoře, klikněte na tlačítko **správce**.
   
    ![Správce](./media/active-directory-saas-freshservice-tutorial/ic790814.png "správce")

3. V hello **Správa uživatelů** klikněte na tlačítko **žadatelů o**.
   
    ![Žadatelů o](./media/active-directory-saas-freshservice-tutorial/ic790818.png "žadatelů o")

4. Klikněte na tlačítko **nový žadatel**.
   
    ![Nové žadatelů o](./media/active-directory-saas-freshservice-tutorial/ic790819.png "nové žadatelů o")

5. V hello **nový žadatel** část, proveďte následující kroky hello:
   
    ![Nový žadatel](./media/active-directory-saas-freshservice-tutorial/ic790820.png "nový žadatel")   

    a. Zadejte hello **křestní jméno** a **e-mailu** atributy platný účet služby Azure Active Directory do hello chcete tooprovision související textových polí.

    b. Klikněte na **Uložit**.
   
    >[!NOTE]
    >Držitel účtu Azure Active Directory Hello získá e-mailu, včetně účet odkaz tooconfirm hello předtím, než se stane aktivní
    >  

>[!NOTE]
>Můžete použít všechny ostatní FreshService uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované FreshService tooprovision AAD uživatelské účty.
>  

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooFreshservice, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Freshservice**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.

Když kliknete na dlaždici Freshservice hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Freshservice aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_203.png

