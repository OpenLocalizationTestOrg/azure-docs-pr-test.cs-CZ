---
title: 'Kurz: Azure Active Directory integrace s Citrix ShareFile | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Citrix ShareFile."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e14fc310-bac4-4f09-99ef-87e5c77288b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2017
ms.author: jeedes
ms.openlocfilehash: d7eaf140e56c40f9f621062849dd8558588ffd1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Kurz: Azure Active Directory integrace s Citrix ShareFile

V tomto kurzu zjistíte, jak toointegrate Citrix ShareFile službou Azure Active Directory (Azure AD).

Integrace Citrix ShareFile s Azure AD poskytuje hello následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup tooCitrix ShareFile.
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooCitrix ShareFile (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s Citrix ShareFile, budete potřebovat hello následující položky:

- Předplatné služby Azure AD
- Citrix ShareFile jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidat Citrix ShareFile z Galerie hello
2. Konfigurace a otestování Azure AD jednotné přihlašování

## <a name="add-citrix-sharefile-from-hello-gallery"></a>Přidat Citrix ShareFile z Galerie hello
tooconfigure hello integrace Citrix ShareFile do Azure AD, je nutné tooadd Citrix ShareFile hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Citrix ShareFile z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![tlačítko Azure Active Directory Hello][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![okno aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **Citrix ShareFile**, vyberte **Citrix ShareFile** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Citrix ShareFile v hello seznamu výsledků](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s Citrix ShareFile podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Citrix ShareFile je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Citrix ShareFile musí toobe navázat.

V systému Citrix ShareFile přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Citrix ShareFile, budete potřebovat následující stavební bloky hello toocomplete:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Citrix ShareFile](#create-a-citrix-sharefile-test-user)**  -toohave protějšek Britta Simon v Citrix ShareFile, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Citrix ShareFile.

**tooconfigure Azure AD jednotné přihlašování s Citrix ShareFile, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Citrix ShareFile** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_samlbase.png)

3. Na hello **Citrix ShareFile domény a adresy URL** část, proveďte následující kroky hello:

    ![Citrix ShareFile domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_url.png)
    
    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.sharefile.com/saml/login`

    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory Citrix ShareFile klienta](https://www.citrix.co.in/products/sharefile/support.html) tooget tuto hodnotu. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-sharefile-tutorial/tutorial_general_400.png)

6. Na hello **konfigurace systému Citrix ShareFile** klikněte na tlačítko **konfigurace Citrix ShareFile** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Konfigurace systému Citrix ShareFile](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_configure.png) 

7. V okně prohlížeče jiný web, přihlaste se k vaší **Citrix ShareFile** společnosti lokality jako správce.

8. V panelu nástrojů hello hello nahoře, klikněte na **správce**.

9. V levém navigačním podokně hello vyberte **nakonfigurovat jednotné přihlašování**.
   
    ![Účet správy](./media/active-directory-saas-sharefile-tutorial/ic773627.png "účet správy")

10. Na hello **jednotného přihlašování nebo konfigurace SAML 2.0** dialogové okno stránky v rámci **základní nastavení**, proveďte následující kroky hello:
   
    ![Jednotné přihlašování](./media/active-directory-saas-sharefile-tutorial/ic773628.png "jednotného přihlašování")
   
    a. Klikněte na tlačítko **povolit SAML**.
    
    b. V **vaše Vystavitel deklarací identity nebo Entity ID** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.

    c. Klikněte na tlačítko **změnu** další toohello **certifikát X.509** pole a pak nahrajte certifikát hello jste si stáhli z portálu Azure hello.
    
    d. V **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.
    
    e. V **adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.

11. Klikněte na tlačítko **Uložit** na hello Citrix ShareFile portálu pro správu.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

   ![Vytvořit testovací uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-sharefile-tutorial/create_aaduser_01.png)

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-sharefile-tutorial/create_aaduser_02.png)

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.

    ![tlačítko Přidat Hello](./media/active-directory-saas-sharefile-tutorial/create_aaduser_03.png)

4. V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-sharefile-tutorial/create_aaduser_04.png)

    a. V hello **název** zadejte **BrittaSimon**.

    b. V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.

    c. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-citrix-sharefile-test-user"></a>Vytvoření zkušebního uživatele Citrix ShareFile

V pořadí tooenable Azure AD Uživatelé toolog do Citrix ShareFile musí být zřízená do Citrix ShareFile. V případě hello Citrix ShareFile zřizování je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour **Citrix ShareFile** klienta.

2. Klikněte na tlačítko **Správa uživatelů \> spravovat uživatelé domovské \> + vytvořit zaměstnanec**.
   
   ![Vytvoření zaměstnance](./media/active-directory-saas-sharefile-tutorial/IC781050.png "vytvoření zaměstnance")

3. Na hello **základní informace** část, proveďte následující kroky:
   
   ![Základní informace](./media/active-directory-saas-sharefile-tutorial/IC799951.png "základní informace")
   
   a. V hello **e-mailovou adresu** textovému poli, typ hello e-mailovou adresu Britta Simon jako  **brittasimon@contoso.com** .
   
   b. V hello **křestní jméno** textovému poli, typ **křestní jméno** uživatele jako **Britta**.
   
   c. V hello **příjmení** textovému poli, typ **příjmení** uživatele jako **Simon**.

4. Klikněte na tlačítko **přidat uživatele**.
  
   >[!NOTE]
   >Držitel účtu Azure AD Hello bude dostávat e-mailu a postupujte podle tooconfirm odkaz svého účtu před stane aktivní. Můžete použít všechny ostatní Citrix ShareFile uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Citrix ShareFile tooprovision uživatelské účty Azure AD.

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooCitrix ShareFile toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit role uživatele hello][200] 

**tooassign Britta Simon tooCitrix ShareFile, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Citrix ShareFile**.

    ![Hello Citrix ShareFile odkaz v seznamu aplikace hello](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_app.png)  

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Podokno Přidat přidružení Hello][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello Citrix ShareFile dlaždici v hello přístupového panelu, měli byste obdržet tooyour automaticky přihlášeného Citrix ShareFile aplikace.
Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_203.png

