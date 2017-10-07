---
title: "aaaAzure IoT Suite – nejčastější dotazy | Microsoft Docs"
description: "Nejčastější dotazy k sadě IoT Suite"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: corywink
ms.openlocfilehash: cb39e24af6d1ce2afea554285512d05b2d7c721e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-iot-suite"></a>Nejčastější dotazy k sadě IoT Suite

Viz také konkrétní připojené factory hello [– nejčastější dotazy](iot-suite-faq-cf.md).

### <a name="where-can-i-find-hello-source-code-for-hello-preconfigured-solutions"></a>Kde najdu hello zdrojového kódu pro hello předkonfigurované řešení?

Hello zdrojového kódu se ukládají v hello následující úložišť GitHub:
* [Předkonfigurované řešení vzdáleného monitorování][lnk-remote-monitoring-github]
* [Předkonfigurované řešení prediktivní údržby][lnk-predictive-maintenance-github]
* [Připojené objekt pro vytváření předkonfigurovaného řešení](https://github.com/Azure/azure-iot-connected-factory)

### <a name="how-do-i-update-toohello-latest-version-of-hello-remote-monitoring-preconfigured-solution-that-uses-hello-iot-hub-device-management-features"></a>Jak aktualizovat toohello nejnovější verzi hello předkonfigurovaného řešení vzdáleného monitorování, používá hello funkce správy zařízení IoT Hub?

* Pokud nasadíte předkonfigurované řešení z lokality https://www.azureiotsuite.com/ hello, vždy nasadí novou instanci třídy hello nejnovější verzi hello řešení.
* Pokud nasadíte předkonfigurované řešení hello příkazového řádku, můžete aktualizovat existující nasazení s nový kód. V tématu [cloudové nasazení] [ lnk-cloud-deployment] v hello Githubu [úložiště][lnk-remote-monitoring-github].

### <a name="how-can-i-add-support-for-a-new-device-method-toohello-remote-monitoring-preconfigured-solution"></a>Jak můžete přidat podporu pro nové zařízení metoda toohello předkonfigurovanému řešení vzdáleného monitorování?

Části hello [přidat podporu pro nové simulátoru toohello metoda] [ lnk-add-method] v hello [přizpůsobení předkonfigurovaného řešení] [ lnk-customize] článku.

### <a name="hello-simulated-device-is-ignoring-my-desired-property-changes-why"></a>Simulované zařízení Hello provedené změny požadovanou vlastnost, proč ignoruje?
V hello předkonfigurované řešení vzdáleného monitorování, kód hello simulované zařízení používá jenom hello **Desired.Config.TemperatureMeanValue** a **Desired.Config.TelemetryInterval** požadovaných vlastností tooupdate hello hlášené vlastnosti. Všechny ostatní žádosti o změnu požadovanou vlastnost se ignorují.

### <a name="my-device-does-not-appear-in-hello-list-of-devices-in-hello-solution-dashboard-why"></a>Moje zařízení nezobrazí v seznamu hello zařízení na řídicím panelu řešení hello, proč?

Hello seznam zařízení na řídicím panelu řešení hello používá dotaz tooreturn hello seznam zařízení. V současné době dotaz nemůže vrátit více než 10 tisíc zařízení. Pokuste více omezující hello kritérií vyhledávání pro svůj dotaz.

### <a name="whats-hello-difference-between-deleting-a-resource-group-in-hello-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Co je hello rozdíl mezi odstranění skupiny prostředků v hello Azure portálu a kliknutím na Odstranit v předkonfigurovaném řešení na stránkách azureiotsuite.com?

* Pokud odstraníte hello předkonfigurované řešení v [azureiotsuite.com][lnk-azureiotsuite], odstranit všechny prostředky hello, které byly zřízeny při vytváření hello předkonfigurované řešení. Pokud jste přidali skupinu prostředků toohello další prostředky, tyto prostředky budou také odstraněny. 
* Pokud odstraníte skupinu prostředků hello hello [portál Azure][lnk-azure-portal], se odstraní pouze prostředky hello v příslušné skupině prostředků. Musíte taky toodelete hello Azure Active Directory aplikace spojené s hello předkonfigurované řešení v hello [portál Azure classic][lnk-classic-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Kolik instancí služby IoT Hub můžete zřídit v jednom předplatném?

Ve výchozím nastavení můžete zřídit [10 centra IoT na jedno předplatné][link-azuresublimits]. Můžete vytvořit [lístek podpory Azure] [ link-azuresupportticket] tooraise toto omezení. V důsledku toho od každé předkonfigurované řešení zřizuje novou službu IoT Hub, můžete zřídit až too10 předkonfigurovaných řešení v daném předplatném. 

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Kolik instancí Azure Cosmos DB můžete zřídit v jednom předplatném?

Padesát. Můžete vytvořit [lístek podpory Azure] [ link-azuresupportticket] tooraise toto omezení, ale ve výchozím nastavení, můžete zřídit 50 instancí Cosmos DB za předplatné. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Kolik bezplatných rozhraní API Map Bing můžu zřídit v jednom předplatném?

Dvě. Můžete vytvořit pouze dvě vnitřní transakce úroveň 1 mapy Bing pro podnikových plánů v předplatné Azure. ve výchozím nastavení s plánem hello vnitřní transakce úrovně 1 se zřídí řešení vzdáleného monitorování Hello. V důsledku toho můžete zřídit až tootwo vzdáleného sledování řešení v předplatném beze změn.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Mám nasazené řešení vzdáleného monitorování se statickou mapou, jak přidám interaktivní mapu Bing?

1. Získání rozhraní API map Bing pro QueryKey Enterprise z [portál Azure][lnk-azure-portal]: 
   
   1. Přejděte toohello skupinu prostředků, je-li vaše rozhraní API map Bing pro podniky hello [portál Azure][lnk-azure-portal].
   2. Klikněte na tlačítko **všechna nastavení**, pak **Správa klíčů**. 
   3. Zobrazí se dva klíče: **MasterKey** a **QueryKey**. Zkopírujte hodnotu hello **QueryKey**.
      
      > [!NOTE]
      > Nemáte účet rozhraní API Map Bing pro podniky? Vytvořit v hello [portál Azure] [ lnk-azure-portal] kliknutím na + nové, vyhledáte rozhraní API map Bing pro podniky a postupujte podle výzvy toocreate.
      > 
      > 
2. Stáhněte dolů hello nejnovější kód z hello [Azure-IoT-Remote-Monitoring][lnk-remote-monitoring-github].
3. Spusťte místní nebo cloudové nasazení podle pokynů hello příkazového řádku nasazení hello složce /docs/ v úložišti hello. 
4. Po spuštění místního nebo cloudového nasazení vyhledejte v kořenové složce hello *. User.config vytvořený během nasazení. Otevřete tento soubor v textovém editoru. 
5. Změna hello následující řádek tooinclude hello hodnotu, kterou jste zkopírovali z vaší **QueryKey**: 
   
   `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Můžu vytvořit předkonfigurované řešení, když mám Microsoft Azure pro DreamSpark?

V současné době nelze vytvořit předkonfigurované řešení s [Microsoft Azure pro DreamSpark] [ lnk-dreamspark] účtu. Ale můžete vytvořit [Bezplatný zkušební účet Azure] [ lnk-30daytrial] si během několika minut, který vám umožní vytvořit předkonfigurované řešení.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a>Můžete vytvořit předkonfigurované řešení, pokud mám předplatné Cloud Solution Provider (CSP)?

V současné době nelze vytvořit předkonfigurované řešení s předplatným Cloud Solution Provider (CSP). Ale můžete vytvořit [Bezplatný zkušební účet Azure] [ lnk-30daytrial] si během několika minut, který vám umožní vytvořit předkonfigurované řešení.

### <a name="how-do-i-delete-an-aad-tenant"></a>Jak se odstraním klienta AAD?

Najdete v příspěvku blogu od Erica Golpeho [návod odstranění klient služby Azure AD][lnk-delete-aad-tennant].

### <a name="next-steps"></a>Další kroky

Můžete také prozkoumat některé hello další funkce a možnosti hello předkonfigurovaná řešení IoT Suite:

* [Přehled řešení předkonfigurované prediktivní údržby][lnk-predictive-overview]
* [Přehled připojené objekt pro vytváření předkonfigurovaného řešení](iot-suite-connected-factory-overview.md)
* [Zabezpečení IoT z hello pozadí][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
[lnk-cloud-deployment]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
[lnk-add-method]: iot-suite-guidance-on-customizing-preconfigured-solutions.md#add-support-for-a-new-method-to-the-simulator
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-predictive-maintenance-github]: https://github.com/Azure/azure-iot-predictive-maintenance
