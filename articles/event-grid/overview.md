---
title: "Přehled událostí mřížky aaaAzure"
description: "Popisuje mřížky událostí Azure a jeho koncepty."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/18/2017
ms.author: babanisa
ms.openlocfilehash: 95dce22e9335df88e81b134143a6c14994c26b8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="an-introduction-tooazure-event-grid"></a>Úvod tooAzure událostí mřížky

Azure mřížky událostí vám umožní tooeasily sestavení aplikace pomocí architektury na základě událostí. Můžete vybrat hello prostředků Azure chcete vytvořit toosubscribe k a poskytnout hello obslužná rutina události nebo Webhooku koncový bod toosend hello události. Událost mřížky má integrovanou podporu pro události pocházející ze služby Azure, jako jsou skupiny prostředků a úložiště objektů BLOB. Událost mřížky má také vlastní podporu pro aplikace a událostí třetích stran, pomocí vlastní témata a vlastní webhooky. 

Můžete použít koncových bodů toodifferent určité události tooroute filtry, vícesměrového vysílání toomultiple koncových bodů a ujistěte se, že události jsou spolehlivě doručovány. Událost mřížky také obsahuje vestavěnou podporou pro třetí strany a vlastní události.

Pro verzi preview hello událostí mřížky podporuje **westus2** a **westcentralus** umístění. Přidá jiných oblastí.

Tento článek obsahuje přehled Azure událostí mřížky. Pokud chcete začít s událostí mřížky tooget, najdete v části [vytvořit a směrování vlastních událostí s Azure událostí mřížky](custom-event-quickstart.md).

![Funkční model událostí mřížky](./media/overview/event-grid-functional-model.png)

Úložiště objektů Blob v současné době není veřejně dostupné jako vydavatel.

## <a name="concepts"></a>Koncepty

V mřížce událostí Azure, která umožňují zprovoznění existují pět koncepty:

* **Události** -co se stalo.
* **Zdroje událostí nebo vydavatelů** – tam, kde došlo k události hello.
* **Témata** -hello koncový bod, kde vydavatelů odesílat události.
* **Odběry událostí** -hello koncového bodu nebo integrované mechanismus tooroute událostí, někdy toomultiple obslužné rutiny. Odběry jsou také používány obslužné rutiny toointelligently filtru příchozí události.
* **Obslužné rutiny událostí** – hello aplikace nebo služby reagující toohello událostí.

Další informace o těchto postupech najdete v tématu [koncepty v mřížce událostí Azure](concepts.md).

## <a name="capabilities"></a>Možnosti

Tady jsou některé klíčové funkce Azure událostí mřížky hello:

* **Jednoduchost** -bodu a klikněte na tlačítko tooaim události z obslužné rutiny události tooany prostředků Azure nebo koncový bod.
* **Rozšířené filtrování** -filtru pro událost typu nebo událostí publikování tooensure cesta obslužné rutiny událostí přijímat pouze relevantní události.
* **FAN-out** -přihlášení k odběru několika koncové body toohello stejné události toosend zkopíruje z hello událostí tooas mnoha místech podle potřeby.
* **Spolehlivost** -využívat znova 24 hodin s exponenciálního omezení rychlosti tooensure doručí události.
* **Platím za událostí** – platíte jenom pro velikost hello použijete událostí mřížky.
* **Vysoká propustnost** -sestavení vysoký počet úloh v mřížce událostí s podporou miliony událostí za sekundu.
* **Předdefinované události** – zprovoznění a rychlé spuštění s integrovanou událostí definovaných prostředků.
* **Vlastní události** -použít událost mřížky trasy, filtr a spolehlivě vlastních událostí doručit ve vaší aplikaci.

## <a name="built-in-publisher-and-handler-integration"></a>Integraci vydavatele a obslužné rutiny

Azure nabízí podporu předdefinované událostí pomocí množství služeb, včetně vydavatele a obslužné rutiny.

### <a name="publishers"></a>Vydavatelé

V současné době hello následující služby Azure mají podporu předdefinované vydavatele pro mřížky událostí:

* Skupiny prostředků (operace správy)
* Předplatná Azure (operace správy)
* Event Hubs
* Vlastní témata

Jinými službami Azure se přidá tohoto roku.

### <a name="handlers"></a>Obslužné rutiny

V současné době hello následující služby Azure mají podporu předdefinované obslužné rutiny událostí mřížky: 

* Funkce Azure
* Logic Apps
* Azure Automation
* Webhooky

Jinými službami Azure se přidá tohoto roku.

## <a name="what-can-i-do-with-event-grid"></a>Co můžete dělat s událostí mřížky?

Azure mřížky událostí poskytuje několik možností, které významně zlepšit bez serveru, ops automatizace a integraci pracovních: 

### <a name="serverless-application-architectures"></a>Architektury aplikací bez serveru

![Aplikaci bez serveru](./media/overview/serverless_web_app.png)

Event Grid propojuje zdroje dat a obslužné rutiny událostí. Například použijte aktivační událost mřížky tooinstantly analýza bez serveru funkce toorun obrázků pokaždé, když je nové fotografie přidat tooa kontejner úložiště objektů blob. 

### <a name="ops-automation"></a>Automatizace OPS

![Automatizace operací](./media/overview/Ops_automation.png)

Událost mřížky vám umožní toospeed automatizace a zjednodušit vynucení zásad. Event Grid může například upozornit službu Azure Automation při vytvoření virtuálního počítače nebo aktivaci služby SQL Database. Tyto události můžete použít tooautomatically zkontrolujte, jestli konfigurace služby jsou kompatibilní, put metadata do nástroje operations, značka virtuálních počítačů nebo soubor pracovních položek.

### <a name="application-integration"></a>Integrace aplikací

![Integrace aplikací](./media/overview/app_integration.png)

Event Grid propojuje vaši aplikaci s dalšími službami. Například vytvořit vlastní téma toosend vaší aplikace událostí data tooEvent mřížky a využít její spolehlivé doručení advanced směrování a přímá integrace s Azure. Alternativně můžete použít událost mřížky s Logic Apps tooprocess kdekoliv a dat bez nutnosti psaní kódu. 

## <a name="how-is-event-grid-different-from-other-azure-integration-services"></a>Jak se liší od jiných služeb integrace se službou Azure mřížky událostí?

Mřížka událostí je eventing propojovacího rozhraní, která umožňuje událostmi, reaktivní programování. Ho se úzce integruje se službami Azure a může být integrovaná v služeb třetích stran. Hello zpráva události obsahuje hello informace, které potřebujete tooreact toochanges služeb a aplikací. Mřížky událostí není datovém kanálu a nejsou poskytovány hello skutečný objekt, který byl aktualizován.

Service Bus je skvěle hodí pro tradiční podnikové aplikace, které vyžadují transakcí, řazení, detekci duplikátů a okamžitou konzistence. Mřížka událostí je určených rychlost, škálování, spektra a nízké náklady v reaktivní modelu. Je architektura skvěle hodí tooserverless.

Událost mřížky doplňuje jinými službami Azure jako Logic Apps a Event Hubs. Aktivační události mřížky hello logiku aplikace toobegin jejího pracovního postupu. Služba Event Hubs pracuje s událostí mřížky tím, že vám tooreact tooevents z zaznamenat centra událostí a kanálů pro příchozí a transformaci dat sestavení.

## <a name="how-much-does-event-grid-cost"></a>Jaké události mřížky náklady?

Azure mřížky událostí používá model tvorby cen platím za událostí, takže platíte jenom se používá.

Událost mřížky stojí 0,60 za mil. operace ($0,30 verzi Preview) a jsou hello nejprve 100 000 operaci za měsíc zdarma. Operace jsou definovány jako příjem příchozích dat událostí, rozšířená shoda, pokus o doručení a správu volání.  Další podrobnosti naleznete na hello [stránce s cenami](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="next-steps"></a>Další kroky

* [Vytvoření a přihlášení k odběru události toocustom](custom-event-quickstart.md) rovnou a začít posílat svůj vlastní koncový bod tooany vlastních událostí pomocí rychlého startu Azure událostí mřížky hello.
* [Použití aplikace logiky jako obslužné rutiny události](monitor-virtual-machine-changes-event-grid-logic-app.md) kurz týkající se vytváření aplikace pomocí Logic Apps tooreact tooevents nabídnutých podle událostí mřížky.
* [Odkazu k REST API mřížky událostí](/rest/api/eventgrid)  
  Poskytuje další odborné informace o hello mřížky událostí Azure a odkaz pro správě přihlášení k odběru událostí, směrování a filtrování.
