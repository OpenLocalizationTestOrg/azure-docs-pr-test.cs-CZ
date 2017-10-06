---
title: "aaaMonitor přístup protokoly, protokoly výkonu, back-end stavu a metrik pro službu Application Gateway | Microsoft Docs"
description: "Zjistěte, jak tooenable a spravovat přístup k protokolům a protokolování výkonu pro službu Application Gateway"
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: tysonn
tags: azure-resource-manager
ms.assetid: 300628b8-8e3d-40ab-b294-3ecc5e48ef98
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: amitsriva
ms.openlocfilehash: 36ebf15c28f776158350ef8e73d617ef68e09266
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-end-health-diagnostic-logs-and-metrics-for-application-gateway"></a><span data-ttu-id="f8c94-103">Stav back-end, diagnostické protokoly a metriky pro službu Application Gateway</span><span class="sxs-lookup"><span data-stu-id="f8c94-103">Back-end health, diagnostic logs, and metrics for Application Gateway</span></span>

<span data-ttu-id="f8c94-104">Pomocí Azure Application Gateway můžete sledovat prostředky v hello následující způsoby:</span><span class="sxs-lookup"><span data-stu-id="f8c94-104">By using Azure Application Gateway, you can monitor resources in hello following ways:</span></span>

* <span data-ttu-id="f8c94-105">[Back-end stavu](#back-end-health): Application Gateway poskytuje hello schopností toomonitor hello hello stav serverů stavu v hello back endové fondy prostřednictvím hello portál Azure a pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f8c94-105">[Back-end health](#back-end-health): Application Gateway provides hello capability toomonitor hello health of hello servers in hello back-end pools through hello Azure portal and through PowerShell.</span></span> <span data-ttu-id="f8c94-106">Můžete také získat stav hello hello back endové fondy prostřednictvím hello výkonu diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="f8c94-106">You can also find hello health of hello back-end pools through hello performance diagnostic logs.</span></span>

* <span data-ttu-id="f8c94-107">[Protokoly](#diagnostic-logs): protokoly umožňují pro výkon, přístup, a další data toobe uložit, nebo využívat z prostředku pro účely monitorování.</span><span class="sxs-lookup"><span data-stu-id="f8c94-107">[Logs](#diagnostic-logs): Logs allow for performance, access, and other data toobe saved or consumed from a resource for monitoring purposes.</span></span>

* <span data-ttu-id="f8c94-108">[Metriky](#metrics): Aplikační brána má aktuálně jeden metriku.</span><span class="sxs-lookup"><span data-stu-id="f8c94-108">[Metrics](#metrics): Application Gateway currently has one metric.</span></span> <span data-ttu-id="f8c94-109">Tato metrika měří hello propustnost hello Aplikační brána v bajtech za sekundu.</span><span class="sxs-lookup"><span data-stu-id="f8c94-109">This metric measures hello throughput of hello application gateway in bytes per second.</span></span>

## <a name="back-end-health"></a><span data-ttu-id="f8c94-110">Back-end stavu</span><span class="sxs-lookup"><span data-stu-id="f8c94-110">Back-end health</span></span>

<span data-ttu-id="f8c94-111">Application Gateway poskytuje hello schopností toomonitor hello stavu jednotlivých členů fondu back-end hello prostřednictvím portálu hello, prostředí PowerShell a hello rozhraní příkazového řádku (CLI).</span><span class="sxs-lookup"><span data-stu-id="f8c94-111">Application Gateway provides hello capability toomonitor hello health of individual members of hello back-end pools through hello portal, PowerShell, and hello command-line interface (CLI).</span></span> <span data-ttu-id="f8c94-112">Můžete také vyhledat agregovaný stav souhrn back endové fondy prostřednictvím hello výkonu diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="f8c94-112">You can also find an aggregated health summary of back-end pools through hello performance diagnostic logs.</span></span> 

<span data-ttu-id="f8c94-113">Sestava stavu back-end Hello odráží výstup hello instancí hello Application Gateway stavu testu toohello back-end.</span><span class="sxs-lookup"><span data-stu-id="f8c94-113">hello back-end health report reflects hello output of hello Application Gateway health probe toohello back-end instances.</span></span> <span data-ttu-id="f8c94-114">Při zjišťování proběhlo úspěšně a hello zpět end může přijímat provoz, bude považován za v pořádku.</span><span class="sxs-lookup"><span data-stu-id="f8c94-114">When probing is successful and hello back end can receive traffic, it's considered healthy.</span></span> <span data-ttu-id="f8c94-115">Jinak považuje není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="f8c94-115">Otherwise, it's considered unhealthy.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f8c94-116">Pokud je skupina zabezpečení sítě (NSG) na podsíť aplikační bránu, otevřete rozsahy portů 65503 65534 v podsíti hello aplikační brány pro příchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="f8c94-116">If there is a network security group (NSG) on an Application Gateway subnet, open port ranges 65503-65534 on hello Application Gateway subnet for inbound traffic.</span></span> <span data-ttu-id="f8c94-117">Tyto porty jsou povinné pro toowork rozhraní API back-end stavu hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-117">These ports are required for hello back-end health API toowork.</span></span>


### <a name="view-back-end-health-through-hello-portal"></a><span data-ttu-id="f8c94-118">Zobrazit stav back-end prostřednictvím portálu hello</span><span class="sxs-lookup"><span data-stu-id="f8c94-118">View back-end health through hello portal</span></span>

<span data-ttu-id="f8c94-119">Back-end stavu hello portál, je zadán automaticky.</span><span class="sxs-lookup"><span data-stu-id="f8c94-119">In hello portal, back-end health is provided automatically.</span></span> <span data-ttu-id="f8c94-120">V existující aplikační brány, vyberte **monitorování** > **back-end stavu**.</span><span class="sxs-lookup"><span data-stu-id="f8c94-120">In an existing application gateway, select **Monitoring** > **Backend health**.</span></span> 

<span data-ttu-id="f8c94-121">Každý člen ve fondu back-end hello je uvedený na této stránce (jestli je síťový adaptér, IP nebo plně kvalifikovaný název domény).</span><span class="sxs-lookup"><span data-stu-id="f8c94-121">Each member in hello back-end pool is listed on this page (whether it's a NIC, IP, or FQDN).</span></span> <span data-ttu-id="f8c94-122">Název fondu back-end, port, název nastavení HTTP back-end a stav se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="f8c94-122">Back-end pool name, port, back-end HTTP settings name, and health status are shown.</span></span> <span data-ttu-id="f8c94-123">Platné hodnoty pro stav jsou **stavu v pořádku**, **není v pořádku**, a **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="f8c94-123">Valid values for health status are **Healthy**, **Unhealthy**, and **Unknown**.</span></span>

> [!NOTE]
> <span data-ttu-id="f8c94-124">Pokud se zobrazí back-end stav **neznámé**, ujistěte se, že přístup toohello back-end není blokován nastavením pravidlo NSG, trasy definované uživatelem (UDR) nebo vlastní DNS ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-124">If you see a back-end health status of **Unknown**, ensure that access toohello back end is not blocked by an NSG rule, a user-defined route (UDR), or a custom DNS in hello virtual network.</span></span>

![Back-end stavu][10]

### <a name="view-back-end-health-through-powershell"></a><span data-ttu-id="f8c94-126">Zobrazit stav back-end pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f8c94-126">View back-end health through PowerShell</span></span>

<span data-ttu-id="f8c94-127">Hello následující kódu PowerShell ukazuje, jak tooview stavu back-end pomocí hello `Get-AzureRmApplicationGatewayBackendHealth` rutiny:</span><span class="sxs-lookup"><span data-stu-id="f8c94-127">hello following PowerShell code shows how tooview back-end health by using hello `Get-AzureRmApplicationGatewayBackendHealth` cmdlet:</span></span>

```powershell
Get-AzureRmApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

### <a name="view-back-end-health-through-azure-cli-20"></a><span data-ttu-id="f8c94-128">Zobrazit stav back-end pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f8c94-128">View back-end health through Azure CLI 2.0</span></span>

```azurecli
az network application-gateway show-backend-health --resource-group AdatumAppGatewayRG --name AdatumAppGateway
```

### <a name="results"></a><span data-ttu-id="f8c94-129">Výsledky</span><span class="sxs-lookup"><span data-stu-id="f8c94-129">Results</span></span>

<span data-ttu-id="f8c94-130">Hello následující fragment kódu ukazuje příklad hello odpovědi:</span><span class="sxs-lookup"><span data-stu-id="f8c94-130">hello following snippet shows an example of hello response:</span></span>

```json
{
"BackendAddressPool": {
    "Id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/appGatewayBackendPool"
},
"BackendHttpSettingsCollection": [
    {
    "BackendHttpSettings": {
        "Id": "/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
    },
    "Servers": [
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        },
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        }
    ]
    }
]
}
```

## <span data-ttu-id="f8c94-131"><a name="diagnostic-logging"></a>Diagnostické protokoly</span><span class="sxs-lookup"><span data-stu-id="f8c94-131"><a name="diagnostic-logging"></a>Diagnostic logs</span></span>

<span data-ttu-id="f8c94-132">Můžete použít různé typy protokolů v Azure toomanage a řešení potíží s application Gateway.</span><span class="sxs-lookup"><span data-stu-id="f8c94-132">You can use different types of logs in Azure toomanage and troubleshoot application gateways.</span></span> <span data-ttu-id="f8c94-133">Některé z těchto protokolů můžete přistupovat prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-133">You can access some of these logs through hello portal.</span></span> <span data-ttu-id="f8c94-134">Všechny protokoly lze extrahovat z Azure Blob storage a zobrazit v různých nástrojů, jako například [analýzy protokolů](../log-analytics/log-analytics-azure-networking-analytics.md), Excel a Power BI.</span><span class="sxs-lookup"><span data-stu-id="f8c94-134">All logs can be extracted from Azure Blob storage and viewed in different tools, such as [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), Excel, and Power BI.</span></span> <span data-ttu-id="f8c94-135">Další informace o různých typech hello protokolů z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="f8c94-135">You can learn more about hello different types of logs from hello following list:</span></span>

* <span data-ttu-id="f8c94-136">**Protokol aktivit**: můžete použít [protokoly Azure aktivity](../monitoring-and-diagnostics/insights-debugging-with-events.md) (dříve označovaný jako operační protokoly a protokoly auditu) tooview všechny operace, které jsou odeslána tooyour předplatného Azure a jejich stav.</span><span class="sxs-lookup"><span data-stu-id="f8c94-136">**Activity log**: You can use [Azure activity logs](../monitoring-and-diagnostics/insights-debugging-with-events.md) (formerly known as operational logs and audit logs) tooview all operations that are submitted tooyour Azure subscription, and their status.</span></span> <span data-ttu-id="f8c94-137">Ve výchozím nastavení se shromažďují položky protokolu aktivity a lze je zobrazit v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f8c94-137">Activity log entries are collected by default, and you can view them in hello Azure portal.</span></span>
* <span data-ttu-id="f8c94-138">**Přístup k protokolu**: můžete použít tento protokol tooview Application Gateway přístupové vzorce a analýza důležité informace, včetně hello volající IP, požadovanou adresu URL, latence odpovědi, návratový kód a bajtů a odhlášení. Protokol přístupu se shromažďují každých 300 sekund.</span><span class="sxs-lookup"><span data-stu-id="f8c94-138">**Access log**: You can use this log tooview Application Gateway access patterns and analyze important information, including hello caller's IP, requested URL, response latency, return code, and bytes in and out. An access log is collected every 300 seconds.</span></span> <span data-ttu-id="f8c94-139">Tento protokol obsahuje jeden záznam za instance aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="f8c94-139">This log contains one record per instance of Application Gateway.</span></span> <span data-ttu-id="f8c94-140">instance brány aplikace Hello lze identifikovat podle vlastnosti instanceId hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-140">hello Application Gateway instance can be identified by hello instanceId property.</span></span>
* <span data-ttu-id="f8c94-141">**Výkon protokolu**: můžete použít tento tooview protokolu výkonu instance aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="f8c94-141">**Performance log**: You can use this log tooview how Application Gateway instances are performing.</span></span> <span data-ttu-id="f8c94-142">Tento protokol zaznamená informace o výkonu pro každou instanci, včetně celkový počet požadavků zpracovaných, propustnost v bajtech, celkový počet požadavků zpracovaných počet chybných požadavků a počet instancí back-end v pořádku a není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="f8c94-142">This log captures performance information for each instance, including total requests served, throughput in bytes, total requests served, failed request count, and healthy and unhealthy back-end instance count.</span></span> <span data-ttu-id="f8c94-143">Protokolu výkonu shromažďovaných každých 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="f8c94-143">A performance log is collected every 60 seconds.</span></span>
* <span data-ttu-id="f8c94-144">**Protokol brány firewall**: můžete použít tento protokolu tooview hello žádosti, které se protokolují prostřednictvím zjišťování nebo zabránění režim služby application gateway, která je konfigurovaná pomocí brány firewall webových aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-144">**Firewall log**: You can use this log tooview hello requests that are logged through either detection or prevention mode of an application gateway that is configured with hello web application firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="f8c94-145">Protokoly jsou k dispozici pouze pro prostředky nasazené v modelu nasazení Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-145">Logs are available only for resources deployed in hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="f8c94-146">Protokoly nelze použít pro prostředky v modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-146">You cannot use logs for resources in hello classic deployment model.</span></span> <span data-ttu-id="f8c94-147">Lépe porozumět hello dva modely, najdete v části hello [nasazení Resource Manager principy a nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md) článku.</span><span class="sxs-lookup"><span data-stu-id="f8c94-147">For a better understanding of hello two models, see hello [Understanding Resource Manager deployment and classic deployment](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>

<span data-ttu-id="f8c94-148">Máte tři možnosti pro ukládání protokolů:</span><span class="sxs-lookup"><span data-stu-id="f8c94-148">You have three options for storing your logs:</span></span>

* <span data-ttu-id="f8c94-149">**Účet úložiště**: účty úložiště jsou nejvhodnější pro protokoly při protokoly jsou uloženy delší dobu a zkontrolovat v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="f8c94-149">**Storage account**: Storage accounts are best used for logs when logs are stored for a longer duration and reviewed when needed.</span></span>
* <span data-ttu-id="f8c94-150">**Služba Event hubs**: Event hubs je skvělou možnost pro integraci s další informace o zabezpečení a tooget výstrahy na vaše prostředky nástroje pro správu událostí (SEIM).</span><span class="sxs-lookup"><span data-stu-id="f8c94-150">**Event hubs**: Event hubs are a great option for integrating with other security information and event management (SEIM) tools tooget alerts on your resources.</span></span>
* <span data-ttu-id="f8c94-151">**Analýza protokolu**: analýzy protokolů je nejvhodnější pro obecné sledování v reálném čase vaší aplikace nebo při prohlížení trendy.</span><span class="sxs-lookup"><span data-stu-id="f8c94-151">**Log Analytics**: Log Analytics is best used for general real-time monitoring of your application or looking at trends.</span></span>

### <a name="enable-logging-through-powershell"></a><span data-ttu-id="f8c94-152">Povolit protokolování pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f8c94-152">Enable logging through PowerShell</span></span>

<span data-ttu-id="f8c94-153">Pro každý prostředek Resource Manager je automaticky povolené protokolování aktivit.</span><span class="sxs-lookup"><span data-stu-id="f8c94-153">Activity logging is automatically enabled for every Resource Manager resource.</span></span> <span data-ttu-id="f8c94-154">Je nutné povolit přístup a toostart protokolování výkonu shromažďování dat hello k dispozici prostřednictvím tyto protokoly.</span><span class="sxs-lookup"><span data-stu-id="f8c94-154">You must enable access and performance logging toostart collecting hello data available through those logs.</span></span> <span data-ttu-id="f8c94-155">tooenable protokolování, použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f8c94-155">tooenable logging, use hello following steps:</span></span>

1. <span data-ttu-id="f8c94-156">Poznamenejte si ID prostředku účtu úložiště, které jsou uložená data protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-156">Note your storage account's resource ID, where hello log data is stored.</span></span> <span data-ttu-id="f8c94-157">Tato hodnota je ve formátu hello: /subscriptions/\<subscriptionId\>/resourceGroups/\<název skupiny prostředků\>/providers/Microsoft.Storage/storageAccounts/\<název účtu úložiště\>.</span><span class="sxs-lookup"><span data-stu-id="f8c94-157">This value is of hello form: /subscriptions/\<subscriptionId\>/resourceGroups/\<resource group name\>/providers/Microsoft.Storage/storageAccounts/\<storage account name\>.</span></span> <span data-ttu-id="f8c94-158">Můžete použít libovolný účet úložiště v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="f8c94-158">You can use any storage account in your subscription.</span></span> <span data-ttu-id="f8c94-159">Tyto informace můžete použít hello Azure toofind portálu.</span><span class="sxs-lookup"><span data-stu-id="f8c94-159">You can use hello Azure portal toofind this information.</span></span>

    ![Portál: ID prostředku pro účet úložiště](./media/application-gateway-diagnostics/diagnostics1.png)

2. <span data-ttu-id="f8c94-161">Poznamenejte si ID prostředku Aplikační brána, pro které je povoleno protokolování.</span><span class="sxs-lookup"><span data-stu-id="f8c94-161">Note your application gateway's resource ID for which logging is enabled.</span></span> <span data-ttu-id="f8c94-162">Tato hodnota je ve formátu hello: /subscriptions/\<subscriptionId\>/resourceGroups/\<název skupiny prostředků\>/providers/Microsoft.Network/applicationGateways/\<aplikační brány název\>.</span><span class="sxs-lookup"><span data-stu-id="f8c94-162">This value is of hello form: /subscriptions/\<subscriptionId\>/resourceGroups/\<resource group name\>/providers/Microsoft.Network/applicationGateways/\<application gateway name\>.</span></span> <span data-ttu-id="f8c94-163">Tyto informace můžete použít portál toofind hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-163">You can use hello portal toofind this information.</span></span>

    ![Portál: ID prostředku application Gateway.](./media/application-gateway-diagnostics/diagnostics2.png)

3. <span data-ttu-id="f8c94-165">Povolte protokolování diagnostiky pomocí hello následující rutiny prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="f8c94-165">Enable diagnostic logging by using hello following PowerShell cmdlet:</span></span>

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
><span data-ttu-id="f8c94-166">Protokoly aktivity nevyžadují, aby účet samostatného úložiště.</span><span class="sxs-lookup"><span data-stu-id="f8c94-166">Activity logs do not require a separate storage account.</span></span> <span data-ttu-id="f8c94-167">použití Hello úložiště pro přístup a protokolování výkonu způsobuje poplatky za služby.</span><span class="sxs-lookup"><span data-stu-id="f8c94-167">hello use of storage for access and performance logging incurs service charges.</span></span>

### <a name="enable-logging-through-hello-azure-portal"></a><span data-ttu-id="f8c94-168">Povolení protokolování prostřednictvím hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f8c94-168">Enable logging through hello Azure portal</span></span>

1. <span data-ttu-id="f8c94-169">V hello portálu Azure, najít prostředek a klikněte na **diagnostické protokoly**.</span><span class="sxs-lookup"><span data-stu-id="f8c94-169">In hello Azure portal, find your resource and click **Diagnostic logs**.</span></span>

   <span data-ttu-id="f8c94-170">Pro službu Application Gateway jsou k dispozici tři protokoly:</span><span class="sxs-lookup"><span data-stu-id="f8c94-170">For Application Gateway, three logs are available:</span></span>

   * <span data-ttu-id="f8c94-171">Přístup k protokolu</span><span class="sxs-lookup"><span data-stu-id="f8c94-171">Access log</span></span>
   * <span data-ttu-id="f8c94-172">Protokolu výkonu</span><span class="sxs-lookup"><span data-stu-id="f8c94-172">Performance log</span></span>
   * <span data-ttu-id="f8c94-173">Protokol brány firewall</span><span class="sxs-lookup"><span data-stu-id="f8c94-173">Firewall log</span></span>

2. <span data-ttu-id="f8c94-174">toostart shromažďování dat, klikněte na tlačítko **zapněte diagnostiku**.</span><span class="sxs-lookup"><span data-stu-id="f8c94-174">toostart collecting data, click **Turn on diagnostics**.</span></span>

   ![Zapnutí diagnostiky][1]

3. <span data-ttu-id="f8c94-176">Hello **nastavení diagnostiky** okno poskytuje hello nastavení pro hello diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="f8c94-176">hello **Diagnostics settings** blade provides hello settings for hello diagnostic logs.</span></span> <span data-ttu-id="f8c94-177">V tomto příkladu analýzy protokolů ukládá protokoly hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-177">In this example, Log Analytics stores hello logs.</span></span> <span data-ttu-id="f8c94-178">Klikněte na tlačítko **konfigurace** pod **analýzy protokolů** tooconfigure pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="f8c94-178">Click **Configure** under **Log Analytics** tooconfigure your workspace.</span></span> <span data-ttu-id="f8c94-179">Můžete také použít centrům událostí a úložiště účet toosave hello diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="f8c94-179">You can also use event hubs and a storage account toosave hello diagnostic logs.</span></span>

   ![Počáteční proces konfigurace hello][2]

4. <span data-ttu-id="f8c94-181">Zvolte existujícímu pracovnímu prostoru služby Operations Management Suite (OMS) nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="f8c94-181">Choose an existing Operations Management Suite (OMS) workspace or create a new one.</span></span> <span data-ttu-id="f8c94-182">Tento příklad používá nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="f8c94-182">This example uses an existing one.</span></span>

   ![Možnosti pro OMS pracovní prostory][3]

5. <span data-ttu-id="f8c94-184">Potvrzení hello nastavení a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f8c94-184">Confirm hello settings and click **Save**.</span></span>

   ![Okno nastavení diagnostiky se výběry][4]

### <a name="activity-log"></a><span data-ttu-id="f8c94-186">Protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="f8c94-186">Activity log</span></span>

<span data-ttu-id="f8c94-187">Ve výchozím nastavení vygeneruje Azure protokol aktivit hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-187">Azure generates hello activity log by default.</span></span> <span data-ttu-id="f8c94-188">protokoly Hello se zachovají 90 dní v úložišti Azure protokoly událostí hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-188">hello logs are preserved for 90 days in hello Azure event logs store.</span></span> <span data-ttu-id="f8c94-189">Další informace o tyto protokoly načtením hello [zobrazování událostí a protokolu aktivity](../monitoring-and-diagnostics/insights-debugging-with-events.md) článku.</span><span class="sxs-lookup"><span data-stu-id="f8c94-189">Learn more about these logs by reading hello [View events and activity log](../monitoring-and-diagnostics/insights-debugging-with-events.md) article.</span></span>

### <a name="access-log"></a><span data-ttu-id="f8c94-190">Přístup k protokolu</span><span class="sxs-lookup"><span data-stu-id="f8c94-190">Access log</span></span>

<span data-ttu-id="f8c94-191">Hello přístup protokol je generovaný pouze v případě, že jste ho povolili na každou instanci aplikace brány, podle popisu v předchozích krocích hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-191">hello access log is generated only if you've enabled it on each Application Gateway instance, as detailed in hello preceding steps.</span></span> <span data-ttu-id="f8c94-192">Hello data se ukládají v účtu úložiště hello, kterou jste zadali, pokud jste povolili protokolování hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-192">hello data is stored in hello storage account that you specified when you enabled hello logging.</span></span> <span data-ttu-id="f8c94-193">Každý přístup Application Gateway je zaznamenána ve formátu JSON, jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="f8c94-193">Each access of Application Gateway is logged in JSON format, as shown in hello following example:</span></span>


|<span data-ttu-id="f8c94-194">Hodnota</span><span class="sxs-lookup"><span data-stu-id="f8c94-194">Value</span></span>  |<span data-ttu-id="f8c94-195">Popis</span><span class="sxs-lookup"><span data-stu-id="f8c94-195">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="f8c94-196">identifikátor instanceId</span><span class="sxs-lookup"><span data-stu-id="f8c94-196">instanceId</span></span>     | <span data-ttu-id="f8c94-197">Aplikační brána instanci tohoto požadavku obsloužit hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-197">Application Gateway instance that served hello request.</span></span>        |
|<span data-ttu-id="f8c94-198">Když</span><span class="sxs-lookup"><span data-stu-id="f8c94-198">clientIP</span></span>     | <span data-ttu-id="f8c94-199">Původní IP pro požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-199">Originating IP for hello request.</span></span>        |
|<span data-ttu-id="f8c94-200">clientPort</span><span class="sxs-lookup"><span data-stu-id="f8c94-200">clientPort</span></span>     | <span data-ttu-id="f8c94-201">Výchozí port pro požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-201">Originating port for hello request.</span></span>       |
|<span data-ttu-id="f8c94-202">HttpMethod</span><span class="sxs-lookup"><span data-stu-id="f8c94-202">httpMethod</span></span>     | <span data-ttu-id="f8c94-203">Metoda HTTP používaný hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="f8c94-203">HTTP method used by hello request.</span></span>       |
|<span data-ttu-id="f8c94-204">requestUri</span><span class="sxs-lookup"><span data-stu-id="f8c94-204">requestUri</span></span>     | <span data-ttu-id="f8c94-205">Identifikátor URI hello přijatý požadavek.</span><span class="sxs-lookup"><span data-stu-id="f8c94-205">URI of hello received request.</span></span>        |
|<span data-ttu-id="f8c94-206">RequestQuery</span><span class="sxs-lookup"><span data-stu-id="f8c94-206">RequestQuery</span></span>     | <span data-ttu-id="f8c94-207">**Server směrovat**: instance fond Back-end, který vám byl zaslán požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-207">**Server-Routed**: Back-end pool instance that was sent hello request.</span></span> </br> <span data-ttu-id="f8c94-208">**X-AzureApplicationGateway-LOG-ID**: ID korelace použitý pro požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-208">**X-AzureApplicationGateway-LOG-ID**: Correlation ID used for hello request.</span></span> <span data-ttu-id="f8c94-209">Může být použité tootroubleshoot provoz problémy na hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="f8c94-209">It can be used tootroubleshoot traffic issues on hello back-end servers.</span></span> </br><span data-ttu-id="f8c94-210">**Stav serveru**: kód odpovědi HTTP, který Application Gateway získali od hello back-end.</span><span class="sxs-lookup"><span data-stu-id="f8c94-210">**SERVER-STATUS**: HTTP response code that Application Gateway received from hello back end.</span></span>       |
|<span data-ttu-id="f8c94-211">UserAgent</span><span class="sxs-lookup"><span data-stu-id="f8c94-211">UserAgent</span></span>     | <span data-ttu-id="f8c94-212">Agent u uživatele z hlavičky požadavku hello HTTP.</span><span class="sxs-lookup"><span data-stu-id="f8c94-212">User agent from hello HTTP request header.</span></span>        |
|<span data-ttu-id="f8c94-213">httpStatus</span><span class="sxs-lookup"><span data-stu-id="f8c94-213">httpStatus</span></span>     | <span data-ttu-id="f8c94-214">Stavový kód HTTP vrácená toohello klienta z aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="f8c94-214">HTTP status code returned toohello client from Application Gateway.</span></span>       |
|<span data-ttu-id="f8c94-215">httpVersion</span><span class="sxs-lookup"><span data-stu-id="f8c94-215">httpVersion</span></span>     | <span data-ttu-id="f8c94-216">Verze protokolu HTTP žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-216">HTTP version of hello request.</span></span>        |
|<span data-ttu-id="f8c94-217">ReceivedBytes</span><span class="sxs-lookup"><span data-stu-id="f8c94-217">receivedBytes</span></span>     | <span data-ttu-id="f8c94-218">Velikost paketu přijaté v bajtech.</span><span class="sxs-lookup"><span data-stu-id="f8c94-218">Size of packet received, in bytes.</span></span>        |
|<span data-ttu-id="f8c94-219">SentBytes</span><span class="sxs-lookup"><span data-stu-id="f8c94-219">sentBytes</span></span>| <span data-ttu-id="f8c94-220">Velikost paket odeslaný v bajtech.</span><span class="sxs-lookup"><span data-stu-id="f8c94-220">Size of packet sent, in bytes.</span></span>|
|<span data-ttu-id="f8c94-221">timeTaken</span><span class="sxs-lookup"><span data-stu-id="f8c94-221">timeTaken</span></span>| <span data-ttu-id="f8c94-222">Délka dobu (v milisekundách), která je potřebná pro žádost o toobe, zpracování a jeho toobe odpovědi odeslat.</span><span class="sxs-lookup"><span data-stu-id="f8c94-222">Length of time (in milliseconds) that it takes for a request toobe processed and its response toobe sent.</span></span> <span data-ttu-id="f8c94-223">Počítá se jako interval hello od času hello při Application Gateway přijímá první bajt hello doba toohello požadavku HTTP při odeslání odpovědi hello dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="f8c94-223">This is calculated as hello interval from hello time when Application Gateway receives hello first byte of an HTTP request toohello time when hello response send operation finishes.</span></span> <span data-ttu-id="f8c94-224">Je důležité, že toonote, který hello Time-Taken pole obvykle zahrnuje hello čas požadavku a odpovědi pakety hello cestách přes síť hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-224">It's important toonote that hello Time-Taken field usually includes hello time that hello request and response packets are traveling over hello network.</span></span> |
|<span data-ttu-id="f8c94-225">Protokol</span><span class="sxs-lookup"><span data-stu-id="f8c94-225">sslEnabled</span></span>| <span data-ttu-id="f8c94-226">Jestli komunikace toohello back endové fondy používat protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="f8c94-226">Whether communication toohello back-end pools used SSL.</span></span> <span data-ttu-id="f8c94-227">Platné hodnoty jsou zapnout a vypnout.</span><span class="sxs-lookup"><span data-stu-id="f8c94-227">Valid values are on and off.</span></span>|
```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/PEERINGTEST/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayAccess",
    "time": "2017-04-26T19:27:38Z",
    "category": "ApplicationGatewayAccessLog",
    "properties": {
        "instanceId": "ApplicationGatewayRole_IN_0",
        "clientIP": "191.96.249.97",
        "clientPort": 46886,
        "httpMethod": "GET",
        "requestUri": "/phpmyadmin/scripts/setup.php",
        "requestQuery": "X-AzureApplicationGateway-CACHE-HIT=0&SERVER-ROUTED=10.4.0.4&X-AzureApplicationGateway-LOG-ID=874f1f0f-6807-41c9-b7bc-f3cfa74aa0b1&SERVER-STATUS=404",
        "userAgent": "-",
        "httpStatus": 404,
        "httpVersion": "HTTP/1.0",
        "receivedBytes": 65,
        "sentBytes": 553,
        "timeTaken": 205,
        "sslEnabled": "off"
    }
}
```

### <a name="performance-log"></a><span data-ttu-id="f8c94-228">Protokolu výkonu</span><span class="sxs-lookup"><span data-stu-id="f8c94-228">Performance log</span></span>

<span data-ttu-id="f8c94-229">Hello výkonu protokol je generovaný pouze v případě, že jste povolili na každou instanci aplikace brány, podle popisu v předchozích krocích hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-229">hello performance log is generated only if you have enabled it on each Application Gateway instance, as detailed in hello preceding steps.</span></span> <span data-ttu-id="f8c94-230">Hello data se ukládají v účtu úložiště hello, kterou jste zadali, pokud jste povolili protokolování hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-230">hello data is stored in hello storage account that you specified when you enabled hello logging.</span></span> <span data-ttu-id="f8c94-231">data protokolu výkonu Hello je vygenerována v intervalech 1 minutu.</span><span class="sxs-lookup"><span data-stu-id="f8c94-231">hello performance log data is generated in 1-minute intervals.</span></span> <span data-ttu-id="f8c94-232">zaznamená se Hello následující data:</span><span class="sxs-lookup"><span data-stu-id="f8c94-232">hello following data is logged:</span></span>


|<span data-ttu-id="f8c94-233">Hodnota</span><span class="sxs-lookup"><span data-stu-id="f8c94-233">Value</span></span>  |<span data-ttu-id="f8c94-234">Popis</span><span class="sxs-lookup"><span data-stu-id="f8c94-234">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="f8c94-235">identifikátor instanceId</span><span class="sxs-lookup"><span data-stu-id="f8c94-235">instanceId</span></span>     |  <span data-ttu-id="f8c94-236">Instance brány aplikace, které výkonu je generován data.</span><span class="sxs-lookup"><span data-stu-id="f8c94-236">Application Gateway instance for which performance data is being generated.</span></span> <span data-ttu-id="f8c94-237">Pro bránu více instancí aplikace je jeden řádek pro každou instanci.</span><span class="sxs-lookup"><span data-stu-id="f8c94-237">For a multiple-instance application gateway, there is one row per instance.</span></span>        |
|<span data-ttu-id="f8c94-238">healthyHostCount</span><span class="sxs-lookup"><span data-stu-id="f8c94-238">healthyHostCount</span></span>     | <span data-ttu-id="f8c94-239">Počet pořádku hostitelích ve fondu back-end hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-239">Number of healthy hosts in hello back-end pool.</span></span>        |
|<span data-ttu-id="f8c94-240">unHealthyHostCount</span><span class="sxs-lookup"><span data-stu-id="f8c94-240">unHealthyHostCount</span></span>     | <span data-ttu-id="f8c94-241">Počet není v pořádku hostitelích ve fondu back-end hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-241">Number of unhealthy hosts in hello back-end pool.</span></span>        |
|<span data-ttu-id="f8c94-242">RequestCount</span><span class="sxs-lookup"><span data-stu-id="f8c94-242">requestCount</span></span>     | <span data-ttu-id="f8c94-243">Počet požadavků zpracovaných.</span><span class="sxs-lookup"><span data-stu-id="f8c94-243">Number of requests served.</span></span>        |
|<span data-ttu-id="f8c94-244">Čekací doba</span><span class="sxs-lookup"><span data-stu-id="f8c94-244">latency</span></span> | <span data-ttu-id="f8c94-245">Čekací doba (v milisekundách) požadavků od hello instance toohello back-end, který obsluhuje žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-245">Latency (in milliseconds) of requests from hello instance toohello back end that serves hello requests.</span></span> |
|<span data-ttu-id="f8c94-246">failedRequestCount</span><span class="sxs-lookup"><span data-stu-id="f8c94-246">failedRequestCount</span></span>| <span data-ttu-id="f8c94-247">Počet neúspěšných požadavků.</span><span class="sxs-lookup"><span data-stu-id="f8c94-247">Number of failed requests.</span></span>|
|<span data-ttu-id="f8c94-248">Propustnost</span><span class="sxs-lookup"><span data-stu-id="f8c94-248">throughput</span></span>| <span data-ttu-id="f8c94-249">Průměrná propustnost od poslední protokolu hello měřená v bajtech za sekundu.</span><span class="sxs-lookup"><span data-stu-id="f8c94-249">Average throughput since hello last log, measured in bytes per second.</span></span>|

```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayPerformance",
    "time": "2016-04-09T00:00:00Z",
    "category": "ApplicationGatewayPerformanceLog",
    "properties":
    {
        "instanceId":"ApplicationGatewayRole_IN_1",
        "healthyHostCount":"4",
        "unHealthyHostCount":"0",
        "requestCount":"185",
        "latency":"0",
        "failedRequestCount":"0",
        "throughput":"119427"
    }
}
```

> [!NOTE]
> <span data-ttu-id="f8c94-250">Latencí se počítá z hello čas, kdy hello první bajt požadavku hello HTTP přijatých toohello čas odeslání poslední bajt hello hello odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="f8c94-250">Latency is calculated from hello time when hello first byte of hello HTTP request is received toohello time when hello last byte of hello HTTP response is sent.</span></span> <span data-ttu-id="f8c94-251">Jeho hello součet hello Application Gateway doba zpracování plus hello sítě náklady toohello zpět ukončení, plus hello dobu, po kterou hello back-end trvá tooprocess hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="f8c94-251">It's hello sum of hello Application Gateway processing time plus hello network cost toohello back end, plus hello time that hello back end takes tooprocess hello request.</span></span>

### <a name="firewall-log"></a><span data-ttu-id="f8c94-252">Protokol brány firewall</span><span class="sxs-lookup"><span data-stu-id="f8c94-252">Firewall log</span></span>

<span data-ttu-id="f8c94-253">Hello brány firewall protokol je generovaný pouze v případě, že jste je povolili pro každý aplikační brány, podle popisu v předchozích krocích hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-253">hello firewall log is generated only if you have enabled it for each application gateway, as detailed in hello preceding steps.</span></span> <span data-ttu-id="f8c94-254">Tento protokol taky vyžaduje, že aby brána firewall hello webové aplikace je nakonfigurovaná na aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="f8c94-254">This log also requires that hello web application firewall is configured on an application gateway.</span></span> <span data-ttu-id="f8c94-255">Hello data se ukládají v účtu úložiště hello, kterou jste zadali, pokud jste povolili protokolování hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-255">hello data is stored in hello storage account that you specified when you enabled hello logging.</span></span> <span data-ttu-id="f8c94-256">zaznamená se Hello následující data:</span><span class="sxs-lookup"><span data-stu-id="f8c94-256">hello following data is logged:</span></span>


|<span data-ttu-id="f8c94-257">Hodnota</span><span class="sxs-lookup"><span data-stu-id="f8c94-257">Value</span></span>  |<span data-ttu-id="f8c94-258">Popis</span><span class="sxs-lookup"><span data-stu-id="f8c94-258">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="f8c94-259">identifikátor instanceId</span><span class="sxs-lookup"><span data-stu-id="f8c94-259">instanceId</span></span>     | <span data-ttu-id="f8c94-260">Instance brány aplikace, které brány firewall data jsou generována.</span><span class="sxs-lookup"><span data-stu-id="f8c94-260">Application Gateway instance for which firewall data is being generated.</span></span> <span data-ttu-id="f8c94-261">Pro bránu více instancí aplikace je jeden řádek pro každou instanci.</span><span class="sxs-lookup"><span data-stu-id="f8c94-261">For a multiple-instance application gateway, there is one row per instance.</span></span>         |
|<span data-ttu-id="f8c94-262">Když</span><span class="sxs-lookup"><span data-stu-id="f8c94-262">clientIp</span></span>     |   <span data-ttu-id="f8c94-263">Původní IP pro požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-263">Originating IP for hello request.</span></span>      |
|<span data-ttu-id="f8c94-264">clientPort</span><span class="sxs-lookup"><span data-stu-id="f8c94-264">clientPort</span></span>     |  <span data-ttu-id="f8c94-265">Výchozí port pro požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-265">Originating port for hello request.</span></span>       |
|<span data-ttu-id="f8c94-266">requestUri</span><span class="sxs-lookup"><span data-stu-id="f8c94-266">requestUri</span></span>     | <span data-ttu-id="f8c94-267">Adresa URL hello přijatý požadavek.</span><span class="sxs-lookup"><span data-stu-id="f8c94-267">URL of hello received request.</span></span>       |
|<span data-ttu-id="f8c94-268">ruleSetType</span><span class="sxs-lookup"><span data-stu-id="f8c94-268">ruleSetType</span></span>     | <span data-ttu-id="f8c94-269">Typ sady pravidel.</span><span class="sxs-lookup"><span data-stu-id="f8c94-269">Rule set type.</span></span> <span data-ttu-id="f8c94-270">k dispozici hodnota Hello je OWASP.</span><span class="sxs-lookup"><span data-stu-id="f8c94-270">hello available value is OWASP.</span></span>        |
|<span data-ttu-id="f8c94-271">ruleSetVersion</span><span class="sxs-lookup"><span data-stu-id="f8c94-271">ruleSetVersion</span></span>     | <span data-ttu-id="f8c94-272">Verze použitá sady pravidel.</span><span class="sxs-lookup"><span data-stu-id="f8c94-272">Rule set version used.</span></span> <span data-ttu-id="f8c94-273">Dostupné jsou hodnoty 2.2.9 a 3.0.</span><span class="sxs-lookup"><span data-stu-id="f8c94-273">Available values are 2.2.9 and 3.0.</span></span>     |
|<span data-ttu-id="f8c94-274">RuleId</span><span class="sxs-lookup"><span data-stu-id="f8c94-274">ruleId</span></span>     | <span data-ttu-id="f8c94-275">ID pravidla hello spuštěním události.</span><span class="sxs-lookup"><span data-stu-id="f8c94-275">Rule ID of hello triggering event.</span></span>        |
|<span data-ttu-id="f8c94-276">Zpráva</span><span class="sxs-lookup"><span data-stu-id="f8c94-276">message</span></span>     | <span data-ttu-id="f8c94-277">Uživatelsky přívětivý zpráva pro hello spuštěním události.</span><span class="sxs-lookup"><span data-stu-id="f8c94-277">User-friendly message for hello triggering event.</span></span> <span data-ttu-id="f8c94-278">Další podrobnosti najdete v části Podrobnosti o hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-278">More details are provided in hello details section.</span></span>        |
|<span data-ttu-id="f8c94-279">Akce</span><span class="sxs-lookup"><span data-stu-id="f8c94-279">action</span></span>     |  <span data-ttu-id="f8c94-280">Akce na vyžádání hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-280">Action taken on hello request.</span></span> <span data-ttu-id="f8c94-281">Dostupné hodnoty jsou blokované a povolené.</span><span class="sxs-lookup"><span data-stu-id="f8c94-281">Available values are Blocked and Allowed.</span></span>      |
|<span data-ttu-id="f8c94-282">Lokality</span><span class="sxs-lookup"><span data-stu-id="f8c94-282">site</span></span>     | <span data-ttu-id="f8c94-283">Web, pro které hello byla vygenerována protokolu.</span><span class="sxs-lookup"><span data-stu-id="f8c94-283">Site for which hello log was generated.</span></span> <span data-ttu-id="f8c94-284">V současné době pouze globální se má zobrazit, protože pravidla jsou globální.</span><span class="sxs-lookup"><span data-stu-id="f8c94-284">Currently, only Global is listed because rules are global.</span></span>|
|<span data-ttu-id="f8c94-285">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="f8c94-285">details</span></span>     | <span data-ttu-id="f8c94-286">Podrobnosti o hello spuštěním události.</span><span class="sxs-lookup"><span data-stu-id="f8c94-286">Details of hello triggering event.</span></span>        |
|<span data-ttu-id="f8c94-287">details.Message</span><span class="sxs-lookup"><span data-stu-id="f8c94-287">details.message</span></span>     | <span data-ttu-id="f8c94-288">Popis pravidla hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-288">Description of hello rule.</span></span>        |
|<span data-ttu-id="f8c94-289">details.data</span><span class="sxs-lookup"><span data-stu-id="f8c94-289">details.data</span></span>     | <span data-ttu-id="f8c94-290">Konkrétní data nalezena v žádosti o tomto pravidle odpovídající hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-290">Specific data found in request that matched hello rule.</span></span>         |
|<span data-ttu-id="f8c94-291">details.File</span><span class="sxs-lookup"><span data-stu-id="f8c94-291">details.file</span></span>     | <span data-ttu-id="f8c94-292">Konfigurační soubor, který obsahoval hello pravidlo.</span><span class="sxs-lookup"><span data-stu-id="f8c94-292">Configuration file that contained hello rule.</span></span>        |
|<span data-ttu-id="f8c94-293">details.Line</span><span class="sxs-lookup"><span data-stu-id="f8c94-293">details.line</span></span>     | <span data-ttu-id="f8c94-294">Číslo řádku v hello konfigurační soubor, který aktivoval hello událostí.</span><span class="sxs-lookup"><span data-stu-id="f8c94-294">Line number in hello configuration file that triggered hello event.</span></span>       |

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
  }
} 

```

### <a name="view-and-analyze-hello-activity-log"></a><span data-ttu-id="f8c94-295">Zobrazit a analyzovat protokol aktivit hello</span><span class="sxs-lookup"><span data-stu-id="f8c94-295">View and analyze hello activity log</span></span>

<span data-ttu-id="f8c94-296">Můžete zobrazit a analyzovat data protokolu aktivit pomocí některé z hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="f8c94-296">You can view and analyze activity log data by using any of hello following methods:</span></span>

* <span data-ttu-id="f8c94-297">**Nástroje Azure**: načtení informací z protokolu aktivit hello pomocí Azure Powershellu hello rozhraní příkazového řádku Azure, hello REST API služby Azure, nebo hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f8c94-297">**Azure tools**: Retrieve information from hello activity log through Azure PowerShell, hello Azure CLI, hello Azure REST API, or hello Azure portal.</span></span> <span data-ttu-id="f8c94-298">Podrobné pokyny pro jednotlivé metody jsou podrobně popsané na hello [operací aktivit s Resource Managerem](../azure-resource-manager/resource-group-audit.md) článku.</span><span class="sxs-lookup"><span data-stu-id="f8c94-298">Step-by-step instructions for each method are detailed in hello [Activity operations with Resource Manager](../azure-resource-manager/resource-group-audit.md) article.</span></span>
* <span data-ttu-id="f8c94-299">**Power BI**: Pokud ještě nemáte [Power BI](https://powerbi.microsoft.com/pricing) účet, můžete zkusit je zdarma.</span><span class="sxs-lookup"><span data-stu-id="f8c94-299">**Power BI**: If you don't already have a [Power BI](https://powerbi.microsoft.com/pricing) account, you can try it for free.</span></span> <span data-ttu-id="f8c94-300">Pomocí hello [obsahu protokoly aktivity Azure pack pro Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), můžete analyzovat svá data pomocí předkonfigurované řídicí panely, které můžete použít nebo přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="f8c94-300">By using hello [Azure Activity Logs content pack for Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), you can analyze your data with preconfigured dashboards that you can use as is or customize.</span></span>

### <a name="view-and-analyze-hello-access-performance-and-firewall-logs"></a><span data-ttu-id="f8c94-301">Zobrazit a analyzovat hello přístup, výkonu a protokoly brány firewall</span><span class="sxs-lookup"><span data-stu-id="f8c94-301">View and analyze hello access, performance, and firewall logs</span></span>

<span data-ttu-id="f8c94-302">Azure [analýzy protokolů](../log-analytics/log-analytics-azure-networking-analytics.md) můžete shromažďovat soubory protokolů událostí a čítače hello z vašeho účtu úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="f8c94-302">Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) can collect hello counter and event log files from your Blob storage account.</span></span> <span data-ttu-id="f8c94-303">Obsahuje vizualizace a výkonné vyhledávání možnosti tooanalyze protokolů.</span><span class="sxs-lookup"><span data-stu-id="f8c94-303">It includes visualizations and powerful search capabilities tooanalyze your logs.</span></span>

<span data-ttu-id="f8c94-304">Můžete se také připojit tooyour účet úložiště a načíst položky protokolu hello JSON pro přístup a protokoly výkonu.</span><span class="sxs-lookup"><span data-stu-id="f8c94-304">You can also connect tooyour storage account and retrieve hello JSON log entries for access and performance logs.</span></span> <span data-ttu-id="f8c94-305">Po stažení hello soubory JSON, můžete je převést tooCSV a zobrazit je v aplikaci Excel, Power BI nebo jakýkoli jiný nástroj, vizualizace dat.</span><span class="sxs-lookup"><span data-stu-id="f8c94-305">After you download hello JSON files, you can convert them tooCSV and view them in Excel, Power BI, or any other data-visualization tool.</span></span>

> [!TIP]
> <span data-ttu-id="f8c94-306">Pokud jste obeznámeni s Visual Studio a základní koncepty změna hodnoty konstanty a proměnné v jazyce C#, můžete použít hello [protokolu nástroje Převaděč](https://github.com/Azure-Samples/networking-dotnet-log-converter) dostupné z Githubu.</span><span class="sxs-lookup"><span data-stu-id="f8c94-306">If you are familiar with Visual Studio and basic concepts of changing values for constants and variables in C#, you can use hello [log converter tools](https://github.com/Azure-Samples/networking-dotnet-log-converter) available from GitHub.</span></span>
> 
> 

## <a name="metrics"></a><span data-ttu-id="f8c94-307">Metriky</span><span class="sxs-lookup"><span data-stu-id="f8c94-307">Metrics</span></span>

<span data-ttu-id="f8c94-308">Metriky jsou funkce u některých prostředků Azure, kde můžete zobrazit čítače výkonu portálu hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-308">Metrics are a feature for certain Azure resources where you can view performance counters in hello portal.</span></span> <span data-ttu-id="f8c94-309">Pro službu Application Gateway jedna metrika je k dispozici nyní.</span><span class="sxs-lookup"><span data-stu-id="f8c94-309">For Application Gateway, one metric is available now.</span></span> <span data-ttu-id="f8c94-310">Tato metrika je propustnost a zobrazí se na portálu hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-310">This metric is throughput, and you can see it in hello portal.</span></span> <span data-ttu-id="f8c94-311">Procházet tooan aplikační brány a klikněte na tlačítko **metriky**.</span><span class="sxs-lookup"><span data-stu-id="f8c94-311">Browse tooan application gateway and click **Metrics**.</span></span> <span data-ttu-id="f8c94-312">tooview hello hodnoty, vyberte propustnost v hello **dostupné metriky** části.</span><span class="sxs-lookup"><span data-stu-id="f8c94-312">tooview hello values, select throughput in hello **Available metrics** section.</span></span> <span data-ttu-id="f8c94-313">V hello následující bitové kopie vidíte příklad s filtry hello používané toodisplay hello data v různých časových rozsahů.</span><span class="sxs-lookup"><span data-stu-id="f8c94-313">In hello following image, you can see an example with hello filters that you can use toodisplay hello data in different time ranges.</span></span>

![Zobrazení metriky s filtry][5]

<span data-ttu-id="f8c94-315">toosee aktuální seznam metriky, najdete v části [podporované metriky s Azure monitorování](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="f8c94-315">toosee a current list of metrics, see [Supported metrics with Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span></span>

### <a name="alert-rules"></a><span data-ttu-id="f8c94-316">Pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="f8c94-316">Alert rules</span></span>

<span data-ttu-id="f8c94-317">Můžete spustit na základě metriky pro prostředek pravidla výstrah.</span><span class="sxs-lookup"><span data-stu-id="f8c94-317">You can start alert rules based on metrics for a resource.</span></span> <span data-ttu-id="f8c94-318">Výstrahu můžete například volat webhook, jehož nebo e-mailu správce, pokud hello propustnost hello aplikační brány je výše, níže nebo na prahovou hodnotu v zadaném období.</span><span class="sxs-lookup"><span data-stu-id="f8c94-318">For example, an alert can call a webhook or email an administrator if hello throughput of hello application gateway is above, below, or at a threshold for a specified period.</span></span>

<span data-ttu-id="f8c94-319">Následující ukázka Hello vás provede vytvořením pravidlo, které pošle e-mailu správce tooan za narušení propustnost a prahové hodnoty:</span><span class="sxs-lookup"><span data-stu-id="f8c94-319">hello following example walks you through creating an alert rule that sends an email tooan administrator after throughput breaches a threshold:</span></span>

1. <span data-ttu-id="f8c94-320">Klikněte na tlačítko **přidat metriky upozornění** tooopen hello **přidat pravidlo** okno.</span><span class="sxs-lookup"><span data-stu-id="f8c94-320">Click **Add metric alert** tooopen hello **Add rule** blade.</span></span> <span data-ttu-id="f8c94-321">Mohou být využity také toto okno, v okně metriky hello.</span><span class="sxs-lookup"><span data-stu-id="f8c94-321">You can also reach this blade from hello metrics blade.</span></span>

   ![Tlačítko "Přidat metriky upozornění"][6]

2. <span data-ttu-id="f8c94-323">Na hello **přidat pravidlo** okně vyplnit hello název, stav a upozornit části a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8c94-323">On hello **Add rule** blade, fill out hello name, condition, and notify sections, and click **OK**.</span></span>

   * <span data-ttu-id="f8c94-324">V hello **podmínku** selektor, vyberte jednu z hodnot hello čtyři: **větší než**, **větší než nebo rovna**, **menší než**, nebo  **Menší než nebo rovna hodnotě**.</span><span class="sxs-lookup"><span data-stu-id="f8c94-324">In hello **Condition** selector, select one of hello four values: **Greater than**, **Greater than or equal**, **Less than**, or **Less than or equal to**.</span></span>

   * <span data-ttu-id="f8c94-325">V hello **období** selektor, vyberte dobu od 5 minut too6 hodin.</span><span class="sxs-lookup"><span data-stu-id="f8c94-325">In hello **Period** selector, select a period from 5 minutes too6 hours.</span></span>

   * <span data-ttu-id="f8c94-326">Pokud vyberete **e-mailu vlastníci, přispěvatelé a čtenáři**, e-mailu hello může být dynamické na základě hello uživatelů, kteří mají přístup toothat prostředků.</span><span class="sxs-lookup"><span data-stu-id="f8c94-326">If you select **Email owners, contributors, and readers**, hello email can be dynamic based on hello users who have access toothat resource.</span></span> <span data-ttu-id="f8c94-327">Jinak, můžete zadat čárkami oddělený seznam uživatelů v hello **email(s) další správce** pole.</span><span class="sxs-lookup"><span data-stu-id="f8c94-327">Otherwise, you can provide a comma-separated list of users in hello **Additional administrator email(s)** box.</span></span>

   ![Přidat pravidlo okno][7]

<span data-ttu-id="f8c94-329">Pokud je prahová hodnota hello nedodržení, dorazí e-mail, který je podobný toohello, jeden v hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="f8c94-329">If hello threshold is breached, an email that's similar toohello one in hello following image arrives:</span></span>

![E-mailu pro porušení prahové hodnoty][8]

<span data-ttu-id="f8c94-331">Po vytvoření metriky upozornění se zobrazí seznam výstrah.</span><span class="sxs-lookup"><span data-stu-id="f8c94-331">A list of alerts appears after you create a metric alert.</span></span> <span data-ttu-id="f8c94-332">Poskytuje přehled o všech hello pravidla výstrah.</span><span class="sxs-lookup"><span data-stu-id="f8c94-332">It provides an overview of all hello alert rules.</span></span>

![Seznam výstrah a pravidla][9]

<span data-ttu-id="f8c94-334">toolearn Další informace o oznámení o výstrahách, najdete v části [dostávat oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="f8c94-334">toolearn more about alert notifications, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="f8c94-335">toounderstand informace o webhooky a jak je možné používat s výstrahy, navštivte [konfigurace webhook, jehož na výstrahu Azure metriky](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="f8c94-335">toounderstand more about webhooks and how you can use them with alerts, visit [Configure a webhook on an Azure metric alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8c94-336">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f8c94-336">Next steps</span></span>

* <span data-ttu-id="f8c94-337">Vizualizovat čítač a protokoly událostí pomocí [analýzy protokolů](../log-analytics/log-analytics-azure-networking-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="f8c94-337">Visualize counter and event logs by using [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md).</span></span>
* <span data-ttu-id="f8c94-338">[Aktivita Azure protokolu s Power BI vizualizovat](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) příspěvku na blogu.</span><span class="sxs-lookup"><span data-stu-id="f8c94-338">[Visualize your Azure activity log with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blog post.</span></span>
* <span data-ttu-id="f8c94-339">[Zobrazení a analýza protokolů Azure aktivity v Power BI a další](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) příspěvku na blogu.</span><span class="sxs-lookup"><span data-stu-id="f8c94-339">[View and analyze Azure activity logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blog post.</span></span>

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png
