---
title: 'Kurz: Azure Active Directory integrace s ArcGIS Online | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ArcGIS Online."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: f3dd55d798cf3256fb2758e011f33946baa405ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Kurz: Azure Active Directory integrace s ArcGIS Online

V tomto kurzu zjistíte, jak toointegrate ArcGIS Online se službou Azure Active Directory (Azure AD).

Integrace ArcGIS Online s Azure AD poskytuje hello následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup tooArcGIS Online
- Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooArcGIS Online (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure

Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

tooenable single sign-on with ArcGIS Online, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in ArcGIS Online.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Požadavky

tooconfigure integrace Azure AD s ArcGIS Online, je třeba hello následující položky:

- Předplatné služby Azure AD
- ArcGIS Online jednotného přihlašování povolené předplatné

> [!NOTE]
> tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání ArcGIS Online z Galerie hello
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-arcgis-online-from-hello-gallery"></a>Přidání ArcGIS Online z Galerie hello
tooconfigure hello integrace ArcGIS Online do služby Azure AD, je nutné tooadd ArcGIS Online hello Galerie tooyour seznamu spravovaných aplikací SaaS.

**tooadd ArcGIS Online z Galerie hello, proveďte následující kroky hello:**

1. V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte příliš**podnikové aplikace, které**. Potom přejděte příliš**všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **novou aplikaci** tlačítka v horní části hello hello dialogové okno tooadd nové aplikace.

    ![Aplikace][3]

4. Hello vyhledávacího pole zadejte **ArcGIS Online**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_search.png)

5. Na panelu výsledků hello vyberte **ArcGIS Online**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s ArcGIS Online na základě testovací uživatele, nazývá "Britta Simon."

Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v ArcGIS Online je tooa uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v ArcGIS Online musí toobe navázat.

Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v ArcGIS Online.

tooconfigure a testu Azure AD jednotné přihlašování s ArcGIS Online, je třeba toocomplete hello stavební bloky následující:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření ArcGIS Online testovacího uživatele](#creating-an-arcgis-online-test-user)**  -toohave protějšek Britta Simon v ArcGIS Online, které je propojené toohello Azure AD reprezentace uživatele.
4. **[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci ArcGIS Online.

**tooconfigure Azure AD jednotné přihlašování s ArcGIS Online, proveďte následující kroky hello:**

1. V portálu Azure, na hello hello **ArcGIS Online** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

3. Na hello **ArcGIS Online domény a adresy URL** část, proveďte následující krok hello:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_url.png)

    V hello **přihlašovací adresa URL** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://<company>.maps.arcgis.com`

    > [!NOTE] 
    > Tato hodnota není skutečné hello. Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory Online klienta ArcGIS](http://support.esri.com/) tooget tuto hodnotu. 

4. Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_general_400.png)

6. V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti ArcGIS.

7. Klikněte na tlačítko **upravovat nastavení**.

    ![Upravit nastavení](./media/active-directory-saas-arcgis-tutorial/ic784742.png "upravit nastavení")

8. Klikněte na tlačítko **zabezpečení**.

    ![Zabezpečení](./media/active-directory-saas-arcgis-tutorial/ic784743.png "zabezpečení")

9. V části **přihlášení Enterprise**, klikněte na tlačítko **nastavit zprostředkovatele IDENTITY**.

    ![Přihlášení Enterprise](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Enterprise přihlášení")

10. Na hello **nastavit zprostředkovatele Identity** konfigurace proveďte hello následující kroky:
   
    ![Nastavení zprostředkovatele Identity](./media/active-directory-saas-arcgis-tutorial/ic784745.png "nastavit zprostředkovatele Identity")
   
    a. V hello **název** textovému poli, zadejte název vaší organizace.

    b. Pro **Metadata pro hello zprostředkovatele Identity Enterprise budou předávána prostřednictvím**, vyberte **A soubor**.

    c. tooupload váš soubor stažený metadata, klikněte na tlačítko **zvolte soubor**.

    d. Klikněte na tlačítko **zprostředkovatele IDENTITY sadu**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!  Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello. Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.

![Vytvořit uživatele Azure AD][100]

**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**

1. V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_01.png) 

2. Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_02.png) 

3. V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_03.png) 

4. Na hello **uživatele** dialogové okno proveďte hello následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_04.png) 

    a. V hello **název** textovému poli, typ **BrittaSimon**.

    b. V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z Britta Simon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-arcgis-online-test-user"></a>Vytvoření ArcGIS Online testovacího uživatele

V pořadí tooenable Azure AD Uživatelé toolog do ArcGIS Online musí být zřízená do ArcGIS Online.  
V případě hello ArcGIS online zřizování je ruční úloha.

**tooprovision uživatelský účet, proveďte následující kroky hello:**

1. Přihlaste se tooyour **ArcGIS** klienta.

2. Klikněte na tlačítko **POZVAT členy**.
   
    ![Pozvěte členy](./media/active-directory-saas-arcgis-tutorial/ic784747.png "pozvat členy")

3. Vyberte **přidat členy automaticky bez odeslání e-mailu**a potom klikněte na **Další**.
   
    ![Automaticky přidat členy](./media/active-directory-saas-arcgis-tutorial/ic784748.png "automaticky přidat členy")

4. Na hello **členy** dialogové okno proveďte hello následující kroky:
   
     ![Přidat a zkontrolovat](./media/active-directory-saas-arcgis-tutorial/ic784749.png "přidat a zkontrolujte")
    
     a. Zadejte hello **e-mailu**, **křestní jméno**, a **příjmení** platného účtu AAD chcete tooprovision.
  
     b. Klikněte na tlačítko **přidat a ZKONTROLUJTE**.
5. Zkontrolujte data hello jste zadali a pak klikněte na tlačítko **přidat členy**.
   
    ![Přidat člena](./media/active-directory-saas-arcgis-tutorial/ic784750.png "přidat člena")
        
    > [!NOTE]
    > Držitel účtu Azure Active Directory Hello bude dostávat e-mailu a postupujte podle tooconfirm odkaz svého účtu před stane aktivní.

### <a name="assigning-hello-azure-ad-test-user"></a>Přiřazení hello Azure AD testovacího uživatele

V této části povolíte tak, že udělíte přístup tooArcGIS Online toouse Britta Simon Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**tooassign Britta Simon tooArcGIS Online, proveďte následující kroky hello:**

1. V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **ArcGIS Online**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_app.png) 

3. V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.

Po kliknutí na tlačítko hello ArcGIS Online dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour ArcGIS Online aplikace.
Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_203.png

