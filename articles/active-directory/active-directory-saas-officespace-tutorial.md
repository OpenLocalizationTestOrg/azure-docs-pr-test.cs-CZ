---
title: 'Kurz: Azure Active Directory integrace s OfficeSpace softwarem | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi OfficeSpace softwarem a Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 95d8413f-db98-4e2c-8097-9142ef1af823
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: b53afb648b8a6057c32c782d857e34c06e152c67
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Kurz: Azure Active Directory integrace s OfficeSpace softwaru

V tomto kurzu zjistíte, jak toointegrate OfficeSpace Software s Azure Active Directory (Azure AD).

Integrace OfficeSpace softwaru s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooOfficeSpace softwaru.
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooOfficeSpace softwaru (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s OfficeSpace softwaru, je třeba hello následující položky:

- Předplatné služby Azure AD
- OfficeSpace softwaru jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání softwaru OfficeSpace z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-officespace-software-from-hello-gallery"></a>Přidání softwaru OfficeSpace z Galerie hello
tooconfigure hello integrace OfficeSpace softwaru do služby Azure AD, je nutné tooadd OfficeSpace softwaru hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd OfficeSpace softwaru z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **OfficeSpace softwaru**, vyberte **OfficeSpace softwaru** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![OfficeSpace Software v hello seznamu výsledků](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s OfficeSpace softwarem podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v OfficeSpace softwaru je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v softwaru OfficeSpace musí toobe navázat.

V softwaru OfficeSpace přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s OfficeSpace softwaru, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele softwaru OfficeSpace](#create-a-officespace-software-test-user)**  -toohave protějšek Britta Simon v OfficeSpace Software, který je propojený toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci OfficeSpace softwaru.

**tooconfigure Azure AD jednotné přihlašování s OfficeSpace softwarem, provést hello následující kroky:**

1. V portálu Azure, na hello hello **OfficeSpace softwaru** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_samlbase.png)

3. Na hello **OfficeSpace softwaru domény a adresy URL** část, proveďte následující kroky hello:

    ![OfficeSpace softwaru domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.officespacesoftware.com/users/sign_in/saml`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`<company name>.officespacesoftware.com`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory klientský Software OfficeSpace](mailto:support@officespacesoftware.com) tooget tyto hodnoty. 

4. OfficeSpace softwarová aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu. Nakonfigurujte hello následující deklarace identity pro tuto aplikaci. Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace. Hello následující snímek obrazovky ukazuje příklad pro tento.
    
    ![Konfigurace atributů](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_attribute.png)

5. V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogovém okně, vyberte **user.mail** jako **uživatelský identifikátor** a pro každý řádek ukazuje v tabulce hello níže proveďte hello následující kroky:
    
    | Název atributu | Hodnota atributu |
    | --- | --- |    
    | E-mailu | User.Mail |
    | jméno | User.DisplayName |
    | křestní_jméno | User.givenName |
    | Příjmení | User.Surname |

    a. Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.

    ![Konfigurace přidat ](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_04.png)

    ![Konfigurace atributů](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_05.png)
    
    b. V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.
    
    c. Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.
    
    d. Klikněte na tlačítko **Ok**
 
6. Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnota hello certifikátu.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_certificate.png) 

7. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-officespace-tutorial/tutorial_general_400.png)

8. Na hello **OfficeSpace softwarové konfigurace** klikněte na tlačítko **konfigurace softwaru OfficeSpace** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**

    ![Konfigurace OfficeSpace softwaru](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_configure.png) 

9. V okně prohlížeče jiný web Přihlaste se ke klientovi OfficeSpace softwaru jako správce.

10. Přejděte příliš**nastavení** a klikněte na tlačítko **konektory**.

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_002.png)

11. Klikněte na tlačítko **ověřování SAML**.

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_003.png)

12. V hello **ověřování SAML** část, proveďte následující kroky hello:

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_004.png)

    a. V hello **adresy url odhlašovací zprostředkovatele** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.

    b. V hello **klienta idp cílová adresa url** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.

    c. Vložení hello **kryptografický otisk** hodnotu, která jste zkopírovali z portálu Azure do hello **otisk certifikátu klienta IDP** textové pole. 

    d. Klikněte na tlačítko **uložit nastavení**.


> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-officespace-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-officespace-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-officespace-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-officespace-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-officespace-software-test-user"></a>Vytvoření zkušebního uživatele OfficeSpace softwaru

Hello cílem této části je toocreate uživatel volal Britta Simon v OfficeSpace softwaru. OfficeSpace Software podporuje za běhu zřizování, který je ve výchozím nastavení povolené.

Neexistuje žádná položka akce pro vás v této části. Nového uživatele se vytvoří během pokusu o tooaccess OfficeSpace softwaru, pokud ještě neexistuje.

> [!NOTE]
> Pokud potřebujete toocreate uživatelé ručně, je nutné tooContact [tým podpory OfficeSpace softwaru](mailto:support@officespacesoftware.com).

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooOfficeSpace softwaru Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign Britta Simon tooOfficeSpace softwaru, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **OfficeSpace softwaru**.

    ![Hello odkaz na OfficeSpace Software v seznamu aplikace hello](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello OfficeSpace softwaru dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour OfficeSpace softwarová aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_203.png

