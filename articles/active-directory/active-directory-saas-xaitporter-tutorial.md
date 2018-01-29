---
title: 'Kurz: Azure Active Directory integrace s XaitPorter | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a XaitPorter."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d33c7cb7-0550-425b-882a-619a713a71b7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2017
ms.author: jeedes
ms.openlocfilehash: 2012d990f7cdcb8c12da5f16db518b261b06a5b7
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-xaitporter"></a>Kurz: Azure Active Directory integrace s XaitPorter

V tomto kurzu zjistěte, jak integrovat XaitPorter s Azure Active Directory (Azure AD).

Integrace XaitPorter s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup k XaitPorter.
- Můžete povolit uživatelům, aby automaticky získat přihlášení k XaitPorter (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s XaitPorter, potřebujete následující položky:

- Předplatné služby Azure AD
- XaitPorter jednotné přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání XaitPorter z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-xaitporter-from-the-gallery"></a>Přidání XaitPorter z Galerie
Při konfiguraci integrace XaitPorter do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS XaitPorter z galerie.

**Pokud chcete přidat XaitPorter z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **XaitPorter**, vyberte **XaitPorter** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.

    ![XaitPorter v seznamu výsledků](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s XaitPorter podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v XaitPorter je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v XaitPorter musí navázat.

V XaitPorter, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s XaitPorter, je třeba dokončit následující stavební bloky:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele XaitPorter](#create-a-xaitporter-test-user)**  – Pokud chcete mít protějšek Britta Simon v XaitPorter propojeném s Azure AD reprezentace daného uživatele.
4. **[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci XaitPorter.

**Ke konfiguraci Azure AD jednotné přihlašování s XaitPorter, proveďte následující kroky:**

1. Na portálu Azure na **XaitPorter** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_samlbase.png)

3. Na **XaitPorter domény a adresy URL** část, proveďte následující kroky:

    ![XaitPorter domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_url.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.xaitporter.com/saml/login`

    b. V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.xaitporter.com`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory XaitPorter klienta](https://www.xait.com/support/) k získání těchto hodnot. 

4. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-xaitporter-tutorial/tutorial_general_400.png)

5. Ke generování **Metadata** adresu url, proveďte následující kroky:

    a. Klikněte na tlačítko **registrace aplikace**.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_appregistrations.png)
   
    b. Klikněte na tlačítko **koncové body** otevřete **koncové body** dialogové okno.  
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_endpointicon.png)

    c. Klikněte na tlačítko Kopírovat kopírování **dokument FEDERAČNÍCH METADAT** adresy url a vložte do poznámkového bloku.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_endpoint.png)
     
    d. Nyní přejděte na stránku vlastností **XaitPorter** a zkopírujte **Id aplikace** pomocí **kopie** tlačítko a vložte do poznámkového bloku.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_appid.png)

    e. Vygenerovat **adresu URL metadat** pomocí následujícího vzorce:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`

6. Zadejte **IP adresu** nebo **adresu URL metadat** k [tým podpory SmartRecruiters](https://www.smartrecruiters.com/about-us/contact-us/)tak, aby XaitPorter můžete zajistit, že IP adresa je dostupný z vašeho XaitPorter Konfigurace seznamu povolených IP adres na jejich stranu instance. 

7. V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti XaitPorter jako správce.

8. Klikněte na **správce**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xaitporter-tutorial/user1.png)

9. Vyberte **správě jednotného přihlašování** z **nastavení systému** rozevíracího seznamu.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xaitporter-tutorial/user2.png)

10. V **SPRAVOVAT jeden přihlašování** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-xaitporter-tutorial/user3.png)

    a. Vyberte **povolit jednotné přihlašování, ověřování**.

    b. V **nastavení zprostředkovatele Identity** textovému poli, vložte **adresu URL metadat** který jste zkopírovali z Azure a klikněte na tlačítko **načíst**.

    c. Vyberte **povolit automatické vytvoření uživatelů**.

    d. Klikněte na **OK**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

   ![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-xaitporter-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-xaitporter-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/active-directory-saas-xaitporter-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/active-directory-saas-xaitporter-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-xaitporter-test-user"></a>Vytvoření zkušebního uživatele XaitPorter

V této části vytvoříte volal Britta Simon v XaitPorter uživatele. Práce s [tým podpory XaitPorter klienta](https://www.xait.com/support/) přidat uživatele do XaitPorter platformy. Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování. 

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu XaitPorter.

![Přiřadit role uživatele][200] 

**Pokud chcete přiřadit Britta Simon XaitPorter, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **XaitPorter**.

    ![V seznamu aplikací na XaitPorter odkaz](./media/active-directory-saas-xaitporter-tutorial/tutorial_xaitporter_app.png)  

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici XaitPorter na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci XaitPorter.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-xaitporter-tutorial/tutorial_general_203.png

