---
title: "Kurz: Azure Active Directory integrace s vytvářením sestav cena Predictix | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a vytváření sestav Predictix ceny."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 691d0c43-3aa1-4220-9e46-e7a88db234ad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: c2ba85f613f5a71de72278a0d1916c135b835b60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-price-reporting"></a>Kurz: Azure Active Directory integrace s vytvářením sestav Predictix cena

V tomto kurzu zjistíte, jak toointegrate Predictix cena vytváření sestav v Azure Active Directory (Azure AD).

Integrace Predictix cena Reporting s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooPredictix cena Reporting.
- Vaši uživatelé tooautomatically get přihlášeného tooPredictix cena Reporting (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s vytvářením sestav cena Predictix tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Vytváření sestav cena Predictix jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Predictix cena Reporting z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-predictix-price-reporting-from-hello-gallery"></a>Přidání Predictix cena Reporting z Galerie hello
tooconfigure hello integrace Predictix cena Reporting do Azure AD, je nutné tooadd Predictix cena Reporting hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Predictix cena generování sestav z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **Predictix cena Reporting**, vyberte **Predictix cena Reporting** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Predictix cena vytváření sestav v seznamu výsledků hello](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s vytvářením sestav cena Predictix podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v sestavách ceníku Predictix je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v sestavách ceníku Predictix musí toobe navázat.

V Predictix cena vytváření sestav, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testování Azure AD jednotné přihlašování s vytvářením sestav cena Predictix, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Reporting cena Predictix](#create-a-predictix-price-reporting-test-user)**  -toohave protějšek Britta Simon v Predictix cena Reporting, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Predictix cena Reporting.

**tooconfigure Azure AD jednotné přihlašování s Predictix cena vytváření sestav, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Predictix cena Reporting** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_samlbase.png)

3. Na hello **Predictix cena Reporting domény a adresy URL** část, proveďte následující kroky hello:

    ![Predictix cena Reporting domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_url.png)

    a. V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname-pricing>.predictix.com/sso/request`

    b. V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:
    | |
    |--|
    | `https://<companyname-pricing>.predictix.com` |
    | `https://<companyname-pricing>.dev.predictix.com` |

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory Predictix cena Reporting klienta](http://www.infor.com/company/customer-center/) tooget tyto hodnoty. 
 
4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_400.png)

6. Na hello **Predictix cena konfigurace služby Reporting** klikněte na tlačítko **nakonfigurovat generování sestav cena Predictix** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurace vytváření sestav Predictix cena](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_configure.png) 

7. tooconfigure jednotného přihlašování na **Predictix cena Reporting** straně, je nutné stáhnout hello toosend **certifikátu (Base64)**, **Sign-Out adresu URL, SAML Entity ID a SAML jednotné přihlašování Adresa URL služby** příliš[Predictix cena Reporting tým podpory](http://www.infor.com/company/customer-center/). Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-predictix-price-reporting-test-user"></a>Vytvoření zkušebního uživatele Reporting Predictix cena

V této části vytvoříte uživatele volal Britta Simon v Predictix cena Reporting. Práce s [Predictix cena Reporting tým podpory](http://www.infor.com/company/customer-center/) tooadd hello uživatelé v platformě Predictix cena Reporting hello.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooPredictix cena Reporting Britta Simon toouse Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign tooPredictix Britta Simon cena vytváření sestav, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Predictix cena Reporting**.

    ![v seznamu aplikace hello odkaz Predictix cena Reporting Hello](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello Predictix cena Reporting dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Predictix cena Reporting aplikace.

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_203.png

