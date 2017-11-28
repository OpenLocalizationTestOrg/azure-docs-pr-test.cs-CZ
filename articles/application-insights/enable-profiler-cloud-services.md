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
# <a name="enable-application-insights-profiler-on-an-azure-cloud-services-resource"></a><span data-ttu-id="acbbc-103">Povolte Insights profileru aplikace na prostředek Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="acbbc-103">Enable Application Insights Profiler on an Azure Cloud Services resource</span></span>

<span data-ttu-id="acbbc-104">Tento návod ukazuje, jak tooenable Azure Application Insights profileru na aplikace ASP.NET hostované prostředek Azure Cloud Services.</span><span class="sxs-lookup"><span data-stu-id="acbbc-104">This walkthrough demonstrates how tooenable Azure Application Insights Profiler on an ASP.NET application hosted by an Azure Cloud Services resource.</span></span> <span data-ttu-id="acbbc-105">Hello příklady zahrnují podporu pro virtuální počítače Azure, Azure Service Fabric a sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="acbbc-105">hello examples include support for Azure Virtual Machines, virtual machine scale sets, and Azure Service Fabric.</span></span> <span data-ttu-id="acbbc-106">Příklady Hello všechny spoléhají na šablony, které podporují hello modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="acbbc-106">hello examples all rely on templates that support hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="acbbc-107">Další informace o modelu nasazení hello zkontrolujte [Azure Resource Manager oproti nasazení classic: pochopení modely nasazení a hello stav svých prostředků](/azure-resource-manager/resource-manager-deployment-model).</span><span class="sxs-lookup"><span data-stu-id="acbbc-107">For more information about hello deployment model, review [Azure Resource Manager vs. classic deployment: Understand deployment models and hello state of your resources](/azure-resource-manager/resource-manager-deployment-model).</span></span>

## <a name="overview"></a><span data-ttu-id="acbbc-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="acbbc-108">Overview</span></span>

<span data-ttu-id="acbbc-109">Hello následující diagram znázorňuje, jak funguje hello profileru pro prostředky Azure Cloud Services.</span><span class="sxs-lookup"><span data-stu-id="acbbc-109">hello following diagram illustrates how hello profiler works for Azure Cloud Services resources.</span></span> <span data-ttu-id="acbbc-110">Jako příklad používá virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="acbbc-110">It uses an Azure virtual machine as an example.</span></span>

<span data-ttu-id="acbbc-111">![Přehled](./media/enable-profiler-compute/overview.png) hello toocollect informace pro zpracování a zobrazení na portálu Azure, je třeba nainstalovat součást hello Diagnostika agenta pro prostředky Azure Cloud Services hello.</span><span class="sxs-lookup"><span data-stu-id="acbbc-111">![Overview](./media/enable-profiler-compute/overview.png) toocollect information for processing and display on hello Azure portal, you must install hello Diagnostics Agent component for hello Azure Cloud Services resources.</span></span> <span data-ttu-id="acbbc-112">Hello zbytek hello návod obsahuje pokyny tooinstall a nakonfigurujte hello Diagnostika agenta tooenable profileru Application Insights.</span><span class="sxs-lookup"><span data-stu-id="acbbc-112">hello rest of hello walkthrough provides guidance on how tooinstall and configure hello Diagnostics Agent tooenable Application Insights Profiler.</span></span>

## <a name="prerequisites-for-hello-walkthrough"></a><span data-ttu-id="acbbc-113">Předpoklady pro návod hello</span><span class="sxs-lookup"><span data-stu-id="acbbc-113">Prerequisites for hello walkthrough</span></span>

* <span data-ttu-id="acbbc-114">Nasazení šablony Resource Manageru, která nainstaluje hello profileru agenty hello virtuálních počítačů ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) nebo sady škálování ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)).</span><span class="sxs-lookup"><span data-stu-id="acbbc-114">A deployment Resource Manager template that installs hello profiler agents on hello VMs ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) or scale sets ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)).</span></span>

* <span data-ttu-id="acbbc-115">Instance služby Application Insights pro profilace povoleno.</span><span class="sxs-lookup"><span data-stu-id="acbbc-115">An Application Insights instance enabled for profiling.</span></span> <span data-ttu-id="acbbc-116">Pokyny najdete v tématu [povolit hello profil](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).</span><span class="sxs-lookup"><span data-stu-id="acbbc-116">For instructions, see [Enable hello profile](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).</span></span>

* <span data-ttu-id="acbbc-117">Rozhraní .NET framework 4.6.1 nebo později nainstalované na hello cíl prostředků Azure Cloud Services.</span><span class="sxs-lookup"><span data-stu-id="acbbc-117">.NET Framework 4.6.1 or later installed on hello target Azure Cloud Services resource.</span></span>

## <a name="create-a-resource-group-in-your-azure-subscription"></a><span data-ttu-id="acbbc-118">Vytvořte skupinu prostředků ve vašem předplatném Azure</span><span class="sxs-lookup"><span data-stu-id="acbbc-118">Create a resource group in your Azure subscription</span></span>
<span data-ttu-id="acbbc-119">Hello následující příklad ukazuje, jak toocreate prostředek skupiny pomocí skriptu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="acbbc-119">hello following example demonstrates how toocreate a resource group by using a PowerShell script:</span></span>

```
New-AzureRmResourceGroup -Name "Replace_With_Resource_Group_Name" -Location "Replace_With_Resource_Group_Location"
```

## <a name="create-an-application-insights-resource-in-hello-resource-group"></a><span data-ttu-id="acbbc-120">Vytvořte prostředek Application Insights ve skupině prostředků hello</span><span class="sxs-lookup"><span data-stu-id="acbbc-120">Create an Application Insights resource in hello resource group</span></span>
<span data-ttu-id="acbbc-121">Na hello **Application Insights** okno, zadejte informace hello prostředku, jak je popsáno v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="acbbc-121">On hello **Application Insights** blade, enter hello information for your resource, as shown in this example:</span></span> 

![Okno Application Insights](./media/enable-profiler-compute/createai.png)

## <a name="apply-an-application-insights-instrumentation-key-in-hello-azure-resource-manager-template"></a><span data-ttu-id="acbbc-123">Použít klíč instrumentace Application Insights do šablony Azure Resource Manageru hello</span><span class="sxs-lookup"><span data-stu-id="acbbc-123">Apply an Application Insights instrumentation key in hello Azure Resource Manager template</span></span>

1. <span data-ttu-id="acbbc-124">Pokud jste ještě hello šablony ještě stáhli, stažení z [Githubu](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json).</span><span class="sxs-lookup"><span data-stu-id="acbbc-124">If you haven't downloaded hello template yet, download it from [GitHub](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json).</span></span>

2. <span data-ttu-id="acbbc-125">Najít klíč hello Application Insights.</span><span class="sxs-lookup"><span data-stu-id="acbbc-125">Find hello Application Insights key.</span></span>
   
   ![Umístění klíče hello](./media/enable-profiler-compute/copyaikey.png)

3. <span data-ttu-id="acbbc-127">Nahraďte hodnotu šablony hello.</span><span class="sxs-lookup"><span data-stu-id="acbbc-127">Replace hello template value.</span></span>
   
   ![Hodnota ve hello šablony](./media/enable-profiler-compute/copyaikeytotemplate.png)

## <a name="create-an-azure-vm-toohost-hello-web-application"></a><span data-ttu-id="acbbc-129">Vytvoření virtuálního počítače Azure toohost hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="acbbc-129">Create an Azure VM toohost hello web application</span></span>
1. <span data-ttu-id="acbbc-130">Vytvořte heslo hello toosave zabezpečený řetězec.</span><span class="sxs-lookup"><span data-stu-id="acbbc-130">Create a secure string toosave hello password.</span></span>

   ```
   $password = ConvertTo-SecureString -String "Replace_With_Your_Password" -AsPlainText -Force
   ```

2. <span data-ttu-id="acbbc-131">Nasazení šablony Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="acbbc-131">Deploy hello Azure Resource Manager template.</span></span>

   <span data-ttu-id="acbbc-132">Změňte adresář hello v hello prostředí PowerShell konzoly toohello složku, která obsahuje vaše šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="acbbc-132">Change hello directory in hello PowerShell console toohello folder that contains your Resource Manager template.</span></span> <span data-ttu-id="acbbc-133">Šablona hello toodeploy spustit hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="acbbc-133">toodeploy hello template, run hello following command:</span></span>

   ```
   New-AzureRmResourceGroupDeployment -ResourceGroupName "Replace_With_Resource_Group_Name" -TemplateFile .\WindowsVirtualMachine.json -adminUsername "Replace_With_your_user_name" -adminPassword $password -dnsNameForPublicIP "Replace_WIth_your_DNS_Name" -Verbose
   ```

<span data-ttu-id="acbbc-134">Po úspěšném spuštění skriptu hello byste měli najít virtuálního počítače s názvem **MyWindowsVM** ve vaší skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="acbbc-134">After hello script runs successfully, you should find a VM named **MyWindowsVM** in your resource group.</span></span>

## <a name="configure-web-deploy-on-hello-vm"></a><span data-ttu-id="acbbc-135">Konfigurace nasazení webu na hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="acbbc-135">Configure Web Deploy on hello VM</span></span>
<span data-ttu-id="acbbc-136">Ujistěte se, že, nasazení webu je povolena na vašem virtuálním počítači, můžete publikovat webovou aplikaci ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="acbbc-136">Make sure that Web Deploy is enabled on your VM so you can publish your web application from Visual Studio.</span></span>

<span data-ttu-id="acbbc-137">tooinstall nasazení webu na virtuálním počítači ručně prostřednictvím WebPI, najdete v části [instalace a konfigurace nasazení webu na IIS 8.0 nebo později](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later).</span><span class="sxs-lookup"><span data-stu-id="acbbc-137">tooinstall Web Deploy on a VM manually via WebPI, see [Installing and Configuring Web Deploy on IIS 8.0 or Later](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later).</span></span> <span data-ttu-id="acbbc-138">Pro příklad tooautomate instalace nasazení webu pomocí šablony Azure Resource Manager, najdete v části [vytvořit, nakonfigurovat a nasadit webové aplikace tooan virtuálního počítače Azure](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/).</span><span class="sxs-lookup"><span data-stu-id="acbbc-138">For an example of how tooautomate installing Web Deploy by using an Azure Resource Manager template, see [Create, configure, and deploy a web application tooan Azure VM](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/).</span></span>

<span data-ttu-id="acbbc-139">Pokud nasazujete aplikaci ASP.NET MVC, přejděte tooServer správce, vyberte **přidat role a funkce** > **webového serveru (IIS)** > **Webový Server**  >  **Vývoj aplikací**a povolit technologii ASP.NET 4.5 na serveru.</span><span class="sxs-lookup"><span data-stu-id="acbbc-139">If you are deploying an ASP.NET MVC application, go tooServer Manager, select **Add Roles and Features** > **Web Server (IIS)** > **Web Server** > **Application Development**, and enable ASP.NET 4.5 on your server.</span></span>

![Přidat ASP.NET](./media/enable-profiler-compute/addaspnet45.png)

## <a name="install-hello-azure-application-insights-sdk-for-your-project"></a><span data-ttu-id="acbbc-141">Nainstalujte hello Azure Application Insights SDK pro svůj projekt</span><span class="sxs-lookup"><span data-stu-id="acbbc-141">Install hello Azure Application Insights SDK for your project</span></span>
1. <span data-ttu-id="acbbc-142">Otevřete vaši webovou aplikaci ASP.NET v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="acbbc-142">Open your ASP.NET web application in Visual Studio.</span></span>

2. <span data-ttu-id="acbbc-143">Klikněte pravým tlačítkem na projekt hello a vyberte **přidat** > **připojené služby**.</span><span class="sxs-lookup"><span data-stu-id="acbbc-143">Right-click hello project and select **Add** > **Connected Services**.</span></span>

3. <span data-ttu-id="acbbc-144">Vyberte **Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="acbbc-144">Select **Application Insights**.</span></span>

4. <span data-ttu-id="acbbc-145">Na stránce hello postupujte podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="acbbc-145">Follow hello instructions on hello page.</span></span> <span data-ttu-id="acbbc-146">Vyberte prostředek Application Insights hello, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="acbbc-146">Select hello Application Insights resource that you created earlier.</span></span>

5. <span data-ttu-id="acbbc-147">Vyberte hello **zaregistrovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="acbbc-147">Select hello **Register** button.</span></span>


## <a name="publish-hello-project-tooan-azure-vm"></a><span data-ttu-id="acbbc-148">Publikování projektu tooan hello virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="acbbc-148">Publish hello project tooan Azure VM</span></span>
<span data-ttu-id="acbbc-149">Existuje několik způsobů toopublish aplikaci tooan virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="acbbc-149">There are several ways toopublish an application tooan Azure VM.</span></span> <span data-ttu-id="acbbc-150">Jedním ze způsobů je toouse Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="acbbc-150">One way is toouse Visual Studio 2017.</span></span>

1. <span data-ttu-id="acbbc-151">Klikněte pravým tlačítkem na projekt hello a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="acbbc-151">Right-click hello project and select **Publish**.</span></span>

2. <span data-ttu-id="acbbc-152">Vyberte **Microsoft Azure Virtual Machines** jako hello publikování cíl a postupujte podle kroků hello.</span><span class="sxs-lookup"><span data-stu-id="acbbc-152">Select **Microsoft Azure Virtual Machines** as hello publish target and follow hello steps.</span></span>

   ![Publikování FromVS](./media/enable-profiler-compute/publishtoVM.png)

3. <span data-ttu-id="acbbc-154">Spuštění zátěžového testu proti vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="acbbc-154">Run a load test against your application.</span></span> <span data-ttu-id="acbbc-155">Měli byste vidět výsledky na webovou stránku hello Application Insights instanci portálu.</span><span class="sxs-lookup"><span data-stu-id="acbbc-155">You should see results on hello Application Insights instance portal webpage.</span></span>


## <a name="enable-hello-profiler"></a><span data-ttu-id="acbbc-156">Povolit hello profileru</span><span class="sxs-lookup"><span data-stu-id="acbbc-156">Enable hello profiler</span></span>
1. <span data-ttu-id="acbbc-157">Přejděte tooyour Application Insights **výkonu** a vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="acbbc-157">Go tooyour Application Insights **Performance** blade and select **Configure**.</span></span>
   
   ![Ikona konfigurace](./media/enable-profiler-compute/enableprofiler1.png)
 
2. <span data-ttu-id="acbbc-159">Vyberte **povolit profileru**.</span><span class="sxs-lookup"><span data-stu-id="acbbc-159">Select **Enable Profiler**.</span></span>
   
   ![Povolit ikonu profileru](./media/enable-profiler-compute/enableprofiler2.png)

## <a name="add-a-performance-test-tooyour-application"></a><span data-ttu-id="acbbc-161">Přidání aplikace tooyour testu výkonu</span><span class="sxs-lookup"><span data-stu-id="acbbc-161">Add a performance test tooyour application</span></span>
<span data-ttu-id="acbbc-162">Takže shromažďujeme některé ukázkové toobe data zobrazí v profileru Application Insights, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="acbbc-162">Follow these steps so we can collect some sample data toobe displayed in Application Insights Profiler:</span></span>

1. <span data-ttu-id="acbbc-163">Vyhledejte prostředek Application Insights toohello, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="acbbc-163">Browse toohello Application Insights resource that you created earlier.</span></span> 

2. <span data-ttu-id="acbbc-164">Přejděte toohello **dostupnosti** okno a přidejte test výkonu, který odešle adresa URL webové požadavky tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="acbbc-164">Go toohello **Availability** blade and add a performance test that sends web requests tooyour application URL.</span></span> 

   ![Přidat test výkonu](./media/enable-profiler-compute/AvailabilityTest.png)

## <a name="view-your-performance-data"></a><span data-ttu-id="acbbc-166">Zobrazit data o výkonu</span><span class="sxs-lookup"><span data-stu-id="acbbc-166">View your performance data</span></span>

1. <span data-ttu-id="acbbc-167">Počkejte 10 až 15 minut pro hello profileru toocollect a analyzovat hello data.</span><span class="sxs-lookup"><span data-stu-id="acbbc-167">Wait 10-15 minutes for hello profiler toocollect and analyze hello data.</span></span> 

2. <span data-ttu-id="acbbc-168">Přejděte toohello **výkonu** okno v prostředku Application Insights a zobrazení, jak vaše aplikace provádí při zatížení.</span><span class="sxs-lookup"><span data-stu-id="acbbc-168">Go toohello **Performance** blade in your Application Insights resource and view how your application is performing when it's under load.</span></span>

   ![Zobrazení výkonu](./media/enable-profiler-compute/aiperformance.png)

3. <span data-ttu-id="acbbc-170">Ikona hello vyberte v části **příklady** tooopen hello **zobrazení trasování** okno.</span><span class="sxs-lookup"><span data-stu-id="acbbc-170">Select hello icon under **Examples** tooopen hello **Trace View** blade.</span></span>

   ![Otevírání okna zobrazení trasování hello](./media/enable-profiler-compute/traceview.png)


## <a name="work-with-an-existing-template"></a><span data-ttu-id="acbbc-172">Práce s existující šablony</span><span class="sxs-lookup"><span data-stu-id="acbbc-172">Work with an existing template</span></span>

1. <span data-ttu-id="acbbc-173">Vyhledejte hello Azure Diagnostics prostředků deklaraci do šablony nasazení.</span><span class="sxs-lookup"><span data-stu-id="acbbc-173">Locate hello Azure Diagnostics resource declaration in your deployment template.</span></span>
   
   <span data-ttu-id="acbbc-174">Pokud nemáte deklaraci, můžete vytvořit jednu, která se podobá hello deklarace v hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="acbbc-174">If you don't have a declaration, you can create one that resembles hello declaration in hello following example.</span></span> <span data-ttu-id="acbbc-175">Můžete aktualizovat šablonu hello z hello [Průzkumníka prostředků Azure web](https://resources.azure.com).</span><span class="sxs-lookup"><span data-stu-id="acbbc-175">You can update hello template from hello [Azure Resource Explorer website](https://resources.azure.com).</span></span>

2. <span data-ttu-id="acbbc-176">Vydavatel změny hello z `Microsoft.Azure.Diagnostics` příliš`AIP.Diagnostics.Test`.</span><span class="sxs-lookup"><span data-stu-id="acbbc-176">Change hello publisher from `Microsoft.Azure.Diagnostics` too`AIP.Diagnostics.Test`.</span></span>

3. <span data-ttu-id="acbbc-177">Pro `typeHandlerVersion`, použijte `0.0`.</span><span class="sxs-lookup"><span data-stu-id="acbbc-177">For `typeHandlerVersion`, use `0.0`.</span></span>

4. <span data-ttu-id="acbbc-178">Ujistěte se, že `autoUpgradeMinorVersion` je nastaven příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="acbbc-178">Make sure that `autoUpgradeMinorVersion` is set too`true`.</span></span>

5. <span data-ttu-id="acbbc-179">Přidat nové hello `ApplicationInsightsProfiler` podřízený instance v hello `WadCfg` objekt nastavení, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="acbbc-179">Add hello new `ApplicationInsightsProfiler` sink instance in hello `WadCfg` settings object, as shown in hello following example:</span></span>

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

## <a name="enable-hello-profiler-on-virtual-machine-scale-sets"></a><span data-ttu-id="acbbc-180">Povolit hello profileru na sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="acbbc-180">Enable hello profiler on virtual machine scale sets</span></span>
<span data-ttu-id="acbbc-181">toosee jak tooenable hello profileru, stáhněte si hello [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) šablony.</span><span class="sxs-lookup"><span data-stu-id="acbbc-181">toosee how tooenable hello profiler, download hello [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) template.</span></span> <span data-ttu-id="acbbc-182">Použijte stejné změn v rozšíření prostředku virtuálního počítače šablony toohello diagnostiky pro škálovací sadu virtuálních počítačů hello hello.</span><span class="sxs-lookup"><span data-stu-id="acbbc-182">Apply hello same changes in a VM template toohello diagnostics extension resource for hello virtual machine scale set.</span></span>

<span data-ttu-id="acbbc-183">Ujistěte se, že každá instance v sadě škálování hello má toohello přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="acbbc-183">Make sure that each instance in hello scale set has access toohello internet.</span></span> <span data-ttu-id="acbbc-184">Hello profileru Agent potom může odeslat hello shromažďovaných vzorků tooApplication statistiky pro zobrazení a analýzu.</span><span class="sxs-lookup"><span data-stu-id="acbbc-184">hello Profiler Agent can then send hello collected samples tooApplication Insights for display and analysis.</span></span>

## <a name="enable-hello-profiler-on-service-fabric-applications"></a><span data-ttu-id="acbbc-185">Povolit hello profileru na aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="acbbc-185">Enable hello profiler on Service Fabric applications</span></span>
1. <span data-ttu-id="acbbc-186">Zřízení hello Service Fabric clusteru toohave hello Azure Diagnostics rozšíření, která nainstaluje hello profileru agenta.</span><span class="sxs-lookup"><span data-stu-id="acbbc-186">Provision hello Service Fabric cluster toohave hello Azure Diagnostics extension that installs hello Profiler Agent.</span></span>

2. <span data-ttu-id="acbbc-187">Nainstalujte hello Application Insights SDK projektu hello a nakonfigurujte hello Application Insights klíč.</span><span class="sxs-lookup"><span data-stu-id="acbbc-187">Install hello Application Insights SDK in hello project and configure hello Application Insights key.</span></span>

3. <span data-ttu-id="acbbc-188">Přidání telemetrických údajů tooinstrument kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="acbbc-188">Add application code tooinstrument telemetry.</span></span>

### <a name="provision-hello-service-fabric-cluster-toohave-hello-azure-diagnostics-extension-that-installs-hello-profiler-agent"></a><span data-ttu-id="acbbc-189">Zřízení hello Service Fabric clusteru toohave hello rozšíření diagnostiky Azure, který nainstaluje hello agenta profileru</span><span class="sxs-lookup"><span data-stu-id="acbbc-189">Provision hello Service Fabric cluster toohave hello Azure Diagnostics extension that installs hello Profiler Agent</span></span>
<span data-ttu-id="acbbc-190">Cluster Service Fabric může být zabezpečené nebo nezabezpečené.</span><span class="sxs-lookup"><span data-stu-id="acbbc-190">A Service Fabric cluster can be secure or non-secure.</span></span> <span data-ttu-id="acbbc-191">Můžete nastavit jeden toobe clusteru brány nezabezpečené, není třeba certifikát pro přístup.</span><span class="sxs-lookup"><span data-stu-id="acbbc-191">You can set one gateway cluster toobe non-secure so it doesn't require a certificate for access.</span></span> <span data-ttu-id="acbbc-192">Clustery, které jsou hostiteli obchodní logiku a data by měla být zabezpečený.</span><span class="sxs-lookup"><span data-stu-id="acbbc-192">Clusters that host business logic and data should be secure.</span></span> <span data-ttu-id="acbbc-193">Můžete povolit hello profileru na zabezpečené i nezabezpečené clusterů Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="acbbc-193">You can enable hello profiler on both secure and non-secure Service Fabric clusters.</span></span> <span data-ttu-id="acbbc-194">Tento návod používá nezabezpečené clusteru jako tooexplain příklad jaké změny jsou požadované tooenable hello profileru.</span><span class="sxs-lookup"><span data-stu-id="acbbc-194">This walkthrough uses a non-secure cluster as an example tooexplain what changes are required tooenable hello profiler.</span></span> <span data-ttu-id="acbbc-195">Můžete zřídit zabezpečené cluster hello stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="acbbc-195">You can provision a secure cluster in hello same way.</span></span>

1. <span data-ttu-id="acbbc-196">Stáhněte si [ServiceFabricCluster.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json).</span><span class="sxs-lookup"><span data-stu-id="acbbc-196">Download [ServiceFabricCluster.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json).</span></span> <span data-ttu-id="acbbc-197">Stejně jako u virtuálních počítačů a sady škálování virtuálního počítače, nahraďte `Application_Insights_Key` váš klíč Application Insights:</span><span class="sxs-lookup"><span data-stu-id="acbbc-197">As you did for VMs and virtual machine scale sets, replace `Application_Insights_Key` with your Application Insights key:</span></span>

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

2. <span data-ttu-id="acbbc-198">Nasazení hello šablony pomocí skriptu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="acbbc-198">Deploy hello template by using a PowerShell script:</span></span>

   ```
   Login-AzureRmAccount
   New-AzureRmResourceGroup -Name [Your_Resource_Group_Name] -Location [Your_Resource_Group_Location] -Verbose -Force
   New-AzureRmResourceGroupDeployment -Name [Choose_An_Arbitrary_Name] -ResourceGroupName [Your_Resource_Group_Name] -TemplateFile [Path_To_Your_Template]

   ```

### <a name="install-hello-application-insights-sdk-in-hello-project-and-configure-hello-application-insights-key"></a><span data-ttu-id="acbbc-199">Instalace hello Application Insights SDK projektu hello a konfigurace Application Insights klíč hello</span><span class="sxs-lookup"><span data-stu-id="acbbc-199">Install hello Application Insights SDK in hello project and configure hello Application Insights key</span></span>
<span data-ttu-id="acbbc-200">Nainstalujte hello Application Insights SDK z hello [balíček NuGet](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span><span class="sxs-lookup"><span data-stu-id="acbbc-200">Install hello Application Insights SDK from hello [NuGet package](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span></span> <span data-ttu-id="acbbc-201">Ujistěte se, že instalujete stabilní verze 2.3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="acbbc-201">Make sure that you install a stable version, 2.3 or later.</span></span> 

<span data-ttu-id="acbbc-202">Informace o konfigurování služby Application Insights ve vašich projektů najdete v tématu [pomocí Service Fabric pomocí služby Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).</span><span class="sxs-lookup"><span data-stu-id="acbbc-202">For information about configuring Application Insights in your projects, see [Using Service Fabric with Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).</span></span>

### <a name="add-application-code-tooinstrument-telemetry"></a><span data-ttu-id="acbbc-203">Přidání telemetrických údajů tooinstrument kódu aplikace</span><span class="sxs-lookup"><span data-stu-id="acbbc-203">Add application code tooinstrument telemetry</span></span>
1. <span data-ttu-id="acbbc-204">Pro všechny část kódu, které chcete tooinstrument, přidat, pomocí příkazu kolem něj.</span><span class="sxs-lookup"><span data-stu-id="acbbc-204">For any piece of code that you want tooinstrument, add a using statement around it.</span></span> 

   <span data-ttu-id="acbbc-205">V následujícím příkladu hello, hello `RunAsync` metoda některé pracuje a hello `telemetryClient` třída zaznamená hello telemetrie po jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="acbbc-205">In hello following  example, hello `RunAsync` method is doing some work, and hello `telemetryClient` class captures hello telemetry after it starts.</span></span> <span data-ttu-id="acbbc-206">událost Hello musí napříč aplikace hello jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="acbbc-206">hello event needs a unique name across hello application.</span></span>

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

2. <span data-ttu-id="acbbc-207">Nasazení clusteru Service Fabric toohello vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="acbbc-207">Deploy your application toohello Service Fabric cluster.</span></span> <span data-ttu-id="acbbc-208">Počkejte toorun aplikace hello 10 minut.</span><span class="sxs-lookup"><span data-stu-id="acbbc-208">Wait for hello app toorun for 10 minutes.</span></span> <span data-ttu-id="acbbc-209">Pro lepší platit může spuštění zátěžového testu v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="acbbc-209">For better effect, you can run a load test on hello app.</span></span> <span data-ttu-id="acbbc-210">Portál Application Insights přejděte toohello **výkonu** okno a měli vidět příklady profilování trasování zobrazí.</span><span class="sxs-lookup"><span data-stu-id="acbbc-210">Go toohello Application Insights portal's **Performance** blade, and you should see examples of profiling traces appear.</span></span>

<!---
Commenting out these sections for now
## Enable hello Profiler on Cloud Services applications
[TODO]
## Enable hello Profiler on classic Azure Virtual Machines
[TODO]
## Enable hello Profiler on on-premise servers
[TODO]
--->

## <a name="next-steps"></a><span data-ttu-id="acbbc-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="acbbc-211">Next steps</span></span>

- <span data-ttu-id="acbbc-212">Hledání pomoci při řešení problémů profileru problémy v [profileru řešení potíží s](app-insights-profiler.md#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="acbbc-212">Find help with troubleshooting profiler issues in [Profiler troubleshooting](app-insights-profiler.md#troubleshooting).</span></span>

- <span data-ttu-id="acbbc-213">Další informace o hello profileru v [Application Insights profileru](app-insights-profiler.md).</span><span class="sxs-lookup"><span data-stu-id="acbbc-213">Read more about hello profiler in [Application Insights Profiler](app-insights-profiler.md).</span></span>
