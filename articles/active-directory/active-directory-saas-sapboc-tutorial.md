---
title: 'Kurz: Azure Active Directory integrace s SAP Business objektu cloudu | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SAP Business objektu cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: a3e9bd93897271531f91bcbc50cd361e8a20551e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a>Kurz: Azure Active Directory integrace s SAP Business objektu cloudu

V tomto kurzu zjistíte, jak toointegrate SAP Business objektu cloudu s Azure Active Directory (Azure AD).

Můžete získat hello při integraci SAP Business objektu cloudu s Azure AD následující výhody:

- Ve službě Azure AD můžete řídit, kdo má přístup tooSAP obchodní objekt cloudu.
- Vaši uživatelé tooSAP obchodní objekt cloudu můžete automaticky přihlásit pomocí jednotného přihlašování a účtu uživatele Azure AD.
- Můžete spravovat své účty pomocí nich centrální umístění, hello portálu Azure.

toolearn Další informace o softwaru, služba (SaaS) aplikace integraci s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

tooset až integrace Azure AD s SAP Business objektu cloudu, je třeba hello následující položky:

- Předplatné služby Azure AD
- SAP Business objektu cloudu, pomocí jednotného přihlašování zapnutý

> [!NOTE]
> Pokud testujete hello kroky v tomto kurzu, doporučujeme, abyste si je vyzkoušeli v provozním prostředí.

Doporučení pro testování hello kroky v tomto kurzu:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. 

Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidejte SAP Business objektu cloudu z Galerie hello.
2. Nastavte a otestujte Azure AD jednotné přihlašování.

## <a name="add-sap-business-object-cloud-from-hello-gallery"></a>Přidat SAP Business objektu cloudu z Galerie hello
tooset až hello integrace SAP cloudu obchodní objekt s Azure AD v galerii hello přidat SAP Business objektu cloudu tooyour seznam spravovaných aplikací SaaS.

tooadd SAP Business objektu cloudu z Galerie hello:

1. V hello [portál Azure](https://portal.azure.com), v levé nabídce text hello, vyberte **Azure Active Directory**. 

    ![tlačítko Azure Active Directory Hello][1]

2. Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.

    ![stránka aplikace Hello Enterprise][2]
    
3. tooadd novou aplikaci, vyberte **novou aplikaci**.

    ![tlačítko nové aplikace Hello][3]

4. Hello vyhledávacího pole zadejte **SAP Business objektu cloudu**.

    ![Hello vyhledávacího pole](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_search.png)

5. Na panelu výsledků hello vyberte **SAP Business objektu cloudu**a potom vyberte **přidat**.

    ![SAP Business objektu cloudu v seznamu výsledků hello](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_addfromgallery.png)

##  <a name="set-up-and-test-azure-ad-single-sign-on"></a>Nastavte a otestujte Azure AD jednotné přihlašování

V této části můžete nastavit a testovací Azure AD jednotného přihlášení SAP Business objektu cloudu podle testovacího uživatele s názvem *Britta Simon*.

Pro toowork jeden přihlašování musí Azure AD tooknow hello Azure AD příslušného uživatele v cloudu objekt SAP Business. Je nutné vytvořit vztah propojení mezi uživatele Azure AD a související uživatelské hello v SAP Business objektu cloudu.

tooestablish hello propojení vztahu v rámci cloudu objekt SAP Business, pro **uživatelské jméno**, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD.

tooconfigure a testování Azure AD jednotného přihlášení SAP Business objektu cloudu, dokončení hello následující úlohy:

1. [Nastavení Azure AD jednotné přihlašování](#set-up-azure-ad-single-sign-on). Nastaví uživatele toouse tuto funkci.
2. [Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user). Testy Azure AD jednotné přihlašování s uživatelem hello Britta Simon.
3. [Vytvořit testovací uživatele s SAP Business objektu cloudu](#create-an-sap-business-object-cloud-test-user). Vytvoří protějšek Britta Simon SAP Business objektu cloudu, který je propojený toohello reprezentace hello uživatele Azure AD.
4. [Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user). Nastaví Britta Simon toouse Azure AD jednotné přihlašování.
5. [Test jednotného přihlašování](#test-single-sign-on). Ověřuje, že konfigurace hello funguje.

### <a name="set-up-azure-ad-single-sign-on"></a>Nastavení Azure AD jednotné přihlašování

V této části můžete zapnout jedním Azure AD přihlášení v hello portálu Azure. Potom nastavíte jednotné přihlašování v aplikaci SAP Business objektu cloudu.

tooset do Azure AD jednotné přihlašování s SAP Business objektu cloudu:

1. V portálu Azure, na hello hello **SAP Business objektu cloudu** stránky integrace aplikací, vyberte **jednotného přihlašování**.

    ![Vybrat jednotné přihlašování][4]

2. Na hello **jednotného přihlašování** stránky, pro **režimu**, vyberte **na základě SAML přihlašování**.
 
    ![Vyberte na základě SAML přihlášení](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_samlbase.png)

3. V části **SAP Business objekt cloudové domény a adresy URL**, dokončení hello následující kroky:

    1. V hello **přihlašovací adresa URL** zadejte adresu URL, která má následující vzor hello: 
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    2. V hello **identifikátor** zadejte adresu URL, která má následující vzor hello:
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    ![Adresy URL stránky SAP Business objekt cloudové domény a adresy URL](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_url.png)
 
    > [!NOTE] 
    > Hello hodnoty v těchto adres URL jsou pouze jako ukázka. Aktualizace hodnoty hello hello skutečné přihlašování adresy URL a identifikátoru adresy URL. tooget hello přihlašovací adresa URL, kontaktujte hello [tým podpory SAP Business objektu cloudu klienta](https://www.sap.com/product/analytics/cloud-analytics.support.html). Hello identifikátoru adresy URL můžete získat tak, že stáhnete hello SAP Business objektu cloudu metadat z konzoly Správce hello. To se vysvětluje dále v kurzu hello. 

4. V části **SAML podpisový certifikát**, vyberte **soubor XML s metadaty**. Uložte soubor metadat hello ve vašem počítači.

    ![Vyberte soubor XML s metadaty](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_certificate.png) 

5. Vyberte **Uložit**.

    ![Vyberte Uložit.](./media/active-directory-saas-sapboc-tutorial/tutorial_general_400.png)

6. V okně prohlížeče jiný web Přihlaste se jako správce v tooyour SAP Business objektu cloudu na webu společnosti.

7. Vyberte **nabídky** > **systému** > **správy**.
    
    ![Vyberte nabídky, pak systém a Správa](./media/active-directory-saas-sapboc-tutorial/config1.png)

8. Na hello **zabezpečení** karty, vyberte hello **upravit** ikona (pera).
    
    ![Na kartě zabezpečení hello vyberte ikonu pro úpravu hello](./media/active-directory-saas-sapboc-tutorial/config2.png)  

9. Pro **metodu ověřování**, vyberte **SAML jednotné přihlašování (SSO)**.

    ![Vyberte SAML jednotné přihlašování pro metodu ověřování hello](./media/active-directory-saas-sapboc-tutorial/config3.png)  

10. toodownload hello služby metadat zprostředkovatelů (krok 1), vyberte **Stáhnout**. V souboru metadat hello, najít a zkopírovat hello **entityID** hodnotu. V hello Azure portálu pod **SAP Business objekt cloudové domény a adresy URL**, vložte hodnotu hello hello **identifikátor** pole.

    ![Zkopírujte a vložte hodnotu entityID hello](./media/active-directory-saas-sapboc-tutorial/config4.png)  

11. tooupload hello služby zprostředkovatele metadat (krok 2) v hello souboru, který jste stáhli z hello portál Azure, v části **nahrát zprostředkovatele Identity metadata**, vyberte **nahrát**.  

    ![V části metadata nahrát zprostředkovatele Identity vyberte možnost odeslat](./media/active-directory-saas-sapboc-tutorial/config5.png)

12. V hello **atribut uživatele** seznamu, vyberte hello atribut uživatele (krok 3), chcete toouse týkající se vaší implementace. Tento atribut uživatele mapuje toohello zprostředkovatele identity. tooenter vlastní atribut na stránku hello uživatele, použijte hello **vlastní mapování SAML** možnost. Nebo můžete vybrat buď **e-mailu** nebo **ID uživatele** jako atribut uživatele hello. V našem příkladu jsme vybrali **e-mailu** vzhledem k tomu můžeme namapované hello deklarace identifikátoru uživatele s hello **userprincipalname** atribut v hello **uživatelské atributy** část v hello Portál Azure. To poskytuje e-mail jedinečný uživatele, který se odešle toohello SAP Business objekt cloudových aplikací v každé úspěšné odpovědi SAML.

    ![Vyberte atribut uživatele](./media/active-directory-saas-sapboc-tutorial/config6.png)

13. účet hello tooverify s hello zprostředkovatele identity (krok 4), v hello **přihlašovací pověření (e-mailu)** zadejte hello uživatele e-mailovou adresu. Pak vyberte **ověřte účet**. Hello systému přidá přihlašovací údaje toohello uživatelský účet.

    ![Zadejte e-mailu a vyberte účet, ověřte](./media/active-directory-saas-sapboc-tutorial/config7.png)

14. Vyberte hello **Uložit** ikonu.

    ![Ikonu Uložit](./media/active-directory-saas-sapboc-tutorial/save.png)

> [!TIP]
> Můžete si přečíst stručným verzi tyto pokyny v hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace! Po přidání aplikace hello výběrem **služby Active Directory** > **podnikové aplikace, které**, vyberte hello **jednotné přihlašování** kartě. Můžete získat přístup k dokumentaci hello vložených v hello **konfigurace** oddíl na hello dolní části stránky hello. Další informace najdete v tématu [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD
V této části vytvoříte testovacího uživatele s názvem Britta Simon v hello portálu Azure.

toocreate testovacího uživatele ve službě Azure AD:

1. V hello portál Azure, v levé nabídce hello, vyberte **Azure Active Directory**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sapboc-tutorial/create_aaduser_01.png) 

2. toodisplay hello seznam uživatelů, vyberte **uživatelů a skupin**a potom vyberte **všichni uživatelé**.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sapboc-tutorial/create_aaduser_02.png) 

3. tooopen hello **uživatele** dialogové okno, vyberte **přidat**.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sapboc-tutorial/create_aaduser_03.png) 

4. V hello **uživatele** dialogové okno, dokončení hello následující kroky:
 
    1. V hello **název** zadejte **BrittaSimon**.

    2. V hello **uživatelské jméno** zadejte hello e-mailovou adresu uživatele hello Britta Simon.

    3. Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.

    4. Vyberte **Vytvořit**.

        ![Dialogové okno uživatelského Hello](./media/active-directory-saas-sapboc-tutorial/create_aaduser_04.png) 

    ![Vytvořit uživatele Azure AD][100]

### <a name="create-an-sap-business-object-cloud-test-user"></a>Vytvořit testovací uživatele s SAP Business objektu cloudu

Azure AD Uživatelé musí být zřízená v cloudu objekt SAP Business, než se můžete přihlásit tooSAP obchodní objekt cloudu. V cloudu objekt SAP Business zřizování je ruční úloha.

tooprovision uživatelský účet:

1. Přihlaste se tooyour SAP Business objektu cloudu na webu společnosti jako správce.

2. Vyberte **nabídky** > **zabezpečení** > **uživatelé**.

    ![Můžete přidat zaměstnance](./media/active-directory-saas-sapboc-tutorial/user1.png)

3. Na hello **uživatelé** , tooadd nové podrobnosti uživatele, vyberte  **+** . 

    ![Stránka Přidat uživatele](./media/active-directory-saas-sapboc-tutorial/user4.png)

    Dokončete hello následující kroky:

    1. V hello **ID uživatele** zadejte ID uživatele hello hello uživatele, jako je třeba **Britta**.

    2. V hello **KŘESTNÍ jméno** zadejte hello křestní jméno uživatele hello, jako je třeba **Britta**.

    3. V hello **příjmení** zadejte příjmení hello hello uživatele, jako je třeba **Simon**.

    4. V hello **ZOBRAZOVANÝ název** zadejte úplný název hello hello uživatele, jako je třeba **Britta Simon**.

    5. V hello **e-mailu** zadejte hello e-mailovou adresu uživatele hello, jako je třeba  **brittasimon@contoso.com** .

    6. Na hello **vybrat role** vyberte hello vhodnou roli pro uživatele hello a potom vyberte **OK**.

      ![Výběr role](./media/active-directory-saas-sapboc-tutorial/user3.png)

    7. Vyberte hello **Uložit** ikonu.  


### <a name="assign-hello-azure-ad-test-user"></a>Přiřadit hello Azure AD testovacího uživatele

V této části můžete povolit uživateli hello Britta Simon toouse Azure AD jednotné přihlašování, poskytněte hello uživatele účtu přístup tooSAP obchodní objekt cloudu.

tooassign tooSAP Britta Simon obchodní objekt cloudu:

1. V hello portálu Azure otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení. Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikace hello vyberte **SAP Business objektu cloudu**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_app.png) 

3. V levé nabídce hello, vyberte **uživatelů a skupin**.

    ![Vyberte uživatele a skupiny][202] 

4. Vyberte **Přidat**. Potom na hello **přidat přiřazení** vyberte **uživatelů a skupin**.

    ![stránka Přidat přiřazení Hello][203]

5. Na hello **uživatelů a skupin** v hello seznam uživatelů, vyberte na stránce **Britta Simon**.

6. Na hello **uživatelů a skupin** vyberte **vyberte**.

7. Na hello **přidat přiřazení** vyberte **přiřadit**.

![Přiřadit role uživatele hello][200] 
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části otestovat vaše konfigurace Azure AD jeden přihlašování pomocí hello přístupového panelu.

Když vyberete dlaždici SAP Business objektu cloudu hello panelu hello přístup, musí být automaticky přihlášeni tooyour SAP Business objekt cloudových aplikací.

Další informace o hello přístupového panelu najdete v tématu [Úvod toohello přístupový panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů toointegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_203.png

