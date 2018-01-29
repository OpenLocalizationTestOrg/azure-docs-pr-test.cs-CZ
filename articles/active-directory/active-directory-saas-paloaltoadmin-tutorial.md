---
title: "Kurz: Azure Active Directory integrace se sítěmi Palo Alto - uživatelského rozhraní správce | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Palo Alto sítě - Správce uživatelského rozhraní."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a826eaec-15af-4c85-8855-8a3374d1efb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jeedes
ms.openlocfilehash: 8e54630d97dee2388ffc9c8877faeac269df1609
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---admin-ui"></a>Kurz: Azure Active Directory integrace s Palo Alto sítě - Správce uživatelského rozhraní

V tomto kurzu zjistěte, jak integrovat Palo Alto Networks – uživatelské rozhraní Správce služby Azure Active Directory (Azure AD).

Integrace Palo Alto sítí - uživatelského rozhraní správce s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup k sítím Palo Alto - uživatelského rozhraní správce.
- Můžete povolit uživatelům, aby automaticky získat přihlášeného k sítím Palo Alto - uživatelského rozhraní správce (jednotné přihlášení) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Palo Alto sítě - Správce uživatelského rozhraní, potřebujete následující položky:

- Předplatné služby Azure AD
- Brána Firewall příští generace Palo Alto sítě nebo – Panorama (centralizovanou správu systém pro bránu firewall)

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidávání sítí Palo Alto - uživatelského rozhraní správce z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-palo-alto-networks---admin-ui-from-the-gallery"></a>Přidávání sítí Palo Alto - uživatelského rozhraní správce z Galerie
Při konfiguraci integrace Palo Alto sítí - uživatelského rozhraní správce do služby Azure AD, potřebujete přidat Palo Alto Networks - uživatelského rozhraní správce z Galerie si na seznam spravovaných aplikací SaaS.

**Chcete-li přidat Palo Alto sítě - Správce uživatelského rozhraní z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **Palo Alto Networks - uživatelského rozhraní správce**, vyberte **Palo Alto Networks - uživatelského rozhraní správce** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.

    ![Palo Alto sítě - Správce uživatelského rozhraní v seznamu výsledků](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_step4-add-from-the-gallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování se sítěmi Palo Alto – uživatelského rozhraní správce podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v sítích Palo Alto - uživatelského rozhraní správce je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v sítích Palo Alto - uživatelského rozhraní Správce musí navázat.

V sítích Palo Alto - uživatelského rozhraní správce, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Palo Alto sítě - Správce uživatelského rozhraní, musíte dokončit následující stavební bloky:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvořte sítě Palo Alto - uživatelského rozhraní správce testovacího uživatele](#create-a-palo-alto-networks---admin-ui-test-user)**  – Pokud chcete mít protějšek Britta Simon v sítích Palo Alto – uživatelské rozhraní správce, které je propojený s Azure AD reprezentace daného uživatele.
4. **[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování ve svých sítích Palo Alto - uživatelského rozhraní Správce aplikací.

**Ke konfiguraci Azure AD jednotné přihlašování s Palo Alto sítě - Správce uživatelského rozhraní, proveďte následující kroky:**

1. Na portálu Azure na **Palo Alto Networks - uživatelského rozhraní správce** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_samlbase.png)

3. Na **Palo Alto sítě - správce domény uživatelského rozhraní a adresy URL** část, proveďte následující kroky:

    ![Palo Alto sítě - správce domény uživatelského rozhraní a adresy URL jeden přihlašování informace](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_url.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<Customer Firewall FQDN>/php/login.php`

    b. V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<Customer Firewall FQDN>/SAML20/SP`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [Palo Alto sítě - Správce uživatelského rozhraní klienta tým podpory](https://support.paloaltonetworks.com/support) k získání těchto hodnot. 
 
4. Palo Alto sítě - Správce uživatelského rozhraní aplikace očekává SAML kontrolní výrazy ve specifickém formátu. Nakonfigurujte následující deklarace identity pro tuto aplikaci. Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace. Následující snímek obrazovky ukazuje příklad pro tento.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_attribute.png)
    
5. V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku výše a proveďte následující kroky: Upozorňujeme, že jsou hodnoty atributu pouze příklad prosím mapují na odpovídající hodnoty pro uživatelské jméno a adminrole. Neexistuje jiný volitelný atribut "accessdomain", který se používá k omezení přístupu správce ke konkrétní virtuálních systémů v bráně firewall.
        
    | Název atributu | Hodnota atributu |
    | --- | --- |    
    | uživatelské jméno | user.userprincipalname |
    | adminrole | customadmin |

    a. Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_attribute_05.png)
    
    b. V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.
    
    c. Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.
    
    d. Klikněte na tlačítko **Ok**

    > [!NOTE]
    > Najdete následující články pro další informace o atributech.
    > 1. Profil pro správu Role pro správce uživatelského rozhraní (adminrole): https://www.paloaltonetworks.com/documentation/80/pan-os/pan-os/firewall-administration/manage-firewall-administrators/configure-an-admin-role-profile
    > 2. Doména přístupu zařízení pro správu uživatelského rozhraní (accessdomain): https://www.paloaltonetworks.com/documentation/80/pan-os/web-interface-help/device/device-access-domain

6. Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_certificate.png) 

7. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_400.png)

8. Otevřete uživatelské rozhraní Správce brány Firewall sítě Palo Alto jako správce v jiném okně prohlížeče.

9. Klikněte na **zařízení**.

    ![Konfigurace Palo Alto jednotné přihlašování](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin1.png)

10. Vyberte **poskytovatele Identity SAML** z levém navigačním panelu a klikněte na tlačítko "Import" k importu souboru metadat.

    ![Konfigurace Palo Alto jednotné přihlašování](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin2.png)

11. Proveďte následující akce v okně Import

    ![Konfigurace Palo Alto jednotné přihlašování](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin3.png)

    a. V **název profilu** textovému poli, zadejte název např uživatelského rozhraní správce Azure AD.
    
    b. V **metadat zprostředkovatelů Identity**, klikněte na tlačítko **Procházet** a vyberte soubor metadata.xml, který jste si stáhli z portálu Azure
    
    c. Klikněte na tlačítko **OK**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

   ![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-palo-alto-networks---admin-ui-test-user"></a>Vytvořte sítě Palo Alto - uživatelského rozhraní správce testovacího uživatele

Palo Alto sítě - Správce uživatelského rozhraní podporuje pouze za běhu zřizování uživatelů, uživatel se vytvoří automaticky v systému po úspěšném ověření Pokud tomu tak není již existuje. Nemusíte provádět veškeré akce v tomto poli.

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon používat tak, že udělíte přístup k sítím Palo Alto - uživatelského rozhraní správce Azure jednotné přihlašování.

![Přiřadit role uživatele][200] 

**Britta Simon přiřadit Palo Alto sítě - Správce uživatelského rozhraní, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Palo Alto Networks - uživatelského rozhraní správce**.

    ![Sítě Palo Alto - odkaz uživatelského rozhraní správce v seznamu aplikací](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_app.png)  

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Po kliknutí na tlačítko Palo Alto Networks - dlaždice správce uživatelského rozhraní na přístupovém panelu jste měli získat automaticky přihlášení k Palo Alto sítě - Správce uživatelského rozhraní aplikace.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje informací:

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_203.png

