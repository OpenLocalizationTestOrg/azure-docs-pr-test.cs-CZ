---
title: "aaaControlling službě Azure web app provoz Azure Traffic Manager"
description: "Tento článek obsahuje souhrnné informace pro Azure Traffic Manager, protože se týká tooAzure webové aplikace."
services: app-service\web
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: dabda633-e72f-4dd4-bf1c-6e945da456fd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2016
ms.author: cephalin
ms.openlocfilehash: a93d4c9370046d54e401e36e7b495af8b711a2aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Řízení síťového provozu webové aplikace Azure nástroje Azure Traffic Manager
> [!NOTE]
> Tento článek obsahuje souhrnné informace pro Microsoft Azure Traffic Manager, protože se týká tooAzure App Service Web Apps. Další informace o Azure Traffic Manager, sám, může navštívením hello odkazy na konci hello tohoto článku.
> 
> 

## <a name="introduction"></a>Úvod
Můžete použít Azure Traffic Manager toocontrol jak požadavky od klientů webové jsou tooweb distribuované aplikace v Azure App Service. Po přidání profilu Azure Traffic Manageru tooa koncových bodů webové aplikace, Azure Traffic Manager uchovává informace o stavu hello vašich webových aplikací (spuštěna, zastavena nebo odstraněné) tak, aby se můžete rozhodnout, která z těchto koncových bodů musí přijímat přenosy.

## <a name="load-balancing-methods"></a>Metody vyrovnávání zatížení
Azure Traffic Manager používá tři metody vyrovnávání zatížení různé. Tyto možnosti jsou popsány v následujícím seznamu, podle kterých se týkají tooAzure webové aplikace hello.

* **Převzetí služeb při selhání**: Pokud máte webové aplikace klony v různých oblastech, můžete použít tato metoda tooconfigure jedné webové aplikace tooservice veškerá komunikace s klienty webové a nakonfigurovat jiné webové aplikace v jiné oblasti tooservice, který provoz v případu hello první webové aplikace nedostupný.
* **Kruhové dotazování**: Pokud máte webové aplikace klony v různých oblastech, můžete použít tento provoz toodistribute metoda rovnoměrně napříč hello webové aplikace v různých oblastech.
* **Výkon**: hello výkonu metoda distribuuje provoz podle hello tooclients čas nejkratší dobu odezvy. Hello metoda výkonu lze použít pro webové aplikace v rámci hello stejné oblasti nebo v různých oblastech.

## <a name="web-apps-and-traffic-manager-profiles"></a>Webové aplikace a profily Traffic Manageru
tooconfigure hello řízení provozu webové aplikace, můžete vytvořit profil v Azure Traffic Manager, který používá jednu z metod, které jsou popsané Vyrovnávání zatížení hello tři a poté přidejte hello koncových bodů (v tomto případě webové aplikace) pro které chcete provoz toohello toocontrol profil. Stav vaší webové aplikace (spuštěna, zastavena nebo odstraněné) je pravidelně informovat toohello profilu tak, aby Azure Traffic Manager může směrovat přenosy odpovídajícím způsobem.

Při použití Azure Traffic Manageru službou Azure, mějte na paměti hello následující body:

* Pro webové pouze nasazení aplikací v rámci hello stejné oblasti, webové aplikace již poskytuje funkce převzetí služeb při selhání a kruhového dotazování bez ohledem tooweb aplikace režimu.
* Pro nasazení v hello stejné oblasti, kterou použít Web Apps ve spojení s jiné cloudové služby Azure, můžete kombinovat oba typy koncových bodů tooenable hybridní scénáře.
* V profilu můžete určit pouze jeden koncový bod webové aplikace podle oblastí. Když vyberete jako koncový bod pro jednu oblast webové aplikace, hello zbývající webové aplikace v této oblasti k dispozici pro výběr pro tento profil.
* Koncové Hello webové aplikace body, které zadáte v profilu Azure Traffic Manager se zobrazí v části hello **názvy domén** části na stránce konfigurace hello hello webové aplikace v hello profilu, ale nebude konfigurovat existuje.
* Po přidání profil webové aplikace tooa hello **adresa URL webu** na hello hello webový řídicím panelu zobrazí stránku portálu aplikace hello vlastní domény adresu URL webové aplikace hello, pokud jste nastavili jednu. Jinak, zobrazí adresu URL profilu Traffic Manageru hello (například `contoso.trafficmgr.com`). Obě hello název přímé domény hello webové aplikace a hello adresa URL správce provozu se nebude zobrazovat na stránce konfigurace hello webové aplikace v rámci hello **názvy domén** části.
* Názvy vlastních domén budou fungovat dle očekávání, ale přidání tooadding je tooyour webové aplikace, je nutné také nakonfigurovat vaše DNS mapy toopoint toohello adresa URL správce provozu. Informace o tom najdete v části tooset i vlastní doménu pro Azure webovou aplikaci, [konfigurace vlastního názvu domény pro webovou stránku Azure](app-service-web-tutorial-custom-domain.md).
* Můžete přidat pouze webových aplikací, které jsou v režimu standard nebo premium tooa profilu Azure Traffic Manageru.

## <a name="next-steps"></a>Další kroky
Koncepční a technický přehled o Azure Traffic Manager, najdete v části [Traffic Manager Přehled](../traffic-manager/traffic-manager-overview.md).

Další informace o používání správce provozu s webovými aplikacemi, najdete v příspěvcích na blogu hello [pomocí Azure Traffic Manager s weby Azure](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) a [Azure Traffic Manager teď můžete integrovat s weby Azure](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).

