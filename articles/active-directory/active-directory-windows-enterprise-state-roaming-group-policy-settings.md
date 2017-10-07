---
title: "nastavení zásad a správu mobilních zařízení aaaGroup | Microsoft Docs"
description: "Poskytuje informace o zásadách skupiny a mobilních zařízení nastavení správy (MDM), které se mají použít na zařízeních vlastněných společností. Tyto zásady jsou použité toohello uživatele zařízení."
services: active-directory
keywords: "Co jsou zásady skupiny a nastavení MDM Enterprise State Roaming, Enterprise State Roaming, cloudu systému windows"
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: 6471a9b3-8dd4-4237-89d1-bfbeca9f8252
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 762419b47014b1fb4d92ac528785e20078afe689
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="group-policy-and-mdm-settings"></a>Nastavení zásad skupiny a správu mobilních zařízení
Používejte tyto zásady skupiny a nastavení správy mobilních zařízení jenom na zařízení ve vlastnictví firmy, protože tyto zásady jsou použité toohello uživatele zařízení. MDM zásad toodisable nastavení synchronizace pro osobní použití, zařízení ve vlastnictví uživatele bude mít negativní vliv na použití hello těchto zařízení. Kromě toho by jiné uživatelské účty na hello zařízení nebude mít vliv také zásadami hello.

Podnikům, které chcete, můžete použít toomanage roamingu pro osobní zařízení (nespravovaný) hello Azure portálu tooenable nebo zakázat roaming, nikoli pomocí zásad skupiny nebo správy mobilních zařízení.
Hello následující tabulky popisují hello nastavení zásad, které jsou k dispozici.

## <a name="mdm-settings"></a>Nastavení MDM
nastavení zásad MDM Hello týká tooboth Windows 10 a Windows 10 Mobile.  Windows 10 Mobile podpora je dostupná pouze pro účet Microsoft na základě roaming prostřednictvím účtu OneDrive.  Podrobnosti najdete příliš "Koncové body a zařízení" části Podrobnosti o jaká zařízení jsou podporovány pro Azure AD na základě, synchronizaci.

| Name (Název) | Popis |
| --- | --- |
| Povolit účet Microsoft připojení |Umožňuje uživatelům tooauthenticate používání účtu Microsoft na zařízení hello |
| Povolit synchronizaci nastavení |Umožňuje uživatelům tooroam Windows nastavení a dat aplikací; Zakázání této zásady zakážete synchronizaci, stejně jako zálohy na mobilních zařízeních |

## <a name="group-policy-settings"></a>Nastavení zásad skupiny
nastavení zásad skupiny Hello použít tooWindows 10 zařízení, které jsou připojené k tooan domény služby Active Directory. Tabulka Hello také obsahuje starší verze nastavení, se zobrazí toomanage nastavení synchronizace, ale které nefungují pro podnikové stavu roamingu pro Windows 10, které jsou uvedené s, nepoužívejte, v popisu hello.

| Name (Název) | Popis |
| --- | --- |
| Účty: Blokovat účty Microsoft |Nastavení této zásady zabrání uživatelům v přidávání nových účtů Microsoft na tomto počítači |
| Není synchronizována |Zabraňuje uživatelům tooroam Windows nastavení a dat aplikací |
| Není synchronizována přizpůsobení |Zakáže synchronizuje hello motivy skupiny |
| Není synchronizována nastavení prohlížeče |Zakáže synchronizuje skupiny hello Internet Explorer |
| Ne synchronizaci hesel |Zakáže synchronizaci hesel skupiny |
| Není synchronizována další nastavení Windows |Zakáže synchronizuje ostatní okna nastavení skupiny |
| Přizpůsobení plochy není synchronizována |Nepoužívejte; nemá žádný vliv |
| Není synchronizována u měřených připojení k |Zakáže roaming v měřená připojení, jako je například mobilní 3 G |
| Není synchronizována aplikace |Nepoužívejte; nemá žádný vliv |
| Není synchronizována nastavení aplikace |Zakáže roaming dat aplikací |
| Není synchronizována nastavení spuštění |Nepoužívejte; nemá žádný vliv |

## <a name="related-topics"></a>Související témata
* [Přehled Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
* [Povolit stav enterprise roaming v Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md)
* [Nastavení a datový roaming – nejčastější dotazy](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Odkaz nastavení roamingu Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Řešení potíží](active-directory-windows-enterprise-state-roaming-troubleshooting.md)

