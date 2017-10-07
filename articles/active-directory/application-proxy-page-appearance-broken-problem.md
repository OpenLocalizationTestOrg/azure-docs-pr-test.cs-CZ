---
title: "stránka aaaApplication nezobrazuje správně pro aplikaci Proxy aplikace | Microsoft Docs"
description: "Pokyny při stránku hello není správně zobrazení v aplikaci Proxy aplikací mít integrované s Azure AD"
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
ms.openlocfilehash: f4abaa4e94c512868f2085affe59cac443784a56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a>Stránka aplikace se nezobrazuje správně pro aplikaci Proxy aplikace

Tento článek vám pomohou tootroubleshoot problémy s aplikacemi Azure Proxy aplikace služby Active Directory když přejdete na stránku toohello, ale něco na stránce hello nevypadá správné.

## <a name="overview"></a>Přehled
Když publikujete aplikaci Proxy aplikace, jsou přístupné pouze stránky v rámci kořenového adresáře, při přístupu k aplikaci hello. Pokud stránku hello nezobrazuje správně, hello kořenové interní adresa URL použitá pro aplikace hello mohou chybět některé prostředky stránky. tooresolve, zajistěte, aby jste publikovali *všechny* hello prostředky pro stránku hello v rámci vaší aplikace.

Můžete ověřit jedná o problém hello otevřením sledovací modul vaší sítě (například aplikaci Fiddler nebo F12 nástroje v Internet Explorer nebo Edge), načítání stránky hello a hledá chyb 404. Který označuje hello stránek, které v současné době nelze nalézt a může být stále nutné toobe publikována.

Jako příklad tento případ, předpokládá jste publikovali aplikaci výdaje pomocí interní URL <http://myapps/expenses>, ale aplikace hello používá šablony stylů hello <http://myapps/style.css>. V takovém případě šablony stylů hello není publikována ve vaší aplikaci, tak načítání aplikace hello výdaje throw 404 Chyba při pokusu o tooload style.css. V tomto příkladu by hello problém vyřešit tím, že publikujete aplikace hello s interní URL <http://myapp/> místo.

## <a name="problems-with-publishing-as-one-application"></a>Problémy s publikování jako jednu aplikaci

Pokud není možné toopublish všechny prostředky v rámci hello stejnou aplikaci, můžete potřebovat toopublish více aplikací a povolit propojení mezi nimi.

toodo Ano, doporučujeme používat hello [vlastní domény](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) řešení. Toto řešení však třeba vlastní hello certifikát pro vaši doménu a aplikace pomocí plně kvalifikované názvy domény (FQDN). Další možnosti najdete v tématu hello [řešení potíží s poškozených odkazů dokumentaci](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).

## <a name="next-steps"></a>Další kroky
[Publikování aplikací pomocí proxy aplikace služby Azure AD](application-proxy-publish-azure-portal.md)
