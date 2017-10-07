---
title: aaaUpgrade tooAzure AD Proxy aplikace | Microsoft Docs
description: "Zvolte, které proxy řešení je nejvhodnější, pokud upgradujete z Microsoft Forefront nebo Unified Gateway přístup."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7dc2633140b384e25792470dadbb7f3fa7992a2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="compare-remote-access-solutions"></a>Porovnání řešení vzdáleného přístupu

Azure Proxy aplikace Active Directory je jedním ze dvou řešení vzdáleného přístupu, které nabízí Microsoft. Hello jiných je Proxy webových aplikací, na místní verzi hello. Tyto dvě řešení nahradit starší produkty, které nabízí Microsoft: Microsoft Forefront Threat Management Gateway (TMG) a Unified brány přístup (UAG). Použijte tento článek toounderstand jak tyto čtyři řešení porovnat tooeach jiné. Pro těch, které jste stále pomocí hello zastaralé TMG nebo UAG řešení, použijte tento článek toohelp plán vaše migrace tooone Dobrý den Proxy aplikace. 


## <a name="feature-comparison"></a>Porovnání funkcí

Pomocí této tabulky toounderstand jak Threat Management Gateway (TMG), Unified brány přístup (UAG), Proxy webové aplikace (WAP) a Azure AD Application Proxy (AP) porovnat tooeach jiné.

| Funkce | TMG | UAG | WAP | AP |
| ------- | --- | --- | --- | --- |
| Ověřování pomocí certifikátu | Ano | Ano | - | - |
| Selektivně publikovat prohlížečových aplikací | Ano | Ano | Ano | Ano |
| Předběžné ověření a jeden přihlášení | Ano | Ano | Ano | Ano | 
| Vrstvy 2 nebo 3 brány firewall | Ano | Ano | - | - |
| Předávání funkce proxy serveru | Ano | - | - | - |
| Funkce sítě VPN | Ano | Ano | - | - |
| Podpora bohaté protokolu | - | Ano | Ano, pokud systém přes protokol HTTP | Ano, pokud je spuštěn prostřednictvím protokolu HTTP nebo prostřednictvím služby Brána vzdálené plochy |
| Slouží jako server proxy služby AD FS | - | Ano | Ano | - |
| Jeden portál pro přístup k aplikaci | - | Ano | - | Ano |
| Překlad odkaz text odpovědi | Ano | Ano | - | Ano | 
| Ověřování se záhlavími | - | Ano | - | Ano, s PingAccess | 
| Cloudového škálovatelného zabezpečení | - | - | - | Ano | 
| Podmíněný přístup | - | Ano | - | Ano |
| Žádné součásti v hello demilitarizovaná zóna (DMZ) | - | - | - | Ano |
| Žádná příchozí připojení | - | - | - | Ano |

Pro většinu scénářů doporučujeme, abyste aplikaci Azure AD jako hello moderní řešení. Proxy webových aplikací je pouze upřednostňované v scénáře, které vyžadují připojení proxy serveru pro službu AD FS a nemůžete použít vlastní domény ve službě Azure Active Directory. 

Azure AD Application Proxy nabízí jedinečné výhody při porovnání toosimilar produkty, včetně:

- Rozšíření Azure AD tooon místní prostředky
   - Cloudového škálovatelného zabezpečení a ochrana
   - Funkce, například podmíněného přístupu a vícefaktorového ověřování jsou snadno tooenable
- Žádné componenet v hello demilitarizovaná zóna
- Žádná příchozí připojení, vyžaduje
- Jeden přístupový panel vaši uživatelé můžete přejít toofor všechny své aplikace, včetně O365, Azure AD integrovaných aplikací SaaS a místní webové aplikace. 


## <a name="next-steps"></a>Další kroky

- [Používat aplikaci Azure AD tooprovide zabezpečený vzdálený přístup tooon místní aplikace](active-directory-application-proxy-get-started.md)
- [Přechod z Forefront TMG a UAG tooApplication Proxy](https://blogs.technet.microsoft.com/isablog/2015/06/30/modernizing-microsoft-application-access-with-web-application-proxy-and-azure-active-directory-application-proxy/).
