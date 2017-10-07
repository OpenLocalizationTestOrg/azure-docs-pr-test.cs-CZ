---
title: "Kurz: Konfigurace Slack pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure Active Directory tooautomatically zřídit a deaktivace zřízení uživatelských účtů tooSlack."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: asmalser-msft
ms.reviewer: asmalser
ms.openlocfilehash: d0a565bbe0bd7e229b9dd99b1ebbaf67d93a2206
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-slack-for-automatic-user-provisioning"></a>Kurz: Konfigurace Slack pro zřizování automatické uživatelů


Hello cílem tohoto kurzu je tooshow hello kroky nutné tooperform Slack a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z tooSlack Azure AD. 

## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klienta služby Azure Active Active directory
*   Systému Slack klienta s hello [Plus plán](https://aadsyncfabric.slack.com/pricing) nebo lépe povolena. 
*   Uživatelský účet v systému Slack s oprávněními správce Team 

Poznámka: hello Azure AD zřizování integrace spoléhá na hello [Slack SCIM API](https://api.slack.com/scim) který je k dispozici tooSlack týmy na hello Plus plán nebo lepší.

## <a name="assigning-users-tooslack"></a>Přiřazení uživatelů tooSlack

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele se budou synchronizovat pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD. 

Než nakonfigurujete a povolíte hello zřizování služby, je nutné toodecide jaké uživatelů nebo skupin v Azure AD představují hello uživatelé, kteří potřebují přístup k aplikaci tooyour Slack. Jakmile se rozhodli, můžete přiřadit tyto aplikace Slack tooyour uživatelů podle pokynů hello tady:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-tooslack"></a>Důležité tipy pro přiřazení uživatelů tooSlack

*   Dále je doporučeno jednoho uživatele Azure AD přiřadit hello tootest tooSlack zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování tooSlack uživatele, je nutné vybrat hello **uživatele** nebo role "Skupina" v dialogovém okně přiřazení hello. role "Výchozí přístup" Hello nefunguje pro zřizování.


## <a name="configuring-user-provisioning-tooslack"></a>Konfigurace tooSlack zřizování uživatelů 

Tato část vás provede připojením vaší služby Azure AD tooSlack uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v systému Slack podle přiřazení uživatelů a skupin ve službě Azure AD.

**Tip:** můžete také tooenabled na základě SAML jednotné přihlašování pro Slack, hello pokynů uvedených v (portál Azure) [https://portal.azure.com]. Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce doplnění navzájem.


### <a name="tooconfigure-automatic-user-account-provisioning-tooslack-in-azure-ad"></a>tooconfigure automatické uživatelský účet zřizování tooSlack ve službě Azure AD:


1)  V hello [portál Azure](https://portal.azure.com), procházet toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

2) Pokud jste již nakonfigurovali Slack pro jednotné přihlašování, vyhledejte instanci Slack pomocí hello vyhledávací pole. Jinak vyberte možnost **přidat** a vyhledejte **Slack** v galerii aplikací hello. Vyberte Slack z výsledků hledání hello a přidejte ji tooyour seznam aplikací.

3)  Vyberte instanci systému Slack a pak vyberte hello **zřizování** kartě.

4)  Sada hello **režimu zřizování** příliš**automatické**.

![Slack zřizování](./media/active-directory-saas-slack-provisioning-tutorial/Slack1.PNG)

5)  V části hello **přihlašovací údaje správce** klikněte na tlačítko **Authorize**. Otevře se dialogové okno Slack autorizace v nové okno prohlížeče. 

6) V novém okně hello se přihlaste pomocí účtu správce Team Slack. v dialogu autorizace výsledné hello, vyberte hello Slack týmu, který má být tooenable zřizování pro a potom vyberte **Authorize**. Po dokončení, vrátí toohello Azure portálu toocomplete hello zřizování konfigurace.

![Dialogové okno autorizace](./media/active-directory-saas-slack-provisioning-tutorial/Slack3.PNG)

7) V hello portálu Azure, klikněte na **Test připojení** tooensure Azure AD můžete připojit tooyour Slack aplikaci. Pokud hello připojení nezdaří, zkontrolujte, zda že váš Slack účet má oprávnění správce týmu a hello "Ověřit" krok opakujte.

8) Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello níže.

9) Klikněte na **Uložit**. 

10) V části hello části mapování, vyberte **synchronizaci uživatelů Azure Active Directory tooSlack**.

11) V hello **mapování atributů** , projděte si hello uživatelské atributy, které se mají synchronizovat z tooSlack Azure AD. Všimněte si, že hello atributy vybrán jako **párování** vlastnosti budou použité toomatch hello uživatelské účty v systému Slack pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

12) tooenable hello zřizování služby Azure AD pro Slack, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části

13) Klikněte na **Uložit**. 

Tato akce spustí hello počáteční synchronizaci všech uživatelů a skupiny přiřazené tooSlack v hello uživatelé a skupiny oddílu. Všimněte si, že počáteční synchronizace hello bude trvat déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 10 minut, dokud se službou hello. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci Slack.

## <a name="optional-configuring-group-object-provisioning-tooslack"></a>[Nepovinné] Konfigurace objektu skupiny zřizování tooSlack 

Volitelně můžete povolit zajišťování hello objektů skupiny z tooSlack Azure AD. To se liší od "přiřazení skupiny uživatelů", v tomto objektu skutečné skupiny hello kromě tooits členy bude replikován z tooSlack Azure AD. Například pokud máte skupinu s názvem "Moje skupina" ve službě Azure AD, bude vytvořen identitical skupinu s názvem "Moje skupina" uvnitř Slack.

### <a name="tooenable-provisioning-of-group-objects"></a>tooenable zřizování objektů skupiny:

1) V části hello části mapování, vyberte **synchronizaci skupinám Azure Active Directory tooSlack**.

2) V okně hello mapování atributů nastavte tooYes povoleno.

3) V hello **mapování atributů** , projděte si hello skupiny atributy, které se mají synchronizovat z tooSlack Azure AD. Všimněte si, že hello atributy vybrán jako **párování** vlastnosti budou použité toomatch hello skupiny v systému Slack pro operace aktualizace. 

4) Klikněte na **Uložit**.

Tento výsledek v tooSlack objekty přiřazené žádné skupiny v hello **uživatelů a skupin** část plně synchronizovaných z tooSlack Azure AD. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci Slack.


## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-enterprise-apps-manage-provisioning.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
