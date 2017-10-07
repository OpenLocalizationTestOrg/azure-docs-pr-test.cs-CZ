---
title: "aaaAdvanced omezování požadavků pomocí Azure API Management"
description: "Zjistěte, jak toocreate a použít flexibilní kvóty a míra omezení zásad Azure API Management."
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: fc813a65-7793-4c17-8bb9-e387838193ae
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: ac87f83118a37bd587fddf044e5c2d6fc2af9031
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-request-throttling-with-azure-api-management"></a>Pokročilé omezování požadavků pomocí Azure API Management
Je možné toothrottle příchozí požadavky je klíčovou roli služby Azure API Management. Buď řízení hello míra požadavků nebo data celkový počet požadavků hello přenesen, umožňuje rozhraní API správy rozhraní API zprostředkovatelů tooprotect příslušných rozhraní API z zneužití a vytvořit hodnotu pro různé úrovně rozhraní API produktu.

## <a name="product-based-throttling"></a>Omezování na základě produktu
toodate hello míra možnosti omezování byly omezené toobeing obor tooa určitý produkt odběr (v podstatě klíč), definované v hello portál vydavatele API Management. To je užitečné pro hello rozhraní API poskytovatele tooapply omezení hello vývojáře, kteří zaregistrovali toouse jejich rozhraní API, ale jeho nepomůže, například v omezení jednotlivých koncoví uživatelé hello rozhraní API. Je možné, že pro jednoho uživatele tooconsume aplikace hello vývojáře hello celý kvóty a pak zabránit ostatním zákazníkům hello vývojáře aplikací mít toouse hello. Navíc několik zákazníci, kteří mohou vytvořit k velkému počtu požadavků může omezit přístup toooccasional uživatele.

## <a name="custom-key-based-throttling"></a>Vlastní klíč na základě omezení
Hello nové [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) a [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) zásady poskytují ovládacího prvku výrazně flexibilnější tootraffic řešení. Tyto nové zásady umožňují toodefine výrazy tooidentify hello klíče, které budou použité tootrack využití provozu. Hello tak, jak to funguje je nejsnažší zobrazené na příkladu. 

## <a name="ip-address-throttling"></a>Omezení IP adres
Hello následující zásady omezení jednoho klienta IP adresu tooonly 10 volání každou minutu, celkem 1 000 000 volání a 10 000 kB šířky pásma za měsíc. 

```xml
<rate-limit-by-key  calls="10"
          renewal-period="60"
          counter-key="@(context.Request.IpAddress)" />

<quota-by-key calls="1000000"
          bandwidth="10000"
          renewal-period="2629800"
          counter-key="@(context.Request.IpAddress)" />
```

Pokud všechny klienty v Internetu hello použili jedinečnou IP adresu, může se jednat o účinný způsob omezení využití podle uživatele. Je však velmi pravděpodobné, že více uživatelů se jednu veřejnou IP adresu z důvodu toothem přístupem hello Internet prostřednictvím zařízení NAT pro sdílení. Bez ohledu na to, pro rozhraní API umožňující přístup bez ověřování hello `IpAddress` může být nejlepší možnost hello.

## <a name="user-identity-throttling"></a>Omezení identity uživatele
Pokud koncový uživatel je ověřen a omezení klíč lze generovat na základě informací, která jednoznačně identifikuje, který uživatel.

```xml
<rate-limit-by-key calls="10"
    renewal-period="60"
    counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
```

V tomto příkladu jsme extrahovat hello autorizační hlavičky, převeďte ho příliš`JWT` objektu a použít hello subjektu hello tokenu tooidentify hello uživatele a použít jej v hello míru omezení klíč. Pokud je identita uživatele hello je uložen v hello `JWT` jako jeden z hello další deklarace identity pak hodnota může použije na příslušné místo.

## <a name="combined-policies"></a>Kombinovaná zásady
Přestože hello nové omezení zásady poskytují větší možnosti než hello existující omezení zásad, je stále hodnota kombinace obou možností. Omezování klíč předplatného produktu ([omezení četnosti volání podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) a [nastavení kvóty využití podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) je skvělým způsobem, tooenable monetizing rozhraní API účtováním podle úrovně využití. Hello přesnější možnosti řízení způsobená možné toothrottle uživatelem je doplňkové a zabrání dlouhodobější snížení kvality hello prostředí jiného chování jednoho uživatele. 

## <a name="client-driven-throttling"></a>Řízené omezení klienta
Když hello omezení klíče definovaná pomocí [výraz zásady](https://msdn.microsoft.com/library/azure/dn910913.aspx), pak je hello rozhraní API poskytovatele, který je výběr, jak má obor hello omezení. Však může být vhodné vývojář toocontrol jak ohodnotili omezit vlastní zákazníků. To může být povolený poskytovatelem rozhraní API hello zavedením vlastní hlavičky tooallow hello vývojář na klienta aplikace toocommunicate hello klíče toohello rozhraní API.

```xml
<rate-limit-by-key calls="100"
          renewal-period="60"
          counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>
```

To umožňuje hello vývojáře klienta aplikace toochoose jak chtějí toocreate hello míru omezení klíč. Pomocí jenom trocha vynalézavosti vývojář klienta vytvářet své vlastní míry vrstev přidělením sady toousers klíče a otáčení hello použití klíče.

## <a name="summary"></a>Souhrn
Azure API Management nabízí rychlost a uvozovky omezení tooboth chránit a přidejte hodnotu tooyour rozhraní API služby. Hello nové omezení zásad pomocí vlastní oboru pravidla povolit, že je lepší kontrolu nad tooenable tyto zásady podrobných aplikace ještě lepší toobuild zákazníků. Hello příklady v tomto článku ukazují použití hello tyto nové zásady ve výrobním míru omezení klíče s klientských IP adres, klient vygeneruje hodnoty a identity uživatele. Existují však mnoho dalších částí uvítací zprávu, která by bylo možné použít jako uživatelský agent, fragmenty cestu adresy URL, velikost zprávy.

## <a name="next-steps"></a>Další kroky
Prosím sdělte svůj názor v hello vlákna služby Disqus pro toto téma. Je skvělým toohear o jiné potenciální klíče hodnoty, které byly logické výběru v vaše scénáře.

## <a name="watch-a-video-overview-of-these-policies"></a>Podívejte se na video s přehledem těchto zásad
Další informace o hello [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) a [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) zásady popsaná v tomto článku, podívejte se prosím hello následující video.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Advanced-Request-Throttling-with-Azure-API-Management/player]
> 
> 

