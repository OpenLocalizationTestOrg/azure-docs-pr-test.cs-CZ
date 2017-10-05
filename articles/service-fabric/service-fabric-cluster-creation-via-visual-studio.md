---
title: "Nastavení clusteru Service Fabric pomocí sady Visual Studio | Microsoft Docs"
description: "Popisuje, jak nastavit cluster Service Fabric pomocí šablony Azure Resource Manageru vytvořený projekt skupiny prostředků Azure v sadě Visual Studio"
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: adegeo
editor: 
ms.assetid: bd2c0511-36c9-4828-8dc3-69e4b6a70567
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/21/2017
ms.author: mikhegn
redirect_url: /azure/service-fabric/service-fabric-cluster-creation-via-arm
ms.openlocfilehash: c43145b96cdbdfaa7e1893e50d027321fe4c0510
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a><span data-ttu-id="24184-103">Nastavení clusteru Service Fabric pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24184-103">Set up a Service Fabric cluster by using Visual Studio</span></span>
<span data-ttu-id="24184-104">Tento článek popisuje, jak nastavit cluster služby Azure Service Fabric pomocí Visual Studia a šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="24184-104">This article describes how to set up an Azure Service Fabric cluster by using Visual Studio and an Azure Resource Manager template.</span></span> <span data-ttu-id="24184-105">Použijeme projektu skupiny prostředků Visual Studio Azure k vytvoření šablony.</span><span class="sxs-lookup"><span data-stu-id="24184-105">We will use a Visual Studio Azure resource group project to create the template.</span></span> <span data-ttu-id="24184-106">Po vytvoření šablony, dá se nasadit přímo do Azure ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24184-106">After the template has been created, it can be deployed directly to Azure from Visual Studio.</span></span> <span data-ttu-id="24184-107">Může taky sloužit ze skriptu, nebo jako součást budovy průběžnou integraci (konfigurace).</span><span class="sxs-lookup"><span data-stu-id="24184-107">It can also be used from a script, or as part of continuous integration (CI) facility.</span></span>

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a><span data-ttu-id="24184-108">Vytvoření šablony clusteru Service Fabric pomocí projektu skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="24184-108">Create a Service Fabric cluster template by using an Azure resource group project</span></span>
<span data-ttu-id="24184-109">Abyste mohli začít, otevřete Visual Studio a vytvoření projektu skupiny prostředků Azure (je k dispozici v **cloudu** složku):</span><span class="sxs-lookup"><span data-stu-id="24184-109">To get started, open Visual Studio and create an Azure resource group project (it is available in the **Cloud** folder):</span></span>

![Dialogové okno Nový projekt s vybrané projekt skupiny prostředků Azure][1]

<span data-ttu-id="24184-111">Můžete vytvořit nové řešení sady Visual Studio pro tento projekt nebo přidat do existujícího řešení.</span><span class="sxs-lookup"><span data-stu-id="24184-111">You can create a new Visual Studio solution for this project or add it to an existing solution.</span></span>

> [!NOTE]
> <span data-ttu-id="24184-112">Pokud nevidíte projekt skupiny prostředků Azure pod uzlem cloudu, nemáte sadu Azure SDK nainstalována.</span><span class="sxs-lookup"><span data-stu-id="24184-112">If you do not see the Azure resource group project under the Cloud node, you do not have the Azure SDK installed.</span></span> <span data-ttu-id="24184-113">Spuštění instalačního programu webové platformy ([ji teď nainstalovat](http://www.microsoft.com/web/downloads/platform.aspx) Pokud máte ještě není) a poté vyhledejte "Azure SDK pro .NET" a nainstalujte verzi, která je kompatibilní s vaší verzí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24184-113">Launch Web Platform Installer ([install it now](http://www.microsoft.com/web/downloads/platform.aspx) if you have not already), and then search for "Azure SDK for .NET" and install the version that is compatible with your version of Visual Studio.</span></span>
> 
> 

<span data-ttu-id="24184-114">Po kliknutí na tlačítko OK, Visual Studio se zeptá, chcete-li vybrat šablonu Resource Manager, kterou chcete vytvořit:</span><span class="sxs-lookup"><span data-stu-id="24184-114">After you hit the OK button, Visual Studio will ask you to select the Resource Manager template you want to create:</span></span>

![Vyberte šablonu Azure dialogové okno s vybraná šablona Service Fabric Cluster][2]

<span data-ttu-id="24184-116">Vyberte šablonu, Service Fabric Cluster a znovu stiskněte tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="24184-116">Select the Service Fabric Cluster template and hit the OK button again.</span></span> <span data-ttu-id="24184-117">Nyní byly vytvořeny projekt a šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="24184-117">The project and the Resource Manager template have now been created.</span></span>

## <a name="prepare-the-template-for-deployment"></a><span data-ttu-id="24184-118">Příprava pro nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="24184-118">Prepare the template for deployment</span></span>
<span data-ttu-id="24184-119">Před nasazením šablony k vytvoření clusteru, je nutné zadat hodnoty pro parametry požadované šablony.</span><span class="sxs-lookup"><span data-stu-id="24184-119">Before the template is deployed to create the cluster, you must provide values for the required template parameters.</span></span> <span data-ttu-id="24184-120">Tyto hodnoty parametrů se načítají z `ServiceFabricCluster.parameters.json` souboru, který je v `Templates` složku projekt skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="24184-120">These parameter values are read from the `ServiceFabricCluster.parameters.json` file, which is in the `Templates` folder of the resource group project.</span></span> <span data-ttu-id="24184-121">Otevřete soubor a zadejte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="24184-121">Open the file and provide the following values:</span></span>

| <span data-ttu-id="24184-122">Název parametru</span><span class="sxs-lookup"><span data-stu-id="24184-122">Parameter name</span></span> | <span data-ttu-id="24184-123">Popis</span><span class="sxs-lookup"><span data-stu-id="24184-123">Description</span></span> |
| --- | --- |
| <span data-ttu-id="24184-124">adminUserName</span><span class="sxs-lookup"><span data-stu-id="24184-124">adminUserName</span></span> |<span data-ttu-id="24184-125">Název účtu správce pro Service Fabric počítačů (uzlů).</span><span class="sxs-lookup"><span data-stu-id="24184-125">The name of the administrator account for Service Fabric machines (nodes).</span></span> |
| <span data-ttu-id="24184-126">certificateThumbprint</span><span class="sxs-lookup"><span data-stu-id="24184-126">certificateThumbprint</span></span> |<span data-ttu-id="24184-127">Kryptografický otisk certifikátu, který zabezpečuje clusteru.</span><span class="sxs-lookup"><span data-stu-id="24184-127">The thumbprint of the certificate that secures the cluster.</span></span> |
| <span data-ttu-id="24184-128">sourceVaultResourceId</span><span class="sxs-lookup"><span data-stu-id="24184-128">sourceVaultResourceId</span></span> |<span data-ttu-id="24184-129">*ID prostředku* služby key vault, kde je uložen certifikát, který zabezpečuje clusteru.</span><span class="sxs-lookup"><span data-stu-id="24184-129">The *resource ID* of the key vault where the certificate that secures the cluster is stored.</span></span> |
| <span data-ttu-id="24184-130">certificateUrlValue</span><span class="sxs-lookup"><span data-stu-id="24184-130">certificateUrlValue</span></span> |<span data-ttu-id="24184-131">Adresa URL certifikátu zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="24184-131">The URL of the cluster security certificate.</span></span> |

<span data-ttu-id="24184-132">Šablony Visual Studio Service Fabric Resource Manageru vytvoří zabezpečené clusteru, který je chráněný pomocí certifikátu.</span><span class="sxs-lookup"><span data-stu-id="24184-132">The Visual Studio Service Fabric Resource Manager template creates a secure cluster that is protected by a certificate.</span></span> <span data-ttu-id="24184-133">Tento certifikát je určený podle parametry poslední tři šablony (`certificateThumbprint`, `sourceVaultValue`, a `certificateUrlValue`), a musí existovat v **Azure Key Vault**.</span><span class="sxs-lookup"><span data-stu-id="24184-133">This certificate is identified by the last three template parameters (`certificateThumbprint`, `sourceVaultValue`, and `certificateUrlValue`), and it must exist in an **Azure Key Vault**.</span></span> <span data-ttu-id="24184-134">Další informace o tom, jak vytvořit certifikát zabezpečení clusteru najdete v tématu [scénáře zabezpečení clusteru Service Fabric](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) článku.</span><span class="sxs-lookup"><span data-stu-id="24184-134">For more information on how to create the cluster security certificate, see [Service Fabric cluster security scenarios](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) article.</span></span>

## <a name="optional-change-the-cluster-name"></a><span data-ttu-id="24184-135">Volitelné: Změna názvu clusteru</span><span class="sxs-lookup"><span data-stu-id="24184-135">Optional: change the cluster name</span></span>
<span data-ttu-id="24184-136">Každý cluster Service Fabric obsahuje název.</span><span class="sxs-lookup"><span data-stu-id="24184-136">Every Service Fabric cluster has a name.</span></span> <span data-ttu-id="24184-137">Při vytváření clusteru s podporou prostředků infrastruktury v Azure název clusteru určuje (spolu s oblasti Azure) systému DNS (Domain Name) název pro cluster.</span><span class="sxs-lookup"><span data-stu-id="24184-137">When a Fabric cluster is created in Azure, cluster name determines (together with the Azure region) the Domain Name System (DNS) name for the cluster.</span></span> <span data-ttu-id="24184-138">Například pokud použijete název clusteru `myBigCluster`a umístění (oblasti Azure) skupiny prostředků, který bude nový cluster hostitelem je Východ USA, bude název DNS clusteru `myBigCluster.eastus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="24184-138">For example, if you name your cluster `myBigCluster`, and the location (Azure region) of the resource group that will host the new cluster is East US, the DNS name of the cluster will be `myBigCluster.eastus.cloudapp.azure.com`.</span></span>

<span data-ttu-id="24184-139">Ve výchozím nastavení je název clusteru, generuje automaticky a jedinečný provedené příponu náhodných se připojuje k předponu "clusteru".</span><span class="sxs-lookup"><span data-stu-id="24184-139">By default the cluster name is generated automatically and made unique by attaching a random suffix to a "cluster" prefix.</span></span> <span data-ttu-id="24184-140">Díky tomu je velmi snadné použít šablonu v rámci **průběžnou integraci** systému (konfigurace).</span><span class="sxs-lookup"><span data-stu-id="24184-140">This makes it very easy to use the template as part of a **continuous integration** (CI) system.</span></span> <span data-ttu-id="24184-141">Pokud chcete použít konkrétní název pro váš cluster, ten, který má smysl, nastavte hodnotu `clusterName` proměnné v souboru šablony Resource Manageru (`ServiceFabricCluster.json`) pro zvolený název.</span><span class="sxs-lookup"><span data-stu-id="24184-141">If you want to use a specific name for your cluster, one that is meaningful to you, set the value of the `clusterName` variable in the Resource Manager template file (`ServiceFabricCluster.json`) to your chosen name.</span></span> <span data-ttu-id="24184-142">Je první proměnné definované v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="24184-142">It is the first variable defined in that file.</span></span>

## <a name="optional-add-public-application-ports"></a><span data-ttu-id="24184-143">Volitelné: přidejte veřejný aplikace porty</span><span class="sxs-lookup"><span data-stu-id="24184-143">Optional: add public application ports</span></span>
<span data-ttu-id="24184-144">Můžete také změnit porty veřejné aplikace pro cluster před nasazením.</span><span class="sxs-lookup"><span data-stu-id="24184-144">You may also want to change the public application ports for the cluster before you deploy it.</span></span> <span data-ttu-id="24184-145">Ve výchozím nastavení otevře se šablony ještě dvě veřejné porty TCP (80 a 8081).</span><span class="sxs-lookup"><span data-stu-id="24184-145">By default, the template opens up just two public TCP ports (80 and 8081).</span></span> <span data-ttu-id="24184-146">Pokud potřebujete více pro vaše aplikace, upravte definici Azure nástroj pro vyrovnávání zatížení v šabloně.</span><span class="sxs-lookup"><span data-stu-id="24184-146">If you need more for your applications, modify the Azure Load Balancer definition in the template.</span></span> <span data-ttu-id="24184-147">Definice je uložený v souboru hlavní šablony (`ServiceFabricCluster.json`).</span><span class="sxs-lookup"><span data-stu-id="24184-147">The definition is stored in the main template file (`ServiceFabricCluster.json`).</span></span> <span data-ttu-id="24184-148">Otevřete tento soubor a vyhledejte `loadBalancedAppPort`.</span><span class="sxs-lookup"><span data-stu-id="24184-148">Open that file and search for `loadBalancedAppPort`.</span></span> <span data-ttu-id="24184-149">Každý z portů je spojena s tři artefakty:</span><span class="sxs-lookup"><span data-stu-id="24184-149">Each port is associated with three artifacts:</span></span>

1. <span data-ttu-id="24184-150">Proměnná šablony, který definuje hodnotu portu TCP pro port:</span><span class="sxs-lookup"><span data-stu-id="24184-150">A template variable that defines the TCP port value for the port:</span></span>
   
    ```json
    "loadBalancedAppPort1": "80"
    ```
2. <span data-ttu-id="24184-151">A *testu* který definuje, jak často a jak dlouho Azure zátěže vyrovnávání pokusí použít konkrétním uzlu Service Fabric před převzetí služeb při selhání na jiný.</span><span class="sxs-lookup"><span data-stu-id="24184-151">A *probe* that defines how frequently and for how long the Azure load balancer attempts to use a specific Service Fabric node before failing over to another one.</span></span> <span data-ttu-id="24184-152">Sondy jsou součástí prostředek pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="24184-152">The probes are part of the Load Balancer resource.</span></span> <span data-ttu-id="24184-153">Tady je definici testu pro první výchozí port aplikace:</span><span class="sxs-lookup"><span data-stu-id="24184-153">Here is the probe definition for the first default application port:</span></span>
   
    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```
3. <span data-ttu-id="24184-154">A *pravidlo Vyrovnávání zatížení* který společně sváže port a kontroly, které umožňuje vyrovnávání napříč sada uzlů clusteru Service Fabric zatížení:</span><span class="sxs-lookup"><span data-stu-id="24184-154">A *load-balancing rule* that ties together the port and the probe, which enables load balancing across a set of Service Fabric cluster nodes:</span></span>
   
    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
   <span data-ttu-id="24184-155">Pokud aplikace, které plánujete nasadit do clusteru potřebovat více porty, můžete je přidat tak, že vytváření další kontroly a definicí pravidel Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="24184-155">If the applications that you plan to deploy to the cluster need more ports, you can add them by creating additional probe and load-balancing rule definitions.</span></span> <span data-ttu-id="24184-156">Další informace o tom, jak pracovat s Vyrovnávání zatížení Azure pomocí šablony Resource Manageru najdete v tématu [začínáte s vytvářením interní nástroj pomocí šablony](../load-balancer/load-balancer-get-started-ilb-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="24184-156">For more information on how to work with Azure Load Balancer through Resource Manager templates, see [Get started creating an internal load balancer using a template](../load-balancer/load-balancer-get-started-ilb-arm-template.md).</span></span>

## <a name="deploy-the-template-by-using-visual-studio"></a><span data-ttu-id="24184-157">Nasazení šablony pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24184-157">Deploy the template by using Visual Studio</span></span>
<span data-ttu-id="24184-158">Po uložení všech hodnot požadovaný parametr v`ServiceFabricCluster.param.dev.json` souboru, jste připraveni k nasazení šablony a vytvořit cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="24184-158">After you have saved all the required parameter values in the`ServiceFabricCluster.param.dev.json` file, you are ready to deploy the template and create your Service Fabric cluster.</span></span> <span data-ttu-id="24184-159">Klikněte pravým tlačítkem na projekt skupiny prostředků v Průzkumníku řešení Visual Studio a zvolte **nasadit | Nové nasazení...** .</span><span class="sxs-lookup"><span data-stu-id="24184-159">Right-click the resource group project in Visual Studio Solution Explorer and choose **Deploy | New Deployment...**.</span></span> <span data-ttu-id="24184-160">Pokud potřeby Visual Studio se zobrazí **nasadit do skupiny prostředků** dialogové okno, žádostí o ověřování v Azure:</span><span class="sxs-lookup"><span data-stu-id="24184-160">If necessary, Visual Studio will show the **Deploy to Resource Group** dialog box, asking you to authenticate to Azure:</span></span>

![Nasazení do skupiny prostředků dialogového okna][3]

<span data-ttu-id="24184-162">Dialogové okno umožňuje vybrat existující skupinu prostředků Resource Manageru pro cluster a vám dává možnost vytvořit novou.</span><span class="sxs-lookup"><span data-stu-id="24184-162">The dialog box lets you choose an existing Resource Manager resource group for the cluster and gives you the option to create a new one.</span></span> <span data-ttu-id="24184-163">Za normálních okolností má smysl použít skupinu samostatné prostředků pro cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="24184-163">It normally makes sense to use a separate resource group for a Service Fabric cluster.</span></span>

<span data-ttu-id="24184-164">Po kliknutí na tlačítko nasadit, Visual Studio zobrazí výzvu k potvrzení hodnot parametrů šablony.</span><span class="sxs-lookup"><span data-stu-id="24184-164">After you hit the Deploy button, Visual Studio will prompt you to confirm the template parameter values.</span></span> <span data-ttu-id="24184-165">Stiskněte tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="24184-165">Hit the **Save** button.</span></span> <span data-ttu-id="24184-166">Jeden parametr nemá hodnotu trvalou: heslo k účtu pro správu pro cluster.</span><span class="sxs-lookup"><span data-stu-id="24184-166">One parameter does not have a persisted value: the administrative account password for the cluster.</span></span> <span data-ttu-id="24184-167">Je třeba zadat hodnotu heslo, když Visual Studio vás vyzve k zadání jednoho.</span><span class="sxs-lookup"><span data-stu-id="24184-167">You need to provide a password value when Visual Studio prompts you for one.</span></span>

> [!NOTE]
> <span data-ttu-id="24184-168">Počínaje sadu Azure SDK 2.9, Visual Studio podporuje čtení hesla z **Azure Key Vault** během nasazení.</span><span class="sxs-lookup"><span data-stu-id="24184-168">Starting with Azure SDK 2.9, Visual Studio supports reading passwords from **Azure Key Vault** during deployment.</span></span> <span data-ttu-id="24184-169">V dialogovém okně Parametry šablony Všimněte si, že `adminPassword` parametr textového pole má moc "klíč" ikonu na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="24184-169">In the template parameters dialog notice that the `adminPassword` parameter text box has a little "key" icon on the right.</span></span> <span data-ttu-id="24184-170">Tato ikona umožňuje vybrat existujícím tajným klíčem trezoru klíčů jako heslo správce pro cluster.</span><span class="sxs-lookup"><span data-stu-id="24184-170">This icon allows you to select an existing key vault secret as the administrative password for the cluster.</span></span> <span data-ttu-id="24184-171">Ujistěte se však na první povolit přístup správce prostředků Azure pro nasazení šablony v Pokročilé zásady přístupu k trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="24184-171">Just make sure to first enable Azure Resource Manager access for template deployment in the Advanced Access Policies of your key vault.</span></span> 
> 
> 

<span data-ttu-id="24184-172">Můžete sledovat průběh procesu nasazení v okně výstupu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24184-172">You can monitor the progress of the deployment process in the Visual Studio output window.</span></span> <span data-ttu-id="24184-173">Po dokončení nasazení šablony je připravený k použití nového clusteru!</span><span class="sxs-lookup"><span data-stu-id="24184-173">Once the template deployment is completed, your new cluster is ready to use!</span></span>

> [!NOTE]
> <span data-ttu-id="24184-174">Pokud prostředí PowerShell nebyl nikdy ke správě Azure z počítače, který teď používáte, musíte udělat ještě housekeeping.</span><span class="sxs-lookup"><span data-stu-id="24184-174">If PowerShell was never used to administer Azure from the machine that you are using now, you need to do a little housekeeping.</span></span>
> 
> 1. <span data-ttu-id="24184-175">Povolit spuštěním skriptů prostředí PowerShell [ `Set-ExecutionPolicy` ](https://technet.microsoft.com/library/hh849812.aspx) příkaz.</span><span class="sxs-lookup"><span data-stu-id="24184-175">Enable PowerShell scripting by running the [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) command.</span></span> <span data-ttu-id="24184-176">Pro počítače, vývoj je obvykle přijatelné "neomezený" zásady.</span><span class="sxs-lookup"><span data-stu-id="24184-176">For development machines, "unrestricted" policy is usually acceptable.</span></span>
> 2. <span data-ttu-id="24184-177">Rozhodněte, jestli chcete povolit shromažďování diagnostických dat z příkazů prostředí Azure PowerShell a spusťte [ `Enable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619303.aspx) nebo [ `Disable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619236.aspx) podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="24184-177">Decide whether to allow diagnostic data collection from Azure PowerShell commands, and run [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) or [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) as necessary.</span></span> <span data-ttu-id="24184-178">Tím se vyhnete nepotřebné výzvy během nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="24184-178">This will avoid unnecessary prompts during template deployment.</span></span>
> 
> 

<span data-ttu-id="24184-179">Pokud nejsou žádné chyby, přejděte k [portál Azure](https://portal.azure.com/) a otevřete skupinu prostředků, které jste nasadili.</span><span class="sxs-lookup"><span data-stu-id="24184-179">If there are any errors, go to the [Azure portal](https://portal.azure.com/) and open the resource group that you deployed to.</span></span> <span data-ttu-id="24184-180">Klikněte na tlačítko **všechna nastavení**, pak klikněte na tlačítko **nasazení** v okně nastavení.</span><span class="sxs-lookup"><span data-stu-id="24184-180">Click **All settings**, then click **Deployments** on the settings blade.</span></span> <span data-ttu-id="24184-181">Selhání nasazení skupiny prostředků existuje zanechává podrobné diagnostické informace.</span><span class="sxs-lookup"><span data-stu-id="24184-181">A failed resource-group deployment leaves detailed diagnostic information there.</span></span>

> [!NOTE]
> <span data-ttu-id="24184-182">Clusterů Service Fabric vyžadují určitý počet uzlů na si zachovat dostupnost a zároveň zachovat stav – označuje jako "údržbu kvora."</span><span class="sxs-lookup"><span data-stu-id="24184-182">Service Fabric clusters require a certain number of nodes to be up to maintain availability and preserve state - referred to as "maintaining quorum."</span></span> <span data-ttu-id="24184-183">Proto není bezpečné a ukončí se všechny počítače v clusteru Pokud jste provedli nejprve [úplného zálohování vaší stavu](service-fabric-reliable-services-backup-restore.md).</span><span class="sxs-lookup"><span data-stu-id="24184-183">Therefore, it is not safe to shut down all of the machines in the cluster unless you have first performed a [full backup of your state](service-fabric-reliable-services-backup-restore.md).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="24184-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="24184-184">Next steps</span></span>
* [<span data-ttu-id="24184-185">Další informace o nastavení cluster Service Fabric pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="24184-185">Learn about setting up Service Fabric cluster using the Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
* [<span data-ttu-id="24184-186">Zjistěte, jak spravovat a nasazovat aplikace Service Fabric pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24184-186">Learn how to manage and deploy Service Fabric applications using Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
