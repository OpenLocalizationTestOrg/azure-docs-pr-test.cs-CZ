---
title: 'Kurz: Azure Active Directory integrace s Concur | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Concur."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: df47f55f-a894-4e01-a82e-0dbf55fc8af1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 13ba364af26a5ce0f1d2b51aaa0f84a4c353b107
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-concur-for-user-provisioning"></a>Kurz: Konfigurace vyústit pro zřizování uživatelů

cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Concur a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooConcur Azure AD.

## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klienta služby Azure Active directory.
*   Concur jednotné přihlašování povolené předplatné.
*   Uživatelský účet v Concur s oprávněními správce týmu.

## <a name="assigning-users-tooconcur"></a>Přiřazení uživatelů tooConcur

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele se synchronizují pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD.

Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci Concur tooyour. Jakmile se rozhodli, můžete přiřadit tyto aplikace Concur tooyour uživatelů podle pokynů hello zde:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-tooconcur"></a>Důležité tipy pro přiřazení uživatelů tooConcur

*   Dále je doporučeno jednoho uživatele Azure AD přiřadit hello tootest tooConcur zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování tooConcur uživatele, musíte vybrat platné uživatelské role. role "Výchozí přístup" Hello nefunguje pro zřizování.

## <a name="enable-user-provisioning"></a>Povolit zřizování uživatelů

Tato část vás provede připojením vaší služby Azure AD tooConcur uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Concur podle přiřazení uživatelů a skupin ve službě Azure AD.

> [!Tip] 
> Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro Concur, hello pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.

### <a name="tooconfigure-user-account-provisioning"></a>tooconfigure uživatel účet zřizování:

Hello cílem této části je toooutline jak tooenable zřizování služby Active Directory uživatelské účty tooConcur.

tooenable aplikací v hello služby výdaje, že má toobe správné nastavení a použití profilu Správce webu služby. Nepřidáte hello WS správce role tooyour stávající profil správce, můžete použít pro funkce správy T & E.

Vyústit konzultanty nebo správce klienta hello musíte vytvořit profil odlišné správce webové služby a správce klienta hello musí používat tento profil pro funkce služby správce webu hello (například povolení aplikace). Tyto profily musí být udržovány odděleně od správce klienta hello denní T & E profil správce (hello T & E správce profil by neměl mít přiřazenou roli WSAdmin hello).

Když vytvoříte toobe profil hello používá k povolení aplikace hello, zadejte do pole profil uživatele hello jméno správce klienta hello. Tím se přiřadí vlastnictví toohello profilu. Po vytvoření jednoho nebo více profilů hello klienta musí přihlásit se přes tento profil tooclick hello "*povolit*" tlačítko partnera aplikace v rámci nabídky hello webové služby.

Pro hello následující z důvodů by neměla provést tuto akci s hello profilu, které používají pro správu normální T & E.

* Hello má klient toobe hello jednu, která klikne na možnost "*Ano*" v dialogu okně hello, které se zobrazí po povolení aplikace. Klikněte na toto tlačítko uznává, že hello klienta je ochotni pro hello partnera aplikace tooaccess svá data, takže jste nebo hello partnera nelze kliknout, tlačítko Ano.

* Pokud správce klienta, která povolila aplikaci pomocí hello T & E správce profil odejde hello společnosti (výsledkem hello profil se deaktivovány), všechny aplikace povoleno pomocí tohoto profilu nefunguje, dokud aplikace hello je povolená s jinou aktivní správce WS profil. Z tohoto důvodu je mají odlišné profily WS správce toocreate.

* Pokud správce odejde hello společnosti, název hello související toohello profilu WS správce může být změněné toohello nahrazení správce, v případě potřeby bez dopadu na hello povoleno, že aplikace, protože není nutné tento profil deaktivovány.

**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**

1. Přihlaste se tooyour **Concur** klienta.

2. Z hello **správy** nabídce vyberte možnost **webové služby**.
   
    ![Klienta Concur](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur klienta")

3. Na levé straně, od hello hello **webové služby** podokně, vyberte **povolit aplikaci partnera**.
   
    ![Povolit aplikaci partnera](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "povolit partnera aplikace")

4. Z hello **povolit aplikaci** seznamu, vyberte **Azure Active Directory**a potom klikněte na **povolit**.
   
    ![Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")

5. Klikněte na tlačítko **Ano** tooclose hello **potvrďte akci** dialogové okno.
   
    ![Akci potvrďte](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "potvrzení akce")

6. V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

7. Pokud jste již nakonfigurovali Concur pro jednotné přihlašování, vyhledejte instanci Concur pomocí hello vyhledávací pole. Jinak vyberte možnost **přidat** a vyhledejte **Concur** v galerii aplikací hello. Vyberte Concur z výsledků hledání hello a přidejte ji tooyour seznam aplikací.

8. Vyberte instanci Concur a pak vyberte hello **zřizování** kartě.

9. Sada hello **režimu zřizování** příliš**automatické**. 
 
    ![Zřizování](./media/active-directory-saas-concur-provisioning-tutorial/provisioning.png)

10. V části hello **přihlašovací údaje správce** zadejte hello **uživatelské jméno** a hello **heslo** vaše Concur správce.

11. V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour Concur aplikaci. Pokud hello připojení selže, zajistěte, aby byl váš účet Concur oprávnění správce týmu.

12. Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello.

13. Klikněte na tlačítko **uložit.**

14. V části hello části mapování, vyberte **tooConcur synchronizaci uživatelů Azure Active Directory.**

15. V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooConcur Azure AD. Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Concur pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

16. tooenable hello zřizování služby Azure AD pro Concur, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části

17. Klikněte na tlačítko **uložit.**

Nyní můžete vytvořit testovací účet. Počkejte, až minut too20 tooverify, který hello účet byl synchronizován tooConcur.

## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurovat jednotné přihlašování](active-directory-saas-concur-tutorial.md)

