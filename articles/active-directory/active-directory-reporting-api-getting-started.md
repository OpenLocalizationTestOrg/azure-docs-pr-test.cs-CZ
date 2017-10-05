---
title: "Začínáme s Azure AD reporting rozhraní API na portálu Azure AD classic | Microsoft Docs"
description: "Jak začít pracovat s Azure Active Directory, vytváření sestav rozhraní API"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/18/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 5e98b660fe19bb8abebf1c3b996b6295a6c4e728
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-the-azure-active-directory-reporting-api-on-the-azure-ad-classic-portal"></a>Začínáme s Azure Active Directory, vytváření sestav rozhraní API na portálu Azure AD classic
*Toto téma je součástí [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).*

Azure Active Directory nabízí celou řadu sestav. Data z těchto sestav můžou být velmi užitečná pro vaše aplikace, jako jsou systémy SIEM nebo nástroje pro auditování a business intelligence. Rozhraní API pro generování sestav v Azure AD poskytují programový přístup k těmto datům prostřednictvím sady rozhraní API založených na REST. Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.

Tento článek vám poskytne informace, které potřebujete začít s Azure AD reporting rozhraní API.
V další části můžete najít další podrobnosti o používání auditu a přihlaste se rozhraní API. Všechny ostatní rozhraní API, najdete v článku [sestav Azure AD a events(preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) článku.

Pro dotazy, problémy nebo připomínky, obraťte se na [AAD Reporting pomoci](mailto:aadreportinghelp@microsoft.com).

## <a name="learning-map"></a>Výuková mapa
1. **Příprava** -mohli otestovat vaši ukázky rozhraní API, které potřebujete k dokončení [požadavky pro přístup k Azure AD reporting rozhraní API](active-directory-reporting-api-prerequisites.md).
2. **Prozkoumejte** -získal první dojem o rozhraní API pro vytváření sestav:
   
   * [Pomocí ukázky pro audit rozhraní API](active-directory-reporting-api-audit-samples.md) 
   * [Pomocí ukázky pro sestavu přihlašovací aktivita rozhraní API](active-directory-reporting-api-sign-in-activity-samples.md)
3. **Přizpůsobení** -vytvořit vlastní řešení: 
   
   * [Pomocí auditu referenční dokumentace rozhraní API](active-directory-reporting-api-audit-reference.md) 
   * [Pomocí odkaz na sestavu API přihlašovací aktivita](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a>Další kroky
Pokud jste zvědaví zobrazíte všechny koncové body k dispozici Azure AD Graph API přechodem na [https://graph.windows.net/tenant-name/reports/$ metadata? api-version = beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).

