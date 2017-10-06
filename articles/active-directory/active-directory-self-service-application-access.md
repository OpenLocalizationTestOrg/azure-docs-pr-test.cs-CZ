---
title: "přístup k aplikaci služby aaaSelf a delegované správy pomocí služby Azure Active Directory | Microsoft Docs"
description: "Tento článek popisuje, jak přistupovat k aplikaci tooenable samoobslužné služby a delegované správy pomocí služby Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 448a7fe8-a162-475e-9ba2-2e3ab59302bc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.reviewer: asmalser
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 90bec3bd71796f22a782929b028db0d18c3aa1c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="self-service-application-access-and-delegated-management-with-azure-active-directory"></a>Přístup k aplikaci Samoobslužné služby a delegované správy pomocí služby Azure Active Directory
Povolení schopnosti samoobslužné služby pro koncové uživatele je běžný scénář pro podnikové IT. Velký počet uživatelů, velký počet aplikací a hello osobě, která je best-informed toomake přístup udělit, že rozhodnutí, která nemusí být správce adresáře hello. Často hello osvědčených uživatel toodecide kdo má přístup k aplikaci je vedoucí týmu nebo jiných delegovaného správce. Je však hello uživatel, který používá aplikace hello a hello uživatel ví, co potřebují mít toodo toobe jejich úlohy.

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku. 

Přístup k aplikaci Samoobslužné služby je funkce [Azure Active Directory Premium](https://azure.microsoft.com/trial/get-started-active-directory/) P1 a P2 licencí, které umožňují správcům adresáře:

* Povolit uživatelům přístup toorequest tooapplications pomocí "Získat další aplikace" dlaždici v hello [přístupový Panel Azure AD](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)
* Sadu uživatelů, kteří aplikace můžete požádat o přístup k
* Nastavte, zda je vyžadováno pro uživatele toobe možné tooself přiřazení přístupu tooan aplikaci schválení
* Sada, kdo by měl schvalovat žádosti o hello a správa přístupu pro každou aplikaci

Dnes tato funkce je podporována pro všechny předem integrované a vlastní aplikace, které podporují federované nebo založené na heslech jednotné přihlašování v hello [galerii aplikací Azure Active Directory](https://azure.microsoft.com/marketplace/active-directory/all/), včetně aplikace, jako je Salesforce, Dropbox, Google Apps a další.
Tento článek popisuje, jak:

* Konfigurace přístupu k aplikaci Samoobslužné služby pro koncové uživatele, včetně konfigurace pracovního postupu volitelné schválení 
* Delegovat správu přístupu pro konkrétní aplikace toohello nejvhodnější osoby ve vaší organizaci a je povolit toouse hello Azure AD přístup k panelu tooapprove žádosti o přístup, přímo přiřadit přístup uživatelé tooselected nebo nastavte (volitelně) přihlašovací údaje pro přístup k aplikaci, když je nakonfigurovaný založené na heslech jednotné přihlašování

## <a name="configuring-self-service-application-access"></a>Konfigurace přístupu k aplikaci Samoobslužné služby
přístup k aplikaci Samoobslužné služby tooenable a nakonfigurovat aplikace, které mohou být přidány nebo požadoval koncovým uživatelům, postupujte podle následujících pokynů.

1. Přihlaste se k hello [portál Azure classic](https://manage.windowsazure.com/).

2.   V části hello **služby Active Directory** část, vyberte adresář a pak vyberte hello **aplikace** kartě. 

3. Vyberte hello **přidat** tlačítko použít hello Galerie možnost tooselect a přidat aplikaci.

4. Po přidání aplikace získáte rychlý Start stránky aplikace hello. Klikněte na tlačítko **nakonfigurovat jednotné přihlašování**, vyberte hello požadované jediný přihlašování režim a uložte konfiguraci hello. 

5. Potom vyberte hello **konfigurace** kartě tooenable uživatelé toorequest přístup toothis aplikace z přístupového panelu hello Azure AD, nastavte **povolit přístup k aplikaci Samoobslužné služby** příliš**Ano**.
  
  ![][1]

6. pracovní postup schválení žádosti o přístup, nakonfigurujte toooptionally nastavit **vyžadovat schválení před udělením přístupu** příliš**Ano**. Pak lze vybrat jeden nebo více schvalovatelů pomocí hello **schvalovatelů** tlačítko.

  Approver může být každý uživatel v organizaci hello pomocí účtu Azure AD a může být zodpovědná za zřizování účtu licencování nebo jiné obchodní proces vaše organizace vyžaduje před udělením přístupu tooan aplikace. Schvalovatel Hello může být i hello vlastník skupiny jednoho nebo více sdílených účet skupiny a můžete přiřadit uživatele tooone hello z těchto skupin toogive, které je přístup přes sdíleného účtu. 

  Pokud je vyžadováno žádné schválení, uživatelé okamžitě získat hello aplikace přidané tootheir Azure AD přístupového panelu. Pokud aplikace hello má byly nastaveny pro [zřizování automatické uživatelů](active-directory-saas-app-provisioning.md), nebo byl nastavený [režimu SSO "spravovaná uživatelem" heslo](active-directory-appssoaccess-whatis.md#password-based-single-sign-on), hello uživatele byste již měli mít uživatel účet a heslo hello znát.

7. Pokud aplikace hello nebyla nakonfigurovaná toouse založené na heslech jednotné přihlašování, pak možnost pro povolení hello schvalovatel tooset hello jednotné přihlašování pověření jménem každého uživatele, je také k dispozici. Další informace najdete v tématu část hello na [Delegovaná správa přístupu](#delegated-application-access-management).

8. Nakonec hello **skupiny uživatelů Self-Assigned** ukazuje hello název hello skupiny, která je použité toostore hello uživatelů, kteří bylo uděleno nebo přiřadit přístup toohello aplikace. Schvalovatel přístup Hello se změní na vlastníka hello této skupiny. Pokud název skupiny hello zobrazí neexistuje, vytvoří se automaticky. Název skupiny hello Volitelně můžete nastavit toohello název existující skupiny.

9. toosave hello konfiguraci, klikněte na tlačítko **Uložit** v hello dolní části obrazovky hello. Uživatelé nyní mohou toorequest přístup toothis aplikace z hello přístupového panelu.

10. tootry hello činnost koncového uživatele, přihlaste se k přístupový panel Azure AD vaší organizace na https://myapps.microsoft.com, pokud možno pomocí jiného účtu, který není approver aplikace. 

11. V části hello **aplikace** , klikněte na hello **získat další aplikace** dlaždici. Tuto dlaždici zobrazí Galerie všechny hello aplikace, které jsou zapnuty pro přístup k aplikaci Samoobslužné služby v adresáři hello s hello možnost toosearch a filtrovat podle kategorie aplikace na levé straně hello. 

12. Kliknutím na aplikaci zahájí zpracování žádosti o hello. Pokud není třeba žádný proces schválení, pak aplikace hello se okamžitě přidají pod hello **aplikace** karta po krátké potvrzení. Pokud je vyžadováno schválení, pak se zobrazí dialogové okno oznamující to a e-mail je odeslán toohello schvalovatelů. Jste přihlášení k hello přístup k panelu jako jiný schvalovatel toosee tento proces žádosti.

13. e-mailu Hello přesměruje hello schvalovatel toosign do přístupového panelu Azure AD hello a hello žádost schválit. Jakmile hello žádost se schválí (a žádné speciální procesy, které definujete prováděly schvalovatel hello), hello uživateli se zobrazí hello aplikace se zobrazí pod jejich **aplikace** karty, které můžete přihlásit do ní.

## <a name="delegated-application-access-management"></a>Správa přístupu k delegované aplikací
Schvalovatel přístup aplikaci lze všechny uživatele ve vaší organizaci, který je nejvhodnější tooapprove osoba hello nebo odepřít přístup toohello aplikace. Tento uživatel může být zodpovědná za zřizování účtu licencování nebo jiné obchodní proces vaše organizace vyžaduje před udělením přístupu tooan aplikace.

Při konfiguraci přístup k aplikaci Samoobslužné služby, které jsou popsané výše, všechny přiřazené aplikace schvalovatelů najdete v části Další **spravovat aplikace** dlaždice na panel přístupu hello Azure AD, která ukazuje, které aplikace, které jsou Hello přístup správce. Kliknutím na tlačítko aplikace zobrazuje obrazovka s několik možností.

![][2]

### <a name="approve-requests"></a>Schválení žádostí
Hello **schválení žádosti o** dlaždice umožňuje schvalovatelů toosee všechny čekající schválení toothat konkrétní aplikace a přesměrování toohello schválení karta kde hello požadavky možné potvrdit nebo odepřít. Schvalovatel Hello dále přijímá automatizovaných e-mailů, vždy, když je vytvořen požadavek, který jim vydává pokyn co toodo.

### <a name="add-users"></a>Přidání uživatelů
Hello **přidat uživatele** dlaždice umožňuje schvalovatelů toodirectly grant vybrané uživatelé přístup toohello aplikace. Po kliknutí na tuto dlaždici, uvidí hello schvalovatel, že zobrazí se dialogové okno umožňuje jejich tooview a hledat uživatele v jejich adresář. Přidání uživatele výsledků v aplikaci hello se zobrazí v těchto uživatele Azure AD přístup panelů nebo Office 365. Pokud se požaduje jakékoli ruční uživatele zřizování v hello aplikace předtím, než uživatel hello je možné toosign v, a poté hello schvalovatel by měl proveďte tento proces před udělením přístupu.  

### <a name="manage-users"></a>Správa uživatelů
Hello **spravovat uživatele** dlaždice umožňuje schvalovatelů toodirectly aktualizace nebo odeberte uživatele, kteří mají přístup toohello aplikace. 

### <a name="configure-password-sso-credentials-if-applicable"></a>Konfigurovat přihlašovací údaje pro jednotné přihlašování k heslo (pokud existuje)
Hello **konfigurace** dlaždice se zobrazují, pouze pokud hello aplikace byla nakonfigurována pomocí hello IT správce toouse založené na heslech jednotné přihlašování a hello správci uděleno hello schvalovatel hello možnost tooset hesla jednotného přihlašování jak je popsáno výše. Při výběru hello schvalovatele pro jak hello přihlašovacích údajů jsou uživatelé šířený tooassigned zobrazí několik možností:

![][3]

* **Uživatelé přihlásit pomocí hesla** – v tomto režimu hello přiřadit uživatelům vědět, co uživatelských jmen a hesel jsou určené pro hello aplikaci a jsou výzvami tooenter je při své první aplikaci toohello přihlášení. Hello scénář odpovídá toohello heslo jednotné přihlašování případ kde hello [uživatelé spravovat pověření](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).
* **Uživatelé se automaticky přihlásíte pomocí samostatné účty, které spravují** – v tomto režimu hello přiřadit uživatele nejsou požadované tooenter nebo vědět své přihlašovací údaje pro konkrétní aplikaci, po přihlášení do aplikace hello. Místo toho hello schvalovatel nastaví hello pověření pro každého uživatele po přiřazení přístupu pomocí hello **přidat uživatele** dlaždici. Když hello uživatel klikne na aplikace hello v jejich přístupového panelu nebo Office 365, jsou automaticky přihlášeni pomocí hello pověření, která nastavuje schvalovatel hello. Hello scénář odpovídá toohello heslo jednotné přihlašování případ kde hello [správci spravují přihlašovací údaje](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).
* **Uživatelé se automaticky přihlásíte pomocí jeden účet, který lze spravovat** -ve speciálním případě tomto případě je vhodné toouse při všechny přiřazené uživatelé potřebují toobe udělen přístup pomocí jednoho sdíleného účtu. Hello nejběžnější případ použití pro tuto funkci je s aplikací sociálních médií, kde organizace má jeden "společnost" účet a více uživatelů potřebovat toomake aktualizace toothat účet. Hello scénář také odpovídá toohello heslo jednotné přihlašování případ kde hello [správci spravují přihlašovací údaje](active-directory-appssoaccess-whatis.md#password-based-single-sign-on). Ale po výběru této možnosti, hello schvalovatel bude výzvami tooenter hello uživatelské jméno a heslo pro jeden sdíleného účtu hello. Po dokončení všech uživatelů přiřazených přihlášení pomocí tohoto účtu, když kliknete na aplikace hello v jejich panelů přístup k Azure AD nebo Office 365.

## <a name="additional-resources"></a>Další zdroje
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

<!--Image references-->
[1]: ./media/active-directory-self-service-application-access/ssaa_admin.PNG
[2]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app.PNG
[3]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app_config.PNG
