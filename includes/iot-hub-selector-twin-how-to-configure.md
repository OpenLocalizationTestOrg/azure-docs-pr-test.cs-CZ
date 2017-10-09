> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
> * [C#/Node.js](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-how-to-configure.md)
> 
> 

## <a name="introduction"></a>Úvod

V [začít pracovat s dvojčata zařízení IoT Hub][lnk-twin-tutorial], jste se dozvěděli, jak tooset zařízení metadata z řešení zpět ukončení pomocí *značky*, sestavy podmínky zařízení z aplikace na zařízení pomocí *hlášené vlastnosti*a tyto informace pomocí jazyka SQL jako dotaz.

V tomto kurzu se dozvíte, jak toouse hello dvojče zařízení hello *potřeby vlastnosti* spolu s *hlášené vlastnosti*, tooremotely konfigurace aplikací pro zařízení. Přesněji řečeno tento kurz ukazuje, jak dvojče zařízení hlášené a požadované vlastnosti povolte vícekrokový konfiguraci aplikace zařízení a poskytovat hello viditelnost toohello back-end řešení hello stav této operace na všech zařízeních. Můžete najít další informace o roli hello konfigurací zařízení v [přehled správy zařízení s centrem IoT][lnk-dm-overview].

Na vysoké úrovni použití dvojčata zařízení umožňuje hello řešení back-end toospecify hello požadované konfigurace pro zařízení, hello spravované, místo abyste odesílali určité příkazy. Toto zařízení hello starosti nastavení hello nejlepší způsob, jak tooupdate konfiguraci (velmi důležité pro scénáře IoT, kde hello možnost tooimmediately provádět určité příkazy vliv na určité zařízení podmínky), vloží při průběžně reporting toohello back-end řešení hello aktuální stav a potenciální chybové stavy hello procesu aktualizace. Tento vzor je instrumentálního toohello správu velkých sad zařízení, protože umožňuje hello řešení back-end toohave úplný přehled hello stavu procesu konfigurace hello ve všech zařízeních.

> [!NOTE]
> Ve scénářích, kde jsou ovládaná zařízení více interaktivní způsobem (zapněte ventilátor z aplikace řízené uživatele), zvažte použití [přímé metody][lnk-methods].
> 
> 

V tomto kurzu hello řešení back-end změny konfigurace telemetrie hello cílové zařízení a v důsledku, který aplikace hello zařízení dodržuje tooapply vícekrokový proces konfigurace aktualizací (např., které vyžadují restartování modulu softwaru, které to kurz simuluje s jednoduché zpožděním).

back-end Hello řešení ukládá konfiguraci hello v dvojče zařízení hello požadované vlastnosti v hello následujícím způsobem:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of hello configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [!NOTE]
> Vzhledem k tomu, že konfigurace může být složité objekty, jsou obvykle přiřazeny jedinečné ID (hodnoty hash nebo [identifikátory GUID][lnk-guid]) toosimplify jejich porovnání.
> 
> 

Hello aplikaci zařízení sestavy své aktuální konfiguraci zrcadlení hello potřeby vlastnost **telemetryConfig** v hello hlášené vlastnosti:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of hello current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Všimněte si, jak hello hlášené **telemetryConfig** má další vlastnost **stav**, používá tooreport hello stav procesu aktualizace konfigurace hello.

Po přijetí nové požadované konfigurace aplikace hello zařízení sestavy čekající konfigurace změnou hello informace:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of hello current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of hello pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Potom později některé aplikace hello zařízení budou hlásit hello úspěch nebo selhání této operace aktualizací hello nad vlastností.
Všimněte si, jak back-end hello řešení je možné, v každém okamžiku tooquery hello stav procesu konfigurace hello ve všech zařízeních hello.

V tomto kurzu získáte informace o následujících postupech:

* Vytvoření aplikace simulovaného zařízení, která přijímá aktualizace konfigurace z back-end hello řešení a hlásí víc aktualizací jako *hlášené vlastnosti* v konfiguraci hello proces aktualizovat.
* Vytvořte aplikaci back-end aktualizace hello požadované konfigurace zařízení, a pak dotazy hello proces aktualizace konfigurace.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier
