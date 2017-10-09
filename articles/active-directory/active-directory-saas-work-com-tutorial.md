---
title: 'Kurz: Azure Active Directory integrace s Work.com | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Work.com."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: dcdc51c884abd78c945b649de99f942d32373cf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>Kurz: Azure Active Directory integrace s Work.com

V tomto kurzu zjistíte, jak toointegrate Work.com s Azure Active Directory (Azure AD).

Integrace Work.com s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooWork.com
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWork.com (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Integrace služby Azure AD s Work.com tooconfigure, je třeba hello následující položky:

- Předplatné služby Azure AD
- Work.com jednotné přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidat Work.com z Galerie hello
2. Konfigurace a otestování Azure AD jednotné přihlašování

## <a name="add-workcom-from-hello-gallery"></a>Přidat Work.com z Galerie hello
tooconfigure hello integrace Work.com do Azure AD, je nutné tooadd Work.com hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd Work.com z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **Work.com**, vyberte **Work.com** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.

    ![Přidat z Galerie](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Work.com podle testovacího uživatele názvem "Britta Simon".

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Work.com je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Work.com musí toobe navázat.

V Work.com, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.

tooconfigure a testu Azure AD jednotné přihlašování s Work.com, potřebujete následující stavební bloky hello toocomplete:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele Work.com](#create-a-workcom-test-user)**  -toohave protějšek Britta Simon v Work.com, která je propojená toohello Azure AD reprezentace uživatele.
4. **[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Work.com.

>[!NOTE]
>tooconfigure jednotné přihlašování, musíte toosetup vlastní název domény Work.com ještě. Je třeba toodefine alespoň domény název, testovací název vaší domény a nasadíte ho tooyour celé organizace.

**tooconfigure Azure AD jednotné přihlašování s Work.com, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **Work.com** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Na základě SAML přihlášení](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_samlbase.png)

3. Na hello **Work.com domény a adresy URL** část, proveďte následující hello:

    ![Část Work.com domény a adresy URL](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_url.png)

    V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`http://<companyname>.my.salesforce.com`

    > [!NOTE] 
    > Tato hodnota není skutečné. Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory Work.com klienta](https://help.salesforce.com/articleView?id=000159855&type=3) tooget tuto hodnotu. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.

    ![Certifikát pro podpis SAML části](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Tlačítko Uložit](./media/active-directory-saas-work-com-tutorial/tutorial_general_400.png)

6. Na hello **Work.com konfigurace** klikněte na tlačítko **konfigurace Work.com** tooopen **konfigurovat přihlášení** okno. Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**

    ![Work.com konfigurační oddíl](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_configure.png) 
7. Přihlaste se tooyour Work.com klienta jako správce.

8. Přejděte příliš**instalační program**.
   
    ![Instalační program](./media/active-directory-saas-work-com-tutorial/ic794108.png "instalační program")

9. V levém navigačním podokně hello v hello **Správa** klikněte na tlačítko **Správa domén** tooexpand hello související části a pak klikněte na **Moje domény** tooopen hello  **Moje doména** stránky. 
   
    ![Moje doména](./media/active-directory-saas-work-com-tutorial/ic767825.png "Moje doména")

10. tooverify, který vaše doména byla nastavena správně, ujistěte se, že je v "**krok 4 nasazené tooUsers**" a zkontrolovat vaše "**Moje nastavení domény**".
   
    ![Domény nasazen tooUser](./media/active-directory-saas-work-com-tutorial/ic784377.png "tooUser nasazené v doméně")

11. Přihlaste se tooyour Work.com klienta.

12. Přejděte příliš**instalační program**.
    
    ![Instalační program](./media/active-directory-saas-work-com-tutorial/ic794108.png "instalační program")

13. Rozbalte hello **ovládacích prvků zabezpečení** nabídce a pak klikněte na tlačítko **nastavení jednotného přihlašování**.
    
    ![Jednotné přihlašování v nastavení](./media/active-directory-saas-work-com-tutorial/ic794113.png "jednotné přihlašování v nastavení")

14. Na hello **nastavení jednotného přihlašování** dialogové okno proveďte hello následující kroky:
    
    ![Povolit SAML](./media/active-directory-saas-work-com-tutorial/ic781026.png "povoleno SAML")
    
    a. Vyberte **povoleno SAML**.
    
    b. Klikněte na možnost **Nové**.

15. V hello **SAML jeden nastavení přihlášení** část, proveďte následující kroky hello:
    
    ![SAML jeden přihlašování nastavení](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML jeden přihlašování nastavení")
    
    a. V hello **název** textovému poli, zadejte název pro svou konfiguraci.  
       
    > [!NOTE]
    > Poskytuje hodnotu pro **název** automaticky vyplnit hello **název rozhraní API** textové pole.
    
    b. V **vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.
    
    c. Klikněte na certifikát tooupload hello stáhli z portálu Azure, **Procházet**.
    
    d. V hello **Entity Id** textovému poli, typ `https://salesforce-work.com`.
    
    e. Jako **typ Identity SAML**, vyberte **kontrolní výraz obsahuje hello federace z objektu uživatele hello**.
    
    f. Jako **umístění Identity SAML**, vyberte **identita je v elementu NameIdentfier hello hello subjektu příkaz**.
    
    g. V **adresu URL pro přihlášení zprostředkovatele Identity** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.

    h. V **adresa URL odhlašovací zprostředkovatele Identity** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.
    
    i. Jako **zprostředkovatele iniciované žádosti vazby služby**, vyberte **HTTP Post**.
    
    j. Klikněte na **Uložit**.

16. Portál classic Work.com, na levém navigačním podokně hello, klikněte na tlačítko **Správa domén** tooexpand hello související části a pak klikněte na **Moje domény** tooopen hello **Moje domény**stránky. 
    
    ![Moje doména](./media/active-directory-saas-work-com-tutorial/ic794115.png "Moje doména")

17. Na hello **Moje domény** stránku hello **Branding přihlašovací stránky** klikněte na tlačítko **upravit**.
    
    ![Branding přihlašovací stránky](./media/active-directory-saas-work-com-tutorial/ic767826.png "Branding přihlašovací stránky")

14. Na hello **Branding přihlašovací stránky** stránku hello **ověřovací služby** část, hello název vaší **nastavení jednotného přihlašování SAML** se zobrazí. Vyberte ji a pak klikněte na tlačítko **Uložit**.
    
    ![Branding přihlašovací stránky](./media/active-directory-saas-work-com-tutorial/ic784366.png "Branding přihlašovací stránky")

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-work-com-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Uživatelé a skupiny -> všichni uživatelé](./media/active-directory-saas-work-com-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.
 
    ![Přidat](./media/active-directory-saas-work-com-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Dialogové okno stránka uživatel](./media/active-directory-saas-work-com-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-workcom-test-user"></a>Vytvoření zkušebního uživatele Work.com
Pro Azure Active Directory uživatelům toobe možné toosign v musí být zřízená tooWork.com. V případě hello Work.com zřizování je ruční úloha.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure zřizování uživatelů, proveďte následující kroky hello:
1. Přihlaste se na tooyour Work.com společnosti lokality jako správce.

2. Přejděte příliš**instalační program**.
   
    ![Instalační program](./media/active-directory-saas-work-com-tutorial/IC794108.png "instalační program")
3. Přejděte příliš**spravovat uživatele \> uživatelé**.
   
    ![Správa uživatelů](./media/active-directory-saas-work-com-tutorial/IC784369.png "Správa uživatelů")

4. Klikněte na tlačítko **nového uživatele**.
   
    ![Všichni uživatelé](./media/active-directory-saas-work-com-tutorial/IC794117.png "všichni uživatelé")

5. V hello uživatele upravte část, provádět hello následující kroky v atributech platný Azure AD účtu chcete tooprovision do hello související textových polí:
   
    ![Úprava uživatele](./media/active-directory-saas-work-com-tutorial/ic794118.png "Úprava uživatele")
   
    a. V hello **křestní jméno** textovému poli, typ hello **křestní jméno** hello uživatele **Britta**.
    
    b. V hello **příjmení** textovému poli, typ hello **příjmení** hello uživatele **Simon**.
    
    c. V hello **Alias** textovému poli, typ hello **název** hello uživatele **BrittaS**.
    
    d. V hello **e-mailu** textovému poli, typ hello **e-mailová adresa** uživatele  **Brittasimon@contoso.com** .
    
    e. V hello **uživatelské jméno** textové pole, zadejte uživatelské jméno uživatele jako  **Brittasimon@contoso.com** .
    
    f. V hello **Přezdívka** textovému poli, zadejte **Přezdívka** uživatele **Simon**.
    
    g. Vyberte **Role**, **uživatelské licence pro**, a **profil**.
    
    h. Klikněte na **Uložit**.  
      
    > [!NOTE]
    > Držitel účtu Azure AD Hello dostane e-mailu, včetně účet odkaz tooconfirm hello předtím, než se stane aktivní.
    > 
    > 

### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooWork.com toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooWork.com, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **Work.com**.

    ![Work.com v seznamu aplikace](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Když kliknete na dlaždici Work.com hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Work.com aplikace.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_203.png

