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
# <a name="advanced-autoscale-configuration-using-resource-manager-templates-for-vm-scale-sets"></a>Pokročilé škálování konfigurace pomocí šablony Resource Manageru pro škálovatelné sady virtuálních počítačů
Můžete v škálování a horizontální sady škálování virtuálního počítače na základě výkonu metriky prahových hodnot, podle plánu opakování, nebo podle konkrétní data. Můžete také nakonfigurovat e-mailu a webhooku oznámení pro akce škálování. Tento návod ukazuje příklad konfigurace všechny tyto objekty ve Škálovací sadě virtuálních počítačů pomocí šablony Resource Manageru.

> [!NOTE]
> Když tento postup vysvětluje hello kroky pro škálovatelné sady virtuálních počítačů, hello stejné informace se vztahují tooautoscaling [cloudové služby](https://azure.microsoft.com/services/cloud-services/), a [služby App Service – webové aplikace](https://azure.microsoft.com/services/app-service/web/).
> Jednoduché škálování vstupně -výstupnímu nastavení ve Škálovací sadě virtuálních počítačů podle metriky výkonu jednoduché například CPU, naleznete toohello [Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) a [Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) dokumenty
>
>

## <a name="walkthrough"></a>Názorný postup
V tomto návodu použijeme [Průzkumníka prostředků Azure](https://resources.azure.com/) tooconfigure a aktualizace nastavení automatického škálování hello sady škálování. Azure Průzkumníka prostředků se snadný způsob toomanage prostředků Azure pomocí šablony Resource Manageru. Pokud jste nový nástroj Průzkumník prostředků tooAzure, přečtěte si [tento ÚVOD](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).

1. Nasaďte nové škálování nastavit s nastavením základní škálování. Tento článek používá hello jeden z Galerie pro rychlý start Azure, který má Windows hello sady škálování pomocí šablony základní škálování. Linux škálování nastaví pracovní hello stejným způsobem.
2. Po vytvoření sady škálování hello přejděte toohello škálování sadu prostředků z Průzkumníka prostředků Azure. Zobrazí následující hello uzlu Microsoft.Insights.

    ![Průzkumník Azure](./media/insights-advanced-autoscale-vmss/azure_explorer_navigate.png)

    provádění šablony Hello vytvořil výchozí nastavení automatického škálování s názvem hello **'autoscalewad'**. Na pravé straně hello můžete zobrazit úplnou definice hello tohoto nastavení automatického škálování. V takovém případě nastavení automatického škálování výchozí hello součástí procesoru % na základě Škálováním na více systémů a škálování pravidlo.  

3. Nyní můžete přidat další profily a pravidla na základě plánu hello nebo konkrétní požadavky. Nastavení automatického škálování se nám vytvořit pomocí tří profilů. profily toounderstand a pravidla v škálování, zkontrolujte [škálování osvědčené postupy](insights-autoscale-best-practices.md).  

    | Profily & pravidla | Popis |
    |--- | --- |
    | **Profil** |**Na základě výkonu nebo metrika** |
    | Pravidlo |Počet zpráv fronty Service Bus > x |
    | Pravidlo |Počet zpráv fronty Service Bus < y |
    | Pravidlo |Využití procesoru % > n |
    | Pravidlo |% Využití procesoru < p |
    | **Profil** |**Den v týdnu hodin ráno (žádná pravidla)** |
    | **Profil** |**Den spuštění produktu (žádná pravidla)** |

4. Zde je hypotetický škálování scénář, který používáme pro tento návod.

    * **Zatížení na základě** -nastavit jako tooscale nebo v podle zatížení hello Moje aplikace hostované na Moje set.* škálování
    * **Velikost fronty zpráv** -je možné použít frontu sběrnice pro hello příchozí zprávy toomy aplikace. I použít počtu hello fronty zpráv a využití procesoru % a v případě, že buď počet zpráv nebo procesoru přístupů hello threshold.* nakonfigurujte profil tootrigger výchozí akce škálování
    * **Čas týden a den** – chci, aby byla týdenní opakovaného 'čas hello dne, na základě profil s názvem 'Hodiny ráno den v týdnu'. Na základě historických dat, lze zjistit, že je lepší toohave jisti, že počet virtuálních počítačů instancí toohandle zatížení Moje aplikace během této time.*
    * **Speciální datum** – po přidání profil produktu spusťte den. I připravte se na konkrétní kalendářní data tak, aby Moje aplikace připravené toohandle hello zatížení z důvodu marketing oznámení a když jsme umístí nového produktu v hello application.*
    * *poslední dva profily Hello může mít jiné metriky na základě pravidel výkonu v nich. V takovém případě rozhodli není toohave jeden a místo toho toorely na metrika výkonu hello výchozí na základě pravidla. Pravidla jsou volitelné pro profily hello opakování a na základě data.*

    Stanovení priorit škálování modul hello profily a pravidel, je také zachycené v hello [osvědčené postupy automatické škálování](insights-autoscale-best-practices.md) článku.
    Seznam běžných metriky pro škálování, najdete v části [běžné metriky pro škálování](insights-autoscale-common-metrics.md)

5. Zkontrolujte, zda jsou na hello **pro čtení a zápis** režimu v Průzkumníku prostředků

    ![Autoscalewad škálování výchozí nastavení](./media/insights-advanced-autoscale-vmss/autoscalewad.png)

6. Klikněte na tlačítko Upravit. **Nahraďte** element hello profily v nastavení automatického škálování s hello následující konfigurace:

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
    Podporované pole a jejich hodnoty, najdete v tématu [dokumentace k REST API škálování](https://msdn.microsoft.com/en-us/library/azure/dn931928.aspx). Vaše nastavení automatického škálování nyní obsahuje tři profily hello vysvětlené dřív.

7. Nakonec, podívejte se na hello škálování **oznámení** části. Oznámení o automatickém škálování umožňují toodo tři věci, pokud škálování nebo v akci je úspěšně spuštěné.
   - Oznámit Dobrý den, správce a spolusprávci předplatného
   - E-mailu sadu uživatelů
   - Aktivovat webhooku volání. Při vyvolání, tento webhook odešle metadata o podmínce hello automatické škálování a sady škálování hello prostředků. toolearn Další informace o hello datovou část webhooku škálování, najdete v části [konfigurace Webhooku & e-mailová oznámení pro škálování](insights-autoscale-to-webhook-email.md).

   Přidejte následující toohello škálování nastavení nahrazení hello vaše **oznámení** element, jehož hodnota je null.

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

   Stiskněte tlačítko **Put** tlačítko v nastavení automatického škálování hello tooupdate Průzkumníka prostředků.

Jste aktualizovali škálování nastavení na tooinclude sady škálování virtuálních počítačů více profilů škálování a škálovat oznámení.

## <a name="next-steps"></a>Další kroky
Použijte tyto odkazy toolearn Další informace o automatické škálování.

[Řešení potíží s škálování s sady škálování virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)

[Běžné metriky pro škálování](insights-autoscale-common-metrics.md)

[Osvědčené postupy pro Azure škálování](insights-autoscale-best-practices.md)

[Spravovat škálování pomocí prostředí PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)

[Spravovat škálování pomocí rozhraní příkazového řádku](insights-cli-samples.md#autoscale)

[Nakonfigurovat Webhooku & e-mailová oznámení pro škálování](insights-autoscale-to-webhook-email.md)
