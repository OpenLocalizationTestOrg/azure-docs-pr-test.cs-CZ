---
title: "Kurz: Azure Active Directory integrace s vyrovná se se zatížením pro správu vzdělávacího procesu | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a vyrovná se se zatížením pro správu vzdělávacího procesu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 3c68c3ac7d6be593476d419f8c015931b206eead
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Kurz: Azure Active Directory integrace s vyrovná se se zatížením pro správu vzdělávacího procesu

V tomto kurzu zjistěte, jak integrovat vyrovná se se zatížením pro správu vzdělávacího procesu s Azure Active Directory (Azure AD).

Integrace vyrovná se se zatížením pro správu vzdělávacího procesu s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k vyrovná se se zatížením pro správu vzdělávacího procesu
- Můžete povolit uživatelům, aby automaticky získat přihlášení k vyrovná se se zatížením vzdělávacího procesu (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, přečtěte si téma. [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s vyrovná se se zatížením pro správu vzdělávacího procesu, potřebujete následující položky:

- Předplatné služby Azure AD
- Vyrovná se se zatížením pro správu vzdělávacího procesu jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání vyrovná se se zatížením pro správu vzdělávacího procesu z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-absorb-lms-from-the-gallery"></a>Přidání vyrovná se se zatížením pro správu vzdělávacího procesu z Galerie
Pokud chcete nakonfigurovat integraci vyrovná se se zatížením pro správu vzdělávacího procesu v do Azure AD, potřebujete přidat vyrovná se se zatížením pro správu vzdělávacího procesu z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat vyrovná se se zatížením pro správu vzdělávacího procesu z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **vyrovná se se zatížením pro správu vzdělávacího procesu**, vyberte **vyrovná se se zatížením pro správu vzdělávacího procesu** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.

    ![Vyrovná se se zatížením pro správu vzdělávacího procesu v seznamu výsledků](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s vyrovná se se zatížením vzdělávacího procesu na základě testovací uživatele, nazývá "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v vyrovná se se zatížením pro správu vzdělávacího procesu je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v vyrovná se se zatížením pro správu vzdělávacího procesu musí navázat.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v vyrovná se se zatížením pro správu vzdělávacího procesu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s vyrovná se se zatížením pro správu vzdělávacího procesu, je třeba dokončit následující stavební bloky:

1. **[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvořit testovací uživatele s vyrovná se se zatížením pro správu vzdělávacího procesu](#create-an-absorb-lms-test-user)**  – Pokud chcete mít protějšek Britta Simon v vzdělávacího procesu vyrovná se se zatížením, propojené služby Azure AD reprezentace daného uživatele.
4. **[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci vyrovná se se zatížením pro správu vzdělávacího procesu.

**Ke konfiguraci Azure AD jednotné přihlašování s vyrovná se se zatížením pro správu vzdělávacího procesu, proveďte následující kroky:**

1. Na portálu Azure na **vyrovná se se zatížením pro správu vzdělávacího procesu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. Na **vyrovná se se zatížením pro správu vzdělávacího procesu domény a adresy URL** část, proveďte následující kroky:

    ![Pro správu vzdělávacího procesu adresy URL jeden přihlašování informace o doméně a vyrovná se se zatížením](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_url.png)

    a. V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.myabsorb.com/Account/SAML`

    b. V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.myabsorb.com/Account/SAML`
     
    > [!NOTE] 
    > Tyto hodnoty nejsou reálné. Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi. Obraťte se na [tým podpory vyrovná se se zatížením klienta pro správu vzdělávacího procesu](https://www.absorblms.com/support) k získání těchto hodnot. 

4. Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_certificate.png) 

6. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-absorblms-tutorial/tutorial_general_400.png)
    
7. Na **vyrovná se se zatížením konfigurace pro správu vzdělávacího procesu** klikněte na tlačítko **konfigurace vyrovná se se zatížením vzdělávacího procesu** otevřete **konfigurovat přihlášení** okno. Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**

    ![Vyrovná se se zatížením konfigurace pro správu vzdělávacího procesu](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_configure.png) 

8. V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti vyrovná se se zatížením pro správu vzdělávacího procesu jako správce.

9. Klikněte **ikonu účtu** v rozhraní správce. 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/1.png)

10. Klikněte na tlačítko **nastavení portálu**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/2.png)
    
11. Klikněte **uživatelé** kartě.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/3.png)

12. Proveďte následující kroky pro přístup k konfigurační pole jednotné přihlašování:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/4.png)

    a. Vyberte odpovídající **režimu**.

    b. Otevřete odebrat certifikát, který jste stáhli z portálu Azure v poznámkovém bloku **---BEGIN CERTIFICATE,** a **---END CERTIFICATE---** značku a potom vložte zbývající obsah **klíč** textové pole.
    
    c. V **Vlastnost Id**, vyberte odpovídající atribut, který jste nakonfigurovali jako identifikátor uživatele ve službě Azure AD (např. Pokud userprinciplename je vybrána ve službě Azure AD, pak uživatelské jméno by zde vybraný.)

    d. V **přihlašovací adresa URL**, vložte **"SAML jeden přihlašování adresa URL služby"** hodnotu zkopírovanou z **konfigurovat přihlášení** okno portálu Azure.

    e. V **adresy URL odhlašovací**, vložte **"Adresa URL Sign-Out"** hodnotu zkopírovanou z **konfigurovat přihlášení** okno portálu Azure.

13. Povolit **"Povolit jenom přihlášení SSO"**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/5.png)

14. Klikněte na tlačítko **"Uložit."**

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-absorblms-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-absorblms-tutorial/create_aaduser_02.png) 

3. V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.
 
    ![Tlačítko Přidat](./media/active-directory-saas-absorblms-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Dialogové okno uživatele](./media/active-directory-saas-absorblms-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.

### <a name="create-an-absorb-lms-test-user"></a>Vytvořit testovací uživatele s vyrovná se se zatížením pro správu vzdělávacího procesu

Pokud chcete povolit uživatelům Azure AD přihlášení do vyrovná se se zatížením pro správu vzdělávacího procesu, se musí být zřízená v k vyrovná se se zatížením pro správu vzdělávacího procesu.  
Pro vyrovná se se zatížením vzdělávacího procesu zřizování je ruční úloha.

**K poskytnutí uživatelského účtu, proveďte následující kroky:**

1. Přihlaste se k serveru vaší společnosti vyrovná se se zatížením pro správu vzdělávacího procesu jako správce.

2. Klikněte na tlačítko **uživatelé** kartě.

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/absorblms_users.png)

3. Klikněte na tlačítko **uživatelé** pod **uživatelé** kartě.

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/absorblms_userssub.png)

4.  Vyberte **uživatele** z **přidat nové** rozevíracího seznamu.

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/absorblms_createuser.png)

5. Na **přidat uživatele** proveďte následující kroky:

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/user.png)

    a. V **křestní jméno** textovému poli, název typu první jako Britta.

    b. V **příjmení** textovému poli, zadejte příjmení jako Simon.
    
    c. V **uživatelské jméno** textovému poli, zadejte uživatelské jméno jako Britta Simon.

    d. V **heslo** textovému poli, zadejte heslo Britta Simon.

    e. V **Potvrdit heslo** textovému poli, zadejte stejné heslo.
    
    f. Nastavit jej jako **ACTIVE**.   

6. Klikněte na tlačítko **"Uložit."**
 
### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu vyrovná se se zatížením pro správu vzdělávacího procesu.

![Přiřadit role uživatele][200]

**Pokud chcete přiřadit Britta Simon vyrovná se se zatížením pro správu vzdělávacího procesu, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **vyrovná se se zatížením pro správu vzdělávacího procesu**.

    ![Odkaz vyrovná se se zatížením pro správu vzdělávacího procesu v seznamu aplikací](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![V podokně Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Klikněte na dlaždici vyrovná se se zatížením pro správu vzdělávacího procesu na přístupovém panelu, můžete se získat automaticky přihlášení k aplikaci vyrovná se se zatížením pro správu vzdělávacího procesu. Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_203.png

