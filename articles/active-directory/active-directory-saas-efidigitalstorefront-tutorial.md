---
title: "Kurz: Azure Active Directory integrace s rozhraním EFI digitální výkladní skříň | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a digitální výkladní skříň EFI."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 33c148fc-d490-4bb9-90c1-d5933679ce4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 38d24096977b093ba5b1af2258618b3cdb783e77
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-efi-digital-storefront"></a>Kurz: Azure Active Directory integrace s rozhraním EFI digitální výkladní skříň

V tomto kurzu zjistěte, jak integrovat EFI digitální výkladní skříň s Azure Active Directory (Azure AD).

Integrace rozhraní EFI digitální výkladní skříň s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup k digitální výkladní skříň EFI.
- Můžete povolit uživatelům, aby automaticky získat přihlášení k digitální výkladní skříň rozhraní EFI (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s rozhraním EFI digitální výkladní skříň, potřebujete následující položky:

- Předplatné služby Azure AD
- Rozhraní EFI digitální výkladní skříň jednotné přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání rozhraní EFI digitální výkladní skříň z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-efi-digital-storefront-from-the-gallery"></a>Přidání rozhraní EFI digitální výkladní skříň z Galerie
Při konfiguraci integrace digitální výkladní skříň EFI do služby Azure AD potřebujete přidat digitální výkladní skříň EFI z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat digitální výkladní skříň EFI z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **digitální výkladní skříň EFI**, vyberte **digitální výkladní skříň EFI** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.

    ![Digitální výkladní skříň EFI v seznamu výsledků](./media/active-directory-saas-efidigitalstorefront-tutorial/tutorial_efidigital_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s rozhraním EFI digitální výkladní skříň, podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v digitální výkladní skříň EFI je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v digitální výkladní skříň EFI musí navázat.

V rozhraní EFI digitální výkladní skříň, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s rozhraním EFI digitální výkladní skříň, je třeba dokončit následující stavební bloky:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele digitální výkladní skříň EFI](#create-a-efi-digital-storefront-test-user)**  – Pokud chcete mít protějšek Britta Simon v digitální výkladní skříň EFI, propojené služby Azure AD reprezentace daného uživatele.
4. **[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci digitální výkladní skříň EFI.

**Ke konfiguraci Azure AD jednotné přihlašování s rozhraním EFI digitální výkladní skříň, proveďte následující kroky:**

1. Na portálu Azure na **digitální výkladní skříň EFI** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-efidigitalstorefront-tutorial/tutorial_efidigital_samlbase.png)

3. Na **EFI digitální výkladní skříň domény a adresy URL** část, proveďte následující kroky:

    ![Rozhraní EFI digitální výkladní skříň domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-efidigitalstorefront-tutorial/tutorial_efidigital_url.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.myprintdesk.net/DSF`

    b. V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.myprintdesk.net/DSF/asp4/`

4. Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-efidigitalstorefront-tutorial/tutorial_efidigital_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-efidigitalstorefront-tutorial/tutorial_general_400.png)

6. Konfigurace jednotného přihlašování na **digitální výkladní skříň EFI** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory digitální výkladní skříň EFI](http://www.efi.com/products/productivity-software/ecommerce-web-to-print/efi-digital-storefront/support/). Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

   ![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-efidigitalstorefront-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-efidigitalstorefront-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/active-directory-saas-efidigitalstorefront-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/active-directory-saas-efidigitalstorefront-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-efi-digital-storefront-test-user"></a>Vytvoření zkušebního uživatele EFI digitální výkladní skříň

V této části vytvoříte uživatele volal Britta Simon v digitální výkladní skříň EFI. Práce s [tým podpory digitální výkladní skříň EFI](http://www.efi.com/products/productivity-software/ecommerce-web-to-print/efi-digital-storefront/support/) přidat uživatele do platformy digitální výkladní skříň EFI. Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování. 

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu digitální výkladní skříň EFI.

![Přiřadit role uživatele][200] 

**Pokud chcete přiřadit Britta Simon digitální výkladní skříň EFI, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **digitální výkladní skříň EFI**.

    ![Odkaz digitální výkladní skříň EFI v seznamu aplikací](./media/active-directory-saas-efidigitalstorefront-tutorial/tutorial_efidigital_app.png)  

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici digitální výkladní skříň EFI na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci digitální výkladní skříň EFI.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-efidigitalstorefront-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-efidigitalstorefront-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-efidigitalstorefront-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-efidigitalstorefront-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-efidigitalstorefront-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-efidigitalstorefront-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-efidigitalstorefront-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-efidigitalstorefront-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-efidigitalstorefront-tutorial/tutorial_general_203.png

