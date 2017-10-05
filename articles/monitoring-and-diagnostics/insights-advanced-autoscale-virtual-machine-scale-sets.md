---
title: "Rozšířené škálování pomocí Azure Virtual Machines | Microsoft Docs"
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
ms.openlocfilehash: 80955535c8d863cd3d8d1b77e2ab8bc016b6d9f3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="advanced-autoscale-configuration-using-resource-manager-templates-for-vm-scale-sets"></a><span data-ttu-id="1756a-103">Pokročilé škálování konfigurace pomocí šablony Resource Manageru pro škálovatelné sady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="1756a-103">Advanced autoscale configuration using Resource Manager templates for VM Scale Sets</span></span>
<span data-ttu-id="1756a-104">Můžete v škálování a horizontální sady škálování virtuálního počítače na základě výkonu metriky prahových hodnot, podle plánu opakování, nebo podle konkrétní data.</span><span class="sxs-lookup"><span data-stu-id="1756a-104">You can scale-in and scale-out in Virtual Machine Scale Sets based on performance metric thresholds, by a recurring schedule, or by a particular date.</span></span> <span data-ttu-id="1756a-105">Můžete také nakonfigurovat e-mailu a webhooku oznámení pro akce škálování.</span><span class="sxs-lookup"><span data-stu-id="1756a-105">You can also configure email and webhook notifications for scale actions.</span></span> <span data-ttu-id="1756a-106">Tento návod ukazuje příklad konfigurace všechny tyto objekty ve Škálovací sadě virtuálních počítačů pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="1756a-106">This walkthrough shows an example of configuring all these objects using a Resource Manager template on a VM Scale Set.</span></span>

> [!NOTE]
> <span data-ttu-id="1756a-107">Když tento postup vysvětluje kroky pro škálovatelné sady virtuálních počítačů, stejné informace platí pro automatické škálování [cloudové služby](https://azure.microsoft.com/services/cloud-services/), a [služby App Service – webové aplikace](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="1756a-107">While this walkthrough explains the steps for VM Scale Sets, the same information applies to autoscaling [Cloud Services](https://azure.microsoft.com/services/cloud-services/), and [App Service - Web Apps](https://azure.microsoft.com/services/app-service/web/).</span></span>
> <span data-ttu-id="1756a-108">Jednoduché škálování vstupně -výstupnímu nastavení ve Škálovací sadě virtuálních počítačů podle metriky výkonu jednoduché například CPU, naleznete na [Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) a [Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) dokumenty</span><span class="sxs-lookup"><span data-stu-id="1756a-108">For a simple scale in/out setting on a VM Scale Set based on a simple performance metric such as CPU, refer to the [Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) and [Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) documents</span></span>
>
>

## <a name="walkthrough"></a><span data-ttu-id="1756a-109">Názorný postup</span><span class="sxs-lookup"><span data-stu-id="1756a-109">Walkthrough</span></span>
<span data-ttu-id="1756a-110">V tomto návodu použijeme [Průzkumníka prostředků Azure](https://resources.azure.com/) ke konfiguraci a nastavení automatického škálování pro sadu škálování aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="1756a-110">In this walkthrough, we use [Azure Resource Explorer](https://resources.azure.com/) to configure and update the autoscale setting for a scale set.</span></span> <span data-ttu-id="1756a-111">Azure Průzkumníka prostředků je snadný způsob, jak spravovat prostředky Azure pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="1756a-111">Azure Resource Explorer is an easy way to manage Azure resources via Resource Manager templates.</span></span> <span data-ttu-id="1756a-112">Pokud jste nový nástroj Průzkumníka prostředků Azure, přečtěte si [tento ÚVOD](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).</span><span class="sxs-lookup"><span data-stu-id="1756a-112">If you are new to Azure Resource Explorer tool, read [this introduction](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).</span></span>

1. <span data-ttu-id="1756a-113">Nasaďte nové škálování nastavit s nastavením základní škálování.</span><span class="sxs-lookup"><span data-stu-id="1756a-113">Deploy a new scale set with a basic autoscale setting.</span></span> <span data-ttu-id="1756a-114">Tento článek používá jeden z Galerie pro rychlý start Azure, který má Windows pomocí šablony základní škálování sady škálování.</span><span class="sxs-lookup"><span data-stu-id="1756a-114">This article uses the one from the Azure QuickStart Gallery, which has a Windows scale set with a basic autoscale template.</span></span> <span data-ttu-id="1756a-115">Sady škálování Linux fungovat stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="1756a-115">Linux scale sets work the same way.</span></span>
2. <span data-ttu-id="1756a-116">Jakmile se vytvoří sada škálování, přejděte do prostředků sady škálování z Průzkumníka prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="1756a-116">After the scale set is created, navigate to the scale set resource from Azure Resource Explorer.</span></span> <span data-ttu-id="1756a-117">Zobrazí následující uzlu Microsoft.Insights.</span><span class="sxs-lookup"><span data-stu-id="1756a-117">You see the following under Microsoft.Insights node.</span></span>

    ![Průzkumník Azure](./media/insights-advanced-autoscale-vmss/azure_explorer_navigate.png)

    <span data-ttu-id="1756a-119">Provádění šablony vytvořil výchozí nastavení automatického škálování s názvem **'autoscalewad'**.</span><span class="sxs-lookup"><span data-stu-id="1756a-119">The template execution has created a default autoscale setting with the name **'autoscalewad'**.</span></span> <span data-ttu-id="1756a-120">Na pravé straně můžete zobrazit úplnou definici tohoto nastavení automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="1756a-120">On the right-hand side, you can view the full definition of this autoscale setting.</span></span> <span data-ttu-id="1756a-121">V takovém případě výchozí nastavení automatického škálování se dodává s pravidlo Škálováním na více systémů a škálování v % na základě využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="1756a-121">In this case, the default autoscale setting comes with a CPU% based scale-out and scale-in rule.</span></span>  

3. <span data-ttu-id="1756a-122">Nyní můžete přidat další profily a pravidla podle plánu nebo konkrétní požadavky.</span><span class="sxs-lookup"><span data-stu-id="1756a-122">You can now add more profiles and rules based on the schedule or specific requirements.</span></span> <span data-ttu-id="1756a-123">Nastavení automatického škálování se nám vytvořit pomocí tří profilů.</span><span class="sxs-lookup"><span data-stu-id="1756a-123">We create an autoscale setting with three profiles.</span></span> <span data-ttu-id="1756a-124">Abyste pochopili, profily a pravidla v škálování, zkontrolujte [škálování osvědčené postupy](insights-autoscale-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="1756a-124">To understand profiles and rules in autoscale, review [Autoscale Best Practices](insights-autoscale-best-practices.md).</span></span>  

    | <span data-ttu-id="1756a-125">Profily & pravidla</span><span class="sxs-lookup"><span data-stu-id="1756a-125">Profiles & Rules</span></span> | <span data-ttu-id="1756a-126">Popis</span><span class="sxs-lookup"><span data-stu-id="1756a-126">Description</span></span> |
    |--- | --- |
    | <span data-ttu-id="1756a-127">**Profil**</span><span class="sxs-lookup"><span data-stu-id="1756a-127">**Profile**</span></span> |<span data-ttu-id="1756a-128">**Na základě výkonu nebo metrika**</span><span class="sxs-lookup"><span data-stu-id="1756a-128">**Performance/metric based**</span></span> |
    | <span data-ttu-id="1756a-129">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="1756a-129">Rule</span></span> |<span data-ttu-id="1756a-130">Počet zpráv fronty Service Bus > x</span><span class="sxs-lookup"><span data-stu-id="1756a-130">Service Bus Queue Message Count > x</span></span> |
    | <span data-ttu-id="1756a-131">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="1756a-131">Rule</span></span> |<span data-ttu-id="1756a-132">Počet zpráv fronty Service Bus < y</span><span class="sxs-lookup"><span data-stu-id="1756a-132">Service Bus Queue Message Count < y</span></span> |
    | <span data-ttu-id="1756a-133">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="1756a-133">Rule</span></span> |<span data-ttu-id="1756a-134">Využití procesoru % > n</span><span class="sxs-lookup"><span data-stu-id="1756a-134">CPU% > n</span></span> |
    | <span data-ttu-id="1756a-135">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="1756a-135">Rule</span></span> |<span data-ttu-id="1756a-136">% Využití procesoru < p</span><span class="sxs-lookup"><span data-stu-id="1756a-136">CPU% < p</span></span> |
    | <span data-ttu-id="1756a-137">**Profil**</span><span class="sxs-lookup"><span data-stu-id="1756a-137">**Profile**</span></span> |<span data-ttu-id="1756a-138">**Den v týdnu hodin ráno (žádná pravidla)**</span><span class="sxs-lookup"><span data-stu-id="1756a-138">**Weekday morning hours (no rules)**</span></span> |
    | <span data-ttu-id="1756a-139">**Profil**</span><span class="sxs-lookup"><span data-stu-id="1756a-139">**Profile**</span></span> |<span data-ttu-id="1756a-140">**Den spuštění produktu (žádná pravidla)**</span><span class="sxs-lookup"><span data-stu-id="1756a-140">**Product Launch day (no rules)**</span></span> |

4. <span data-ttu-id="1756a-141">Zde je hypotetický škálování scénář, který používáme pro tento návod.</span><span class="sxs-lookup"><span data-stu-id="1756a-141">Here is a hypothetical scaling scenario that we use for this walk-through.</span></span>

    * <span data-ttu-id="1756a-142">**Zatížení na základě** -nastavit jako škálování nebo v závislosti na zatížení na Moje aplikace hostované na Moje set.* škálování</span><span class="sxs-lookup"><span data-stu-id="1756a-142">**Load based** - I'd like to scale out or in based on the load on my application hosted on my scale set.*</span></span>
    * <span data-ttu-id="1756a-143">**Velikost fronty zpráv** -použít frontu sběrnice pro příchozí zprávy pro Moje aplikace.</span><span class="sxs-lookup"><span data-stu-id="1756a-143">**Message Queue size** - I use a Service Bus Queue for the incoming messages to my application.</span></span> <span data-ttu-id="1756a-144">I pomocí fronty počtu zpráv a % využití procesoru a nakonfigurovat výchozí profil spustíte akci škálování, pokud některý z počtu zpráv nebo procesor přístupy threshold.*</span><span class="sxs-lookup"><span data-stu-id="1756a-144">I use the queue's message count and CPU% and configure a default profile to trigger a scale action if either of message count or CPU hits the threshold.*</span></span>
    * <span data-ttu-id="1756a-145">**Čas týden a den** – chci, aby týdenní opakovaného 'čas dne, na základě profilu volat 'Hodiny ráno den v týdnu'.</span><span class="sxs-lookup"><span data-stu-id="1756a-145">**Time of week and day** - I want a weekly recurring 'time of the day' based profile called 'Weekday Morning Hours'.</span></span> <span data-ttu-id="1756a-146">Na základě historických dat, lze zjistit, že je lepší určitého počtu instancí virtuálních počítačů pro zpracování během této time.* zatížení Moje aplikace</span><span class="sxs-lookup"><span data-stu-id="1756a-146">Based on historical data, I know it is better to have certain number of VM instances to handle my application's load during this time.*</span></span>
    * <span data-ttu-id="1756a-147">**Speciální datum** – po přidání profil produktu spusťte den.</span><span class="sxs-lookup"><span data-stu-id="1756a-147">**Special Dates** - I added a 'Product Launch Day' profile.</span></span> <span data-ttu-id="1756a-148">I připravte se na konkrétní kalendářní data tak, aby Moje aplikace připravené pro zpracování zátěže z důvodu marketing oznámení a application.* jsme chápat nového produktu</span><span class="sxs-lookup"><span data-stu-id="1756a-148">I plan ahead for specific dates so my application is ready to handle the load due marketing announcements and when we put a new product in the application.*</span></span>
    * <span data-ttu-id="1756a-149">*Poslední dva profily může mít jiné metriky na základě pravidel výkonu v nich. V takovém případě rozhodla, že nemají jeden a místo toho spoléhají na výchozí metriku výkonu na základě pravidla. Pravidla jsou volitelné pro profily opakování a na základě data.*</span><span class="sxs-lookup"><span data-stu-id="1756a-149">*The last two profiles can also have other performance metric based rules within them. In this case, I decided not to have one and instead to rely on the default performance metric based rules. Rules are optional for the recurring and date-based profiles.*</span></span>

    <span data-ttu-id="1756a-150">Stanovení priorit škálování modul profily a pravidel, je také zachycené v [osvědčené postupy automatické škálování](insights-autoscale-best-practices.md) článku.</span><span class="sxs-lookup"><span data-stu-id="1756a-150">Autoscale engine's prioritization of the profiles and rules is also captured in the [autoscaling best practices](insights-autoscale-best-practices.md) article.</span></span>
    <span data-ttu-id="1756a-151">Seznam běžných metriky pro škálování, najdete v části [běžné metriky pro škálování](insights-autoscale-common-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="1756a-151">For a list of common metrics for autoscale, refer [Common metrics for Autoscale](insights-autoscale-common-metrics.md)</span></span>

5. <span data-ttu-id="1756a-152">Ujistěte se, že jste na **pro čtení a zápis** režimu v Průzkumníku prostředků</span><span class="sxs-lookup"><span data-stu-id="1756a-152">Make sure you are on the **Read/Write** mode in Resource Explorer</span></span>

    ![Autoscalewad škálování výchozí nastavení](./media/insights-advanced-autoscale-vmss/autoscalewad.png)

6. <span data-ttu-id="1756a-154">Klikněte na tlačítko Upravit.</span><span class="sxs-lookup"><span data-stu-id="1756a-154">Click Edit.</span></span> <span data-ttu-id="1756a-155">**Nahraďte** 'profily' element v nastavení automatického škálování s následující konfigurací:</span><span class="sxs-lookup"><span data-stu-id="1756a-155">**Replace** the 'profiles' element in autoscale setting with the following configuration:</span></span>

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
    <span data-ttu-id="1756a-157">Podporované pole a jejich hodnoty, najdete v tématu [dokumentace k REST API škálování](https://msdn.microsoft.com/en-us/library/azure/dn931928.aspx).</span><span class="sxs-lookup"><span data-stu-id="1756a-157">For supported fields and their values, see [Autoscale REST API documentation](https://msdn.microsoft.com/en-us/library/azure/dn931928.aspx).</span></span> <span data-ttu-id="1756a-158">Vaše nastavení automatického škálování nyní obsahuje tři profily vysvětlené dřív.</span><span class="sxs-lookup"><span data-stu-id="1756a-158">Now your autoscale setting contains the three profiles explained previously.</span></span>

7. <span data-ttu-id="1756a-159">Nakonec, podívejte se na škálování **oznámení** části.</span><span class="sxs-lookup"><span data-stu-id="1756a-159">Finally, look at the Autoscale **notification** section.</span></span> <span data-ttu-id="1756a-160">Oznámení o automatickém škálování umožňují provádějí tři věci, pokud škálování nebo v akci je úspěšně spuštěné.</span><span class="sxs-lookup"><span data-stu-id="1756a-160">Autoscale notifications allow you to do three things when a scale-out or in action is successfully triggered.</span></span>
   - <span data-ttu-id="1756a-161">Oznámit správce a spolusprávci předplatného</span><span class="sxs-lookup"><span data-stu-id="1756a-161">Notify the admin and co-admins of your subscription</span></span>
   - <span data-ttu-id="1756a-162">E-mailu sadu uživatelů</span><span class="sxs-lookup"><span data-stu-id="1756a-162">Email a set of users</span></span>
   - <span data-ttu-id="1756a-163">Aktivovat webhooku volání.</span><span class="sxs-lookup"><span data-stu-id="1756a-163">Trigger a webhook call.</span></span> <span data-ttu-id="1756a-164">Při vyvolání, odešle tento webhook metadata o automatické škálování podmínku a měřítka nastavení prostředku.</span><span class="sxs-lookup"><span data-stu-id="1756a-164">When fired, this webhook sends metadata about the autoscaling condition and the scale set resource.</span></span> <span data-ttu-id="1756a-165">Další informace o datové části webhooku škálování najdete v tématu [konfigurace Webhooku & e-mailová oznámení pro škálování](insights-autoscale-to-webhook-email.md).</span><span class="sxs-lookup"><span data-stu-id="1756a-165">To learn more about the payload of autoscale webhook, see [Configure Webhook & Email Notifications for Autoscale](insights-autoscale-to-webhook-email.md).</span></span>

   <span data-ttu-id="1756a-166">Přidejte následující nahrazení nastavení automatického škálování vašeho **oznámení** element, jehož hodnota je null.</span><span class="sxs-lookup"><span data-stu-id="1756a-166">Add the following to the Autoscale setting replacing your **notification** element whose value is null</span></span>

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

   <span data-ttu-id="1756a-167">Stiskněte tlačítko **Put** tlačítko v Průzkumníku prostředků se aktualizovat nastavení automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="1756a-167">Hit **Put** button in Resource Explorer to update the autoscale setting.</span></span>

<span data-ttu-id="1756a-168">Jste aktualizovali nastavení automatického škálování na škálování virtuálního počítače nastavit zahrnují několik profilů škálování a škálovat oznámení.</span><span class="sxs-lookup"><span data-stu-id="1756a-168">You have updated an autoscale setting on a VM Scale set to include multiple scale profiles and scale notifications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1756a-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1756a-169">Next Steps</span></span>
<span data-ttu-id="1756a-170">Pomocí těchto odkazů můžete získat další informace o automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="1756a-170">Use these links to learn more about autoscaling.</span></span>

[<span data-ttu-id="1756a-171">Řešení potíží s škálování s sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1756a-171">TroubleShoot Autoscale with Virtual Machine Scale Sets</span></span>](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)

[<span data-ttu-id="1756a-172">Běžné metriky pro škálování</span><span class="sxs-lookup"><span data-stu-id="1756a-172">Common Metrics for Autoscale</span></span>](insights-autoscale-common-metrics.md)

[<span data-ttu-id="1756a-173">Osvědčené postupy pro Azure škálování</span><span class="sxs-lookup"><span data-stu-id="1756a-173">Best Practices for Azure Autoscale</span></span>](insights-autoscale-best-practices.md)

[<span data-ttu-id="1756a-174">Spravovat škálování pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1756a-174">Manage Autoscale using PowerShell</span></span>](insights-powershell-samples.md#create-and-manage-autoscale-settings)

[<span data-ttu-id="1756a-175">Spravovat škálování pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="1756a-175">Manage Autoscale using CLI</span></span>](insights-cli-samples.md#autoscale)

[<span data-ttu-id="1756a-176">Nakonfigurovat Webhooku & e-mailová oznámení pro škálování</span><span class="sxs-lookup"><span data-stu-id="1756a-176">Configure Webhook & Email Notifications for Autoscale</span></span>](insights-autoscale-to-webhook-email.md)
