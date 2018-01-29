---
title: 'Kurz: Azure Active Directory integrace s Atlassian cloudu | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Atlassian cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: jeedes
ms.openlocfilehash: db9e9c7ae8380612bac9d0aeaaaf6df78cba523f
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Kurz: Azure Active Directory integrace s Atlassian cloudu

V tomto kurzu zjistěte, jak integrovat Atlassian cloudu s Azure Active Directory (Azure AD).

Integrace Atlassian cloudu s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup do cloudu Atlassian.
- Můžete povolit uživatelům být automaticky přihlášeni (jednotné přihlášení) Atlassian cloudu s účty služby Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě, portálu Azure.

Další informace o softwaru jako integraci aplikace služby (SaaS) s Azure AD najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD s cloudem Atlassian, potřebujete následující položky:

- Předplatné služby Azure AD.
- Povolit zabezpečení kontrolního výrazu SAML (Markup Language) jednotné přihlašování pro produkty Atlassian cloudu, musíte nastavit Identity Manager. Další informace o [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager).

> [!NOTE]
> Při testování kroky v tomto kurzu, doporučujeme, abyste nepoužívali provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle následujících doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

* Přidání Atlassian cloudu z Galerie
* Konfigurace a testování Azure AD jednotného přihlašování

## <a name="add-atlassian-cloud-from-the-gallery"></a>Přidat Atlassian cloudu z Galerie
Při konfiguraci integrace Atlassian cloudu s Azure AD, přidejte Atlassian cloudu z Galerie si na seznam spravovaných aplikací SaaS následujícím způsobem:

1. V [portál Azure](https://portal.azure.com), v levém podokně, vyberte **Azure Active Directory** tlačítko. 

    ![Tlačítko Azure Active Directory][1]

2. Vyberte **podnikové aplikace, které** > **všechny aplikace**.

    ![V podokně podnikových aplikací][2]
    
3. Chcete-li přidat aplikaci, vyberte **novou aplikaci**.

    !["Nová aplikace" tlačítko][3]

4. Do vyhledávacího pole zadejte **Atlassian cloudu**, vyberte v seznamu výsledků **Atlassian cloudu**a potom vyberte **přidat**.

    ![Atlassian cloudu v seznamu výsledků](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Atlassian cloudové na testovacího uživatele s názvem *Britta Simon*.

Azure AD pro jednotné přihlašování pro práci, musí identifikovat Atlassian cloudu uživatel a jeho protějšku ve službě Azure AD. Jinými slovy je potřeba vytvořit vztah propojení mezi uživatele Azure AD a související uživatelské v Atlassian cloudu.

K navázání vztahu odkaz, přiřadit jako Atlassian Cloud *uživatelské jméno* stejnou hodnotu, která je přiřazena k Azure AD *uživatelské jméno*.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Atlassian cloudu, musíte dokončit stavebních bloků v následujících částech.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Atlassian cloudu.

Ke konfiguraci Azure AD jednotné přihlašování s cloudem Atlassian, postupujte takto:

1. Na portálu Azure v **Atlassian cloudu** podokno integrace aplikací, vyberte **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. V **jednotného přihlašování** okno v **režimu přihlašování** vyberte **na základě SAML přihlašování**.
 
    ![Okno přihlášení](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. V části Konfigurace aplikace v režimu spouštěná IDP **Atlassian cloudové domény a adresy URL**, postupujte takto:

    ![Atlassian cloudové domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)
    
    a. V **identifikátor** zadejte  **`https://auth.atlassian.com/saml/<unique ID>`** .
    
    b. V **adresa URL odpovědi** zadejte  **`https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`** .

    c. V **předávání stavu** pole, zadejte adresu URL pomocí následující syntaxe:  **`https://<instancename>.atlassian.net`** .

4. Konfigurace aplikace v režimu spouštěná SP, vyberte **zobrazit upřesňující nastavení adresy URL** a pak na **přihlásit na adrese URL** pole, zadejte adresu URL pomocí následující syntaxe:  **`https://<instancename>.atlassian.net`**  .

    ![Atlassian cloudové domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    > [!NOTE] 
    > Předchozí hodnoty nejsou skutečné. Aktualizaci s skutečné identifikátor, adresa URL odpovědi a přihlašování hodnoty adresy URL. Skutečné hodnoty můžete získat na obrazovce konfigurace SAML Atlassian cloudu. Později v tomto kurzu objasníme hodnoty.

5. V části **SAML podpisový certifikát**, vyberte **Certificate(Base64)**a potom uložte soubor certifikátu v počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. Atlassian cloudové aplikace se očekává, že najít SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, můžete přidat mapování vlastních atributů do vaší konfigurace atributy tokenu SAML. 

    Ve výchozím nastavení **uživatelský identifikátor** hodnota je namapována na user.userprincipalname. Tato hodnota k namapování k user.mail změňte. Můžete také další odpovídající hodnotu podle nastavení vaší organizace, ale ve většině případů by měla fungovat e-mailu.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_attribute.png) 

7. Vyberte **Uložit**.

    ![Konfigurovat jednotné přihlašování tlačítko Uložit](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

8. Otevřete **konfigurovat přihlášení** okno v **konfigurace cloudu Atlassian** vyberte **konfigurace cloudu Atlassian**. 

9. V **Stručná referenční příručka** část, zkopírujte **SAML Entity ID** a **SAML jeden přihlašování adresa URL služby**. 

    ![Konfigurace cloudu Atlassian](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

10. Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, přihlaste se k portálu Atlassian s přihlašovacími údaji správce.

11. Přejděte na **Správa webu Atlassian** > **organizace a zabezpečení**. Pokud jste tak již neučinili, vytvořte a název vaší organizace a v levém podokně vyberte **domény**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

12. Vyberte způsob, jakým chcete ověřte svoji doménu: **DNS** nebo **HTTPS**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_17.png)

13. Pro ověření DNS v **domén** vyberte **DNS** kartě a proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_18.png)

    a. Zkopírujte hodnotu pro vaše textový záznam (záznam TXT), vyberte **kopie**.

    b. Chcete-li přidat záznam, přejděte na stránku nastavení v DNS.

    c. Vyberte možnost pro přidávání nového záznamu a vložte hodnotu, kterou jste zkopírovali v **domén** okna **hodnotu** pole. Vaše záznam DNS může také označovat je jako **odpovědí** nebo **popis**.

    d. Vaše záznam DNS může také zahrnovat následující pole:
    
    * V **typ záznamu** zadejte **TXT**.
    * V **název nebo hostitele nebo Alias** pole, ponechte výchozí hodnotu (@ nebo je prázdný).
    * V **dobu live (TTL)** zadejte **86400**.
    
    e.  Záznam uložte.

14. Vraťte se na **domén** okno v organizaci správy a potom vyberte **ověřit doménu**. V **domény** , zadejte název domény a pak vyberte **ověřit doménu**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_19.png)  

    > [!NOTE]
    > Vzhledem k tomu, že může trvat až 72 hodin, než se projeví změny záznam TXT, nebude vědět hned zda ověření domény byla úspěšná. Chcete-li zobrazit stav ověření, zkontrolujte **domén** okno hned po dokončení tohoto postupu. Aktualizovaný stav se zobrazí jako *ověřeno*, jak je znázorněno na následujícím obrázku:
    > 
    > ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_20.png)
    > 
    > 

15. Pro ověření protokol HTTPS v **domén** vyberte **HTTPS** kartě a proveďte následující kroky:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_21.png)

    a. Chcete-li stáhnout soubor HTML, vyberte **stažení souboru**.

    b. Nahrajte soubor HTML do kořenového adresáře vaší domény.

16. Vraťte se na **domén** v organizaci pro správu a vyberte **ověřit doménu**. V **ověřit doménu** okno v **domény** zadejte vaše **název domény**a potom vyberte **ověřit doménu**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_22.png)

17. Pokud proces ověření můžete vyhledat soubor, který jste nahráli do kořenového adresáře, stav domény bude aktualizován na *ověřeno*, jak je vidět tady:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_23.png)

    > [!NOTE]
    > Další informace najdete v tématu [ověření domény Atlassian](https://confluence.atlassian.com/cloud/domain-verification-873871234.html).

18. V levém podokně vyberte **SAML jednotné přihlašování**. Pokud jste tak již neučinili, přihlásit k odběru do Atlassian Identity Manager.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

19. V **konfigurace přidat SAML** okno, postupujte takto:

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. V **zprostředkovatele Identity Entity ID** pole, vložte ID entity SAML, který jste zkopírovali z portálu Azure.

    b. V **zprostředkovatele Identity jednotného přihlašování k adrese URL** pole, vložte adresu URL služby jednotného přihlašování SAML, který jste zkopírovali z portálu Azure.

    c. Otevřete certifikát stažený z portálu Azure v souboru .txt, zkopírujte hodnotu (bez *začít certifikát* a *End Certificate* řádky) a potom vložte jej do **veřejné X509 certifikát** pole.
    
    d. Vyberte **uložte konfiguraci**.
     
20. Aby se zajistilo, že jste nastavili správné adresy URL, aktualizujte nastavení služby Azure AD následujícím způsobem:
  
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. V okně SAML zkopírovat **SP Identity ID** a pak na portálu Azure v cloudu Atlassian **domény a adresy URL**, vložte jej do **identifikátor** pole.
    
    b. V okně SAML zkopírovat **adresa URL služby SP Assertion příjemce** a pak na portálu Azure v cloudu Atlassian **domény a adresy URL**, vložte jej do **adresa URL odpovědi** pole.  
        Adresa URL přihlašování je adresa URL klienta ve vašem cloudu Atlassian. 

    > [!NOTE]
    > Pokud jste stávající zákazník, po provedení aktualizace **SP Identity ID** a **adresa URL služby SP Assertion příjemce** hodnoty na portálu Azure vyberte **Ano, aktualizace konfigurace**. Pokud jste nového zákazníka, můžete tento krok přeskočit. 
    
21. Na portálu Azure vyberte **Uložit**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> Jak při instalaci aplikace, můžete si přečíst stručným verzi podle předchozích pokynů v [portál Azure](https://portal.azure.com). Po přidání této aplikace z **služby Active Directory** > **podnikové aplikace, které** vyberte **jednotné přihlašování** kartě a potom přejdete embedded dokumentace v **konfigurace** v dolní části okna. Další informace najdete v tématu [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

V této části vytvoříte testovacího uživatele Britta Simon na portálu Azure, následujícím způsobem:

   ![Vytvořit testovací uživatele Azure AD][100]

1. Na portálu Azure, v levém podokně, vyberte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, vyberte **uživatelů a skupin** > **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png)

3. V **všichni uživatelé** vyberte **přidat**.

    ![Tlačítko Přidat](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png)

4. V **uživatele** okno, postupujte takto:

    ![Okno uživatele](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Vyberte **Vytvořit**.
  
### <a name="create-an-atlassian-cloud-test-user"></a>Vytvořit testovací uživatele s Atlassian cloudu

Pokud chcete povolit uživatelům Azure AD přihlášení do cloudu Atlassian, zřídit uživatelské účty ručně v cloudu Atlassian následujícím způsobem:

1. V **správy** podokně, vyberte **uživatelé**.

    ![Uživatelé cloudové Atlassian odkaz](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. Chcete-li vytvořit uživatele v cloudu Atlassian, vyberte **pozvání uživatele**.

    ![Vytvořit uživatele s Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. V **e-mailová adresa** zadejte e-mailovou adresu uživatele a pak mu přiřaďte přístup k aplikaci. 

    ![Vytvořit uživatele s Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. K odeslání pozvánky e-mailu pro uživatele, vyberte **uživatele pozvat**.  
    Pozvánka e-mailu je odeslán uživateli, a po přijetí pozvánky, uživatel je v systému aktivní. 

>[!NOTE] 
>Můžete také hromadně – vytvořte uživatele tak, že vyberete **vytvořit hromadné** tlačítka na **uživatelé** části.

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolit uživatele Britta Simon používat tak, že udělíte přístup do cloudu Atlassian Azure jednotné přihlašování. Chcete-li to provést, postupujte takto:

![Přiřadit role uživatele][200] 

1. Na portálu Azure otevřete **aplikace** zobrazení, přejděte do zobrazení adresáře a pak vyberte **podnikové aplikace, které** > **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V **aplikace** seznamu, vyberte **Atlassian cloudu**.

    ![V seznamu aplikací na odkaz Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png)  

3. V levém podokně vyberte **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Vyberte **přidat** a pak na **přidat přiřazení** podokně, vyberte **uživatelů a skupin**.

    ![V podokně Přidat přiřazení][203]

5. V **uživatelů a skupin** okno v **uživatelé** seznamu, vyberte **Britta Simon**.

6. V **uživatelů a skupin** vyberte **vyberte**.

7. V **přidat přiřazení** vyberte **přiřadit**.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části otestovat vaše konfigurace Azure AD jeden přihlašování pomocí přístupového panelu.

Když vyberete **Atlassian cloudu** dlaždici na přístupovém panelu můžete by měl být přihlášeni automaticky do vaší aplikace Atlassian cloudu.
Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS v Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

