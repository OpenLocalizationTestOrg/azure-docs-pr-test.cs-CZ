---
title: "aaaUser zřizování tooan Azure AD application Gallery je pořízení hodiny nebo víc | Microsoft Docs"
description: "Jak toofind out proč zřizování aplikace tooyour možná trvá déle, než jste očekávání"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 0658f041724c91ddd1997cc7759393b46680f13a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="user-provisioning-tooan-azure-ad-gallery-application-is-taking-hours-or-more"></a>Zřizování aplikace Galerie tooan Azure AD uživatelů je pořízení hodin nebo více

Při prvním povolení automatické zřizování pro aplikace, hello počáteční synchronizace může trvat hodiny tooseveral 20 minut, v závislosti na velikosti hello hello adresář Azure AD a hello počet uživatelů v oboru pro zřizování. 

Následné synchronizace po počáteční synchronizaci hello být rychlejší jako hello zřizování služby ukládá vodoznaky, které představují hello stav obou systémů po počáteční synchronizaci hello, zvýšení výkonu následné synchronizace.

## <a name="how-tooimprove-provisioning-performance"></a>Jak tooimprove zřizování výkonu

Pokud počáteční synchronizace hello trvá víc než několik hodin, je jednou z věcí můžete provést tooimprove výkonu:

-   **Filtry oboru uživatele.** Oboru filtrů povolit toofine tune hello data, která hello zřizování služby extrahuje z Azure AD pomocí filtrování uživatelů na základě hodnot určitým atributem. Další informace o filtry oborů najdete v tématu [zřizování aplikace na základě atributů s filtry oborů](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

## <a name="next-steps"></a>Další kroky
[Automatizace zřizování uživatelů a jeho rušení tooSaaS aplikací s Azure Active Directory](active-directory-saas-app-provisioning.md)

