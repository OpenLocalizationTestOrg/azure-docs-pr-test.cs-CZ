---
title: "aaaAzure Service Fabric se API Management přehled | Microsoft Docs"
description: "Tento článek slouží Úvod toousing Azure API Management jako aplikace Service Fabric tooyour brány."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: vturecek
ms.openlocfilehash: f01dc570a11e68cd4a2d878abbe6019e209e2f5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-with-azure-api-management-overview"></a>Service Fabric s Azure API Management – přehled

Cloudové aplikace obvykle nutné front-endu brány tooprovide jediný bod příjem příchozích dat pro uživatele, zařízení nebo jiné aplikace. V Service Fabric bránu být jakékoli bezstavové služby, jako [aplikace ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md), nebo jiné služby, které jsou určené pro příchozí provoz, například [Event Hubs](https://docs.microsoft.com/azure/event-hubs/), [IoT Hub](https://docs.microsoft.com/azure/iot-hub/), nebo [Azure API Management](https://docs.microsoft.com/azure/api-management/).

Tento článek slouží Úvod toousing Azure API Management jako aplikace Service Fabric tooyour brány. API Management se integruje přímo s Service Fabric, což vám toopublish rozhraní API s bohatou sadu směrování pravidla tooyour back endové Service Fabric služby. 

## <a name="architecture"></a>Architektura
Běžné architektura Service Fabric používá jednostránkovou webovou aplikaci, která provádí volání HTTP tooback endové služby, které zveřejňují rozhraní API HTTP. Hello [Service Fabric Začínáme ukázkovou aplikaci](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started) ukazuje příklad této architektury.

V tomto scénáři bezstavové webové služby slouží jako brány hello do hello aplikace Service Fabric. Tento postup vyžaduje, že toowrite webová služba, která může proxy protokolu HTTP požadavků tooback endové služby, jak je znázorněno v následujícím diagramu hello:

![Service Fabric s Azure API Management přehled topologie][sf-web-app-stateless-gateway]

Jelikož aplikace v složitost, takže hello brány, které musí představovat rozhraní API před řadu back endové služby. Azure API Management je navrženou toohandle komplexní rozhraní API pomocí pravidel směrování, řízení přístupu, omezení rychlosti, sledování, protokolování událostí a ukládání odpovědí do mezipaměti s minimálním úsilím na vaší straně. Azure API Management podporuje zjišťování služby Service Fabric, oddílu řešení a trasy toointelligently výběr repliky požadavky přímo tooback endové služby v Service Fabric, nemáte toowrite bezstavové brány rozhraní API. 

V tomto scénáři hello webové uživatelské rozhraní je stále vyhovovat prostřednictvím webové služby, při volání rozhraní API HTTP jsou spravovaná a směrován přes Azure API Management, jak je znázorněno v následujícím diagramu hello:

![Service Fabric s Azure API Management přehled topologie][sf-apim-web-app]

## <a name="application-scenarios"></a>Scénáře aplikací

Služby v Service Fabric může být bezstavové nebo stavová a jejich může být rozděleny do oddílů pomocí jednoho ze tří schémata: singleton, int-64 rozsahu a s názvem. Rozlišení koncového bodu služby vyžaduje identifikace na konkrétní oddíl instance konkrétní služby. Při rozpoznávání koncový bod služby, jak hello název instance služby (například `fabric:/myapp/myservice`) a také hello konkrétní oddíl hello služby se musí určit, s výjimkou v případě hello jednotlivý prvek oddílu.

Azure API Management můžete použít s libovolnou kombinaci bezstavové služby, stavové služby a všechny schéma rozdělení oddílů.

## <a name="send-traffic-tooa-stateless-service"></a>Odeslat provoz tooa bezstavové služby

V nejjednodušším případě hello přenosy předávaly instance tooa bezstavové služby. tooachieve, API Management operace obsahuje zásady zpracování příchozích s Service Fabric back-end, který mapuje tooa instance konkrétní bezstavové služby v Service Fabric hello back-end. Požadavky odeslané toothat služby odešlou tooa náhodných repliky instance hello bezstavové služby.

#### <a name="example"></a>Příklad
V následujícím scénáři hello, aplikace Service Fabric obsahuje bezstavové služby s názvem `fabric:/app/fooservice`, která zpřístupňuje rozhraní API interní HTTP. Název instance služby Hello je dobře známé a může být pevně přímo v hello zásady zpracování příchozí API Management. 

![Service Fabric s Azure API Management přehled topologie][sf-apim-static-stateless]

## <a name="send-traffic-tooa-stateful-service"></a>Odeslat provoz tooa stavové služby

Podobně jako bezstavové služby scénáři toohello provozu může být přeposílán tooa stavové služby instance. V takovém případě API Management operace obsahuje zásady zpracování příchozích s Service Fabric back-end, který mapuje oddílu konkrétní tooa požadavek konkrétní *stateful* instance služby. Hello oddílu toomap každý požadavek toois počítaný metodou lambda pomocí některé z hello příchozí požadavek HTTP, jako je například hodnota v hello zadejte cestu adresy URL. Hello zásad může být nakonfigurované toosend požadavky toohello pouze primární replikou nebo tooa náhodných repliky pro operace čtení.

#### <a name="example"></a>Příklad

V následujícím scénáři hello, aplikace Service Fabric obsahuje oddílů stavové služby s názvem `fabric:/app/userservice` který zpřístupňuje rozhraní API interní HTTP. Název instance služby Hello je dobře známé a může být pevně přímo v hello zásady zpracování příchozí API Management.  

Hello služby je rozdělený do oddílů pomocí hello Int64 schéma oddílu s dva oddíly a klíče rozsah, který zahrnuje `Int64.MinValue` příliš`Int64.MaxValue`. zásady back-end Hello vypočítá klíč oddílu v tomto rozsahu převedením hello `id` hodnota zadaná v hello adresu URL požadavku cesta tooa 64bitové celé číslo, i když libovolný algoritmus může být klíč oddílu použité sem toocompute hello. 

![Service Fabric s Azure API Management přehled topologie][sf-apim-static-stateful]

## <a name="send-traffic-toomultiple-stateless-services"></a>Odesílat provoz toomultiple bezstavové služby

V pokročilejších scénářích můžete definovat, mapy požadavky toomore než jedna služba instance API Management operace. V takovém případě každé operace obsahuje zásady, která mapuje požadavky, že instance tooa konkrétní služby založené na hodnotách z hello příchozí požadavek HTTP, jako třeba hello řetězce cestu nebo dotaz adresy URL a v případě hello stavové služby oddílu v rámci instance služby hello . 

tooachieve to API Management operace obsahuje zásady zpracování příchozích s Service Fabric back-end, který mapuje tooa instance bezstavové služby v závislosti na hodnotách načíst z příchozího požadavku HTTP hello back endové hello Service Fabric. Instance služby tooa požadavky jsou odesílány tooa náhodných repliky instance služby hello.

#### <a name="example"></a>Příklad

V tomto příkladu je vytvořena nová instance bezstavové služby pro každého uživatele, aplikace se dynamicky generovaném názvem pomocí hello následující vzorec:
 
 - `fabric:/app/users/<username>`

 Každá služba má jedinečný název, ale nejsou známé názvy hello počáteční, protože hello služby jsou vytvořené v odpovědi toouser nebo správce vstup a proto nemůže být pevně zakódované do APIM zásady nebo pravidla směrování. Místo toho hello název hello služby toowhich toosend požadavek se generuje ve definice zásady back-end hello z hello `name` hodnota zadaná pro cestu požadavku hello adresy URL. Například:

  - A požadavku příliš`/api/users/foo` je směrované tooservice instance`fabric:/app/users/foo`
  - A požadavku příliš`/api/users/bar` je směrované tooservice instance`fabric:/app/users/bar`

![Service Fabric s Azure API Management přehled topologie][sf-apim-dynamic-stateless]

## <a name="send-traffic-toomultiple-stateful-services"></a>Odesílat provoz toomultiple stavové služby

Podobně jako příklad toohello bezstavové služby, API Management operace můžete namapovat požadavky toomore než jeden **stateful** instance služby, ve které je případ také pravděpodobně nutné tooperform oddílu řešení pro každý stavové služby instance.

tooachieve to API Management operace obsahuje zásady zpracování příchozích s Service Fabric back-end, který mapuje tooa instance stavové služby v závislosti na hodnotách načíst z příchozího požadavku HTTP hello back endové hello Service Fabric. Kromě toomapping žádost o instanci služby toospecific, hello požadavek lze také namapované tooa konkrétní oddíl v rámci instance služby hello a volitelně tooeither hello primární repliku nebo náhodné sekundární repliky v rámci oddílu hello.

#### <a name="example"></a>Příklad

V tomto příkladu je vytvořena nová instance stavové služby pro každého uživatele aplikace hello dynamicky generovaném názvem pomocí hello následující vzorec:
 
 - `fabric:/app/users/<username>`

 Každá služba má jedinečný název, ale nejsou známé názvy hello počáteční, protože hello služby jsou vytvořené v odpovědi toouser nebo správce vstup a proto nemůže být pevně zakódované do APIM zásady nebo pravidla směrování. Místo toho hello název hello služby toowhich toosend požadavek se generuje ve definice zásady back-end hello z hello `name` cestu požadavku adresa URL zadaná hodnota hello. Například:

  - A požadavku příliš`/api/users/foo` je směrované tooservice instance`fabric:/app/users/foo`
  - A požadavku příliš`/api/users/bar` je směrované tooservice instance`fabric:/app/users/bar`

Každá instance služby je také rozdělena na oddíly pomocí hello Int64 schéma oddílu s dva oddíly a klíče rozsah, který zahrnuje `Int64.MinValue` příliš`Int64.MaxValue`. zásady back-end Hello vypočítá klíč oddílu v tomto rozsahu převedením hello `id` hodnota zadaná v hello adresu URL požadavku cesta tooa 64bitové celé číslo, i když libovolný algoritmus může být klíč oddílu použité sem toocompute hello. 

![Service Fabric s Azure API Management přehled topologie][sf-apim-dynamic-stateful]

## <a name="next-steps"></a>Další kroky

Postupujte podle hello [úvodní příručka](service-fabric-api-management-quick-start.md) tooset si vaše první Service Fabric cluster s API Management a toku požadavky prostřednictvím tooyour služby API Management.

<!-- links -->

<!-- pics -->
[sf-apim-web-app]: ./media/service-fabric-api-management-overview/sf-apim-web-app.png
[sf-web-app-stateless-gateway]: ./media/service-fabric-api-management-overview/sf-web-app-stateless-gateway.png
[sf-apim-static-stateless]: ./media/service-fabric-api-management-overview/sf-apim-static-stateless.png
[sf-apim-static-stateful]: ./media/service-fabric-api-management-overview/sf-apim-static-stateful.png
[sf-apim-dynamic-stateless]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateless.png
[sf-apim-dynamic-stateful]: ./media/service-fabric-api-management-overview/sf-apim-dynamic-stateful.png