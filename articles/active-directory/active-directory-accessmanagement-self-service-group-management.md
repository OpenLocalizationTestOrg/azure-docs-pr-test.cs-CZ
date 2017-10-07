---
title: "aaaSetting služby Azure Active Directory pro samoobslužnou aplikace access management | Microsoft Docs"
description: "Samoobslužná správa skupin umožňuje uživatelům toocreate a spravovat skupiny zabezpečení nebo skupiny Office 365 ve službě Azure Active Directory a nabízí uživatelům hello možnost toorequest skupiny zabezpečení nebo členství ve skupině Office 365"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 904d5c70-c34a-46c4-a9a7-d1efecf4821c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/27/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 2a73f4ed2532d41143fe5abe2fef1322d971a5c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Nastavení služby Azure Active Directory pro samoobslužnou správu skupin
Samoobslužná správa skupin umožňuje uživatelům toocreate a spravovat skupiny zabezpečení nebo skupiny Office 365 ve službě Azure Active Directory (Azure AD). Uživatelé si mohou vyžádat také skupiny zabezpečení nebo členství ve skupině Office 365, a pak hello vlastníkem skupiny hello můžete schválit nebo odmítnout členství. Tímto způsobem může být každodenní řízení členství ve skupině delegované toopeople, kteří chápou obchodní kontext takového členství hello. Funkce samoobslužné správy skupin jsou dostupné jenom pro skupiny zabezpečení a skupiny Office 365. Nejsou dostupné pro skupiny zabezpečení s povoleným e-mailem a pro distribuční seznamy.

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku.

Samoobslužná správa skupin v současné době obsahuje dva základní scénáře: delegovanou správu skupin a samoobslužnou správu skupin.

* **Delegovaná správa skupin** příkladem je správce, který spravuje přístup k aplikaci SaaS tooa, kterou společnost hello používá. Správa těchto přístupových práv je čím dál náročnější, takže správce požádá hello firmy vlastníka toocreate novou skupinu. Hello správce přiřadí přístup hello aplikace toohello nové skupiny a přidá všechny osoby již přístup k aplikaci toohello toohello skupinu. Hello majitel firmy potom může přidat další uživatele a tito uživatelé jsou automaticky zřízeného toohello aplikace. majitel firmy Hello nepotřebuje toowait přístupu toomanage hello správce pro uživatele. Pokud správce hello udělí hello stejné oprávnění správce tooa v případě jiné obchodní skupiny, pak tato osoba můžete také spravovat přístup pro své vlastní uživatele. Majitel firmy hello ani hello manager můžete zobrazit nebo spravovat druhé strany uživatele. Hello správce může stále vidět všechny uživatele, kteří mají přístup toohello aplikace a přístupová práva bloku v případě potřeby.
* **Samoobslužná správa skupin** – příkladem tohoto scénáře jsou dva uživatelé, kteří mají nezávisle nastavené weby SharePoint Online. Chtějí toogive každé že druhé strany přístup k webům tootheir týmy. tooaccomplish, mohou vytvářet jedné skupiny ve službě Azure AD, a ve službě SharePoint Online každý z nich vybere této skupiny tooprovide přístup tootheir lokality. Pokud chce uživatel získat přístup, požádá o něj hello přístupového panelu a po schválení získá přístup tooboth weby SharePoint Online automaticky. Později jeden z nich rozhodne, že všichni uživatelé s přístupem k hello lokality také získat přístup tooa určité aplikaci SaaS. Správce Hello hello aplikace SaaS můžete přidat přístupová práva pro web služby SharePoint Online toohello aplikace hello. Všechny žádosti, které získání schválení od toho poskytuje přístup toohello oba weby SharePoint Online a také toothis aplikaci SaaS.

## <a name="making-a-group-available-for-end-user-self-service"></a>Zpřístupnění skupiny pro samoobslužné funkce koncových uživatelů
1. V hello [portál Azure classic](https://manage.windowsazure.com), otevřete svůj adresář Azure AD.
2. Na hello **konfigurace** nastavte **delegovaná Správa skupin** tooEnabled.
3. Nastavit **uživatelé můžou vytvářet skupiny zabezpečení** nebo **uživatelé můžou vytvářet skupiny Office** tooEnabled.

Když **uživatelé můžou vytvářet skupiny zabezpečení** je povoleno, všichni uživatelé v adresáři nové skupiny zabezpečení toocreate jsou povoleny a přidejte členy toothese skupiny. Tyto nové skupiny se také zobrazí v hello Panel přístupu pro všechny uživatele. Pokud hello nastavení zásad skupiny hello umožňuje, další uživatelé mohou vytvářet toojoin požadavky těchto skupin. Pokud je zakázaná možnost **Uživatelé můžou vytvářet skupiny zabezpečení**, uživatelé nemůžou vytvářet skupiny a nemůžou měnit existující skupiny, ve kterých figurují jako vlastníci. Mohou však stále spravovat členství hello v těchto skupinách a schvalovat žádosti jiných uživatelů toojoin jejich skupiny.

Můžete také použít **uživatelů, kteří můžou využívat samoobslužné funkce pro skupiny zabezpečení** tooachieve podrobnějšího řízení přístupu přes Samoobslužná správa skupin pro vaše uživatele. Když **uživatelé můžou vytvářet skupiny** je povoleno, všichni uživatelé ve vašem adresáři jsou povoleny toocreate nové skupiny a přidat členy toothese skupiny. Současným nastavením **uživatelů, kteří můžou využívat samoobslužné funkce pro skupiny zabezpečení** tooSome, jste omezení skupiny správy tooonly na omezenou skupinu uživatelů. Když tento přepínač nastavíte tooSome, musíte přidat skupinu uživatelů toohello SSGMSecurityGroupsUsers předtím, než můžete vytvářet nové skupiny a přidat členy toothem. Nastavením **uživatelů, kteří můžou využívat samoobslužné funkce pro skupiny zabezpečení** tooAll, povolit všechny uživatele ve skupinách nové toocreate adresáře.

Můžete taky hello **skupiny, které můžou využívat samoobslužné funkce pro skupiny zabezpečení** pole toospecify vlastní název pro skupinu, jejíž členové můžou využívat samoobslužné funkce.

## <a name="next-steps"></a>Další kroky
Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu tooresources pomocí skupin Azure Active Directory](active-directory-manage-groups.md)
* [Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Představení služby Azure Active Directory](active-directory-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
