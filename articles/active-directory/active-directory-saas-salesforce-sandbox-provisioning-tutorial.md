---
title: "Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a izolovaného prostoru služby Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bab73fda-6754-411d-9288-f73ecdaa486d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: 06ff50050845383a602b0edd6fca953ddd37cebd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-salesforce-sandbox-for-automatic-user-provisioning"></a>Kurz: Konfigurace služby Salesforce izolovaného prostoru pro zřizování automatické uživatelů

cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v izolovaného prostoru Salesforce a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z Azure AD tooSalesforce izolovaného prostoru.

## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klienta služby Azure Active directory.
*   Pro služby Salesforce izolovaného prostoru pro pracovní nebo Salesforce izolovaného prostoru pro vzdělávací organizace musí mít platný klienta. Bezplatný zkušební účet můžete použít buď služby.
*   Uživatelský účet v izolovaném prostoru Salesforce s oprávněními správce týmu.

## <a name="assigning-users-toosalesforce-sandbox"></a>Přiřazení uživatelů tooSalesforce izolovaného prostoru

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele jsou synchronizovány pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.

Před konfigurací a povolení hello zřizování služby, musíte toodecide jaké uživatelů nebo skupin ve službě Azure AD představují hello uživatele, kteří potřebují přístup k tooyour aplikace Salesforce izolovaného prostoru. Jakmile se rozhodli, můžete přiřadit tyto uživatele tooyour aplikace Salesforce izolovaného prostoru podle pokynů hello tady:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toosalesforce-sandbox"></a>Důležité tipy pro přiřazení uživatelů tooSalesforce izolovaného prostoru

* Dále je doporučeno jednoho uživatele Azure AD je přiřazen tooSalesforce izolovaného prostoru tootest hello zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

* Při přiřazování uživatelů tooSalesforce izolovaného prostoru, musíte vybrat platné uživatelské role. role "Výchozí přístup" Hello nefunguje pro zřizování.

> [!NOTE]
> Tato aplikace importuje vlastní role ze služby Salesforce izolovaného prostoru v rámci hello zřizování, který hello může zákazník tooselect při přiřazování uživatelů.

## <a name="enable-automated-user-provisioning"></a>Povolit automatické zřizování uživatelů

Tato část vás provede připojením izolovaného vaší služby Azure AD tooSalesforce uživatelský účet zřizování rozhraní API a konfiguraci hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v izolovaném prostoru Salesforce na základě uživatele a skupiny přiřazení ve službě Azure AD.

>[!Tip]
>Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro izolovaný prostor Salesforce, hello pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="tooconfigure-automatic-user-account-provisioning"></a>tooconfigure automatické uživatel účet zřizování:

Hello cílem této části je toooutline jak tooenable zřizování uživatelů služby Active Directory uživatele účtů tooSalesforce izolovaného prostoru.

1. V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali Salesforce izolovaného prostoru pro jednotné přihlašování, vyhledávání pro instanci služby Salesforce izolovaného prostoru pomocí hello vyhledávací pole. Jinak vyberte možnost **přidat** a vyhledejte **izolovaného prostoru Salesforce** v galerii aplikací hello. Vyberte izolovaného prostoru Salesforce z výsledků hledání hello a přidejte ji tooyour seznam aplikací.

3. Vyberte instanci služby Salesforce izolovaného prostoru a pak vyberte hello **zřizování** kartě.

4. Sada hello **režimu zřizování** příliš**automatické**. 
    ![Zřizování](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/provisioning.png)

5. V části hello **přihlašovací údaje správce** části, zadejte následující nastavení konfigurace hello:
   
    a. V hello **uživatelské jméno správce** textovému poli, zadejte název, který má hello účtu Salesforce izolovaného **správce systému** profil v Salesforce.com přiřazen.
   
    b. V hello **heslo správce** textovému poli, zadejte hello heslo pro tento účet.

6. tooget vašem tokenu zabezpečení izolovaného prostoru služby Salesforce, otevřete novou kartu a přihlásit hello stejný účet správce izolovaného prostoru služby Salesforce. Na hello pravém horním rohu stránky hello, klikněte na své jméno a potom klikněte na **Moje nastavení**.

     ![Povolit automatické uživatele zajišťování](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "povolit zřizování automatické uživatelů")
7. V levém navigačním podokně hello, klikněte na tlačítko **osobní** tooexpand hello související části a pak klikněte na **resetovat Moje zabezpečení tokenu**.
  
    ![Povolit automatické uživatele zajišťování](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "povolit zřizování automatické uživatelů")
8. Na hello **resetovat Moje zabezpečení tokenu** klikněte na tlačítko hello **resetovat tokenu zabezpečení** tlačítko.

    ![Povolit automatické uživatele zajišťování](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "povolit zřizování automatické uživatelů")
9. Zkontrolujte e-mailovou schránku hello spojené s tímto účtem správce. Vyhledejte e-mailu ze služby Salesforce Sandbox.com, který obsahuje hello nový token zabezpečení.
10. Zkopírujte hello tokenu, přejděte tooyour okno Azure AD a vložte ho do hello **soketu tokenu** pole.

11. V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour izolovaného prostoru Salesforce aplikace.

12. V hello **e-mailové oznámení** zadejte hello e-mailovou adresu uživatele nebo skupiny, kdo by měly dostávat oznámení zřizování chyby a zaškrtněte políčko hello.

13. Klikněte na tlačítko **uložit.**  
    
14.  V části hello části mapování, vyberte **synchronizaci uživatelů Azure Active Directory tooSalesforce izolovaného prostoru.**

15. V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z Azure AD tooSalesforce izolovaného prostoru. Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Salesforce izolovaného prostoru pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

16. tooenable hello zřizování služby Azure AD pro izolovaný prostor Salesforce, změna hello **Stav zřizování** příliš**na** v části Nastavení hello

17. Klikněte na tlačítko **uložit.**


Spustí počáteční synchronizaci hello všechny uživatele nebo skupiny přiřazené tooSalesforce izolovaného prostoru v části Uživatelé a skupiny hello. počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby v aplikaci Salesforce izolovaného prostoru.

Nyní můžete vytvořit testovací účet. Počkejte, až minut too20 tooverify, který hello účet byl synchronizován toosalesforce.

## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-salesforcesandbox-tutorial.md)