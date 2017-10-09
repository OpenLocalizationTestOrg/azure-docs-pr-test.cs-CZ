---
title: 'Kurz: Azure Active Directory integrace s Salesforce | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a služby Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: a916be8dbf0b4c6173cda873936a53cd1f3ff12b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-salesforce-for-automatic-user-provisioning"></a>Kurz: Konfigurace služby Salesforce pro zřizování automatické uživatelů

cílem Hello tohoto kurzu je tooshow hello kroky požadované tooperform v Salesforce a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooSalesforce Azure AD.

## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klienta služby Azure Active directory.
*   Pro pracovní nebo Salesforce pro vzdělávací organizace musí mít platný klient pro služby Salesforce. Bezplatný zkušební účet můžete použít buď služby.
*   Uživatelský účet v Salesforce s oprávněními správce týmu.

## <a name="assigning-users-toosalesforce"></a>Přiřazení uživatelů tooSalesforce

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.

Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci Salesforce tooyour. Jakmile se rozhodli, můžete přiřadit aplikaci Salesforce tooyour těchto uživatelů podle pokynů hello tady:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toosalesforce"></a>Důležité tipy pro přiřazení uživatelů tooSalesforce

*   Dále je doporučeno jednoho uživatele Azure AD je přiřazen hello tootest tooSalesforce zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

*  Při přiřazování tooSalesforce uživatele, musíte vybrat platné uživatelské role. role "Výchozí přístup" Hello nefunguje pro zřizování

    > [!NOTE]
    > Tato aplikace importuje vlastní role ze služby Salesforce jako součást hello zřizování, který hello může zákazník tooselect při přiřazování uživatelů

## <a name="enable-automated-user-provisioning"></a>Povolit automatické zřizování uživatelů

Tato část vás provede připojením vaší služby Azure AD tooSalesforce uživatelský účet zřizování rozhraní API a konfiguraci hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Salesforce podle přiřazení uživatelů a skupin ve službě Azure AD .

>[!Tip]
>Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro služby Salesforce, hello pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="tooconfigure-automatic-user-account-provisioning"></a>tooconfigure automatické uživatel účet zřizování:

Hello cílem této části je toooutline jak tooenable zřizování uživatelů služby Active Directory uživatele tooSalesforce účty.

1. V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali Salesforce pro jednotné přihlašování, vyhledávání pro instanci služby Salesforce pomocí hello vyhledávací pole. Jinak vyberte možnost **přidat** a vyhledejte **Salesforce** v galerii aplikací hello. Vyberte Salesforce z výsledků hledání hello a přidejte ji tooyour seznam aplikací.

3. Vyberte instanci služby Salesforce a pak vyberte hello **zřizování** kartě.

4. Sada hello **režimu zřizování** příliš**automatické**. 
![Zřizování](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)

5. V části hello **přihlašovací údaje správce** části, zadejte následující nastavení konfigurace hello:
   
    a. V hello **uživatelské jméno správce** textovému poli, typ Salesforce účet název, který má hello **správce systému** profil v Salesforce.com přiřazen.
   
    b. V hello **heslo správce** textovému poli, zadejte hello heslo pro tento účet.

6. tooget vašem tokenu zabezpečení služby Salesforce, otevřete novou kartu a přihlášení do hello stejný účet správce služby Salesforce. Na hello pravém horním rohu stránky hello, klikněte na své jméno a potom klikněte na **Moje nastavení**.

     ![Povolit automatické uživatele zajišťování](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "povolit zřizování automatické uživatelů")
7. V levém navigačním podokně hello, klikněte na tlačítko **osobní** tooexpand hello související části a pak klikněte na **resetovat Moje zabezpečení tokenu**.
  
    ![Povolit automatické uživatele zajišťování](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "povolit zřizování automatické uživatelů")
8. Na hello **resetovat Moje zabezpečení tokenu** klikněte na tlačítko **resetovat tokenu zabezpečení** tlačítko.

    ![Povolit automatické uživatele zajišťování](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "povolit zřizování automatické uživatelů")
9. Zkontrolujte e-mailovou schránku hello spojené s tímto účtem správce. Vyhledejte e-mailu z Salesforce.com, který obsahuje hello nový token zabezpečení.
10. Zkopírujte hello tokenu, přejděte tooyour okno Azure AD a vložte ho do hello **soketu tokenu** pole.

11. V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit aplikaci tooyour Salesforce.

12. V hello **e-mailové oznámení** zadejte hello e-mailovou adresu uživatele nebo skupiny, kdo by měly dostávat oznámení zřizování chyby a zaškrtněte políčko hello níže.

13. Klikněte na tlačítko **uložit.**  
    
14.  V části hello části mapování, vyberte **tooSalesforce synchronizaci uživatelů Azure Active Directory.**

15. V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooSalesforce Azure AD. Všimněte si, že hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Salesforce pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

16. tooenable hello zřizování služby Azure AD pro služby Salesforce, změna hello **Stav zřizování** příliš**na** v části Nastavení hello

17. Klikněte na tlačítko **uložit.**

Tím se spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooSalesforce v hello uživatelé a skupiny oddílu. Všimněte si, že počáteční synchronizace hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci Salesforce.

Nyní můžete vytvořit testovací účet. Počkejte, až minut too20 tooverify, který hello účet byl synchronizován tooSalesforce.

## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-salesforce-tutorial.md)