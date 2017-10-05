---
title: "Povolit u prostředku cloudové služby Azure Application Insights profileru | Microsoft Docs"
description: "Zjistěte, jak nastavit profileru na aplikace ASP.NET hostované prostředek Azure Cloud Services."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: 5ff062ac81dca9d8b205cec966d2a9c11a4005b6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="enable-application-insights-profiler-on-an-azure-cloud-services-resource"></a>Povolte Insights profileru aplikace na prostředek Azure Cloud Services

Tento návod ukazuje, jak povolit Azure Application Insights profileru na aplikace ASP.NET hostované prostředek Azure Cloud Services. Příklady zahrnují podporu pro virtuální počítače Azure, Azure Service Fabric a sady škálování virtuálního počítače. Všechny příklady spoléhají na šablony, které podporují modelu nasazení Azure Resource Manager. Další informace o modelu nasazení, zkontrolujte [Azure Resource Manager oproti nasazení classic: pochopení modely nasazení a stav svých prostředků](/azure-resource-manager/resource-manager-deployment-model).

## <a name="overview"></a>Přehled

Následující diagram znázorňuje, jak funguje profileru pro prostředky Azure Cloud Services. Jako příklad používá virtuální počítač Azure.

![Přehled](./media/enable-profiler-compute/overview.png) ke shromažďování informací pro zpracování a zobrazení na portálu Azure, musíte nainstalovat součást agenta diagnostiky pro prostředky Azure Cloud Services. Zbytek průvodce obsahuje pokyny k instalaci a konfiguraci agenta diagnostiky povolit profileru Application Insights.

## <a name="prerequisites-for-the-walkthrough"></a>Předpoklady pro Průvodce

* Šablonu nasazení Resource Manager, který nainstaluje profileru agenty na virtuálních počítačích ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) nebo sady škálování ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)).

* Instance služby Application Insights pro profilace povoleno. Pokyny najdete v tématu [povolit profil](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).

* Rozhraní .NET framework 4.6.1 v cílovému prostředku Azure Cloud Services nebo vyšší.

## <a name="create-a-resource-group-in-your-azure-subscription"></a>Vytvořte skupinu prostředků ve vašem předplatném Azure
Následující příklad ukazuje, jak vytvořit skupinu prostředků s použitím skriptu prostředí PowerShell:

```
New-AzureRmResourceGroup -Name "Replace_With_Resource_Group_Name" -Location "Replace_With_Resource_Group_Location"
```

## <a name="create-an-application-insights-resource-in-the-resource-group"></a>Vytvořte prostředek Application Insights ve skupině prostředků
Na **Application Insights** okno, zadejte informace o vašem prostředku, jak je popsáno v tomto příkladu: 

![Okno Application Insights](./media/enable-profiler-compute/createai.png)

## <a name="apply-an-application-insights-instrumentation-key-in-the-azure-resource-manager-template"></a>Použít klíč instrumentace Application Insights do šablony Azure Resource Manageru

1. Pokud ještě nebyly stáhne šablonu, ji stáhnout z [Githubu](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json).

2. Vyhledání klíče Application Insights.
   
   ![Umístění klíče](./media/enable-profiler-compute/copyaikey.png)

3. Nahraďte hodnotu šablony.
   
   ![Hodnota nahradit v šabloně](./media/enable-profiler-compute/copyaikeytotemplate.png)

## <a name="create-an-azure-vm-to-host-the-web-application"></a>Vytvoření virtuálního počítače Azure k hostování webové aplikace
1. Vytvořte zabezpečený řetězec uložit heslo.

   ```
   $password = ConvertTo-SecureString -String "Replace_With_Your_Password" -AsPlainText -Force
   ```

2. Nasazení šablony Azure Resource Manager.

   Změňte adresář v konzole PowerShell do složky, která obsahuje vaše šablony Resource Manageru. Pokud chcete nasadit šablonu, spusťte následující příkaz:

   ```
   New-AzureRmResourceGroupDeployment -ResourceGroupName "Replace_With_Resource_Group_Name" -TemplateFile .\WindowsVirtualMachine.json -adminUsername "Replace_With_your_user_name" -adminPassword $password -dnsNameForPublicIP "Replace_WIth_your_DNS_Name" -Verbose
   ```

Po úspěšném spuštění skriptu, byste měli najít virtuálního počítače s názvem **MyWindowsVM** ve vaší skupině prostředků.

## <a name="configure-web-deploy-on-the-vm"></a>Konfigurovat Web nasadit ve virtuálním počítači
Ujistěte se, že, nasazení webu je povolena na vašem virtuálním počítači, můžete publikovat webovou aplikaci ze sady Visual Studio.

Na virtuálním počítači ručně prostřednictvím WebPI nainstalujte nástroj nasazení webu, najdete v tématu [instalace a konfigurace nasazení webu na IIS 8.0 nebo později](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later). Příklad toho, jak k automatizaci instalace nasazení webu pomocí šablony Azure Resource Manager, naleznete v části [vytvořit, nakonfigurovat a nasadit webovou aplikaci pro virtuální počítač Azure](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/).

Pokud nasazujete aplikaci ASP.NET MVC, přejděte na správce serveru, vyberte **přidat role a funkce** > **webového serveru (IIS)** > **Webový Server**  >  **Vývoj aplikací**a povolit technologii ASP.NET 4.5 na serveru.

![Přidat ASP.NET](./media/enable-profiler-compute/addaspnet45.png)

## <a name="install-the-azure-application-insights-sdk-for-your-project"></a>Nainstalovat Azure Application Insights SDK pro svůj projekt
1. Otevřete vaši webovou aplikaci ASP.NET v sadě Visual Studio.

2. Klikněte pravým tlačítkem na projekt a vyberte **přidat** > **připojené služby**.

3. Vyberte **Application Insights**.

4. Postupujte podle pokynů na stránce. Vyberte prostředek Application Insights, kterou jste vytvořili dříve.

5. Vyberte **zaregistrovat** tlačítko.


## <a name="publish-the-project-to-an-azure-vm"></a>Publikování tohoto projektu na virtuální počítač Azure
Existuje několik způsobů, jak publikovat aplikaci na virtuální počítač Azure. Jednou z možností je použití Visual Studio 2017.

1. Klikněte pravým tlačítkem na projekt a vyberte **publikovat**.

2. Vyberte **Microsoft Azure Virtual Machines** jako publikování cíle a postupujte podle pokynů.

   ![Publikování FromVS](./media/enable-profiler-compute/publishtoVM.png)

3. Spuštění zátěžového testu proti vaší aplikace. Měli byste vidět výsledky na webové stránce portálu pro Application Insights instance.


## <a name="enable-the-profiler"></a>Povolit profileru
1. Přejděte na Application Insights **výkonu** a vyberte **konfigurace**.
   
   ![Ikona konfigurace](./media/enable-profiler-compute/enableprofiler1.png)
 
2. Vyberte **povolit profileru**.
   
   ![Povolit ikonu profileru](./media/enable-profiler-compute/enableprofiler2.png)

## <a name="add-a-performance-test-to-your-application"></a>Přidat test výkonu do vaší aplikace
Takže shromažďujeme ukázková data v Application Insights profileru zobrazí, postupujte takto:

1. Přejděte do prostředku Application Insights, kterou jste vytvořili dříve. 

2. Přejděte na **dostupnosti** okno a přidat test výkonu, který odesílá webových požadavků na adresu URL aplikace. 

   ![Přidat test výkonu](./media/enable-profiler-compute/AvailabilityTest.png)

## <a name="view-your-performance-data"></a>Zobrazit data o výkonu

1. Počkejte 10 až 15 minut profileru shromažďovat a analyzovat data. 

2. Přejděte na **výkonu** okno v prostředku Application Insights a zobrazení, jak vaše aplikace provádí při zatížení.

   ![Zobrazení výkonu](./media/enable-profiler-compute/aiperformance.png)

3. Vyberte ikonu pod **příklady** otevřete **zobrazení trasování** okno.

   ![Otevírání okna zobrazení trasování](./media/enable-profiler-compute/traceview.png)


## <a name="work-with-an-existing-template"></a>Práce s existující šablony

1. Vyhledejte deklaraci prostředků Azure Diagnostics do šablony nasazení.
   
   Pokud nemáte deklaraci, můžete vytvořit jednu, která se podobá deklarace v následujícím příkladu. Můžete aktualizovat šablonu z [Průzkumníka prostředků Azure web](https://resources.azure.com).

2. Změnit vydavatele z `Microsoft.Azure.Diagnostics` k `AIP.Diagnostics.Test`.

3. Pro `typeHandlerVersion`, použijte `0.0`.

4. Ujistěte se, že `autoUpgradeMinorVersion` je nastaven na `true`.

5. Přidejte nové `ApplicationInsightsProfiler` podřízený instance v `WadCfg` objekt nastavení, jak je znázorněno v následujícím příkladu:

```
"resources": [
        {
          "type": "extensions",
          "name": "Microsoft.Insights.VMDiagnosticsSettings",
          "apiVersion": "2016-03-30",
          "properties": {
            "publisher": "AIP.Diagnostics.Test",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "0.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "WadCfg": {
                "SinksConfig": {
                  "Sink": [
                    {
                      "name": "Give a descriptive short name. E.g.: MyApplicationInsightsProfilerSink",
                      "ApplicationInsightsProfiler": "Enter the Application Insights instance instrumentation key guid here"
                    }
                  ]
                },
                "DiagnosticMonitorConfiguration": {
                    ...
                }
                ...
              }
              ...
            }
            ...
          }
          ...
]
```

## <a name="enable-the-profiler-on-virtual-machine-scale-sets"></a>Povolit profileru na sady škálování virtuálního počítače
Chcete-li zjistit, jak povolit profileru, stáhněte [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) šablony. Použijte stejné změny v šabloně virtuálního počítače k prostředku rozšíření diagnostiky pro škálovací sadu virtuálních počítačů.

Ujistěte se, že každá instance v sadě škálování má přístup k Internetu. Agent profileru potom může odeslat shromážděných ukázky Application insights pro zobrazení a analýzu.

## <a name="enable-the-profiler-on-service-fabric-applications"></a>Povolit profileru na aplikace Service Fabric
1. Zřídit cluster Service Fabric tak, aby měl rozšíření diagnostiky Azure, který nainstaluje agenta profileru.

2. Nainstalujte Application Insights SDK do projektu a nakonfigurujte klíč Application Insights.

3. Přidejte do nástrojích telemetrie kód aplikace.

### <a name="provision-the-service-fabric-cluster-to-have-the-azure-diagnostics-extension-that-installs-the-profiler-agent"></a>Zřídit cluster Service Fabric tak, aby měl rozšíření diagnostiky Azure, který nainstaluje agenta profileru
Cluster Service Fabric může být zabezpečené nebo nezabezpečené. Můžete nastavit jeden cluster brány, aby nezabezpečené nevyžaduje certifikát pro přístup. Clustery, které jsou hostiteli obchodní logiku a data by měla být zabezpečený. Můžete povolit profileru na zabezpečené i nezabezpečené clusterů Service Fabric. Tento návod používá nezabezpečené clusteru jako příklad vysvětlit, jaké změny jsou požadovány pro povolení profileru. Zabezpečení clusteru můžete zřídit stejným způsobem.

1. Stáhněte si [ServiceFabricCluster.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json). Stejně jako u virtuálních počítačů a sady škálování virtuálního počítače, nahraďte `Application_Insights_Key` váš klíč Application Insights:

   ```
   "publisher": "AIP.Diagnostics.Test",
                 "settings": {
                   "WadCfg": {
                     "SinksConfig": {
                       "Sink": [
                         {
                           "name": "MyApplicationInsightsProfilerSinkVMSS",
                           "ApplicationInsightsProfiler": "[Application_Insights_Key]"
                         }
                       ]
                     },
   ```

2. Nasazení šablony pomocí skriptu prostředí PowerShell:

   ```
   Login-AzureRmAccount
   New-AzureRmResourceGroup -Name [Your_Resource_Group_Name] -Location [Your_Resource_Group_Location] -Verbose -Force
   New-AzureRmResourceGroupDeployment -Name [Choose_An_Arbitrary_Name] -ResourceGroupName [Your_Resource_Group_Name] -TemplateFile [Path_To_Your_Template]

   ```

### <a name="install-the-application-insights-sdk-in-the-project-and-configure-the-application-insights-key"></a>Nainstalovat Application Insights SDK do projektu a nakonfigurovat klíče Application Insights
Nainstalujte Application Insights SDK z [balíček NuGet](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/). Ujistěte se, že instalujete stabilní verze 2.3 nebo novější. 

Informace o konfigurování služby Application Insights ve vašich projektů najdete v tématu [pomocí Service Fabric pomocí služby Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).

### <a name="add-application-code-to-instrument-telemetry"></a>Přidání kódu aplikace do nástrojích telemetrie
1. Pro jakékoliv kód, který chcete instrumentace, přidat, pomocí příkazu kolem něj. 

   V následujícím příkladu `RunAsync` metoda provádí některé práce a `telemetryClient` třída zaznamená telemetrii po jeho spuštění. Událost musí jedinečný název v aplikaci.

   ```
   protected override async Task RunAsync(CancellationToken cancellationToken)
       {
           // TODO: Replace the following sample code with your own logic
           //       or remove this RunAsync override if it's not needed in your service.

           while (true)
           {
               using( var operation = telemetryClient.StartOperation<RequestTelemetry>("[Insert_Event_Unique_Name]"))
               {
                   cancellationToken.ThrowIfCancellationRequested();

                   ++this.iterations;

                   ServiceEventSource.Current.ServiceMessage(this.Context, "Working-{0}", this.iterations);

                   await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
               }

           }
       }
   ```

2. Nasaďte aplikaci do clusteru Service Fabric. Počkejte, než pro aplikaci spustit po dobu 10 minut. Pro lepší platit může spuštění zátěžového testu v aplikaci. Přejděte na portálu služby Application Insights **výkonu** okno a měli vidět příklady profilování trasování zobrazí.

<!---
Commenting out these sections for now
## Enable the Profiler on Cloud Services applications
[TODO]
## Enable the Profiler on classic Azure Virtual Machines
[TODO]
## Enable the Profiler on on-premise servers
[TODO]
--->

## <a name="next-steps"></a>Další kroky

- Hledání pomoci při řešení problémů profileru problémy v [profileru řešení potíží s](app-insights-profiler.md#troubleshooting).

- Další informace o profileru v [Application Insights profileru](app-insights-profiler.md).
