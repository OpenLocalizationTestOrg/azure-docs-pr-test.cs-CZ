> [!div class="op_single_selector"]
> * [<span data-ttu-id="d5793-101">Node.js</span><span class="sxs-lookup"><span data-stu-id="d5793-101">Node.js</span></span>](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
> * [<span data-ttu-id="d5793-102">C#/Node.js</span><span class="sxs-lookup"><span data-stu-id="d5793-102">C#/Node.js</span></span>](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)
> * [<span data-ttu-id="d5793-103">C#</span><span class="sxs-lookup"><span data-stu-id="d5793-103">C#</span></span>](../articles/iot-hub/iot-hub-csharp-csharp-twin-how-to-configure.md)
> 
> 

## <a name="introduction"></a><span data-ttu-id="d5793-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="d5793-104">Introduction</span></span>

<span data-ttu-id="d5793-105">V [začít pracovat s dvojčata zařízení IoT Hub][lnk-twin-tutorial], jste se dozvěděli, jak nastavit metadat zařízení z back-end vašeho řešení pomocí *značky*, sestavy podmínky zařízení z aplikace na zařízení pomocí *hlášené vlastnosti*a tyto informace pomocí jazyka SQL jako dotaz.</span><span class="sxs-lookup"><span data-stu-id="d5793-105">In [Get started with IoT Hub device twins][lnk-twin-tutorial], you learned how to set device metadata from your solution back end using *tags*, report device conditions from a device app using *reported properties*, and query this information using a SQL-like language.</span></span>

<span data-ttu-id="d5793-106">V tomto kurzu se dozvíte, jak používat dvojče zařízení *potřeby vlastnosti* spolu s *hlášené vlastnosti*, pro vzdálenou konfiguraci aplikací pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="d5793-106">In this tutorial, you will learn how to use the the device twin's *desired properties* along with *reported properties*, to remotely configure device apps.</span></span> <span data-ttu-id="d5793-107">Přesněji řečeno tento kurz ukazuje, jak dvojče zařízení hlášené a požadované vlastnosti povolte vícekrokový konfiguraci aplikace zařízení a poskytovat viditelnost na back-end řešení stavu této operace na všech zařízeních.</span><span class="sxs-lookup"><span data-stu-id="d5793-107">More specifically, this tutorial shows how a device twin's reported and desired properties enable a multi-step configuration of a device application, and provide the visibility to the solution back end of the status of this operation across all devices.</span></span> <span data-ttu-id="d5793-108">Můžete najít další informace o roli konfigurací zařízení v [přehled správy zařízení s centrem IoT][lnk-dm-overview].</span><span class="sxs-lookup"><span data-stu-id="d5793-108">You can find more information regarding the role of device configurations in [Overview of device management with IoT Hub][lnk-dm-overview].</span></span>

<span data-ttu-id="d5793-109">Použití dvojčata zařízení na vysoké úrovni, umožňuje řešení back-endu zadejte požadovanou konfiguraci pro spravovaná zařízení, místo abyste odesílali určité příkazy.</span><span class="sxs-lookup"><span data-stu-id="d5793-109">At a high level, using device twins enables the solution back end to specify the desired configuration for the managed devices, instead of sending specific commands.</span></span> <span data-ttu-id="d5793-110">Toto zařízení starosti nastavení nejlepší způsob, jak aktualizaci konfigurace (velmi důležité pro scénáře IoT, kde určité zařízení podmínky vliv na schopnost okamžitě provádět určité příkazy), vloží při průběžně zprávy zpět řešení Ukončete aktuální stav a potenciální chybové stavy procesu aktualizace.</span><span class="sxs-lookup"><span data-stu-id="d5793-110">This puts the device in charge of setting up the best way to update its configuration (very important in IoT scenarios where specific device conditions affect the ability to immediately carry out specific commands), while continually reporting to the solution back end the current state and potential error conditions of the update process.</span></span> <span data-ttu-id="d5793-111">Tento vzor je instrumentálního na správu velkých sad zařízení, protože umožňuje back-end řešení tak, aby měl úplný přehled o stavu procesu konfigurace ve všech zařízeních.</span><span class="sxs-lookup"><span data-stu-id="d5793-111">This pattern is instrumental to the management of large sets of devices, as it enables the solution back end to have full visibility of the state of the configuration process across all devices.</span></span>

> [!NOTE]
> <span data-ttu-id="d5793-112">Ve scénářích, kde jsou ovládaná zařízení více interaktivní způsobem (zapněte ventilátor z aplikace řízené uživatele), zvažte použití [přímé metody][lnk-methods].</span><span class="sxs-lookup"><span data-stu-id="d5793-112">In scenarios where devices are controlled in a more interactive fashion (turn on a fan from a user-controlled app), consider using [direct methods][lnk-methods].</span></span>
> 
> 

<span data-ttu-id="d5793-113">V tomto kurzu back-end řešení změny konfigurace telemetrie cílové zařízení a v důsledku této, aplikace zařízení odpovídá několika krocích k instalaci aktualizace konfigurace (například nutnosti restartování modulu softwaru, které tento kurz simuluje s jednoduché zpožděním).</span><span class="sxs-lookup"><span data-stu-id="d5793-113">In this tutorial, the solution back end changes the telemetry configuration of a target device and, as a result of that, the device app follows a multi-step process to apply a configuration update (for example, requiring a software module restart, which this tutorial simulates with a simple delay).</span></span>

<span data-ttu-id="d5793-114">Back-end řešení ukládá konfiguraci v požadované vlastnosti dvojče zařízení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d5793-114">The solution back end stores the configuration in the device twin's desired properties in the following way:</span></span>

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [!NOTE]
> <span data-ttu-id="d5793-115">Vzhledem k tomu, že konfigurace může být složité objekty, jsou obvykle přiřazeny jedinečné ID (hodnoty hash nebo [identifikátory GUID][lnk-guid]) ke zjednodušení jejich porovnání.</span><span class="sxs-lookup"><span data-stu-id="d5793-115">Since configurations can be complex objects, they are usually assigned unique ids (hashes or [GUIDs][lnk-guid]) to simplify their comparisons.</span></span>
> 
> 

<span data-ttu-id="d5793-116">Aplikace zařízení sestavy své aktuální konfiguraci zrcadlení požadovanou vlastnost **telemetryConfig** ve vlastnostech hlášené:</span><span class="sxs-lookup"><span data-stu-id="d5793-116">The device app reports its current configuration mirroring the desired property **telemetryConfig** in the reported properties:</span></span>

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

<span data-ttu-id="d5793-117">Poznámka: jak hlášení **telemetryConfig** má další vlastnost **stav**používané nahlásit stav procesu aktualizace konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d5793-117">Note how the reported **telemetryConfig** has an additional property **status**, used to report the state of the configuration update process.</span></span>

<span data-ttu-id="d5793-118">Po přijetí nové požadované konfigurace aplikace zařízení sestavy čekající konfigurace změnou informace:</span><span class="sxs-lookup"><span data-stu-id="d5793-118">When a new desired configuration is received, the device app reports a pending configuration by changing the information:</span></span>

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

<span data-ttu-id="d5793-119">Potom později některé aplikace zařízení budou hlásit úspěch nebo selhání této operace aktualizací vlastnost výše.</span><span class="sxs-lookup"><span data-stu-id="d5793-119">Then, at some later time, the device app will report the success or failure of this operation by updating the above property.</span></span>
<span data-ttu-id="d5793-120">Všimněte si, jak je možné, back-end řešení kdykoli dotaz na stav procesu konfigurace ve všech zařízeních.</span><span class="sxs-lookup"><span data-stu-id="d5793-120">Note how the solution back end is able, at any time, to query the status of the configuration process across all the devices.</span></span>

<span data-ttu-id="d5793-121">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="d5793-121">This tutorial shows you how to:</span></span>

* <span data-ttu-id="d5793-122">Vytvoření aplikace simulovaného zařízení, která přijímá aktualizace konfigurace z back-end řešení a hlásí víc aktualizací jako *hlášené vlastnosti* na konfiguraci proces aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="d5793-122">Create a simulated device app that receives configuration updates from the solution back end, and reports multiple updates as *reported properties* on the configuration update process.</span></span>
* <span data-ttu-id="d5793-123">Vytvořte aplikaci back-end, která aktualizuje požadovanou konfiguraci zařízení a následně se dotazuje proces aktualizace konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d5793-123">Create a back-end app that updates the desired configuration of a device, and then queries the configuration update process.</span></span>

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier
