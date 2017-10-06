---
title: "aaaOverview víceklientské zpět končí Azure Application Gateway | Microsoft Docs"
description: "Tato stránka obsahuje přehled podpory hello aplikační brány pro back-EndY více klientů."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: 
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.openlocfilehash: b7da8c9c68e34bd83ad2b828fab62c00caea354a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-gateway-support-for-multi-tenant-back-ends"></a>Podpora služby Application Gateway pro back-endy s více tenanty

Azure Application Gateway jako součást fondů back-end podporuje škálovací sady virtuálních počítačů, síťová rozhraní, veřejné a privátní IP adresy nebo plně kvalifikovaný název domény. Ve výchozím nastavení aplikační brány nezmění hello příchozí Hlavička hostitele HTTP od klienta hello a odešle hello hlavičky beze změny toohello back-end. Existuje mnoho služby, jako je [Azure Web Apps](../app-service-web/app-service-web-overview.md) a [API Management](../api-management/api-management-key-concepts.md) které jsou více klientů ve své podstatě a spoléhají na konkrétního hostitele hlavičky nebo SNI rozšíření tooresolve toohello správný koncový bod. Aplikační brána teď podporuje hello možnost pro uživatele toooverwrite hello příchozí hlavičky protokolu HTTP hostitele na základě nastavení back-end HTTP hello. Tato schopnost umožňuje podporu back-endů s více tenanty Azure Web Apps a API Management. Tato možnost je dostupná pro standardní hello i SKU firewall webových aplikací. Víceklientské back-end podporu také funguje se scénáři SSL tooend SSL pro ukončení a end.

![scénář webové aplikace](./media/application-gateway-web-app-overview/scenario.png)

Hello možnost toospecify hostitele přepsání je definováno v hello nastavení HTTP a mohou být použité tooany zpět ukončit fondu při vytváření pravidel. Víceklientské zpět končí podpora hello následující dva způsoby přepsání rozšíření SNI a hlavičky hostitele.

1. Hello možnost tooset hello hostitele název tooa pevná hodnota ve hello nastavení HTTP. Tato funkce zajišťuje přepsána tuto hlavičku hostitele hello toothis hodnoty pro všechny přenosy toohello back-end fondu kterou platí nastavení hello protokolu HTTP. Pokud používáte end tooend SSL, tento název přepsané hostitele se používá v hello SNI rozšíření. Tato funkce umožňuje scénáře, kde farmu fond back-end očekává hlavičku hostitele, který se liší od hello příchozí hlavička zákazníků hostitele.

2. název hostitele Hello možnost tooderive hello z hello IP adresu nebo plně kvalifikovaný název domény hello členy fondu back-end. Nastavení HTTP také zadejte název hostitele možnost toopick hello z členem fondu back-end FQDN pokud nakonfigurovaný s názvem hostitele, hello možnost tooderive od člena fondu jednotlivých back-end. Pokud používáte end tooend SSL, tento název hostitele je odvozený od hello plně kvalifikovaný název domény a používá se v hello SNI rozšíření. Tato funkce umožňuje scénáře, kde fond back-end může mít dvě nebo více služeb PaaS víceklientské jako webové aplikace Azure a člen tooeach hlavičky hostitele hello žádost obsahuje název hostitele hello odvozené od jeho plně kvalifikovaný název domény.

> [!NOTE]
> V obou předchozích případech hello hello nastavení má vliv pouze chování hello provoz za provozu a není hello chování test stavu. Vlastní sondy již podporu hello možnost toospecify hlavičku hostitele v konfiguraci testu hello. Vlastní testy paměti teď také podporují hello možnost tooderive hello hostitele záhlaví chování z hello konfigurovaná nastavení protokolu HTTP. Tato konfigurace může být určen pomocí hello `PickHostNameFromback endAddress` parametr v konfiguraci testu hello. Pro koncové tooend funkce toowork musí být hello test a nastavení HTTP hello upravené tooreflect hello správnou konfiguraci.

Díky této funkci zadejte zákazníkům hello možnosti v nastavení hello HTTP a vlastní testy toohello odpovídající konfiguraci. Toto nastavení je pak vázaný tooa naslouchací proces a back end fondu pomocí pravidla.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak tooset až aplikační brány s webovou aplikaci jako back end člena fondu navštivte stránky: [nakonfigurovat App Service web apps s aplikační brány](application-gateway-web-app-powershell.md)
