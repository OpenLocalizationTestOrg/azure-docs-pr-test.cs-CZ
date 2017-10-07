---
title: "aaaEnable Azure Application Insights profileru na prostředku cloudové služby | Microsoft Docs"
description: "Zjistěte, jak tooset až hello profileru na aplikace ASP.NET hostované prostředek Azure Cloud Services."
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
ms.openlocfilehash: b9ac3bca513bf4518f44780389a9f2945f6ccc98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-application-insights-profiler-on-an-azure-cloud-services-resource"></a>Povolte Insights profileru aplikace na prostředek Azure Cloud Services

Tento návod ukazuje, jak tooenable Azure Application Insights profileru na aplikace ASP.NET hostované prostředek Azure Cloud Services. Hello příklady zahrnují podporu pro virtuální počítače Azure, Azure Service Fabric a sady škálování virtuálního počítače. Příklady Hello všechny spoléhají na šablony, které podporují hello modelu nasazení Azure Resource Manager. Další informace o modelu nasazení hello zkontrolujte [Azure Resource Manager oproti nasazení classic: pochopení modely nasazení a hello stav svých prostředků](/azure-resource-manager/resource-manager-deployment-model).

## <a name="overview"></a>Přehled

Hello následující diagram znázorňuje, jak funguje hello profileru pro prostředky Azure Cloud Services. Jako příklad používá virtuální počítač Azure.

![Přehled](./media/enable-profiler-compute/overview.png) hello toocollect informace pro zpracování a zobrazení na portálu Azure, je třeba nainstalovat součást hello Diagnostika agenta pro prostředky Azure Cloud Services hello. Hello zbytek hello návod obsahuje pokyny tooinstall a nakonfigurujte hello Diagnostika agenta tooenable profileru Application Insights.

## <a name="prerequisites-for-hello-walkthrough"></a>Předpoklady pro návod hello

* Nasazení šablony Resource Manageru, která nainstaluje hello profileru agenty hello virtuálních počítačů ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) nebo sady škálování ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)).

* Instance služby Application Insights pro profilace povoleno. Pokyny najdete v tématu [povolit hello profil](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).

* Rozhraní .NET framework 4.6.1 nebo později nainstalované na hello cíl prostředků Azure Cloud Services.

## <a name="create-a-resource-group-in-your-azure-subscription"></a>Vytvořte skupinu prostředků ve vašem předplatném Azure
Hello následující příklad ukazuje, jak toocreate prostředek skupiny pomocí skriptu prostředí PowerShell:

```
New-AzureRmResourceGroup -Name "Replace_With_Resource_Group_Name" -Location "Replace_With_Resource_Group_Location"
```

## <a name="create-an-application-insights-resource-in-hello-resource-group"></a>Vytvořte prostředek Application Insights ve skupině prostředků hello
Na hello **Application Insights** okno, zadejte informace hello prostředku, jak je popsáno v tomto příkladu: 

![Okno Application Insights](./media/enable-profiler-compute/createai.png)

## <a name="apply-an-application-insights-instrumentation-key-in-hello-azure-resource-manager-template"></a>Použít klíč instrumentace Application Insights do šablony Azure Resource Manageru hello

1. Pokud jste ještě hello šablony ještě stáhli, stažení z [Githubu](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json).

2. Najít klíč hello Application Insights.
   
   ![Umístění klíče hello](./media/enable-profiler-compute/copyaikey.png)

3. Nahraďte hodnotu šablony hello.
   
   ![Hodnota ve hello šablony](./media/enable-profiler-compute/copyaikeytotemplate.png)

## <a name="create-an-azure-vm-toohost-hello-web-application"></a>Vytvoření virtuálního počítače Azure toohost hello webové aplikace
1. Vytvořte heslo hello toosave zabezpečený řetězec.

   ```
   $password = ConvertTo-SecureString -String "Replace_With_Your_Password" -AsPlainText -Force
   ```

2. Nasazení šablony Azure Resource Manager hello.

   Změňte adresář hello v hello prostředí PowerShell konzoly toohello složku, která obsahuje vaše šablony Resource Manageru. Šablona hello toodeploy spustit hello následující příkaz:

   ```
   New-AzureRmResourceGroupDeployment -ResourceGroupName "Replace_With_Resource_Group_Name" -TemplateFile .\WindowsVirtualMachine.json -adminUsername "Replace_With_your_user_name" -adminPassword $password -dnsNameForPublicIP "Replace_WIth_your_DNS_Name" -Verbose
   ```

Po úspěšném spuštění skriptu hello byste měli najít virtuálního počítače s názvem **MyWindowsVM** ve vaší skupině prostředků.

## <a name="configure-web-deploy-on-hello-vm"></a>Konfigurace nasazení webu na hello virtuálních počítačů
Ujistěte se, že, nasazení webu je povolena na vašem virtuálním počítači, můžete publikovat webovou aplikaci ze sady Visual Studio.

tooinstall nasazení webu na virtuálním počítači ručně prostřednictvím WebPI, najdete v části [instalace a konfigurace nasazení webu na IIS 8.0 nebo později](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later). Pro příklad tooautomate instalace nasazení webu pomocí šablony Azure Resource Manager, najdete v části [vytvořit, nakonfigurovat a nasadit webové aplikace tooan virtuálního počítače Azure](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/).

Pokud nasazujete aplikaci ASP.NET MVC, přejděte tooServer správce, vyberte **přidat role a funkce** > **webového serveru (IIS)** > **Webový Server**  >  **Vývoj aplikací**a povolit technologii ASP.NET 4.5 na serveru.

![Přidat ASP.NET](./media/enable-profiler-compute/addaspnet45.png)

## <a name="install-hello-azure-application-insights-sdk-for-your-project"></a>Nainstalujte hello Azure Application Insights SDK pro svůj projekt
1. Otevřete vaši webovou aplikaci ASP.NET v sadě Visual Studio.

2. Klikněte pravým tlačítkem na projekt hello a vyberte **přidat** > **připojené služby**.

3. Vyberte **Application Insights**.

4. Na stránce hello postupujte podle pokynů hello. Vyberte prostředek Application Insights hello, kterou jste vytvořili dříve.

5. Vyberte hello **zaregistrovat** tlačítko.


## <a name="publish-hello-project-tooan-azure-vm"></a>Publikování projektu tooan hello virtuálního počítače Azure
Existuje několik způsobů toopublish aplikaci tooan virtuálního počítače Azure. Jedním ze způsobů je toouse Visual Studio 2017.

1. Klikněte pravým tlačítkem na projekt hello a vyberte **publikovat**.

2. Vyberte **Microsoft Azure Virtual Machines** jako hello publikování cíl a postupujte podle kroků hello.

   ![Publikování FromVS](./media/enable-profiler-compute/publishtoVM.png)

3. Spuštění zátěžového testu proti vaší aplikace. Měli byste vidět výsledky na webovou stránku hello Application Insights instanci portálu.


## <a name="enable-hello-profiler"></a>Povolit hello profileru
1. Přejděte tooyour Application Insights **výkonu** a vyberte **konfigurace**.
   
   ![Ikona konfigurace](./media/enable-profiler-compute/enableprofiler1.png)
 
2. Vyberte **povolit profileru**.
   
   ![Povolit ikonu profileru](./media/enable-profiler-compute/enableprofiler2.png)

## <a name="add-a-performance-test-tooyour-application"></a>Přidání aplikace tooyour testu výkonu
Takže shromažďujeme některé ukázkové toobe data zobrazí v profileru Application Insights, postupujte takto:

1. Vyhledejte prostředek Application Insights toohello, kterou jste vytvořili dříve. 

2. Přejděte toohello **dostupnosti** okno a přidejte test výkonu, který odešle adresa URL webové požadavky tooyour aplikace. 

   ![Přidat test výkonu](./media/enable-profiler-compute/AvailabilityTest.png)

## <a name="view-your-performance-data"></a>Zobrazit data o výkonu

1. Počkejte 10 až 15 minut pro hello profileru toocollect a analyzovat hello data. 

2. Přejděte toohello **výkonu** okno v prostředku Application Insights a zobrazení, jak vaše aplikace provádí při zatížení.

   ![Zobrazení výkonu](./media/enable-profiler-compute/aiperformance.png)

3. Ikona hello vyberte v části **příklady** tooopen hello **zobrazení trasování** okno.

   ![Otevírání okna zobrazení trasování hello](./media/enable-profiler-compute/traceview.png)


## <a name="work-with-an-existing-template"></a>Práce s existující šablony

1. Vyhledejte hello Azure Diagnostics prostředků deklaraci do šablony nasazení.
   
   Pokud nemáte deklaraci, můžete vytvořit jednu, která se podobá hello deklarace v hello následující ukázka. Můžete aktualizovat šablonu hello z hello [Průzkumníka prostředků Azure web](https://resources.azure.com).

2. Vydavatel změny hello z `Microsoft.Azure.Diagnostics` příliš`AIP.Diagnostics.Test`.

3. Pro `typeHandlerVersion`, použijte `0.0`.

4. Ujistěte se, že `autoUpgradeMinorVersion` je nastaven příliš`true`.

5. Přidat nové hello `ApplicationInsightsProfiler` podřízený instance v hello `WadCfg` objekt nastavení, jak je znázorněno v hello následující ukázka:

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
                      "ApplicationInsightsProfiler": "Enter hello Application Insights instance instrumentation key guid here"
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

## <a name="enable-hello-profiler-on-virtual-machine-scale-sets"></a>Povolit hello profileru na sady škálování virtuálního počítače
toosee jak tooenable hello profileru, stáhněte si hello [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) šablony. Použijte stejné změn v rozšíření prostředku virtuálního počítače šablony toohello diagnostiky pro škálovací sadu virtuálních počítačů hello hello.

Ujistěte se, že každá instance v sadě škálování hello má toohello přístup k Internetu. Hello profileru Agent potom může odeslat hello shromažďovaných vzorků tooApplication statistiky pro zobrazení a analýzu.

## <a name="enable-hello-profiler-on-service-fabric-applications"></a>Povolit hello profileru na aplikace Service Fabric
1. Zřízení hello Service Fabric clusteru toohave hello Azure Diagnostics rozšíření, která nainstaluje hello profileru agenta.

2. Nainstalujte hello Application Insights SDK projektu hello a nakonfigurujte hello Application Insights klíč.

3. Přidání telemetrických údajů tooinstrument kódu aplikace.

### <a name="provision-hello-service-fabric-cluster-toohave-hello-azure-diagnostics-extension-that-installs-hello-profiler-agent"></a>Zřízení hello Service Fabric clusteru toohave hello rozšíření diagnostiky Azure, který nainstaluje hello agenta profileru
Cluster Service Fabric může být zabezpečené nebo nezabezpečené. Můžete nastavit jeden toobe clusteru brány nezabezpečené, není třeba certifikát pro přístup. Clustery, které jsou hostiteli obchodní logiku a data by měla být zabezpečený. Můžete povolit hello profileru na zabezpečené i nezabezpečené clusterů Service Fabric. Tento návod používá nezabezpečené clusteru jako tooexplain příklad jaké změny jsou požadované tooenable hello profileru. Můžete zřídit zabezpečené cluster hello stejným způsobem.

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

2. Nasazení hello šablony pomocí skriptu prostředí PowerShell:

   ```
   Login-AzureRmAccount
   New-AzureRmResourceGroup -Name [Your_Resource_Group_Name] -Location [Your_Resource_Group_Location] -Verbose -Force
   New-AzureRmResourceGroupDeployment -Name [Choose_An_Arbitrary_Name] -ResourceGroupName [Your_Resource_Group_Name] -TemplateFile [Path_To_Your_Template]

   ```

### <a name="install-hello-application-insights-sdk-in-hello-project-and-configure-hello-application-insights-key"></a>Instalace hello Application Insights SDK projektu hello a konfigurace Application Insights klíč hello
Nainstalujte hello Application Insights SDK z hello [balíček NuGet](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/). Ujistěte se, že instalujete stabilní verze 2.3 nebo novější. 

Informace o konfigurování služby Application Insights ve vašich projektů najdete v tématu [pomocí Service Fabric pomocí služby Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).

### <a name="add-application-code-tooinstrument-telemetry"></a>Přidání telemetrických údajů tooinstrument kódu aplikace
1. Pro všechny část kódu, které chcete tooinstrument, přidat, pomocí příkazu kolem něj. 

   V následujícím příkladu hello, hello `RunAsync` metoda některé pracuje a hello `telemetryClient` třída zaznamená hello telemetrie po jeho spuštění. událost Hello musí napříč aplikace hello jedinečný název.

   ```
   protected override async Task RunAsync(CancellationToken cancellationToken)
       {
           // TODO: Replace hello following sample code with your own logic
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

2. Nasazení clusteru Service Fabric toohello vaší aplikace. Počkejte toorun aplikace hello 10 minut. Pro lepší platit může spuštění zátěžového testu v aplikaci hello. Portál Application Insights přejděte toohello **výkonu** okno a měli vidět příklady profilování trasování zobrazí.

<!---
Commenting out these sections for now
## Enable hello Profiler on Cloud Services applications
[TODO]
## Enable hello Profiler on classic Azure Virtual Machines
[TODO]
## Enable hello Profiler on on-premise servers
[TODO]
--->

## <a name="next-steps"></a>Další kroky

- Hledání pomoci při řešení problémů profileru problémy v [profileru řešení potíží s](app-insights-profiler.md#troubleshooting).

- Další informace o hello profileru v [Application Insights profileru](app-insights-profiler.md).
