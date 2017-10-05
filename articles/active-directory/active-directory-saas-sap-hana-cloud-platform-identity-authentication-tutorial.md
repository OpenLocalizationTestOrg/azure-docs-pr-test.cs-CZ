---
title: "Kurz: Azure Active Directory integrace s SAP HANA cloudové platformy identitu ověřování | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SAP HANA cloudové platformy identitu ověřování."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 7799bf03cc6705f805a48f329a265a3d84bed55f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform-identity-authentication"></a>Kurz: Azure Active Directory integrace s SAP HANA cloudové platformy identitu ověřování

V tomto kurzu zjistěte, jak integrovat SAP HANA cloudové platformy identitu ověřování s Azure Active Directory (Azure AD). Ověření Identity SAP HANA cloudové platformy slouží jako proxy server poskytovatelů identity pro přístup k SAP aplikacím pomocí služby Azure AD jako hlavní deklarací identity.

Integrace SAP HANA cloudové platformy identitu ověřování s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, kdo má přístup k aplikaci SAP
- Můžete povolit uživatelům, aby automaticky získat přihlášení k SAP aplikace jednotné přihlašování (SSO) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s SAP HANA cloudové platformy identitu ověřování, potřebujete následující položky:

- Předplatné služby Azure AD
- A **SAP HANA cloudové platformy identitu ověřování** předplatné povolené jednotné přihlašování


>[!NOTE] 
>K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.
>

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.

Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání ověření Identity SAP HANA cloudové platformy z Galerie
2. Konfigurace a testování Azure AD SSO

Než začnete technické informace, je důležité pochopit, koncepty, které se chystáte podívejte se na. Ověření Identity SAP HANA cloudové platformy a Azure Active Directory federation umožňuje implementovat jednotného přihlašování napříč aplikacím nebo službám chráněným pomocí AAD (jako IdP) s aplikacemi SAP a službám chráněným pomocí SAP HANA cloudové platformy identitu ověřování.

V současné době SAP HANA cloudové platformy identitu ověřování funguje jako zprostředkovatel Identity Proxy aplikací SAP. Azure Active Directory pak funguje jako počáteční zprostředkovatele Identity v rámci této instalace. 

Následující diagram znázorňuje toto:    

![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

S tímto nastavením budou klientovi SAP HANA cloudové platformy identitu ověřování nakonfigurované jako důvěryhodné aplikace v Azure Active Directory. 

Všechny aplikace SAP a službách, které chcete chránit pomocí tímto způsobem jsou následně nakonfigurované v konzole pro správu SAP HANA cloudové platformy identitu ověřování!

To znamená, že autorizace pro udělení přístupu k SAP aplikace a služby se musí provést v SAP HANA cloudové platformy identitu ověřování pro taková konfigurace (na rozdíl od konfigurace ověřování ve službě Azure Active Directory).

Konfigurace ověření Identity SAP HANA cloudové platformy jako aplikaci prostřednictvím Azure Active Directory Marketplace, nemusíte postará o konfiguraci potřebné jednotlivé deklaracích / SAML kontrolní výrazy a transformace potřebné k vytvoření platné ověřovací token pro aplikace SAP.

>[!NOTE] 
>Obě strany, pouze byl testován aktuálně jednotného přihlašování k webu. Toky potřebné pro aplikaci API nebo rozhraní API API komunikace by měla fungovat, ale nebyly testovány, ještě. Bude být testována jako součást následné aktivity.
>

## <a name="add-sap-hana-cloud-platform-identity-authentication-from-the-gallery"></a>Přidat SAP HANA cloudové platformy identitu ověřování z Galerie
Při konfiguraci integrace SAP HANA cloudové platformy identitu ověřování do služby Azure AD potřebujete přidat SAP HANA cloudové platformy identitu ověřování z Galerie si na seznam spravovaných aplikací SaaS.

**Pokud chcete přidat SAP HANA cloudové platformy identitu ověřování z galerie, proveďte následující kroky:**

1. V [ **portálu pro správu Azure**](https://portal.azure.com), v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **SAP HANA cloudové platformy identitu ověřování**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_01.png)

5. Na panelu výsledků vyberte **SAP HANA cloudové platformy identitu ověřování**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_02.png)


##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování
V této části konfiguraci a testování Azure AD přihlášení SSO se SAP HANA cloudové platformy Identity ověřování na základě testovací uživatele, nazývá "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v SAP HANA cloudové platformy Identity ověřování je pro uživatele ve službě Azure AD. Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v ověření Identity SAP HANA cloudové platformy je potřeba navázat.

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v SAP HANA cloudové platformy identitu ověřování.

Nakonfigurovat a otestovat Azure AD přihlášení SSO se SAP HANA cloudové platformy identitu ověřování, musíte dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zkušebního uživatele SAP HANA cloudové platformy identitu ověřování](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)**  – Pokud chcete mít protějšek Britta Simon v SAP HANA cloudové platformy ověření Identity propojeném s Azure AD reprezentace jí.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-sso"></a>Konfigurace Azure AD jednotného přihlašování

V této části Povolení jednotného přihlašování Azure AD na portálu Azure Management portal a nakonfigurovat jednotné přihlašování v aplikaci SAP HANA cloudové platformy identitu ověřování.

Ověření Identity SAP HANA cloudové platformy aplikace očekává SAML kontrolní výrazy ve specifickém formátu. Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace. Následující snímek obrazovky ukazuje příklad pro tento.

![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_03.png)

**Ke konfiguraci Azure AD jednotné přihlašování s SAP HANA cloudové platformy identitu ověřování, proveďte následující kroky:**

1. Na portálu Azure Management portal na **SAP HANA cloudové platformy identitu ověřování** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.
 
    ![Konfigurovat jednotné přihlašování][5]

3. V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, pokud vaše aplikace SAP očekává atribut například "jméno". V dialogovém okně atributy tokenu SAML přidejte atribut "jméno".
 1. Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.
 
    ![Konfigurovat jednotné přihlašování][6]

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_05.png)
 2. V **název atributu** textovému poli, zadejte název atributu "jméno".
 3. Z **hodnota atributu** vyberte hodnotu atributu "user.givenname".
 4. Klikněte na tlačítko **OK**.

4. Na **SAP HANA cloudové platformy identitu ověřování domény a adresy URL** část, proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_06.png)
 1. V **přihlašovací adresa URL** textovému poli, zadejte přihlašovací na adresu URL pro aplikaci SAP.
 2. V **identifikátor** textovému poli, zadejte hodnotu následující vzoru:`<entity-id>.accounts.ondemand.com` 
    * Pokud si nejste jisti tuto hodnotu, postupujte podle dokumentace SAP HANA cloudové platformy identitu ověřování v [konfiguraci klienta SAML 2.0](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

5. Na **konfiguraci Identity ověřování cloudové platformy SAP HANA** klikněte na tlačítko **konfigurace SAP HANA cloudové platformy identitu ověřování** otevřete **konfigurovat přihlášení** dialogové okno. Potom klikněte na **SAML XML Metadata** a uložte soubor v počítači.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_07.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_08.png)

6. Chcete-li získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, přejděte na SAP HANA cloudové platformy identitu ověřování konzole pro správu. Adresa URL má vzoru následující:`https://<tenant-id>.accounts.ondemand.com/admin`
 * Potom postupujte podle dokumentace na ověření Identity SAP HANA cloudové platformy na [konfigurace Microsoft Azure AD jako podnikové poskytovatele Identity v SAP HANA cloudové platformy identitu ověřování](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

7. V portálu pro správu Azure, klikněte na **Uložit** tlačítko.
8. Následující kroky pokračujte, pouze pokud chcete přidat a povolení jednotného přihlašování pro jinou aplikaci SAP. Opakujte kroky v části "Přidání SAP HANA cloudové platformy identitu ověřování z Galerie" přidat další instanci SAP HANA cloudové platformy identitu ověřování.
9. Na portálu Azure Management portal na **SAP HANA cloudové platformy identitu ověřování** stránky integrace aplikací, klikněte na tlačítko **propojené přihlášení**.

    ![Konfigurovat propojené přihlášení](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)
10. Potom uložte konfiguraci.

>[!NOTE] 
>Nová aplikace bude využívat Konfigurace jednotného přihlašování pro předchozí aplikaci SAP. Zkontrolujte prosím, že používáte stejné podnikové poskytovatelů identit v SAP HANA cloudové platformy identitu ověřování konzole pro správu.
>

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele v názvem Britta Simon nového portálu.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png) 

2. Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png) 

3. V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png) 
  1. V **název** textovému poli, typ **BrittaSimon**.
  2. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.
  3. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.
  4. Klikněte na možnost **Vytvořit**. 

### <a name="create-a-sap-hana-cloud-platform-identity-authentication-test-user"></a>Vytvoření zkušebního uživatele SAP HANA cloudové platformy identitu ověřování

Nemusíte vytvořit uživatele na SAP HANA cloudové platformy identitu ověřování. Uživatelé, kteří jsou v úložišti uživatele Azure AD můžete použít funkci jednotného přihlašování.

Ověření Identity SAP HANA cloudové platformy podporuje možnost federaci identit. Tato možnost umožňuje aplikace a zjistit, pokud ověřený zprostředkovatelem podnikové identitě uživatele existovat v úložišti z SAP HANA cloudové platformy Identity ověření uživatele. 

Ve výchozím nastavení je zakázána možnost federaci identit. Pokud je povoleno federaci identit, mohou pouze uživatelé, kteří jsou importovány v SAP HANA cloudové platformy identitu ověřování přístup k aplikaci. 

Další informace o tom, jak povolit nebo zakázat federaci identit s SAP HANA cloudové platformy identitu ověřování najdete v části Povolit federaci identit s SAP HANA cloudové platformy identitu ověřování v [konfigurace federaci identit s úložiště systému SAP HANA cloudové platformy Identity ověření uživatele.](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k SAP HANA cloudové platformy identitu ověřování.

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon SAP HANA cloudové platformy identitu ověřování, proveďte následující kroky:**

1. V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **SAP HANA cloudové platformy identitu ověřování**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_09.png)

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    

### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování k použití na přístupovém panelu.

Když kliknete na dlaždici SAP HANA cloudové platformy Identity ověřování na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci SAP HANA cloudové platformy identitu ověřování.


## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_06.png

[100]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_203.png