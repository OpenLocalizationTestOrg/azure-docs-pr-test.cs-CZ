---
title: 'Kurz: Azure Active Directory integrace s ArcGIS Online | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ArcGIS Online."
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
ms.openlocfilehash: df72270ca6443b456c079b22425f1660aa522389
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Kurz: Azure Active Directory integrace s ArcGIS Online

V tomto kurzu zjistěte, jak integrovat ArcGIS Online s Azure Active Directory (Azure AD).

Integrace ArcGIS Online s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k ArcGIS Online
- Můžete povolit uživatelům, aby automaticky získat přihlášení k ArcGIS Online (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

To enable single sign-on with ArcGIS Online, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in ArcGIS Online.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s ArcGIS Online, potřebujete následující položky:

- Předplatné služby Azure AD
- ArcGIS Online jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání ArcGIS Online z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-arcgis-online-from-the-gallery"></a>Přidání ArcGIS Online z Galerie
Při konfiguraci integrace služby ArcGIS Online do služby Azure AD, musíte přidat ArcGIS Online z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat ArcGIS Online z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno pro přidání nové aplikace.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **ArcGIS Online**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_search.png)

5. Na panelu výsledků vyberte **ArcGIS Online**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s ArcGIS Online na základě testovací uživatele, nazývá "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ArcGIS Online je pro uživatele ve službě Azure AD. Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v ArcGIS Online.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v ArcGIS Online.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s ArcGIS Online, musíte dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření ArcGIS Online testovacího uživatele](#creating-an-arcgis-online-test-user)**  – Pokud chcete mít protějšek Britta Simon v ArcGIS Online propojeném s Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci ArcGIS Online.

**Ke konfiguraci Azure AD jednotné přihlašování s ArcGIS Online, proveďte následující kroky:**

1. Na portálu Azure na **ArcGIS Online** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

3. Na **ArcGIS Online domény a adresy URL** část, proveďte následující krok:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_url.png)

    V **přihlašovací adresa URL** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://<company>.maps.arcgis.com`

    > [!NOTE] 
    > Tato hodnota není reálné. Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování. Obraťte se na [tým podpory Online klienta ArcGIS](http://support.esri.com/) získat tuto hodnotu. 

4. Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.

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

10. Na **nastavit zprostředkovatele Identity** konfigurace proveďte následující kroky:
   
    ![Nastavení zprostředkovatele Identity](./media/active-directory-saas-arcgis-tutorial/ic784745.png "nastavit zprostředkovatele Identity")
   
    a. V **název** textovému poli, zadejte název vaší organizace.

    b. Pro **Metadata pro zprostředkovatele Identity Enterprise budou předávána prostřednictvím**, vyberte **A soubor**.

    c. Chcete-li nahrát soubor stažený metadata, klikněte na tlačítko **zvolte soubor**.

    d. Klikněte na tlačítko **zprostředkovatele IDENTITY sadu**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_01.png) 

2. Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_02.png) 

3. V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z Britta Simon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-arcgis-online-test-user"></a>Vytvoření ArcGIS Online testovacího uživatele

Pokud chcete povolit uživatelům Azure AD přihlášení do ArcGIS Online, musí být zřízená do ArcGIS Online.  
V případě ArcGIS Online zřizování je ruční úloha.

**K poskytnutí uživatelského účtu, proveďte následující kroky:**

1. Přihlaste se k vaší **ArcGIS** klienta.

2. Klikněte na tlačítko **POZVAT členy**.
   
    ![Pozvěte členy](./media/active-directory-saas-arcgis-tutorial/ic784747.png "pozvat členy")

3. Vyberte **přidat členy automaticky bez odeslání e-mailu**a potom klikněte na **Další**.
   
    ![Automaticky přidat členy](./media/active-directory-saas-arcgis-tutorial/ic784748.png "automaticky přidat členy")

4. Na **členy** dialogové okno stránky, proveďte následující kroky:
   
     ![Přidat a zkontrolovat](./media/active-directory-saas-arcgis-tutorial/ic784749.png "přidat a zkontrolujte")
    
     a. Zadejte **e-mailu**, **křestní jméno**, a **příjmení** platného účtu AAD chcete zřídit.
  
     b. Klikněte na tlačítko **přidat a ZKONTROLUJTE**.
5. Zkontrolujte data, která jste zadali a pak klikněte na tlačítko **přidat členy**.
   
    ![Přidat člena](./media/active-directory-saas-arcgis-tutorial/ic784750.png "přidat člena")
        
    > [!NOTE]
    > Držitel účtu Azure Active Directory bude dostávat e-mailu a postupujte podle odkaz potvrďte svůj účet, pak se změní na aktivní.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon tak, že udělíte přístup k ArcGIS Online používat Azure jednotné přihlašování.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon ArcGIS Online, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **ArcGIS Online**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici ArcGIS Online na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci ArcGIS Online.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
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

