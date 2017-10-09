> [!div class="op_single_selector"]
> * [<span data-ttu-id="796e0-101">Node.js</span><span class="sxs-lookup"><span data-stu-id="796e0-101">Node.js</span></span>](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
> * [<span data-ttu-id="796e0-102">C#/Node.js</span><span class="sxs-lookup"><span data-stu-id="796e0-102">C#/Node.js</span></span>](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)
> * [<span data-ttu-id="796e0-103">C#</span><span class="sxs-lookup"><span data-stu-id="796e0-103">C#</span></span>](../articles/iot-hub/iot-hub-csharp-csharp-twin-how-to-configure.md)
> 
> 

## <a name="introduction"></a><span data-ttu-id="796e0-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="796e0-104">Introduction</span></span>

<span data-ttu-id="796e0-105">V [začít pracovat s dvojčata zařízení IoT Hub][lnk-twin-tutorial], jste se dozvěděli, jak tooset zařízení metadata z řešení zpět ukončení pomocí *značky*, sestavy podmínky zařízení z aplikace na zařízení pomocí *hlášené vlastnosti*a tyto informace pomocí jazyka SQL jako dotaz.</span><span class="sxs-lookup"><span data-stu-id="796e0-105">In [Get started with IoT Hub device twins][lnk-twin-tutorial], you learned how tooset device metadata from your solution back end using *tags*, report device conditions from a device app using *reported properties*, and query this information using a SQL-like language.</span></span>

<span data-ttu-id="796e0-106">V tomto kurzu se dozvíte, jak toouse hello dvojče zařízení hello *potřeby vlastnosti* spolu s *hlášené vlastnosti*, tooremotely konfigurace aplikací pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="796e0-106">In this tutorial, you will learn how toouse hello hello device twin's *desired properties* along with *reported properties*, tooremotely configure device apps.</span></span> <span data-ttu-id="796e0-107">Přesněji řečeno tento kurz ukazuje, jak dvojče zařízení hlášené a požadované vlastnosti povolte vícekrokový konfiguraci aplikace zařízení a poskytovat hello viditelnost toohello back-end řešení hello stav této operace na všech zařízeních.</span><span class="sxs-lookup"><span data-stu-id="796e0-107">More specifically, this tutorial shows how a device twin's reported and desired properties enable a multi-step configuration of a device application, and provide hello visibility toohello solution back end of hello status of this operation across all devices.</span></span> <span data-ttu-id="796e0-108">Můžete najít další informace o roli hello konfigurací zařízení v [přehled správy zařízení s centrem IoT][lnk-dm-overview].</span><span class="sxs-lookup"><span data-stu-id="796e0-108">You can find more information regarding hello role of device configurations in [Overview of device management with IoT Hub][lnk-dm-overview].</span></span>

<span data-ttu-id="796e0-109">Na vysoké úrovni použití dvojčata zařízení umožňuje hello řešení back-end toospecify hello požadované konfigurace pro zařízení, hello spravované, místo abyste odesílali určité příkazy.</span><span class="sxs-lookup"><span data-stu-id="796e0-109">At a high level, using device twins enables hello solution back end toospecify hello desired configuration for hello managed devices, instead of sending specific commands.</span></span> <span data-ttu-id="796e0-110">Toto zařízení hello starosti nastavení hello nejlepší způsob, jak tooupdate konfiguraci (velmi důležité pro scénáře IoT, kde hello možnost tooimmediately provádět určité příkazy vliv na určité zařízení podmínky), vloží při průběžně reporting toohello back-end řešení hello aktuální stav a potenciální chybové stavy hello procesu aktualizace.</span><span class="sxs-lookup"><span data-stu-id="796e0-110">This puts hello device in charge of setting up hello best way tooupdate its configuration (very important in IoT scenarios where specific device conditions affect hello ability tooimmediately carry out specific commands), while continually reporting toohello solution back end hello current state and potential error conditions of hello update process.</span></span> <span data-ttu-id="796e0-111">Tento vzor je instrumentálního toohello správu velkých sad zařízení, protože umožňuje hello řešení back-end toohave úplný přehled hello stavu procesu konfigurace hello ve všech zařízeních.</span><span class="sxs-lookup"><span data-stu-id="796e0-111">This pattern is instrumental toohello management of large sets of devices, as it enables hello solution back end toohave full visibility of hello state of hello configuration process across all devices.</span></span>

> [!NOTE]
> <span data-ttu-id="796e0-112">Ve scénářích, kde jsou ovládaná zařízení více interaktivní způsobem (zapněte ventilátor z aplikace řízené uživatele), zvažte použití [přímé metody][lnk-methods].</span><span class="sxs-lookup"><span data-stu-id="796e0-112">In scenarios where devices are controlled in a more interactive fashion (turn on a fan from a user-controlled app), consider using [direct methods][lnk-methods].</span></span>
> 
> 

<span data-ttu-id="796e0-113">V tomto kurzu hello řešení back-end změny konfigurace telemetrie hello cílové zařízení a v důsledku, který aplikace hello zařízení dodržuje tooapply vícekrokový proces konfigurace aktualizací (např., které vyžadují restartování modulu softwaru, které to kurz simuluje s jednoduché zpožděním).</span><span class="sxs-lookup"><span data-stu-id="796e0-113">In this tutorial, hello solution back end changes hello telemetry configuration of a target device and, as a result of that, hello device app follows a multi-step process tooapply a configuration update (for example, requiring a software module restart, which this tutorial simulates with a simple delay).</span></span>

<span data-ttu-id="796e0-114">back-end Hello řešení ukládá konfiguraci hello v dvojče zařízení hello požadované vlastnosti v hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="796e0-114">hello solution back end stores hello configuration in hello device twin's desired properties in hello following way:</span></span>

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
> <span data-ttu-id="796e0-115">Vzhledem k tomu, že konfigurace může být složité objekty, jsou obvykle přiřazeny jedinečné ID (hodnoty hash nebo [identifikátory GUID][lnk-guid]) toosimplify jejich porovnání.</span><span class="sxs-lookup"><span data-stu-id="796e0-115">Since configurations can be complex objects, they are usually assigned unique ids (hashes or [GUIDs][lnk-guid]) toosimplify their comparisons.</span></span>
> 
> 

<span data-ttu-id="796e0-116">Hello aplikaci zařízení sestavy své aktuální konfiguraci zrcadlení hello potřeby vlastnost **telemetryConfig** v hello hlášené vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="796e0-116">hello device app reports its current configuration mirroring hello desired property **telemetryConfig** in hello reported properties:</span></span>

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

<span data-ttu-id="796e0-117">Všimněte si, jak hello hlášené **telemetryConfig** má další vlastnost **stav**, používá tooreport hello stav procesu aktualizace konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="796e0-117">Note how hello reported **telemetryConfig** has an additional property **status**, used tooreport hello state of hello configuration update process.</span></span>

<span data-ttu-id="796e0-118">Po přijetí nové požadované konfigurace aplikace hello zařízení sestavy čekající konfigurace změnou hello informace:</span><span class="sxs-lookup"><span data-stu-id="796e0-118">When a new desired configuration is received, hello device app reports a pending configuration by changing hello information:</span></span>

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

<span data-ttu-id="796e0-119">Potom později některé aplikace hello zařízení budou hlásit hello úspěch nebo selhání této operace aktualizací hello nad vlastností.</span><span class="sxs-lookup"><span data-stu-id="796e0-119">Then, at some later time, hello device app will report hello success or failure of this operation by updating hello above property.</span></span>
<span data-ttu-id="796e0-120">Všimněte si, jak back-end hello řešení je možné, v každém okamžiku tooquery hello stav procesu konfigurace hello ve všech zařízeních hello.</span><span class="sxs-lookup"><span data-stu-id="796e0-120">Note how hello solution back end is able, at any time, tooquery hello status of hello configuration process across all hello devices.</span></span>

<span data-ttu-id="796e0-121">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="796e0-121">This tutorial shows you how to:</span></span>

* <span data-ttu-id="796e0-122">Vytvoření aplikace simulovaného zařízení, která přijímá aktualizace konfigurace z back-end hello řešení a hlásí víc aktualizací jako *hlášené vlastnosti* v konfiguraci hello proces aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="796e0-122">Create a simulated device app that receives configuration updates from hello solution back end, and reports multiple updates as *reported properties* on hello configuration update process.</span></span>
* <span data-ttu-id="796e0-123">Vytvořte aplikaci back-end aktualizace hello požadované konfigurace zařízení, a pak dotazy hello proces aktualizace konfigurace.</span><span class="sxs-lookup"><span data-stu-id="796e0-123">Create a back-end app that updates hello desired configuration of a device, and then queries hello configuration update process.</span></span>

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier
