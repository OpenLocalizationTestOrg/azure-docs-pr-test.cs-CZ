---
title: aaaAzure IoT Suite a Logic Apps | Microsoft Docs
description: "Kurz týkající se jak toohook až Logic Apps tooAzure IoT Suite pro obchodní proces."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4629a7af-56ca-4b21-a769-5fa18bc3ab07
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: corywink
ms.openlocfilehash: 6ef7311ac38f4e2ddb032cff0fb73591da5f76c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-connect-logic-app-tooyour-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Kurz: Připojte aplikace logiky tooyour Azure IoT Suite vzdálené monitorování předkonfigurované řešení
Hello [Microsoft Azure IoT Suite] [ lnk-internetofthings] předkonfigurovanému řešení vzdáleného monitorování je skvělým způsobem tooget rychle začít s sadu začátku do konce funkce, která exemplifies řešení IoT. Tento kurz vás provede jak tooadd aplikace logiky tooyour Microsoft Azure IoT Suite předkonfigurované řešení vzdáleného monitorování. Tyto kroky ukazují, jak může trvat i další řešení IoT propojením tooa obchodní proces.

*Pokud hledáte návod na tom, jak tooprovision vzdálené monitorování předkonfigurované řešení, přečtěte si téma [kurz: Začínáme s řešeními IoT předkonfigurované hello][lnk-getstarted].*

Než začnete tento kurz, proveďte následující kroky:

* Zřízení hello vzdálené monitorování předkonfigurované řešení ve vašem předplatném Azure.
* Vytvoření tooenable účet sendgrid vám umožňuje toosend e-mailu, která aktivuje obchodní proces. Můžete si zaregistrovat Bezplatný zkušební účet v [sendgrid vám umožňuje](https://sendgrid.com/) kliknutím **zkuste zdarma**. Po registraci pro bezplatný zkušební účet musíte toocreate [klíč rozhraní API](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) v Sendgridu, která uděluje oprávnění toosend e-mailu. Později v kurzu hello potřebujete tento klíč rozhraní API.

toocomplete tohoto kurzu budete potřebovat Visual Studio 2015 nebo Visual Studio 2017 akce hello toomodify v back-end hello předkonfigurovaných řešení.

Za předpokladu, že jste už zřízené vzdáleného monitorování předkonfigurované řešení, přejděte toohello skupinu prostředků pro toto řešení v hello [portál Azure][lnk-azureportal]. Skupina prostředků Hello má hello stejný název jako hello řešení, kterou si zvolili při zřízení řešení vzdáleného monitorování. Ve skupině prostředků hello zobrazí se všechny hello zřízení prostředků Azure pro vaše řešení s výjimkou hello aplikaci Azure Active Directory, které můžete najít v hello portálu Azure Classic. Hello následující snímek obrazovky ukazuje příklad **skupiny prostředků** okno pro vzdálené monitorování předkonfigurované řešení:

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

toobegin, nastavte hello logiku aplikace toouse s hello předkonfigurované řešení.

## <a name="set-up-hello-logic-app"></a>Nastavit hello aplikace logiky
1. Klikněte na tlačítko **přidat** hello horní části vaší okně skupiny prostředků v hello portálu Azure.
2. Vyhledejte **aplikace logiky**, vyberte ho a pak klikněte na tlačítko **vytvořit**.
3. Vyplňte hello **název** a použití hello stejné **předplatné** a **skupiny prostředků** jste použili při zřizování řešení vzdáleného monitorování. Klikněte na možnost **Vytvořit**.
   
    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)
4. Po dokončení nasazení uvidíte jako prostředek hello aplikace logiky, která je uvedena ve vaší skupině prostředků.
5. Klikněte na tlačítko hello aplikace logiky toonavigate toohello aplikace logiky okně vyberte hello **prázdné aplikace logiky** šablony tooopen hello **logiku aplikace Návrhář**.
   
    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)
6. Vyberte **požadavku**. Tuto akci Určuje, že příchozí požadavek HTTP s konkrétní JSON formátu aktivační událost jednání datové části.
7. Vložte následující kód do hello požadavku schématu JSON textu hello:
   
    ```json
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
   
   > [!NOTE]
   > Po uložení aplikace logiky hello, ale nejdřív je nutné přidat akci můžete zkopírovat hello URL pro hello HTTP post.
   > 
   > 
8. Klikněte na tlačítko **+ nový krok** pod ruční aktivační událost. Pak klikněte na tlačítko **přidat akci**.
   
    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)
9. Vyhledejte **sendgrid vám umožňuje - odesílání e-mailu** a klikněte na něj.
   
    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)
10. Zadejte název hello připojení, jako například **SendGridConnection**, zadejte hello **klíč rozhraní API sendgrid vám umožňuje** jste vytvořili při nastavení účtu Sendgridu a klikněte na tlačítko **vytvořit**.
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)
11. Přidání e-mailových adres, vlastní tooboth hello **z** a **k** pole. Přidat **vzdáleného sledování upozornění [DeviceId]** toohello **subjektu** pole. V hello **obsahu e-mailu** pole, přidejte **zařízení [DeviceId] ohlásil [measurementName] s hodnotou [measuredValue].** Můžete přidat **[DeviceId]**, **[measurementName]**, a **[measuredValue]** kliknutím v hello **můžete vložit data z předchozích kroků**části.
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)
12. Klikněte na tlačítko **Uložit** v horní nabídce hello.
13. Klikněte na tlačítko hello **požadavku** aktivační události a zkopírujte hello **Http Post toothis URL** hodnotu. Později v tomto kurzu musíte tuto adresu URL.

> [!NOTE]
> Logic Apps umožňují toorun [mnoho různých typů akce] [ lnk-logic-apps-actions] včetně akce v Office 365. 
> 
> 

## <a name="set-up-hello-eventprocessor-web-job"></a>Nastavit hello EventProcessor webové úlohy
V této části je připojit vaše toohello předkonfigurované řešení aplikace logiky, které jste vytvořili. toocomplete této úlohy přidejte hello URL tootrigger hello aplikace logiky toohello akci, která aktivuje se v případě hodnotu senzor zařízení překračuje prahovou hodnotu.

1. Použijte váš git tooclone hello nejnovější verze klienta hello [azure-iot-remote-monitoring úložiště github][lnk-rmgithub]. Například:
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. V sadě Visual Studio otevřete hello **RemoteMonitoring.sln** z místní kopie hello hello úložiště.
3. Otevřete hello **ActionRepository.cs** souboru v hello **infrastruktury\\úložiště** složky.
4. Aktualizace hello **actionIds** slovník s hello **Http Post toothis URL** jste si poznamenali u aplikace logiky následujícím způsobem:
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post toothis URL>" },
        { "Raise Alarm", "<Http Post toothis URL>" }
    };
    ```
5. Uložení změn hello v řešení a ukončení Visual Studio.

## <a name="deploy-from-hello-command-line"></a>Nasazení z příkazového řádku hello
V této části nasadíte vaší aktualizovanou verzi hello vzdálené monitorování řešení tooreplace hello verzi aktuálně spuštěných v Azure.

1. Následující hello [dev nastavení] [ lnk-devsetup] tooset pokyny prostředí pro nasazení.
2. toodeploy místně, postupujte podle hello [místní nasazení] [ lnk-localdeploy] pokyny.
3. toodeploy toohello cloudu a aktualizovat vaše stávající nasazení cloudu, postupujte podle hello [cloudové nasazení] [ lnk-clouddeploy] pokyny. Použijte název hello původní nasazení jako název nasazení hello. Například pokud byla volána původní nasazení hello **demologicapp**, použijte následující příkaz hello:
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   Když hello vytvořit skript se spustí, ujistěte se, toouse hello stejný účet Azure, předplatné, oblast a instance služby Active Directory, které jste použili při zřizování řešení hello.

## <a name="see-your-logic-app-in-action"></a>V tématu aplikace logiky v akci
Hello předkonfigurovanému řešení vzdáleného monitorování má dvě pravidla ve výchozím nastavení při zřizování řešení. Obě pravidla jsou na hello **SampleDevice001** zařízení:

* Teplotní > 38.00
* Vlhkosti > 48.00

pravidlo teploty Hello aktivuje hello **vyvolat alarmů** akce a hello vlhkosti pravidlo aktivuje hello **SendMessage** akce. Za předpokladu, že jste použili hello stejnou adresu URL pro obě akce hello **ActionRepository** třídy, vaše aplikace logiky aktivačních událostí pro buď pravidlo. Obě pravidla použít toosend sendgrid vám umožňuje e-mailu toohello **k** adresu s podrobnostmi hello výstrahy.

> [!NOTE]
> Hello aplikace logiky pokračuje tootrigger pokaždé, když se dosáhla prahová hodnota hello. nepotřebné e-mailů tooavoid, můžete zakázat hello pravidla ve vašem řešení portálu nebo zakázat hello aplikace logiky v hello [portál Azure][lnk-azureportal].
> 
> 

Kromě toho tooreceiving e-mailů, můžete také zjistit spuštění hello aplikace logiky hello portálu:

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Další kroky
Teď, když jste použili aplikaci logiky tooconnect hello předkonfigurované řešení tooa obchodní proces, můžete další informace o hello možností pro přizpůsobení hello předkonfigurované řešení:

* [Použití dynamické telemetrie s hello předkonfigurovanému řešení vzdáleného monitorování][lnk-dynamic]
* [Informace metadat zařízení v hello předkonfigurovanému řešení vzdáleného monitorování][lnk-devinfo]

[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
