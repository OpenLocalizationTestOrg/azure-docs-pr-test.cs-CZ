---
title: "Kurz: Konfigurace LinkedIn Learning pro zřizování automatické uživatelů s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure Active Directory tooautomatically zřídit a deaktivace zřízení uživatelských účtů tooLinkedIn učení."
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
ms.date: 04/15/2017
ms.author: asmalser-msft
ms.openlocfilehash: e0503e09ab384723ffb73d6ef1df8be6abfc9294
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-linkedin-learning-for-automatic-user-provisioning"></a>Kurz: Konfigurace LinkedIn Learning pro zřizování automatické uživatelů


Hello cílem tohoto kurzu je tooshow hello kroky nutné tooperform v LinkedIn učení a Azure AD tooautomatically zřídit a deaktivace zřízení uživatelských účtů z Azure AD tooLinkedIn učení. 

## <a name="prerequisites"></a>Požadavky

Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:

*   Klient služby Azure Active Directory
*   Klienta LinkedIn učení 
*   Účet správce v LinkedIn Learning s toohello přístup k centru účtů LinkedIn

> [!NOTE]
> Azure Active Directory se integruje s LinkedIn Learning pomocí hello [SCIM](http://www.simplecloud.info/) protokolu.

## <a name="assigning-users-toolinkedin-learning"></a>Přiřazení uživatelů tooLinkedIn učení

Azure Active Directory používá koncept názvem "přiřazení" toodetermine uživatelů, kteří obdrželi přístup tooselected aplikace. V kontextu hello zřizování účtu automatické uživatele se budou synchronizovat pouze hello uživatelů a skupin, které byly "přiřazeny" tooan aplikace ve službě Azure AD. 

Než nakonfigurujete a povolíte hello zřizování služby, budete potřebovat toodecide kterými se uživatelé nebo skupiny ve službě Azure AD představují hello uživatele, kteří potřebují přístup k tooLinkedIn učení. Jakmile se rozhodli, můžete přiřadit tyto uživatele tooLinkedIn učení podle pokynů hello tady:

[Přiřadit uživatele nebo skupinu tooan firemní aplikace](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toolinkedin-learning"></a>Důležité tipy pro přiřazení uživatelů tooLinkedIn učení

*   Dále je doporučeno jednoho uživatele Azure AD přiřadit tooLinkedIn Learning tootest hello zřizování konfigurace. Další uživatele nebo skupiny může být přiřazen později.

*   Při přiřazování uživatelů tooLinkedIn učení, je nutné vybrat hello **uživatele** role v dialogovém okně přiřazení hello. role "Výchozí přístup" Hello nefunguje pro zřizování.


## <a name="configuring-user-provisioning-toolinkedin-learning"></a>Zřizování tooLinkedIn konfiguraci uživatelů učení

Tato část vás provede připojením Learning vaší služby Azure AD tooLinkedIn SCIM uživatelský účet zřizování rozhraní API a konfigurace hello zřizování služby toocreate, aktualizovat a zakázat přiřazené uživatelské účty v LinkedIn Learning na základě uživatele a skupiny přiřazení ve službě Azure AD.

> [!TIP]
> Můžete také zvolit tooenabled na základě SAML jednotné přihlašování pro LinkedIn učení se hello pokynů uvedených v [portál Azure](https://portal.azure.com). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatické zřizování, i když tyto dvě funkce vzájemně doplňují.


### <a name="tooconfigure-automatic-user-account-provisioning-toolinkedin-learning-in-azure-ad"></a>tooconfigure automatické uživatelský účet zřizování tooLinkedIn učení v Azure AD:


prvním krokem Hello je tooretrieve LinkedIn přístupový token. Pokud jste správce podnikové sítě, můžete zřídit samoobslužné přístupový token. V centru váš účet, přejděte příliš**nastavení &gt; globální nastavení** a otevřete hello **SCIM instalace** panelu.

> [!NOTE]
> Pokud se připojujete centra účtů hello přímo a nikoli v rámci odkaz, můžete dosáhnout pomocí následujících kroků hello.

1)  Přihlaste se tooAccount Center.

2)  Vyberte **správce &gt; nastavení správce** .

3)  Klikněte na tlačítko **Advanced integrace** na levém bočním panelu hello. Jste směrovanou toohello centra účtů.

4)  Klikněte na tlačítko **+ přidat novou konfiguraci SCIM** a použijte postup hello vyplněním každé pole.

> Když autoassign licencí není povolen, znamená to, že je synchronizovat pouze data uživatele.

![LinkedIn učení zřizování](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_1.PNG)

> Pokud je povoleno autolicense přiřazení, budete potřebovat toonote instanci aplikace a typ licence. Licence jsou přiřazeny na první přijde, nejprve sloužit základ, dokud jsou provedeny všechny licence hello.

![LinkedIn učení zřizování](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_2.PNG)

5)  Klikněte na tlačítko **vygenerovat token**. Měli byste vidět zobrazení tokenu přístupu v části hello **přístupový token** pole.

6)  Uložte přístup tokenu tooyour schránky nebo počítač před opuštěním stránky hello.

7) V dalším kroku přihlásit toohello [portál Azure](https://portal.azure.com)a vyhledejte toohello **Azure Active Directory > podnikové aplikace > všechny aplikace** části.

8) Pokud jste již nakonfigurovali LinkedIn Learning pro jednotné přihlašování, vyhledejte instanci LinkedIn Learning pomocí hello vyhledávací pole. Jinak vyberte možnost **přidat** a vyhledejte **LinkedIn Learning** v galerii aplikací hello. Vyberte LinkedIn Learning z výsledků hledání hello a přidejte ji tooyour seznam aplikací.

9)  Vyberte instanci LinkedIn učení a pak vyberte hello **zřizování** kartě.

10) Sada hello **režimu zřizování** příliš**automatické**.

![LinkedIn učení zřizování](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_3.PNG)

11)  Vyplňte následující pole v části hello **přihlašovací údaje správce** :

* V hello **URL klienta** zadejte https://api.linkedin.com.

* V hello **tajný klíč tokenu** pole, zadejte hello přístupový token jste vygenerovali v kroku 1 a klikněte na **Test připojení** .

* Měli byste vidět úspěšné oznámení na straně ÚETUChcete hello portálu.

12) Zadejte hello e-mailovou adresu uživatele nebo skupiny, který by měly dostávat oznámení zřizování Chyba v hello **e-mailové oznámení** pole a zaškrtněte políčko hello níže.

13) Klikněte na **Uložit**. 

14) V hello **mapování atributů** , projděte si atributy hello uživatelů a skupin, které se mají synchronizovat ze služby Azure AD tooLinkedIn učení. Všimněte si, že hello atributy vybrán jako **párování** vlastnosti budou použité toomatch hello uživatelské účty a skupiny v LinkedIn Learning pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

![LinkedIn učení zřizování](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_4.PNG)

15) tooenable hello zřizování služby Azure AD pro LinkedIn Learning, změna hello **Stav zřizování** příliš**na** v hello **nastavení** části

16) Klikněte na **Uložit**. 

Tato akce spustí počáteční synchronizaci hello všechny uživatele a skupiny přiřazené tooLinkedIn učení v části Uživatelé a skupiny hello. Všimněte si, že počáteční synchronizace hello bude trvat déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello. Můžete použít hello **podrobnosti synchronizace** části toomonitor průběh a postupujte podle pokynů odkazy tooprovisioning aktivity sestavy, které popisují všechny akce prováděné hello zřizování služby ve vaší aplikaci LinkedIn Learning.


## <a name="additional-resources"></a>Další zdroje

* [Správa uživatelů zřizování účtu pro podnikové aplikace](active-directory-enterprise-apps-manage-provisioning.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
