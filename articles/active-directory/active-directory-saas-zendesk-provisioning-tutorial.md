---
title: "Kurz: Konfigurace služby ZenDesk pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure Active Directory tooautomatically zřídit a deaktivace zřízení uživatelských účtů tooZenDesk."
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
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: 200e8790ec1755f5cf927274ceb38527dd993f3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-zendesk-for-automatic-user-provisioning"></a>Kurz: Konfigurace služby ZenDesk pro zřizování automatické uživatelů


cílem Hello tohoto kurzu je tooshow hello kroky nutné tooperform v Zendesku a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooZenDesk Azure AD. 

## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klienta služby Azure Active directory
*   Klientovi služby ZenDesk s hello [plánu podnikového](https://www.zendesk.com/product/pricing/) nebo lépe povolena. 
*   Uživatelský účet v Zendesku se oprávnění správce 

> [!NOTE]
> Hello Azure AD zřizování integrace spoléhá na hello [ZenDesk REST API](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), která je k dispozici tooZenDesk týmy hello základní plán nebo vyšší.

## <a name="assigning-users-toozendesk"></a>Přiřazení uživatelů tooZenDesk

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele jsou synchronizovány pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD. 

Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide, jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci služby ZenDesk tooyour. Jakmile se rozhodli, můžete přiřadit tyto aplikace ZenDesk tooyour uživatelů podle pokynů hello zde:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toozendesk"></a>Důležité tipy pro přiřazení uživatelů tooZenDesk

*   Dále je doporučeno jednoho uživatele Azure AD je přiřazen hello tootest tooZenDesk zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování tooZenDesk uživatele, je nutné vybrat buď hello **uživatele** role nebo jinou platnou specifické pro aplikaci rolí (Pokud je k dispozici) v dialogovém okně přiřazení hello. Hello **výchozího přístupu k** role nefunguje pro zřizování a tito uživatelé se přeskočí.

> [!NOTE]
> Jako přidané funkce hello zřizování služby načte všechny vlastní role, které jsou definované v Zendesku a naimportuje je do Azure AD, kde mohou být vybrány v dialogovém okně vybrat roli hello. Tyto role se nebude zobrazovat v hello portál Azure po povolení hello zřizování služby a jeden synchronizační cyklus byla dokončena.

## <a name="configuring-user-provisioning-toozendesk"></a>Konfigurace tooZenDesk zřizování uživatelů 

Tato část vás provede připojením vaší služby Azure AD tooZenDesk uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v Zendesku podle přiřazení uživatelů a skupin ve službě Azure AD.

> [!TIP] 
> Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro ZenDesk, hello pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.


### <a name="configure-automatic-user-account-provisioning-toozendesk-in-azure-ad"></a>Konfigurace automatického uživatelského účtu zřizování tooZenDesk ve službě Azure AD


1. V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2. Pokud jste již nakonfigurovali ZenDesk pro jednotné přihlašování, vyhledávání pro instanci služby ZenDesk pomocí hello vyhledávací pole. Jinak vyberte možnost **přidat** a vyhledejte **ZenDesk** v galerii aplikací hello. Vyberte ZenDesk z výsledků hledání hello a přidejte ji tooyour seznam aplikací.

3. Vyberte instanci služby ZenDesk a pak vyberte hello **zřizování** kartě.

4. Sada hello **režimu zřizování** příliš**automatické**.

    ![Zřizování této služby](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk1.png)

5. V části hello **přihlašovací údaje správce** část, vstupní hello **uživatelské jméno správce & tokenkey & domény** generované účtu vaší ZenDesk (hello token můžete najít v rámci vašeho účtu: **správce**   >  **Rozhraní API** > **nastavení**). 

    ![Zřizování této služby](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk2.png)

6. V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour ZenDesk aplikaci. Pokud hello připojení selže, ujistěte se, že vašeho účtu ZenDesk má oprávnění správce a opakujte krok 5.

7. Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello "odesílat e-mailové oznámení, pokud dojde k chybě."

8. Klikněte na **Uložit**. 

9. V části hello části mapování, vyberte **synchronizaci uživatelů Azure Active Directory tooZenDesk**.

10. V hello **mapování atributů** , projděte si hello uživatelské atributy, které jsou synchronizované z tooZenDesk Azure AD. Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelské účty v Zendesku pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

11. tooenable hello zřizování služby Azure AD pro ZenDesk, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části

12. Klikněte na **Uložit**. 

Tato operace spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooZenDesk v hello uživatelé a skupiny oddílu. počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizováním služby.

Další informace o zřizování hello Azure AD tooread jak protokolů najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-enterprise-apps-manage-provisioning.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Další kroky

* [Zjistěte, jak tooreview protokoly a získat sestavy o zřizování aktivity](active-directory-saas-provisioning-reporting.md)
