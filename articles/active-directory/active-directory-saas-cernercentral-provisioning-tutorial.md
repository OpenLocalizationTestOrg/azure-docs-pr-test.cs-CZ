---
title: "Kurz: Konfigurace Cerner střed pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak Azure Active Directory tooautomatically tooconfigure zřídit soupisky tooa uživatelé v Cerner střed."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: asmalser-msft
ms.openlocfilehash: e96da98e783d24e7f34ae924824f909eead75f54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-cerner-central-for-automatic-user-provisioning"></a>Kurz: Konfigurace pro zřizování uživatelů automatické Cerner – střed

cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Cerner střed a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z Azure AD tooa uživatele soupisky in – střed Cerner. 


## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klient služby Azure Active Directory
*   Klient Cerner – střed 

> [!NOTE]
> Azure Active Directory se integruje s centrální Cerner pomocí hello [SCIM](http://www.simplecloud.info/) protokolu.

## <a name="assigning-users-toocerner-central"></a>Přiřazení uživatelů tooCerner – střed

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele jsou synchronizovány pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD. 

Než nakonfigurujete a povolíte hello zřizování služby, byste měli rozhodnout, jaké uživatelů nebo skupin ve službě Azure AD představují hello uživatelů, kteří potřebují přístup k tooCerner střed. Jakmile se rozhodli, můžete přiřadit tyto uživatele tooCerner – střed podle pokynů hello tady:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toocerner-central"></a>Důležité tipy pro přiřazení uživatelů tooCerner – střed

*   Dále je doporučeno jednoho uživatele Azure AD přiřadit hello centrální tootest tooCerner zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

* Po dokončení pro jednoho uživatele počáteční testování Cerner střed doporučuje přiřazení hello celý seznam uživatelů určený tooaccess soupisky žádné Cerner řešení (nikoli pouze Cerner střed) toobe zřízený tooCerner na uživatele.  Jiná řešení Cerner využívají tento seznam uživatelů v soupisky hello uživatele.

*   Při přiřazování uživatelů tooCerner – střed, je nutné vybrat hello **uživatele** role v dialogovém okně přiřazení hello. Uživatelé s rolí "Výchozí přístup" hello jsou vyloučeny z zřizování.


## <a name="configuring-user-provisioning-toocerner-central"></a>Konfigurace uživatele zřizování tooCerner – střed

Tato část vás provede připojením soupisky střed vaší služby Azure AD tooCerner uživatele pomocí rozhraní API pro zřizování na Cerner SCIM uživatelského účtu a konfiguraci hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatele na základě účtů v Cerner – střed na přiřazení uživatelů a skupin ve službě Azure AD.

> [!TIP]
> Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro Cerner ústředí, hello pokynů uvedených v [portál Azure (https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce vzájemně doplňují. Další informace najdete v tématu hello [Cerner střed jeden přihlašování kurzu](active-directory-saas-cernercentral-tutorial.md).


### <a name="tooconfigure-automatic-user-account-provisioning-toocerner-central-in-azure-ad"></a>tooconfigure automatické uživatelský účet zřizování tooCerner střed ve službě Azure AD:


V pořadí tooprovision uživatelské účty tooCerner – střed budete potřebovat toorequest účet Cerner střed systému z Cerner a vygenerování tokenu nosiče OAuth, Azure AD můžete použít tooconnect tooCerner SCIM koncový bod. Doporučujeme také, že integrace hello provést v prostředí izolovaného prostoru Cerner před nasazením tooproduction.

1.  prvním krokem Hello je osoby hello tooensure Správa hello Cerner a integrace Azure AD mají CernerCare účet, který je požadovaný tooaccess hello dokumentace nezbytné toocomplete hello pokyny. V případě potřeby používejte adresy URL hello níže toocreate CernerCare účty v každé příslušné prostředí.

   * Izolovaný prostor: https://sandboxcernercare.com/accounts/create

   * Produkční: https://cernercare.com/accounts/create  

2.  Účet systému dále musí být vytvořen pro Azure AD. Použijte pokyny hello níže toorequest účet systému pro vaše prostředí izolovaného prostoru a provozní.

   * Pokyny: https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account

   * Izolovaný prostor: https://sandboxcernercentral.com/system-accounts/

   * Produkční: https://cernercentral.com/system-accounts/

3.  V dalším kroku generovat tokenu nosiče OAuth pro všechny systémové účty. toodo se hello postupujte podle pokynů níže.

   * Pokyny: https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token

   * Izolovaný prostor: https://sandboxcernercentral.com/system-accounts/

   * Produkční: https://cernercentral.com/system-accounts/

4. Nakonec můžete ID sféry soupisky tooacquire uživatelů pro obě hello izolovaného prostoru a provozní prostředí v Cerner toocomplete hello konfiguraci. Informace o tom tooacquire, najdete v tématu: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM. 

5. Teď můžete konfigurovat Azure AD tooprovision uživatelské účty tooCerner. Přihlaste se toohello [portál Azure](https://portal.azure.com)a vyhledejte toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

6. Pokud jste již nakonfigurovali Cerner střed pro jednotné přihlašování, vyhledejte instanci Cerner střed pomocí hello vyhledávací pole. Jinak vyberte možnost **přidat** a vyhledejte **Cerner střed** v galerii aplikací hello. Vyberte Cerner střed z výsledků hledání hello a přidejte ji tooyour seznam aplikací.

7.  Vyberte instanci Cerner střed a pak vyberte hello **zřizování** kartě.

8.  Sada hello **režimu zřizování** příliš**automatické**.

   ![Střed Cerner zřizování](./media/active-directory-saas-cernercentral-provisioning-tutorial/Cerner.PNG)

9.  Vyplňte následující pole v části hello **přihlašovací údaje správce**:

   * V hello **URL klienta** pole, zadejte adresu URL ve formátu hello níže nahraďte "User-soupisky-sféry-ID" s ID sféry hello jste získali v kroku #4.

> Izolovaný prostor: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

> Produkční: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

   * V hello **tajný klíč tokenu** pole, zadejte tokenu nosiče OAuth hello jste vygenerovali v kroku #3 a klikněte na **Test připojení**.

   * Měli byste vidět úspěšné oznámení na straně ÚETUChcete hello portálu.

10. Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello níže.

11. Klikněte na **Uložit**. 

12. V hello **mapování atributů** , projděte si hello uživatele a skupiny toobe atributy synchronizované z Azure AD tooCerner střed. Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty a skupiny v centrální Cerner pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

13. tooenable hello zřizování služby Azure AD pro Cerner ústředí, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části

14. Klikněte na **Uložit**. 

Tím se spustí počáteční synchronizaci hello všechny uživatele nebo skupiny přiřazené tooCerner střed v části Uživatelé a skupiny hello. počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud běží hello zřizování služby Azure AD. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci Cerner střed.

Další informace o zřizování hello Azure AD tooread jak protokolů najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).

## <a name="additional-resources"></a>Další zdroje

* [Střed Cerner: Publikování dat identity pomocí služby Azure AD](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD)
* [Kurz: Konfigurace Cerner střed pro jednotné přihlašování s Azure Active Directory](active-directory-saas-cernercentral-tutorial.md)
* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-enterprise-apps-manage-provisioning.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Další kroky
* [Zjistěte, jak tooreview protokoly a sestavy get na zřizování aktivity](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).
