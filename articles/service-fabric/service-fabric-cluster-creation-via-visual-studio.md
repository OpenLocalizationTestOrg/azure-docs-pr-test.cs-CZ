---
title: "aaaSetting se cluster Service Fabric pomocí sady Visual Studio | Microsoft Docs"
description: "Popisuje, jak clusteru tooset až Service Fabric pomocí šablony Azure Resource Manageru vytvořený projekt skupiny prostředků Azure v sadě Visual Studio"
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
ms.openlocfilehash: adb0dd2169a28b46e832c6f06c998cbed0c473f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a><span data-ttu-id="de6d6-103">Nastavení clusteru Service Fabric pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de6d6-103">Set up a Service Fabric cluster by using Visual Studio</span></span>
<span data-ttu-id="de6d6-104">Tento článek popisuje, jak clusteru tooset nahoru Azure Service Fabric pomocí Visual Studia a šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="de6d6-104">This article describes how tooset up an Azure Service Fabric cluster by using Visual Studio and an Azure Resource Manager template.</span></span> <span data-ttu-id="de6d6-105">Budeme používat šablony Visual Studio Azure prostředků skupiny projektu toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="de6d6-105">We will use a Visual Studio Azure resource group project toocreate hello template.</span></span> <span data-ttu-id="de6d6-106">Po vytvoření šablony hello, dá se nasadit přímo tooAzure ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="de6d6-106">After hello template has been created, it can be deployed directly tooAzure from Visual Studio.</span></span> <span data-ttu-id="de6d6-107">Může taky sloužit ze skriptu, nebo jako součást budovy průběžnou integraci (konfigurace).</span><span class="sxs-lookup"><span data-stu-id="de6d6-107">It can also be used from a script, or as part of continuous integration (CI) facility.</span></span>

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a><span data-ttu-id="de6d6-108">Vytvoření šablony clusteru Service Fabric pomocí projektu skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="de6d6-108">Create a Service Fabric cluster template by using an Azure resource group project</span></span>
<span data-ttu-id="de6d6-109">spuštění tooget, otevřete Visual Studio a vytvoření projektu skupiny prostředků Azure (je k dispozici v hello **cloudu** složku):</span><span class="sxs-lookup"><span data-stu-id="de6d6-109">tooget started, open Visual Studio and create an Azure resource group project (it is available in hello **Cloud** folder):</span></span>

![Dialogové okno Nový projekt s vybrané projekt skupiny prostředků Azure][1]

<span data-ttu-id="de6d6-111">Můžete vytvořit nové řešení sady Visual Studio pro tento projekt nebo ho přidat tooan existující řešení.</span><span class="sxs-lookup"><span data-stu-id="de6d6-111">You can create a new Visual Studio solution for this project or add it tooan existing solution.</span></span>

> [!NOTE]
> <span data-ttu-id="de6d6-112">Pokud nevidíte hello projekt skupiny prostředků Azure v rámci uzlu hello cloudu, není nutné hello nainstalované sady Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="de6d6-112">If you do not see hello Azure resource group project under hello Cloud node, you do not have hello Azure SDK installed.</span></span> <span data-ttu-id="de6d6-113">Spuštění instalačního programu webové platformy ([ji teď nainstalovat](http://www.microsoft.com/web/downloads/platform.aspx) Pokud máte ještě není) a poté vyhledejte "Azure SDK pro .NET" a verze hello instalace, který je kompatibilní s vaší verzí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="de6d6-113">Launch Web Platform Installer ([install it now](http://www.microsoft.com/web/downloads/platform.aspx) if you have not already), and then search for "Azure SDK for .NET" and install hello version that is compatible with your version of Visual Studio.</span></span>
> 
> 

<span data-ttu-id="de6d6-114">Po kliknutí na tlačítko OK hello, Visual Studio se vás zeptá šablony Resource Manageru hello tooselect chcete toocreate:</span><span class="sxs-lookup"><span data-stu-id="de6d6-114">After you hit hello OK button, Visual Studio will ask you tooselect hello Resource Manager template you want toocreate:</span></span>

![Vyberte šablonu Azure dialogové okno s vybraná šablona Service Fabric Cluster][2]

<span data-ttu-id="de6d6-116">Vyberte šablonu hello Service Fabric Cluster a tlačítko přístupů hello OK znovu.</span><span class="sxs-lookup"><span data-stu-id="de6d6-116">Select hello Service Fabric Cluster template and hit hello OK button again.</span></span> <span data-ttu-id="de6d6-117">Nyní byly vytvořeny Hello projekt a šablony Resource Manageru hello.</span><span class="sxs-lookup"><span data-stu-id="de6d6-117">hello project and hello Resource Manager template have now been created.</span></span>

## <a name="prepare-hello-template-for-deployment"></a><span data-ttu-id="de6d6-118">Příprava hello šablony pro nasazení</span><span class="sxs-lookup"><span data-stu-id="de6d6-118">Prepare hello template for deployment</span></span>
<span data-ttu-id="de6d6-119">Předtím, než je šablona hello nasazené toocreate hello clusteru, je nutné zadat hodnoty pro parametry hello požadované šablony.</span><span class="sxs-lookup"><span data-stu-id="de6d6-119">Before hello template is deployed toocreate hello cluster, you must provide values for hello required template parameters.</span></span> <span data-ttu-id="de6d6-120">Tyto hodnoty parametrů se načítají z hello `ServiceFabricCluster.parameters.json` souboru, který je v hello `Templates` složky hello projekt skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="de6d6-120">These parameter values are read from hello `ServiceFabricCluster.parameters.json` file, which is in hello `Templates` folder of hello resource group project.</span></span> <span data-ttu-id="de6d6-121">Otevřete soubor hello a zadejte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="de6d6-121">Open hello file and provide hello following values:</span></span>

| <span data-ttu-id="de6d6-122">Název parametru</span><span class="sxs-lookup"><span data-stu-id="de6d6-122">Parameter name</span></span> | <span data-ttu-id="de6d6-123">Popis</span><span class="sxs-lookup"><span data-stu-id="de6d6-123">Description</span></span> |
| --- | --- |
| <span data-ttu-id="de6d6-124">adminUserName</span><span class="sxs-lookup"><span data-stu-id="de6d6-124">adminUserName</span></span> |<span data-ttu-id="de6d6-125">Hello název účtu správce hello Service Fabric počítačů (uzlů).</span><span class="sxs-lookup"><span data-stu-id="de6d6-125">hello name of hello administrator account for Service Fabric machines (nodes).</span></span> |
| <span data-ttu-id="de6d6-126">certificateThumbprint</span><span class="sxs-lookup"><span data-stu-id="de6d6-126">certificateThumbprint</span></span> |<span data-ttu-id="de6d6-127">Hello kryptografický otisk certifikátu hello, který zabezpečuje hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="de6d6-127">hello thumbprint of hello certificate that secures hello cluster.</span></span> |
| <span data-ttu-id="de6d6-128">sourceVaultResourceId</span><span class="sxs-lookup"><span data-stu-id="de6d6-128">sourceVaultResourceId</span></span> |<span data-ttu-id="de6d6-129">Hello *ID prostředku* hello klíče trezoru se uloží hello certifikát, který zabezpečuje hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="de6d6-129">hello *resource ID* of hello key vault where hello certificate that secures hello cluster is stored.</span></span> |
| <span data-ttu-id="de6d6-130">certificateUrlValue</span><span class="sxs-lookup"><span data-stu-id="de6d6-130">certificateUrlValue</span></span> |<span data-ttu-id="de6d6-131">Adresa URL Hello hello certifikát zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="de6d6-131">hello URL of hello cluster security certificate.</span></span> |

<span data-ttu-id="de6d6-132">šablony Visual Studio Service Fabric Resource Manageru Hello vytvoří zabezpečené clusteru, který je chráněný pomocí certifikátu.</span><span class="sxs-lookup"><span data-stu-id="de6d6-132">hello Visual Studio Service Fabric Resource Manager template creates a secure cluster that is protected by a certificate.</span></span> <span data-ttu-id="de6d6-133">Tento certifikát je určený podle hello poslední tři parametry šablony (`certificateThumbprint`, `sourceVaultValue`, a `certificateUrlValue`), a musí existovat v **Azure Key Vault**.</span><span class="sxs-lookup"><span data-stu-id="de6d6-133">This certificate is identified by hello last three template parameters (`certificateThumbprint`, `sourceVaultValue`, and `certificateUrlValue`), and it must exist in an **Azure Key Vault**.</span></span> <span data-ttu-id="de6d6-134">Další informace o tom, jak toocreate hello certifikát zabezpečení clusteru najdete v tématu [scénáře zabezpečení clusteru Service Fabric](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) článku.</span><span class="sxs-lookup"><span data-stu-id="de6d6-134">For more information on how toocreate hello cluster security certificate, see [Service Fabric cluster security scenarios](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) article.</span></span>

## <a name="optional-change-hello-cluster-name"></a><span data-ttu-id="de6d6-135">Volitelné: Změna názvu clusteru hello</span><span class="sxs-lookup"><span data-stu-id="de6d6-135">Optional: change hello cluster name</span></span>
<span data-ttu-id="de6d6-136">Každý cluster Service Fabric obsahuje název.</span><span class="sxs-lookup"><span data-stu-id="de6d6-136">Every Service Fabric cluster has a name.</span></span> <span data-ttu-id="de6d6-137">Při vytváření clusteru s podporou prostředků infrastruktury v Azure (spolu s hello oblasti Azure) určuje název clusteru hello systému DNS (Domain Name) název pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="de6d6-137">When a Fabric cluster is created in Azure, cluster name determines (together with hello Azure region) hello Domain Name System (DNS) name for hello cluster.</span></span> <span data-ttu-id="de6d6-138">Například pokud použijete název clusteru `myBigCluster`a hello umístěním (oblastí Azure) hello skupiny prostředků, který bude hostitelem hello nového clusteru je Východ USA, bude název DNS hello hello clusteru `myBigCluster.eastus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="de6d6-138">For example, if you name your cluster `myBigCluster`, and hello location (Azure region) of hello resource group that will host hello new cluster is East US, hello DNS name of hello cluster will be `myBigCluster.eastus.cloudapp.azure.com`.</span></span>

<span data-ttu-id="de6d6-139">Ve výchozím nastavení je název clusteru hello generovány automaticky a jedinečný provedené připojení předponu náhodných příponu tooa "clusteru".</span><span class="sxs-lookup"><span data-stu-id="de6d6-139">By default hello cluster name is generated automatically and made unique by attaching a random suffix tooa "cluster" prefix.</span></span> <span data-ttu-id="de6d6-140">Díky tomu šablony velmi snadné toouse hello jako součást **průběžnou integraci** systému (konfigurace).</span><span class="sxs-lookup"><span data-stu-id="de6d6-140">This makes it very easy toouse hello template as part of a **continuous integration** (CI) system.</span></span> <span data-ttu-id="de6d6-141">Pokud chcete, aby toouse určitý název pro váš cluster, ten, který je smysluplný tooyou, nastavte hodnotu hello hello `clusterName` proměnné v souboru šablony Resource Manageru hello (`ServiceFabricCluster.json`) tooyour zvolený název.</span><span class="sxs-lookup"><span data-stu-id="de6d6-141">If you want toouse a specific name for your cluster, one that is meaningful tooyou, set hello value of hello `clusterName` variable in hello Resource Manager template file (`ServiceFabricCluster.json`) tooyour chosen name.</span></span> <span data-ttu-id="de6d6-142">Je hello první proměnné definované v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="de6d6-142">It is hello first variable defined in that file.</span></span>

## <a name="optional-add-public-application-ports"></a><span data-ttu-id="de6d6-143">Volitelné: přidejte veřejný aplikace porty</span><span class="sxs-lookup"><span data-stu-id="de6d6-143">Optional: add public application ports</span></span>
<span data-ttu-id="de6d6-144">Můžete také toochange hello veřejné aplikace porty pro hello cluster před nasazením.</span><span class="sxs-lookup"><span data-stu-id="de6d6-144">You may also want toochange hello public application ports for hello cluster before you deploy it.</span></span> <span data-ttu-id="de6d6-145">Ve výchozím nastavení otevře se hello šablony ještě dvě veřejné porty TCP (80 a 8081).</span><span class="sxs-lookup"><span data-stu-id="de6d6-145">By default, hello template opens up just two public TCP ports (80 and 8081).</span></span> <span data-ttu-id="de6d6-146">Pokud potřebujete více pro vaše aplikace, upravte hello nástroj pro vyrovnávání zatížení Azure v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="de6d6-146">If you need more for your applications, modify hello Azure Load Balancer definition in hello template.</span></span> <span data-ttu-id="de6d6-147">definice Hello je uložený v souboru hlavní šablonu hello (`ServiceFabricCluster.json`).</span><span class="sxs-lookup"><span data-stu-id="de6d6-147">hello definition is stored in hello main template file (`ServiceFabricCluster.json`).</span></span> <span data-ttu-id="de6d6-148">Otevřete tento soubor a vyhledejte `loadBalancedAppPort`.</span><span class="sxs-lookup"><span data-stu-id="de6d6-148">Open that file and search for `loadBalancedAppPort`.</span></span> <span data-ttu-id="de6d6-149">Každý z portů je spojena s tři artefakty:</span><span class="sxs-lookup"><span data-stu-id="de6d6-149">Each port is associated with three artifacts:</span></span>

1. <span data-ttu-id="de6d6-150">Proměnné šablony, která definuje hello hodnota portu TCP pro hello port:</span><span class="sxs-lookup"><span data-stu-id="de6d6-150">A template variable that defines hello TCP port value for hello port:</span></span>
   
    ```json
    "loadBalancedAppPort1": "80"
    ```
2. <span data-ttu-id="de6d6-151">A *testu* který definuje, jak často a jak dlouho hello nástroje pro vyrovnávání zatížení Azure přes tooanother jeden pokusů o zadání toouse konkrétním uzlu Service Fabric, než selže.</span><span class="sxs-lookup"><span data-stu-id="de6d6-151">A *probe* that defines how frequently and for how long hello Azure load balancer attempts toouse a specific Service Fabric node before failing over tooanother one.</span></span> <span data-ttu-id="de6d6-152">sondy Hello jsou součástí hello prostředek pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="de6d6-152">hello probes are part of hello Load Balancer resource.</span></span> <span data-ttu-id="de6d6-153">Zde je definice testu hello hello první výchozí aplikace port:</span><span class="sxs-lookup"><span data-stu-id="de6d6-153">Here is hello probe definition for hello first default application port:</span></span>
   
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
3. <span data-ttu-id="de6d6-154">A *pravidlo Vyrovnávání zatížení* který společně sváže hello port a hello kontroly, které umožňuje vyrovnávání napříč sada uzlů clusteru Service Fabric zatížení:</span><span class="sxs-lookup"><span data-stu-id="de6d6-154">A *load-balancing rule* that ties together hello port and hello probe, which enables load balancing across a set of Service Fabric cluster nodes:</span></span>
   
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
   <span data-ttu-id="de6d6-155">Pokud aplikace hello plánování toodeploy toohello clusteru potřebují další porty, můžete je přidat tak, že vytváření další kontroly a definicí pravidel Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="de6d6-155">If hello applications that you plan toodeploy toohello cluster need more ports, you can add them by creating additional probe and load-balancing rule definitions.</span></span> <span data-ttu-id="de6d6-156">Další informace o tom, toowork s Vyrovnávání zatížení Azure pomocí šablony Resource Manageru, najdete v části [začínáte s vytvářením interní nástroj pomocí šablony](../load-balancer/load-balancer-get-started-ilb-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="de6d6-156">For more information on how toowork with Azure Load Balancer through Resource Manager templates, see [Get started creating an internal load balancer using a template](../load-balancer/load-balancer-get-started-ilb-arm-template.md).</span></span>

## <a name="deploy-hello-template-by-using-visual-studio"></a><span data-ttu-id="de6d6-157">Nasazení šablony hello pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de6d6-157">Deploy hello template by using Visual Studio</span></span>
<span data-ttu-id="de6d6-158">Po uložení všech hello požadovaný parametr hodnoty v`ServiceFabricCluster.param.dev.json` souboru, jsou připravené toodeploy hello šablony a vytvořit cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="de6d6-158">After you have saved all hello required parameter values in the`ServiceFabricCluster.param.dev.json` file, you are ready toodeploy hello template and create your Service Fabric cluster.</span></span> <span data-ttu-id="de6d6-159">Klikněte pravým tlačítkem na projekt skupiny prostředků hello v Průzkumníku řešení Visual Studio a zvolte **nasadit | Nové nasazení...** . Pokud potřeby Visual Studio se zobrazí hello **nasazení tooResource skupiny** dialogového okna, s tooauthenticate tooAzure:</span><span class="sxs-lookup"><span data-stu-id="de6d6-159">Right-click hello resource group project in Visual Studio Solution Explorer and choose **Deploy | New Deployment...**. If necessary, Visual Studio will show hello **Deploy tooResource Group** dialog box, asking you tooauthenticate tooAzure:</span></span>

![Nasazení skupiny tooResource dialogové okno][3]

<span data-ttu-id="de6d6-161">Dialogové okno Hello způsobem můžete vybrat existující skupinu prostředků Resource Manager hello clusteru a poskytuje hello toocreate možnost Nový.</span><span class="sxs-lookup"><span data-stu-id="de6d6-161">hello dialog box lets you choose an existing Resource Manager resource group for hello cluster and gives you hello option toocreate a new one.</span></span> <span data-ttu-id="de6d6-162">Za normálních okolností má smysl toouse skupinu samostatné prostředků pro cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="de6d6-162">It normally makes sense toouse a separate resource group for a Service Fabric cluster.</span></span>

<span data-ttu-id="de6d6-163">Po kliknutí na tlačítko nasadit hello, Visual Studio zobrazí výzvu hodnot parametrů šablony tooconfirm hello.</span><span class="sxs-lookup"><span data-stu-id="de6d6-163">After you hit hello Deploy button, Visual Studio will prompt you tooconfirm hello template parameter values.</span></span> <span data-ttu-id="de6d6-164">Přístupů hello **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="de6d6-164">Hit hello **Save** button.</span></span> <span data-ttu-id="de6d6-165">Jeden parametr nemá hodnotu trvalou: hello heslo účtu správce pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="de6d6-165">One parameter does not have a persisted value: hello administrative account password for hello cluster.</span></span> <span data-ttu-id="de6d6-166">Když Visual Studio vás vyzve k zadání jednoho musíte tooprovide hodnotu heslo.</span><span class="sxs-lookup"><span data-stu-id="de6d6-166">You need tooprovide a password value when Visual Studio prompts you for one.</span></span>

> [!NOTE]
> <span data-ttu-id="de6d6-167">Počínaje sadu Azure SDK 2.9, Visual Studio podporuje čtení hesla z **Azure Key Vault** během nasazení.</span><span class="sxs-lookup"><span data-stu-id="de6d6-167">Starting with Azure SDK 2.9, Visual Studio supports reading passwords from **Azure Key Vault** during deployment.</span></span> <span data-ttu-id="de6d6-168">V dialogovém okně Parametry šablony hello Všimněte si, že hello `adminPassword` parametr textového pole má malé ikony "klíč" na hello správné.</span><span class="sxs-lookup"><span data-stu-id="de6d6-168">In hello template parameters dialog notice that hello `adminPassword` parameter text box has a little "key" icon on hello right.</span></span> <span data-ttu-id="de6d6-169">Tato ikona umožňuje tooselect existujícím tajným klíčem trezoru klíčů jako hello hesla pro správu pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="de6d6-169">This icon allows you tooselect an existing key vault secret as hello administrative password for hello cluster.</span></span> <span data-ttu-id="de6d6-170">Ujistěte se, toofirst povolení přístupu Azure Resource Manageru pro nasazení šablony hello pokročilé zásady přístupu trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="de6d6-170">Just make sure toofirst enable Azure Resource Manager access for template deployment in hello Advanced Access Policies of your key vault.</span></span> 
> 
> 

<span data-ttu-id="de6d6-171">Můžete sledovat průběh procesu nasazení hello v okně výstupu sady Visual Studio hello hello.</span><span class="sxs-lookup"><span data-stu-id="de6d6-171">You can monitor hello progress of hello deployment process in hello Visual Studio output window.</span></span> <span data-ttu-id="de6d6-172">Po dokončení nasazení šablony hello nového clusteru je připravené toouse!</span><span class="sxs-lookup"><span data-stu-id="de6d6-172">Once hello template deployment is completed, your new cluster is ready toouse!</span></span>

> [!NOTE]
> <span data-ttu-id="de6d6-173">Pokud PowerShell nikdy použité tooadminister Azure z hello počítače, který teď používáte, musíte toodo trochu housekeeping.</span><span class="sxs-lookup"><span data-stu-id="de6d6-173">If PowerShell was never used tooadminister Azure from hello machine that you are using now, you need toodo a little housekeeping.</span></span>
> 
> 1. <span data-ttu-id="de6d6-174">Povolit skriptování spuštěním hello prostředí PowerShell [ `Set-ExecutionPolicy` ](https://technet.microsoft.com/library/hh849812.aspx) příkaz.</span><span class="sxs-lookup"><span data-stu-id="de6d6-174">Enable PowerShell scripting by running hello [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) command.</span></span> <span data-ttu-id="de6d6-175">Pro počítače, vývoj je obvykle přijatelné "neomezený" zásady.</span><span class="sxs-lookup"><span data-stu-id="de6d6-175">For development machines, "unrestricted" policy is usually acceptable.</span></span>
> 2. <span data-ttu-id="de6d6-176">Rozhodněte, jestli tooallow shromažďování diagnostických dat z příkazů prostředí Azure PowerShell a spusťte [ `Enable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619303.aspx) nebo [ `Disable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619236.aspx) podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="de6d6-176">Decide whether tooallow diagnostic data collection from Azure PowerShell commands, and run [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) or [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) as necessary.</span></span> <span data-ttu-id="de6d6-177">Tím se vyhnete nepotřebné výzvy během nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="de6d6-177">This will avoid unnecessary prompts during template deployment.</span></span>
> 
> 

<span data-ttu-id="de6d6-178">Pokud nejsou žádné chyby, přejděte toohello [portál Azure](https://portal.azure.com/) a otevřete hello skupinu prostředků, které jste nasadili.</span><span class="sxs-lookup"><span data-stu-id="de6d6-178">If there are any errors, go toohello [Azure portal](https://portal.azure.com/) and open hello resource group that you deployed to.</span></span> <span data-ttu-id="de6d6-179">Klikněte na tlačítko **všechna nastavení**, pak klikněte na tlačítko **nasazení** v okně Nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="de6d6-179">Click **All settings**, then click **Deployments** on hello settings blade.</span></span> <span data-ttu-id="de6d6-180">Selhání nasazení skupiny prostředků existuje zanechává podrobné diagnostické informace.</span><span class="sxs-lookup"><span data-stu-id="de6d6-180">A failed resource-group deployment leaves detailed diagnostic information there.</span></span>

> [!NOTE]
> <span data-ttu-id="de6d6-181">Clusterů Service Fabric vyžadují určitý počet uzlů toobe dostupnost toomaintain a zachovávají stav – odkazované tooas "údržbu kvora."</span><span class="sxs-lookup"><span data-stu-id="de6d6-181">Service Fabric clusters require a certain number of nodes toobe up toomaintain availability and preserve state - referred tooas "maintaining quorum."</span></span> <span data-ttu-id="de6d6-182">Proto není bezpečné tooshut dolů všechny hello počítače v clusteru hello Pokud jste provedli nejprve [úplného zálohování vaší stavu](service-fabric-reliable-services-backup-restore.md).</span><span class="sxs-lookup"><span data-stu-id="de6d6-182">Therefore, it is not safe tooshut down all of hello machines in hello cluster unless you have first performed a [full backup of your state](service-fabric-reliable-services-backup-restore.md).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="de6d6-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="de6d6-183">Next steps</span></span>
* [<span data-ttu-id="de6d6-184">Další informace o nastavení clusteru Service Fabric pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="de6d6-184">Learn about setting up Service Fabric cluster using hello Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
* [<span data-ttu-id="de6d6-185">Zjistěte, jak toomanage a nasazování aplikací Service Fabric pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de6d6-185">Learn how toomanage and deploy Service Fabric applications using Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
