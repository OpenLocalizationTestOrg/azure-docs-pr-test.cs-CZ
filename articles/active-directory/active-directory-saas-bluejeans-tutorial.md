---
title: 'Kurz: Azure Active Directory integrace s BlueJeans | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a BlueJeans."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: jeedes
ms.openlocfilehash: 03bf65852b8d3cf14aebf155891a028db86e78d0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Kurz: Azure Active Directory integrace s BlueJeans

V tomto kurzu zjistěte, jak integrovat BlueJeans s Azure Active Directory (Azure AD).

Integrace BlueJeans s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k BlueJeans
- Můžete povolit uživatelům, aby automaticky získat přihlášení k BlueJeans (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s BlueJeans, potřebujete následující položky:

- Předplatné služby Azure AD
- BlueJeans jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání BlueJeans z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-bluejeans-from-the-gallery"></a>Přidání BlueJeans z Galerie
Při konfiguraci integrace BlueJeans do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS BlueJeans z galerie.

**Pokud chcete přidat BlueJeans z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **BlueJeans**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_search.png)

5. Na panelu výsledků vyberte **BlueJeans**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s BlueJeans podle testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v BlueJeans je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v BlueJeans musí navázat.

V BlueJeans, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s BlueJeans, je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele BlueJeans](#creating-a-bluejeans-test-user)**  – Pokud chcete mít protějšek Britta Simon v BlueJeans propojeném s Azure AD reprezentace daného uživatele.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci BlueJeans.

**Ke konfiguraci Azure AD jednotné přihlašování s BlueJeans, proveďte následující kroky:**

1. Na portálu Azure na **BlueJeans** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. Na **BlueJeans domény a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_url.png)

    a. V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.BlueJeans.com`

    b. V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.BlueJeans.com`

    > [!NOTE] 
    > Tyto hodnoty nejsou skutečné. Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor. Obraťte se na [tým podpory BlueJeans klienta](https://support.bluejeans.com/contact) k získání těchto hodnot. 
 
4. Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_general_400.png)

6. Na **BlueJeans konfigurace** klikněte na tlačítko **konfigurace BlueJeans** otevřete **konfigurovat přihlášení** okno. Kopírování **Sign-Out adresu URL, adresy URL pro změnu hesla a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. V okně prohlížeče jiný web, přihlaste se k vaší **BlueJeans** společnosti lokality jako správce.

8. Přejděte na **správce \> nastavení skupiny \> zabezpečení**.
   
   ![Správce](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "správce")

9. V **zabezpečení** část, proveďte následující kroky:
   
   ![Jednotné přihlašování SAML na](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "jednotného přihlašování k SAML")   
   
   a. Vyberte **jednotného přihlašování k SAML**.
  
   b. Vyberte **zapněte automatické zřizování**.

10. Přesunout pomocí následujících kroků:

    ![Certifikát cesta](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "certifikátu cesta")
    
    a. Klikněte na tlačítko **zvolit soubor**a pak nahrajte stažený certifikát.
   
    b. Vložení **SAML jeden přihlašování adresa URL služby** do **přihlašovací adresa URL** textové pole.
   
    c. Vložení **heslo změnit adresu URL** do **heslo změnit adresu URL** textové pole.
   
    d. Vložení **Sign-Out URL** do **adresy URL odhlašovací** textové pole.

11. Přesunout pomocí následujících kroků:
    
    ![Uložit změny](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "uložit změny")
    
    a. V **id uživatele** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
   
    b. V **e-mailu** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
   
    c. Klikněte na tlačítko **uložit změny**.

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_01.png) 

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_02.png) 

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-bluejeans-test-user"></a>Vytvoření zkušebního uživatele BlueJeans

Pokud chcete povolit uživatelům Azure AD přihlášení k BlueJeans, musí být zřízená do BlueJeans.  

V případě BlueJeans zřizování je ruční úloha.

**Ke zřízení uživatelských účtů, proveďte následující kroky:**

1. Přihlaste se k vaší **BlueJeans** společnosti lokality jako správce.

2. Přejděte na **správce \> Správa uživatelů \> přidat uživatele**.
   
   ![Správce](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "správce")
   
   >[!IMPORTANT]
   >**Přidat uživatele** karta je dostupná pouze v případě, v **karta zabezpečení**, **zapněte automatické zřizování** není zaškrtnuta. 
   
3. V **přidat uživatele** část, proveďte následující kroky:

    ![Přidat uživatele](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "přidat uživatele")
    
    a. Zadejte **uživatelské jméno BlueJeans**, **e-mailová adresa**, **BlueJeans schůzku ID**, **moderátora hesla**, **úplný název**, **společnosti** platného účtu AAD chcete mají být zahrnuty do související textových polí.
    
    b. Klikněte na tlačítko **přidat uživatele**.

>[!NOTE]
>Můžete použít všechny ostatní BlueJeans uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované BlueJeans zřídit AAD uživatelské účty. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu BlueJeans.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon BlueJeans, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **BlueJeans**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici BlueJeans na přístupovém panelu, měli byste obdržet přihlašovací stránku BlueJeans aplikace.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_203.png

