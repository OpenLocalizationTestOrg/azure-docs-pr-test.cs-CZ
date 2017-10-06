---
title: "Přehled aaaRedirect Azure Application Gateway. | Microsoft Docs"
description: "Další informace o funkci přesměrování hello Azure Application Gateway"
services: application-gateway
documentationcenter: na
author: amsriva
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/18/2017
ms.author: amsriva
ms.openlocfilehash: 7149e3bd000d336c51995fb0e90f971b4d9ba2be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-gateway-redirect-overview"></a>Přehled přesměrování brány aplikace

Běžný scénář pro mnoho webových aplikací je toosupport automatické HTTP tooHTTPS přesměrování tooensure k veškerá komunikace mezi aplikací a svým uživatelům dochází prostřednictvím šifrované cesty. V posledních hello zákazníci použili techniky, jako je například vytváření vyhrazené back-end fondu, jehož jediným účelem je tooredirect požadavky, které obdrží na HTTP tooHTTPS.  Aplikační brána teď podporuje možnost tooredirect provoz na hello Application Gateway. To zjednodušuje konfiguraci aplikací, optimalizuje využití prostředků hello a podporuje nové scénáře přesměrování včetně globální a na základě cestu přesměrování. Podpora aplikací brány přesměrování není omezen tooHTTP -> HTTPS přesměrování samostatně. Toto je obecná přesměrování mechanismus, který umožňuje přesměrování provozu přijme jeden naslouchací proces tooanother naslouchání služby Application Gateway. Také podporuje přesměrování tooan externí web také. Podpora přesměrování brány aplikace nabízí hello následující možnosti:

1. Globální přesměrování z naslouchací proces tooanother jeden naslouchací proces ve hello brány. To umožňuje tooHTTPS přesměrování protokolu HTTP v lokalitě.
2. Na základě cestu přesměrování. Tento typ přesměrování umožňuje přesměrování protokolu HTTP tooHTTPS pouze v konkrétní lokalitě oblast, třeba nákupní košík oblasti odlišené/košíku / *.
3. Přesměrování tooexternal lokality.

![přesměrování](./media/application-gateway-redirect-overview/redirect.png)

Díky této změně zákazníci potřebovat toocreate nový objekt konfigurace přesměrování, který určuje naslouchací proces cíl hello nebo externí web toowhich přesměrování se požaduje. element konfigurace Hello také podporuje tooenable možnosti připojování hello URI cesta a dotaz, že adresa URL přesměrování toohello řetězec. Zákazníci mohou také zvolte, jestli je přesměrování dočasného (kód stavu HTTP 302) nebo trvalé přesměrování (kód stavu protokolu HTTP 301). Po vytvoření této konfigurace přesměrování je naslouchací proces zdroj připojené toohello prostřednictvím nové pravidlo. Pokud používáte základní pravidlo, konfigurace přesměrování hello je přidružený naslouchací proces zdroje a globální přesměrování. Při použití pravidla na základě cesty hello přesměrování konfigurace je definován na mapě cesta URL hello a proto se vztahuje pouze toohello specifickou cestu oblasti lokality.

### <a name="next-steps"></a>Další kroky

[Konfigurace adresy URL přesměrování na služby application gateway](application-gateway-configure-redirect-powershell.md)
