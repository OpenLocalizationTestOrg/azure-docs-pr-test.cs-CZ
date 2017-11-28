---
title: "aaaAdvanced škálování pomocí Azure Virtual Machines | Microsoft Docs"
description: "Používá správce prostředků a škálovatelné sady virtuálních počítačů s více pravidel a profily, které odeslání e-mailu a volání adresy URL webhooku s akcí škálování."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 7e3576e2-4a2b-4736-b5ae-98c4689cdd2b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2016
ms.author: ancav
ms.openlocfilehash: ecb01e3094f715837b75ef07a7dbecdf0f2e78f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-autoscale-configuration-using-resource-manager-templates-for-vm-scale-sets"></a><span data-ttu-id="ffede-103">Pokročilé škálování konfigurace pomocí šablony Resource Manageru pro škálovatelné sady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="ffede-103">Advanced autoscale configuration using Resource Manager templates for VM Scale Sets</span></span>
<span data-ttu-id="ffede-104">Můžete v škálování a horizontální sady škálování virtuálního počítače na základě výkonu metriky prahových hodnot, podle plánu opakování, nebo podle konkrétní data.</span><span class="sxs-lookup"><span data-stu-id="ffede-104">You can scale-in and scale-out in Virtual Machine Scale Sets based on performance metric thresholds, by a recurring schedule, or by a particular date.</span></span> <span data-ttu-id="ffede-105">Můžete také nakonfigurovat e-mailu a webhooku oznámení pro akce škálování.</span><span class="sxs-lookup"><span data-stu-id="ffede-105">You can also configure email and webhook notifications for scale actions.</span></span> <span data-ttu-id="ffede-106">Tento návod ukazuje příklad konfigurace všechny tyto objekty ve Škálovací sadě virtuálních počítačů pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="ffede-106">This walkthrough shows an example of configuring all these objects using a Resource Manager template on a VM Scale Set.</span></span>

> [!NOTE]
> <span data-ttu-id="ffede-107">Když tento postup vysvětluje hello kroky pro škálovatelné sady virtuálních počítačů, hello stejné informace se vztahují tooautoscaling [cloudové služby](https://azure.microsoft.com/services/cloud-services/), a [služby App Service – webové aplikace](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="ffede-107">While this walkthrough explains hello steps for VM Scale Sets, hello same information applies tooautoscaling [Cloud Services](https://azure.microsoft.com/services/cloud-services/), and [App Service - Web Apps](https://azure.microsoft.com/services/app-service/web/).</span></span>
> <span data-ttu-id="ffede-108">Jednoduché škálování vstupně -výstupnímu nastavení ve Škálovací sadě virtuálních počítačů podle metriky výkonu jednoduché například CPU, naleznete toohello [Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) a [Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) dokumenty</span><span class="sxs-lookup"><span data-stu-id="ffede-108">For a simple scale in/out setting on a VM Scale Set based on a simple performance metric such as CPU, refer toohello [Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) and [Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) documents</span></span>
>
>

## <a name="walkthrough"></a><span data-ttu-id="ffede-109">Názorný postup</span><span class="sxs-lookup"><span data-stu-id="ffede-109">Walkthrough</span></span>
<span data-ttu-id="ffede-110">V tomto návodu použijeme [Průzkumníka prostředků Azure](https://resources.azure.com/) tooconfigure a aktualizace nastavení automatického škálování hello sady škálování.</span><span class="sxs-lookup"><span data-stu-id="ffede-110">In this walkthrough, we use [Azure Resource Explorer](https://resources.azure.com/) tooconfigure and update hello autoscale setting for a scale set.</span></span> <span data-ttu-id="ffede-111">Azure Průzkumníka prostředků se snadný způsob toomanage prostředků Azure pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="ffede-111">Azure Resource Explorer is an easy way toomanage Azure resources via Resource Manager templates.</span></span> <span data-ttu-id="ffede-112">Pokud jste nový nástroj Průzkumník prostředků tooAzure, přečtěte si [tento ÚVOD](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).</span><span class="sxs-lookup"><span data-stu-id="ffede-112">If you are new tooAzure Resource Explorer tool, read [this introduction](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).</span></span>

1. <span data-ttu-id="ffede-113">Nasaďte nové škálování nastavit s nastavením základní škálování.</span><span class="sxs-lookup"><span data-stu-id="ffede-113">Deploy a new scale set with a basic autoscale setting.</span></span> <span data-ttu-id="ffede-114">Tento článek používá hello jeden z Galerie pro rychlý start Azure, který má Windows hello sady škálování pomocí šablony základní škálování.</span><span class="sxs-lookup"><span data-stu-id="ffede-114">This article uses hello one from hello Azure QuickStart Gallery, which has a Windows scale set with a basic autoscale template.</span></span> <span data-ttu-id="ffede-115">Linux škálování nastaví pracovní hello stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="ffede-115">Linux scale sets work hello same way.</span></span>
2. <span data-ttu-id="ffede-116">Po vytvoření sady škálování hello přejděte toohello škálování sadu prostředků z Průzkumníka prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="ffede-116">After hello scale set is created, navigate toohello scale set resource from Azure Resource Explorer.</span></span> <span data-ttu-id="ffede-117">Zobrazí následující hello uzlu Microsoft.Insights.</span><span class="sxs-lookup"><span data-stu-id="ffede-117">You see hello following under Microsoft.Insights node.</span></span>

    ![Průzkumník Azure](./media/insights-advanced-autoscale-vmss/azure_explorer_navigate.png)

    <span data-ttu-id="ffede-119">provádění šablony Hello vytvořil výchozí nastavení automatického škálování s názvem hello **'autoscalewad'**.</span><span class="sxs-lookup"><span data-stu-id="ffede-119">hello template execution has created a default autoscale setting with hello name **'autoscalewad'**.</span></span> <span data-ttu-id="ffede-120">Na pravé straně hello můžete zobrazit úplnou definice hello tohoto nastavení automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="ffede-120">On hello right-hand side, you can view hello full definition of this autoscale setting.</span></span> <span data-ttu-id="ffede-121">V takovém případě nastavení automatického škálování výchozí hello součástí procesoru % na základě Škálováním na více systémů a škálování pravidlo.</span><span class="sxs-lookup"><span data-stu-id="ffede-121">In this case, hello default autoscale setting comes with a CPU% based scale-out and scale-in rule.</span></span>  

3. <span data-ttu-id="ffede-122">Nyní můžete přidat další profily a pravidla na základě plánu hello nebo konkrétní požadavky.</span><span class="sxs-lookup"><span data-stu-id="ffede-122">You can now add more profiles and rules based on hello schedule or specific requirements.</span></span> <span data-ttu-id="ffede-123">Nastavení automatického škálování se nám vytvořit pomocí tří profilů.</span><span class="sxs-lookup"><span data-stu-id="ffede-123">We create an autoscale setting with three profiles.</span></span> <span data-ttu-id="ffede-124">profily toounderstand a pravidla v škálování, zkontrolujte [škálování osvědčené postupy](insights-autoscale-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="ffede-124">toounderstand profiles and rules in autoscale, review [Autoscale Best Practices](insights-autoscale-best-practices.md).</span></span>  

    | <span data-ttu-id="ffede-125">Profily & pravidla</span><span class="sxs-lookup"><span data-stu-id="ffede-125">Profiles & Rules</span></span> | <span data-ttu-id="ffede-126">Popis</span><span class="sxs-lookup"><span data-stu-id="ffede-126">Description</span></span> |
    |--- | --- |
    | <span data-ttu-id="ffede-127">**Profil**</span><span class="sxs-lookup"><span data-stu-id="ffede-127">**Profile**</span></span> |<span data-ttu-id="ffede-128">**Na základě výkonu nebo metrika**</span><span class="sxs-lookup"><span data-stu-id="ffede-128">**Performance/metric based**</span></span> |
    | <span data-ttu-id="ffede-129">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="ffede-129">Rule</span></span> |<span data-ttu-id="ffede-130">Počet zpráv fronty Service Bus > x</span><span class="sxs-lookup"><span data-stu-id="ffede-130">Service Bus Queue Message Count > x</span></span> |
    | <span data-ttu-id="ffede-131">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="ffede-131">Rule</span></span> |<span data-ttu-id="ffede-132">Počet zpráv fronty Service Bus < y</span><span class="sxs-lookup"><span data-stu-id="ffede-132">Service Bus Queue Message Count < y</span></span> |
    | <span data-ttu-id="ffede-133">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="ffede-133">Rule</span></span> |<span data-ttu-id="ffede-134">Využití procesoru % > n</span><span class="sxs-lookup"><span data-stu-id="ffede-134">CPU% > n</span></span> |
    | <span data-ttu-id="ffede-135">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="ffede-135">Rule</span></span> |<span data-ttu-id="ffede-136">% Využití procesoru < p</span><span class="sxs-lookup"><span data-stu-id="ffede-136">CPU% < p</span></span> |
    | <span data-ttu-id="ffede-137">**Profil**</span><span class="sxs-lookup"><span data-stu-id="ffede-137">**Profile**</span></span> |<span data-ttu-id="ffede-138">**Den v týdnu hodin ráno (žádná pravidla)**</span><span class="sxs-lookup"><span data-stu-id="ffede-138">**Weekday morning hours (no rules)**</span></span> |
    | <span data-ttu-id="ffede-139">**Profil**</span><span class="sxs-lookup"><span data-stu-id="ffede-139">**Profile**</span></span> |<span data-ttu-id="ffede-140">**Den spuštění produktu (žádná pravidla)**</span><span class="sxs-lookup"><span data-stu-id="ffede-140">**Product Launch day (no rules)**</span></span> |

4. <span data-ttu-id="ffede-141">Zde je hypotetický škálování scénář, který používáme pro tento návod.</span><span class="sxs-lookup"><span data-stu-id="ffede-141">Here is a hypothetical scaling scenario that we use for this walk-through.</span></span>

    * <span data-ttu-id="ffede-142">**Zatížení na základě** -nastavit jako tooscale nebo v podle zatížení hello Moje aplikace hostované na Moje set.* škálování</span><span class="sxs-lookup"><span data-stu-id="ffede-142">**Load based** - I'd like tooscale out or in based on hello load on my application hosted on my scale set.*</span></span>
    * <span data-ttu-id="ffede-143">**Velikost fronty zpráv** -je možné použít frontu sběrnice pro hello příchozí zprávy toomy aplikace.</span><span class="sxs-lookup"><span data-stu-id="ffede-143">**Message Queue size** - I use a Service Bus Queue for hello incoming messages toomy application.</span></span> <span data-ttu-id="ffede-144">I použít počtu hello fronty zpráv a využití procesoru % a v případě, že buď počet zpráv nebo procesoru přístupů hello threshold.* nakonfigurujte profil tootrigger výchozí akce škálování</span><span class="sxs-lookup"><span data-stu-id="ffede-144">I use hello queue's message count and CPU% and configure a default profile tootrigger a scale action if either of message count or CPU hits hello threshold.*</span></span>
    * <span data-ttu-id="ffede-145">**Čas týden a den** – chci, aby byla týdenní opakovaného 'čas hello dne, na základě profil s názvem 'Hodiny ráno den v týdnu'.</span><span class="sxs-lookup"><span data-stu-id="ffede-145">**Time of week and day** - I want a weekly recurring 'time of hello day' based profile called 'Weekday Morning Hours'.</span></span> <span data-ttu-id="ffede-146">Na základě historických dat, lze zjistit, že je lepší toohave jisti, že počet virtuálních počítačů instancí toohandle zatížení Moje aplikace během této time.*</span><span class="sxs-lookup"><span data-stu-id="ffede-146">Based on historical data, I know it is better toohave certain number of VM instances toohandle my application's load during this time.*</span></span>
    * <span data-ttu-id="ffede-147">**Speciální datum** – po přidání profil produktu spusťte den.</span><span class="sxs-lookup"><span data-stu-id="ffede-147">**Special Dates** - I added a 'Product Launch Day' profile.</span></span> <span data-ttu-id="ffede-148">I připravte se na konkrétní kalendářní data tak, aby Moje aplikace připravené toohandle hello zatížení z důvodu marketing oznámení a když jsme umístí nového produktu v hello application.*</span><span class="sxs-lookup"><span data-stu-id="ffede-148">I plan ahead for specific dates so my application is ready toohandle hello load due marketing announcements and when we put a new product in hello application.*</span></span>
    * <span data-ttu-id="ffede-149">*poslední dva profily Hello může mít jiné metriky na základě pravidel výkonu v nich. V takovém případě rozhodli není toohave jeden a místo toho toorely na metrika výkonu hello výchozí na základě pravidla. Pravidla jsou volitelné pro profily hello opakování a na základě data.*</span><span class="sxs-lookup"><span data-stu-id="ffede-149">*hello last two profiles can also have other performance metric based rules within them. In this case, I decided not toohave one and instead toorely on hello default performance metric based rules. Rules are optional for hello recurring and date-based profiles.*</span></span>

    <span data-ttu-id="ffede-150">Stanovení priorit škálování modul hello profily a pravidel, je také zachycené v hello [osvědčené postupy automatické škálování](insights-autoscale-best-practices.md) článku.</span><span class="sxs-lookup"><span data-stu-id="ffede-150">Autoscale engine's prioritization of hello profiles and rules is also captured in hello [autoscaling best practices](insights-autoscale-best-practices.md) article.</span></span>
    <span data-ttu-id="ffede-151">Seznam běžných metriky pro škálování, najdete v části [běžné metriky pro škálování](insights-autoscale-common-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="ffede-151">For a list of common metrics for autoscale, refer [Common metrics for Autoscale](insights-autoscale-common-metrics.md)</span></span>

5. <span data-ttu-id="ffede-152">Zkontrolujte, zda jsou na hello **pro čtení a zápis** režimu v Průzkumníku prostředků</span><span class="sxs-lookup"><span data-stu-id="ffede-152">Make sure you are on hello **Read/Write** mode in Resource Explorer</span></span>

    ![Autoscalewad škálování výchozí nastavení](./media/insights-advanced-autoscale-vmss/autoscalewad.png)

6. <span data-ttu-id="ffede-154">Klikněte na tlačítko Upravit.</span><span class="sxs-lookup"><span data-stu-id="ffede-154">Click Edit.</span></span> <span data-ttu-id="ffede-155">**Nahraďte** element hello profily v nastavení automatického škálování s hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="ffede-155">**Replace** hello 'profiles' element in autoscale setting with hello following configuration:</span></span>

    ![Profily](./media/insights-advanced-autoscale-vmss/profiles.png)

    ```
    {
            "name": "Perf_Based_Scale",
            "capacity": {
              "minimum": "2",
              "maximum": "12",
              "default": "2"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "MessageCount",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 10
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "MessageCount",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 3
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "Percentage CPU",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/<this_vmss_name>",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT30M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 85
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "Percentage CPU",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/<this_vmss_name>",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT30M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 60
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          },
          {
            "name": "Weekday_Morning_Hours_Scale",
            "capacity": {
              "minimum": "4",
              "maximum": "12",
              "default": "4"
            },
            "rules": [],
            "recurrence": {
              "frequency": "Week",
              "schedule": {
                "timeZone": "Pacific Standard Time",
                "days": [
                  "Monday",
                  "Tuesday",
                  "Wednesday",
                  "Thursday",
                  "Friday"
                ],
                "hours": [
                  6
                ],
                "minutes": [
                  0
                ]
              }
            }
          },
          {
            "name": "Product_Launch_Day",
            "capacity": {
              "minimum": "6",
              "maximum": "20",
              "default": "6"
            },
            "rules": [],
            "fixedDate": {
              "timeZone": "Pacific Standard Time",
              "start": "2016-06-20T00:06:00Z",
              "end": "2016-06-21T23:59:00Z"
            }
          }
    ```
    <span data-ttu-id="ffede-157">Podporované pole a jejich hodnoty, najdete v tématu [dokumentace k REST API škálování](https://msdn.microsoft.com/en-us/library/azure/dn931928.aspx).</span><span class="sxs-lookup"><span data-stu-id="ffede-157">For supported fields and their values, see [Autoscale REST API documentation](https://msdn.microsoft.com/en-us/library/azure/dn931928.aspx).</span></span> <span data-ttu-id="ffede-158">Vaše nastavení automatického škálování nyní obsahuje tři profily hello vysvětlené dřív.</span><span class="sxs-lookup"><span data-stu-id="ffede-158">Now your autoscale setting contains hello three profiles explained previously.</span></span>

7. <span data-ttu-id="ffede-159">Nakonec, podívejte se na hello škálování **oznámení** části.</span><span class="sxs-lookup"><span data-stu-id="ffede-159">Finally, look at hello Autoscale **notification** section.</span></span> <span data-ttu-id="ffede-160">Oznámení o automatickém škálování umožňují toodo tři věci, pokud škálování nebo v akci je úspěšně spuštěné.</span><span class="sxs-lookup"><span data-stu-id="ffede-160">Autoscale notifications allow you toodo three things when a scale-out or in action is successfully triggered.</span></span>
   - <span data-ttu-id="ffede-161">Oznámit Dobrý den, správce a spolusprávci předplatného</span><span class="sxs-lookup"><span data-stu-id="ffede-161">Notify hello admin and co-admins of your subscription</span></span>
   - <span data-ttu-id="ffede-162">E-mailu sadu uživatelů</span><span class="sxs-lookup"><span data-stu-id="ffede-162">Email a set of users</span></span>
   - <span data-ttu-id="ffede-163">Aktivovat webhooku volání.</span><span class="sxs-lookup"><span data-stu-id="ffede-163">Trigger a webhook call.</span></span> <span data-ttu-id="ffede-164">Při vyvolání, tento webhook odešle metadata o podmínce hello automatické škálování a sady škálování hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="ffede-164">When fired, this webhook sends metadata about hello autoscaling condition and hello scale set resource.</span></span> <span data-ttu-id="ffede-165">toolearn Další informace o hello datovou část webhooku škálování, najdete v části [konfigurace Webhooku & e-mailová oznámení pro škálování](insights-autoscale-to-webhook-email.md).</span><span class="sxs-lookup"><span data-stu-id="ffede-165">toolearn more about hello payload of autoscale webhook, see [Configure Webhook & Email Notifications for Autoscale](insights-autoscale-to-webhook-email.md).</span></span>

   <span data-ttu-id="ffede-166">Přidejte následující toohello škálování nastavení nahrazení hello vaše **oznámení** element, jehož hodnota je null.</span><span class="sxs-lookup"><span data-stu-id="ffede-166">Add hello following toohello Autoscale setting replacing your **notification** element whose value is null</span></span>

   ```
   "notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": true,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]

   ```

   <span data-ttu-id="ffede-167">Stiskněte tlačítko **Put** tlačítko v nastavení automatického škálování hello tooupdate Průzkumníka prostředků.</span><span class="sxs-lookup"><span data-stu-id="ffede-167">Hit **Put** button in Resource Explorer tooupdate hello autoscale setting.</span></span>

<span data-ttu-id="ffede-168">Jste aktualizovali škálování nastavení na tooinclude sady škálování virtuálních počítačů více profilů škálování a škálovat oznámení.</span><span class="sxs-lookup"><span data-stu-id="ffede-168">You have updated an autoscale setting on a VM Scale set tooinclude multiple scale profiles and scale notifications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ffede-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ffede-169">Next Steps</span></span>
<span data-ttu-id="ffede-170">Použijte tyto odkazy toolearn Další informace o automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="ffede-170">Use these links toolearn more about autoscaling.</span></span>

[<span data-ttu-id="ffede-171">Řešení potíží s škálování s sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="ffede-171">TroubleShoot Autoscale with Virtual Machine Scale Sets</span></span>](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)

[<span data-ttu-id="ffede-172">Běžné metriky pro škálování</span><span class="sxs-lookup"><span data-stu-id="ffede-172">Common Metrics for Autoscale</span></span>](insights-autoscale-common-metrics.md)

[<span data-ttu-id="ffede-173">Osvědčené postupy pro Azure škálování</span><span class="sxs-lookup"><span data-stu-id="ffede-173">Best Practices for Azure Autoscale</span></span>](insights-autoscale-best-practices.md)

[<span data-ttu-id="ffede-174">Spravovat škálování pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ffede-174">Manage Autoscale using PowerShell</span></span>](insights-powershell-samples.md#create-and-manage-autoscale-settings)

[<span data-ttu-id="ffede-175">Spravovat škálování pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="ffede-175">Manage Autoscale using CLI</span></span>](insights-cli-samples.md#autoscale)

[<span data-ttu-id="ffede-176">Nakonfigurovat Webhooku & e-mailová oznámení pro škálování</span><span class="sxs-lookup"><span data-stu-id="ffede-176">Configure Webhook & Email Notifications for Autoscale</span></span>](insights-autoscale-to-webhook-email.md)
