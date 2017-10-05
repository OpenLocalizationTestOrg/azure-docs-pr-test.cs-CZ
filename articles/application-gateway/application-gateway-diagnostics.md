---
title: "Monitorovat přístup k protokolům, protokolování výkonu, back-end stavu a metrik pro službu Application Gateway | Microsoft Docs"
description: "Zjistěte, jak povolit a spravovat přístup k protokolům a protokolování výkonu pro službu Application Gateway"
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
ms.openlocfilehash: 12c252340b82aba5ee69b12db83353750782e7c5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="back-end-health-diagnostic-logs-and-metrics-for-application-gateway"></a><span data-ttu-id="a4504-103">Stav back-end, diagnostické protokoly a metriky pro službu Application Gateway</span><span class="sxs-lookup"><span data-stu-id="a4504-103">Back-end health, diagnostic logs, and metrics for Application Gateway</span></span>

<span data-ttu-id="a4504-104">Pomocí Azure Application Gateway můžete sledovat prostředky následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="a4504-104">By using Azure Application Gateway, you can monitor resources in the following ways:</span></span>

* <span data-ttu-id="a4504-105">[Back-end stavu](#back-end-health): Application Gateway poskytuje schopnost sledovat stav serverů v back endové fondy prostřednictvím portálu Azure a pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a4504-105">[Back-end health](#back-end-health): Application Gateway provides the capability to monitor the health of the servers in the back-end pools through the Azure portal and through PowerShell.</span></span> <span data-ttu-id="a4504-106">Můžete také získat stav fondu back-end prostřednictvím protokolování diagnostiky výkonu.</span><span class="sxs-lookup"><span data-stu-id="a4504-106">You can also find the health of the back-end pools through the performance diagnostic logs.</span></span>

* <span data-ttu-id="a4504-107">[Protokoly](#diagnostic-logs): protokoly umožňují pro výkon, přístupu a další data ukládání nebo používán z prostředků pro účely monitorování.</span><span class="sxs-lookup"><span data-stu-id="a4504-107">[Logs](#diagnostic-logs): Logs allow for performance, access, and other data to be saved or consumed from a resource for monitoring purposes.</span></span>

* <span data-ttu-id="a4504-108">[Metriky](#metrics): Aplikační brána má aktuálně jeden metriku.</span><span class="sxs-lookup"><span data-stu-id="a4504-108">[Metrics](#metrics): Application Gateway currently has one metric.</span></span> <span data-ttu-id="a4504-109">Tato metrika měří propustnost Aplikační brána v bajtech za sekundu.</span><span class="sxs-lookup"><span data-stu-id="a4504-109">This metric measures the throughput of the application gateway in bytes per second.</span></span>

## <a name="back-end-health"></a><span data-ttu-id="a4504-110">Back-end stavu</span><span class="sxs-lookup"><span data-stu-id="a4504-110">Back-end health</span></span>

<span data-ttu-id="a4504-111">Application Gateway poskytuje možnost pro sledování stavu jednotlivých členů fondu back-end prostřednictvím portálu, prostředí PowerShell a rozhraní příkazového řádku (CLI).</span><span class="sxs-lookup"><span data-stu-id="a4504-111">Application Gateway provides the capability to monitor the health of individual members of the back-end pools through the portal, PowerShell, and the command-line interface (CLI).</span></span> <span data-ttu-id="a4504-112">Můžete také vyhledat agregovaný stav souhrn back endové fondy prostřednictvím protokolování diagnostiky výkonu.</span><span class="sxs-lookup"><span data-stu-id="a4504-112">You can also find an aggregated health summary of back-end pools through the performance diagnostic logs.</span></span> 

<span data-ttu-id="a4504-113">Sestava stavu back-end odráží výstup test stavu Application Gateway na back-end instance.</span><span class="sxs-lookup"><span data-stu-id="a4504-113">The back-end health report reflects the output of the Application Gateway health probe to the back-end instances.</span></span> <span data-ttu-id="a4504-114">Při zjišťování úspěšné a back-end může přijímat provoz, bude považován za v pořádku.</span><span class="sxs-lookup"><span data-stu-id="a4504-114">When probing is successful and the back end can receive traffic, it's considered healthy.</span></span> <span data-ttu-id="a4504-115">Jinak považuje není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="a4504-115">Otherwise, it's considered unhealthy.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a4504-116">Pokud je skupina zabezpečení sítě (NSG) na podsíť aplikační bránu, otevřete rozsahy portů 65503 65534 na podsíť aplikační brány pro příchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="a4504-116">If there is a network security group (NSG) on an Application Gateway subnet, open port ranges 65503-65534 on the Application Gateway subnet for inbound traffic.</span></span> <span data-ttu-id="a4504-117">Tyto porty jsou povinné pro back-end stav rozhraní API pro práci.</span><span class="sxs-lookup"><span data-stu-id="a4504-117">These ports are required for the back-end health API to work.</span></span>


### <a name="view-back-end-health-through-the-portal"></a><span data-ttu-id="a4504-118">Zobrazit stav back-end prostřednictvím portálu</span><span class="sxs-lookup"><span data-stu-id="a4504-118">View back-end health through the portal</span></span>

<span data-ttu-id="a4504-119">Na portálu se automaticky poskytuje stavu back-end.</span><span class="sxs-lookup"><span data-stu-id="a4504-119">In the portal, back-end health is provided automatically.</span></span> <span data-ttu-id="a4504-120">V existující aplikační brány, vyberte **monitorování** > **back-end stavu**.</span><span class="sxs-lookup"><span data-stu-id="a4504-120">In an existing application gateway, select **Monitoring** > **Backend health**.</span></span> 

<span data-ttu-id="a4504-121">Každý člen ve fondu back-end je uvedený na této stránce (jestli je síťový adaptér, IP nebo plně kvalifikovaný název domény).</span><span class="sxs-lookup"><span data-stu-id="a4504-121">Each member in the back-end pool is listed on this page (whether it's a NIC, IP, or FQDN).</span></span> <span data-ttu-id="a4504-122">Název fondu back-end, port, název nastavení HTTP back-end a stav se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="a4504-122">Back-end pool name, port, back-end HTTP settings name, and health status are shown.</span></span> <span data-ttu-id="a4504-123">Platné hodnoty pro stav jsou **stavu v pořádku**, **není v pořádku**, a **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="a4504-123">Valid values for health status are **Healthy**, **Unhealthy**, and **Unknown**.</span></span>

> [!NOTE]
> <span data-ttu-id="a4504-124">Pokud se zobrazí back-end stav **neznámé**, ujistěte se, zda není blokován přístup k back-end nastavením pravidlo NSG, trasy definované uživatelem (UDR) nebo vlastní DNS ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="a4504-124">If you see a back-end health status of **Unknown**, ensure that access to the back end is not blocked by an NSG rule, a user-defined route (UDR), or a custom DNS in the virtual network.</span></span>

![Back-end stavu][10]

### <a name="view-back-end-health-through-powershell"></a><span data-ttu-id="a4504-126">Zobrazit stav back-end pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a4504-126">View back-end health through PowerShell</span></span>

<span data-ttu-id="a4504-127">Následující kód prostředí PowerShell ukazuje, jak zobrazit stav back-end pomocí `Get-AzureRmApplicationGatewayBackendHealth` rutiny:</span><span class="sxs-lookup"><span data-stu-id="a4504-127">The following PowerShell code shows how to view back-end health by using the `Get-AzureRmApplicationGatewayBackendHealth` cmdlet:</span></span>

```powershell
Get-AzureRmApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

### <a name="view-back-end-health-through-azure-cli-20"></a><span data-ttu-id="a4504-128">Zobrazit stav back-end pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a4504-128">View back-end health through Azure CLI 2.0</span></span>

```azurecli
az network application-gateway show-backend-health --resource-group AdatumAppGatewayRG --name AdatumAppGateway
```

### <a name="results"></a><span data-ttu-id="a4504-129">Výsledky</span><span class="sxs-lookup"><span data-stu-id="a4504-129">Results</span></span>

<span data-ttu-id="a4504-130">Následující fragment kódu ukazuje příklad odpovědi:</span><span class="sxs-lookup"><span data-stu-id="a4504-130">The following snippet shows an example of the response:</span></span>

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

## <span data-ttu-id="a4504-131"><a name="diagnostic-logging"></a>Diagnostické protokoly</span><span class="sxs-lookup"><span data-stu-id="a4504-131"><a name="diagnostic-logging"></a>Diagnostic logs</span></span>

<span data-ttu-id="a4504-132">Různé typy protokolů v Azure můžete použít ke správě a odstraňování potíží application Gateway.</span><span class="sxs-lookup"><span data-stu-id="a4504-132">You can use different types of logs in Azure to manage and troubleshoot application gateways.</span></span> <span data-ttu-id="a4504-133">Některé z těchto protokolů můžete přistupovat prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="a4504-133">You can access some of these logs through the portal.</span></span> <span data-ttu-id="a4504-134">Všechny protokoly lze extrahovat z Azure Blob storage a zobrazit v různých nástrojů, jako například [analýzy protokolů](../log-analytics/log-analytics-azure-networking-analytics.md), Excel a Power BI.</span><span class="sxs-lookup"><span data-stu-id="a4504-134">All logs can be extracted from Azure Blob storage and viewed in different tools, such as [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), Excel, and Power BI.</span></span> <span data-ttu-id="a4504-135">Se více o různých typech protokolů z následujícího seznamu:</span><span class="sxs-lookup"><span data-stu-id="a4504-135">You can learn more about the different types of logs from the following list:</span></span>

* <span data-ttu-id="a4504-136">**Protokol aktivit**: můžete použít [protokoly Azure aktivity](../monitoring-and-diagnostics/insights-debugging-with-events.md) (dříve označovaný jako operační protokoly a protokoly auditu) Chcete-li zobrazit všechny operace, které se odešlou do vašeho předplatného Azure a jejich stav.</span><span class="sxs-lookup"><span data-stu-id="a4504-136">**Activity log**: You can use [Azure activity logs](../monitoring-and-diagnostics/insights-debugging-with-events.md) (formerly known as operational logs and audit logs) to view all operations that are submitted to your Azure subscription, and their status.</span></span> <span data-ttu-id="a4504-137">Ve výchozím nastavení se shromažďují položky protokolu aktivity a lze je zobrazit na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a4504-137">Activity log entries are collected by default, and you can view them in the Azure portal.</span></span>
* <span data-ttu-id="a4504-138">**Přístup k protokolu**: můžete tento protokol zobrazit Application Gateway přístupové vzorce a analyzovat důležité informace, včetně IP volajícího, požadovanou adresu URL, latence odpovědi, návratový kód a bajtů a odhlášení. Protokol přístupu se shromažďují každých 300 sekund.</span><span class="sxs-lookup"><span data-stu-id="a4504-138">**Access log**: You can use this log to view Application Gateway access patterns and analyze important information, including the caller's IP, requested URL, response latency, return code, and bytes in and out. An access log is collected every 300 seconds.</span></span> <span data-ttu-id="a4504-139">Tento protokol obsahuje jeden záznam za instance aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a4504-139">This log contains one record per instance of Application Gateway.</span></span> <span data-ttu-id="a4504-140">Instance aplikační brány lze identifikovat podle vlastnost ID instance.</span><span class="sxs-lookup"><span data-stu-id="a4504-140">The Application Gateway instance can be identified by the instanceId property.</span></span>
* <span data-ttu-id="a4504-141">**Výkon protokolu**: Tento protokol můžete zobrazit, jak fungují instance aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a4504-141">**Performance log**: You can use this log to view how Application Gateway instances are performing.</span></span> <span data-ttu-id="a4504-142">Tento protokol zaznamená informace o výkonu pro každou instanci, včetně celkový počet požadavků zpracovaných, propustnost v bajtech, celkový počet požadavků zpracovaných počet chybných požadavků a počet instancí back-end v pořádku a není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="a4504-142">This log captures performance information for each instance, including total requests served, throughput in bytes, total requests served, failed request count, and healthy and unhealthy back-end instance count.</span></span> <span data-ttu-id="a4504-143">Protokolu výkonu shromažďovaných každých 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="a4504-143">A performance log is collected every 60 seconds.</span></span>
* <span data-ttu-id="a4504-144">**Protokol brány firewall**: Tento protokol můžete zobrazit žádosti, které se protokolují prostřednictvím zjišťování nebo zabránění režim služby application gateway, která je konfigurovaná pomocí brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="a4504-144">**Firewall log**: You can use this log to view the requests that are logged through either detection or prevention mode of an application gateway that is configured with the web application firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="a4504-145">Protokoly jsou k dispozici pouze pro prostředky nasazené v modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a4504-145">Logs are available only for resources deployed in the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="a4504-146">Protokoly nelze použít pro prostředky v modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="a4504-146">You cannot use logs for resources in the classic deployment model.</span></span> <span data-ttu-id="a4504-147">Lépe pochopit dva modely, najdete v článku [nasazení Resource Manager principy a nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md) článku.</span><span class="sxs-lookup"><span data-stu-id="a4504-147">For a better understanding of the two models, see the [Understanding Resource Manager deployment and classic deployment](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>

<span data-ttu-id="a4504-148">Máte tři možnosti pro ukládání protokolů:</span><span class="sxs-lookup"><span data-stu-id="a4504-148">You have three options for storing your logs:</span></span>

* <span data-ttu-id="a4504-149">**Účet úložiště**: účty úložiště jsou nejvhodnější pro protokoly při protokoly jsou uloženy delší dobu a zkontrolovat v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="a4504-149">**Storage account**: Storage accounts are best used for logs when logs are stored for a longer duration and reviewed when needed.</span></span>
* <span data-ttu-id="a4504-150">**Služba Event hubs**: Event hubs je skvělou možnost pro integraci s další informace o zabezpečení a získat výstrahy na vaše prostředky nástroje pro správu událostí (SEIM).</span><span class="sxs-lookup"><span data-stu-id="a4504-150">**Event hubs**: Event hubs are a great option for integrating with other security information and event management (SEIM) tools to get alerts on your resources.</span></span>
* <span data-ttu-id="a4504-151">**Analýza protokolu**: analýzy protokolů je nejvhodnější pro obecné sledování v reálném čase vaší aplikace nebo při prohlížení trendy.</span><span class="sxs-lookup"><span data-stu-id="a4504-151">**Log Analytics**: Log Analytics is best used for general real-time monitoring of your application or looking at trends.</span></span>

### <a name="enable-logging-through-powershell"></a><span data-ttu-id="a4504-152">Povolit protokolování pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a4504-152">Enable logging through PowerShell</span></span>

<span data-ttu-id="a4504-153">Pro každý prostředek Resource Manager je automaticky povolené protokolování aktivit.</span><span class="sxs-lookup"><span data-stu-id="a4504-153">Activity logging is automatically enabled for every Resource Manager resource.</span></span> <span data-ttu-id="a4504-154">Je nutné povolit přístup a výkon protokolování spustit shromažďování dat, které jsou k dispozici prostřednictvím tyto protokoly.</span><span class="sxs-lookup"><span data-stu-id="a4504-154">You must enable access and performance logging to start collecting the data available through those logs.</span></span> <span data-ttu-id="a4504-155">Chcete-li protokolování povolit, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a4504-155">To enable logging, use the following steps:</span></span>

1. <span data-ttu-id="a4504-156">Poznamenejte si ID prostředku účtu úložiště, které jsou uložená data protokolu.</span><span class="sxs-lookup"><span data-stu-id="a4504-156">Note your storage account's resource ID, where the log data is stored.</span></span> <span data-ttu-id="a4504-157">Tato hodnota je ve formátu: /subscriptions/\<subscriptionId\>/resourceGroups/\<název skupiny prostředků\>/providers/Microsoft.Storage/storageAccounts/\<název účtu úložiště\>.</span><span class="sxs-lookup"><span data-stu-id="a4504-157">This value is of the form: /subscriptions/\<subscriptionId\>/resourceGroups/\<resource group name\>/providers/Microsoft.Storage/storageAccounts/\<storage account name\>.</span></span> <span data-ttu-id="a4504-158">Můžete použít libovolný účet úložiště v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="a4504-158">You can use any storage account in your subscription.</span></span> <span data-ttu-id="a4504-159">Na portálu Azure můžete najít tyto informace.</span><span class="sxs-lookup"><span data-stu-id="a4504-159">You can use the Azure portal to find this information.</span></span>

    ![Portál: ID prostředku pro účet úložiště](./media/application-gateway-diagnostics/diagnostics1.png)

2. <span data-ttu-id="a4504-161">Poznamenejte si ID prostředku Aplikační brána, pro které je povoleno protokolování.</span><span class="sxs-lookup"><span data-stu-id="a4504-161">Note your application gateway's resource ID for which logging is enabled.</span></span> <span data-ttu-id="a4504-162">Tato hodnota je ve formátu: /subscriptions/\<subscriptionId\>/resourceGroups/\<název skupiny prostředků\>/providers/Microsoft.Network/applicationGateways/\<název brány aplikace \>.</span><span class="sxs-lookup"><span data-stu-id="a4504-162">This value is of the form: /subscriptions/\<subscriptionId\>/resourceGroups/\<resource group name\>/providers/Microsoft.Network/applicationGateways/\<application gateway name\>.</span></span> <span data-ttu-id="a4504-163">Na portálu můžete najít tyto informace.</span><span class="sxs-lookup"><span data-stu-id="a4504-163">You can use the portal to find this information.</span></span>

    ![Portál: ID prostředku application Gateway.](./media/application-gateway-diagnostics/diagnostics2.png)

3. <span data-ttu-id="a4504-165">Povolte protokolování diagnostiky pomocí následující rutiny prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="a4504-165">Enable diagnostic logging by using the following PowerShell cmdlet:</span></span>

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
><span data-ttu-id="a4504-166">Protokoly aktivity nevyžadují, aby účet samostatného úložiště.</span><span class="sxs-lookup"><span data-stu-id="a4504-166">Activity logs do not require a separate storage account.</span></span> <span data-ttu-id="a4504-167">Používání úložiště pro přístup a protokolování výkonu způsobuje poplatky za služby.</span><span class="sxs-lookup"><span data-stu-id="a4504-167">The use of storage for access and performance logging incurs service charges.</span></span>

### <a name="enable-logging-through-the-azure-portal"></a><span data-ttu-id="a4504-168">Povolit protokolování prostřednictvím portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a4504-168">Enable logging through the Azure portal</span></span>

1. <span data-ttu-id="a4504-169">Na portálu Azure najít prostředek a klikněte na tlačítko **diagnostické protokoly**.</span><span class="sxs-lookup"><span data-stu-id="a4504-169">In the Azure portal, find your resource and click **Diagnostic logs**.</span></span>

   <span data-ttu-id="a4504-170">Pro službu Application Gateway jsou k dispozici tři protokoly:</span><span class="sxs-lookup"><span data-stu-id="a4504-170">For Application Gateway, three logs are available:</span></span>

   * <span data-ttu-id="a4504-171">Přístup k protokolu</span><span class="sxs-lookup"><span data-stu-id="a4504-171">Access log</span></span>
   * <span data-ttu-id="a4504-172">Protokolu výkonu</span><span class="sxs-lookup"><span data-stu-id="a4504-172">Performance log</span></span>
   * <span data-ttu-id="a4504-173">Protokol brány firewall</span><span class="sxs-lookup"><span data-stu-id="a4504-173">Firewall log</span></span>

2. <span data-ttu-id="a4504-174">Chcete-li spustit shromažďování dat, klikněte na tlačítko **zapněte diagnostiku**.</span><span class="sxs-lookup"><span data-stu-id="a4504-174">To start collecting data, click **Turn on diagnostics**.</span></span>

   ![Zapnutí diagnostiky][1]

3. <span data-ttu-id="a4504-176">**Nastavení diagnostiky** okno obsahuje nastavení pro diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="a4504-176">The **Diagnostics settings** blade provides the settings for the diagnostic logs.</span></span> <span data-ttu-id="a4504-177">V tomto příkladu analýzy protokolů ukládá protokoly.</span><span class="sxs-lookup"><span data-stu-id="a4504-177">In this example, Log Analytics stores the logs.</span></span> <span data-ttu-id="a4504-178">Klikněte na tlačítko **konfigurace** pod **analýzy protokolů** konfigurace pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="a4504-178">Click **Configure** under **Log Analytics** to configure your workspace.</span></span> <span data-ttu-id="a4504-179">Můžete taky centrům událostí a účet úložiště pro uložení diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="a4504-179">You can also use event hubs and a storage account to save the diagnostic logs.</span></span>

   ![Spuštění procesu konfigurace][2]

4. <span data-ttu-id="a4504-181">Zvolte existujícímu pracovnímu prostoru služby Operations Management Suite (OMS) nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="a4504-181">Choose an existing Operations Management Suite (OMS) workspace or create a new one.</span></span> <span data-ttu-id="a4504-182">Tento příklad používá nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="a4504-182">This example uses an existing one.</span></span>

   ![Možnosti pro OMS pracovní prostory][3]

5. <span data-ttu-id="a4504-184">Potvrďte nastavení a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a4504-184">Confirm the settings and click **Save**.</span></span>

   ![Okno nastavení diagnostiky se výběry][4]

### <a name="activity-log"></a><span data-ttu-id="a4504-186">Protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="a4504-186">Activity log</span></span>

<span data-ttu-id="a4504-187">Ve výchozím nastavení vygeneruje Azure protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="a4504-187">Azure generates the activity log by default.</span></span> <span data-ttu-id="a4504-188">Protokoly se zachovají 90 dní v úložišti Azure protokoly událostí.</span><span class="sxs-lookup"><span data-stu-id="a4504-188">The logs are preserved for 90 days in the Azure event logs store.</span></span> <span data-ttu-id="a4504-189">Další informace o tyto protokoly načtením [zobrazování událostí a protokolu aktivity](../monitoring-and-diagnostics/insights-debugging-with-events.md) článku.</span><span class="sxs-lookup"><span data-stu-id="a4504-189">Learn more about these logs by reading the [View events and activity log](../monitoring-and-diagnostics/insights-debugging-with-events.md) article.</span></span>

### <a name="access-log"></a><span data-ttu-id="a4504-190">Přístup k protokolu</span><span class="sxs-lookup"><span data-stu-id="a4504-190">Access log</span></span>

<span data-ttu-id="a4504-191">Přístup k protokolu se vygeneruje pouze v případě, že jste ho povolili každé instance aplikační brány, jak je podrobně uvedeno v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="a4504-191">The access log is generated only if you've enabled it on each Application Gateway instance, as detailed in the preceding steps.</span></span> <span data-ttu-id="a4504-192">Data je uložený v účtu úložiště, které jste zadali při jste povolili protokolování.</span><span class="sxs-lookup"><span data-stu-id="a4504-192">The data is stored in the storage account that you specified when you enabled the logging.</span></span> <span data-ttu-id="a4504-193">Každý přístup Application Gateway je zaznamenána ve formátu JSON, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="a4504-193">Each access of Application Gateway is logged in JSON format, as shown in the following example:</span></span>


|<span data-ttu-id="a4504-194">Hodnota</span><span class="sxs-lookup"><span data-stu-id="a4504-194">Value</span></span>  |<span data-ttu-id="a4504-195">Popis</span><span class="sxs-lookup"><span data-stu-id="a4504-195">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="a4504-196">identifikátor instanceId</span><span class="sxs-lookup"><span data-stu-id="a4504-196">instanceId</span></span>     | <span data-ttu-id="a4504-197">Instance brány aplikace, který žádost zpracoval.</span><span class="sxs-lookup"><span data-stu-id="a4504-197">Application Gateway instance that served the request.</span></span>        |
|<span data-ttu-id="a4504-198">Když</span><span class="sxs-lookup"><span data-stu-id="a4504-198">clientIP</span></span>     | <span data-ttu-id="a4504-199">Původní IP pro požadavek.</span><span class="sxs-lookup"><span data-stu-id="a4504-199">Originating IP for the request.</span></span>        |
|<span data-ttu-id="a4504-200">clientPort</span><span class="sxs-lookup"><span data-stu-id="a4504-200">clientPort</span></span>     | <span data-ttu-id="a4504-201">Výchozí port pro požadavek.</span><span class="sxs-lookup"><span data-stu-id="a4504-201">Originating port for the request.</span></span>       |
|<span data-ttu-id="a4504-202">HttpMethod</span><span class="sxs-lookup"><span data-stu-id="a4504-202">httpMethod</span></span>     | <span data-ttu-id="a4504-203">Metoda HTTP používaný žádosti.</span><span class="sxs-lookup"><span data-stu-id="a4504-203">HTTP method used by the request.</span></span>       |
|<span data-ttu-id="a4504-204">requestUri</span><span class="sxs-lookup"><span data-stu-id="a4504-204">requestUri</span></span>     | <span data-ttu-id="a4504-205">Identifikátor URI přijatý požadavek.</span><span class="sxs-lookup"><span data-stu-id="a4504-205">URI of the received request.</span></span>        |
|<span data-ttu-id="a4504-206">RequestQuery</span><span class="sxs-lookup"><span data-stu-id="a4504-206">RequestQuery</span></span>     | <span data-ttu-id="a4504-207">**Server směrovat**: instance fond Back-end, který vám byl zaslán požadavek.</span><span class="sxs-lookup"><span data-stu-id="a4504-207">**Server-Routed**: Back-end pool instance that was sent the request.</span></span> </br> <span data-ttu-id="a4504-208">**X-AzureApplicationGateway-LOG-ID**: ID korelace použitou pro danou žádost.</span><span class="sxs-lookup"><span data-stu-id="a4504-208">**X-AzureApplicationGateway-LOG-ID**: Correlation ID used for the request.</span></span> <span data-ttu-id="a4504-209">Může sloužit k řešení potíží provoz na back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="a4504-209">It can be used to troubleshoot traffic issues on the back-end servers.</span></span> </br><span data-ttu-id="a4504-210">**Stav serveru**: kód odpovědi HTTP, který Application Gateway dostali z back-end.</span><span class="sxs-lookup"><span data-stu-id="a4504-210">**SERVER-STATUS**: HTTP response code that Application Gateway received from the back end.</span></span>       |
|<span data-ttu-id="a4504-211">UserAgent</span><span class="sxs-lookup"><span data-stu-id="a4504-211">UserAgent</span></span>     | <span data-ttu-id="a4504-212">Agent u uživatele z hlavičky žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="a4504-212">User agent from the HTTP request header.</span></span>        |
|<span data-ttu-id="a4504-213">httpStatus</span><span class="sxs-lookup"><span data-stu-id="a4504-213">httpStatus</span></span>     | <span data-ttu-id="a4504-214">Stavový kód HTTP vrácen do klienta z aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a4504-214">HTTP status code returned to the client from Application Gateway.</span></span>       |
|<span data-ttu-id="a4504-215">httpVersion</span><span class="sxs-lookup"><span data-stu-id="a4504-215">httpVersion</span></span>     | <span data-ttu-id="a4504-216">Verze protokolu HTTP žádosti.</span><span class="sxs-lookup"><span data-stu-id="a4504-216">HTTP version of the request.</span></span>        |
|<span data-ttu-id="a4504-217">ReceivedBytes</span><span class="sxs-lookup"><span data-stu-id="a4504-217">receivedBytes</span></span>     | <span data-ttu-id="a4504-218">Velikost paketu přijaté v bajtech.</span><span class="sxs-lookup"><span data-stu-id="a4504-218">Size of packet received, in bytes.</span></span>        |
|<span data-ttu-id="a4504-219">SentBytes</span><span class="sxs-lookup"><span data-stu-id="a4504-219">sentBytes</span></span>| <span data-ttu-id="a4504-220">Velikost paket odeslaný v bajtech.</span><span class="sxs-lookup"><span data-stu-id="a4504-220">Size of packet sent, in bytes.</span></span>|
|<span data-ttu-id="a4504-221">timeTaken</span><span class="sxs-lookup"><span data-stu-id="a4504-221">timeTaken</span></span>| <span data-ttu-id="a4504-222">Délka dobu (v milisekundách), která je potřebná pro zpracování požadavku a odpovědi na odeslání.</span><span class="sxs-lookup"><span data-stu-id="a4504-222">Length of time (in milliseconds) that it takes for a request to be processed and its response to be sent.</span></span> <span data-ttu-id="a4504-223">Počítá se jako interval od okamžiku, kdy Application Gateway přijímá první bajt požadavku HTTP na čas, kdy odpovědi odeslat dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="a4504-223">This is calculated as the interval from the time when Application Gateway receives the first byte of an HTTP request to the time when the response send operation finishes.</span></span> <span data-ttu-id="a4504-224">Je důležité si uvědomit, že pole Time-Taken obvykle zahrnuje čas, jsou pakety žádostí a odpovědí přenášeny po síti.</span><span class="sxs-lookup"><span data-stu-id="a4504-224">It's important to note that the Time-Taken field usually includes the time that the request and response packets are traveling over the network.</span></span> |
|<span data-ttu-id="a4504-225">Protokol</span><span class="sxs-lookup"><span data-stu-id="a4504-225">sslEnabled</span></span>| <span data-ttu-id="a4504-226">Jestli komunikaci s back endové fondy používat protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="a4504-226">Whether communication to the back-end pools used SSL.</span></span> <span data-ttu-id="a4504-227">Platné hodnoty jsou zapnout a vypnout.</span><span class="sxs-lookup"><span data-stu-id="a4504-227">Valid values are on and off.</span></span>|
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

### <a name="performance-log"></a><span data-ttu-id="a4504-228">Protokolu výkonu</span><span class="sxs-lookup"><span data-stu-id="a4504-228">Performance log</span></span>

<span data-ttu-id="a4504-229">V protokolu výkonu se vygeneruje pouze v případě, že jste povolili každé instance aplikační brány, jak je podrobně uvedeno v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="a4504-229">The performance log is generated only if you have enabled it on each Application Gateway instance, as detailed in the preceding steps.</span></span> <span data-ttu-id="a4504-230">Data je uložený v účtu úložiště, které jste zadali při jste povolili protokolování.</span><span class="sxs-lookup"><span data-stu-id="a4504-230">The data is stored in the storage account that you specified when you enabled the logging.</span></span> <span data-ttu-id="a4504-231">Data protokolu výkonu je generován v intervalech 1 minutu.</span><span class="sxs-lookup"><span data-stu-id="a4504-231">The performance log data is generated in 1-minute intervals.</span></span> <span data-ttu-id="a4504-232">Se protokolují tato data:</span><span class="sxs-lookup"><span data-stu-id="a4504-232">The following data is logged:</span></span>


|<span data-ttu-id="a4504-233">Hodnota</span><span class="sxs-lookup"><span data-stu-id="a4504-233">Value</span></span>  |<span data-ttu-id="a4504-234">Popis</span><span class="sxs-lookup"><span data-stu-id="a4504-234">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="a4504-235">identifikátor instanceId</span><span class="sxs-lookup"><span data-stu-id="a4504-235">instanceId</span></span>     |  <span data-ttu-id="a4504-236">Instance brány aplikace, které výkonu je generován data.</span><span class="sxs-lookup"><span data-stu-id="a4504-236">Application Gateway instance for which performance data is being generated.</span></span> <span data-ttu-id="a4504-237">Pro bránu více instancí aplikace je jeden řádek pro každou instanci.</span><span class="sxs-lookup"><span data-stu-id="a4504-237">For a multiple-instance application gateway, there is one row per instance.</span></span>        |
|<span data-ttu-id="a4504-238">healthyHostCount</span><span class="sxs-lookup"><span data-stu-id="a4504-238">healthyHostCount</span></span>     | <span data-ttu-id="a4504-239">Počet pořádku hostitelích ve fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="a4504-239">Number of healthy hosts in the back-end pool.</span></span>        |
|<span data-ttu-id="a4504-240">unHealthyHostCount</span><span class="sxs-lookup"><span data-stu-id="a4504-240">unHealthyHostCount</span></span>     | <span data-ttu-id="a4504-241">Počet není v pořádku hostitelích ve fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="a4504-241">Number of unhealthy hosts in the back-end pool.</span></span>        |
|<span data-ttu-id="a4504-242">RequestCount</span><span class="sxs-lookup"><span data-stu-id="a4504-242">requestCount</span></span>     | <span data-ttu-id="a4504-243">Počet požadavků zpracovaných.</span><span class="sxs-lookup"><span data-stu-id="a4504-243">Number of requests served.</span></span>        |
|<span data-ttu-id="a4504-244">Čekací doba</span><span class="sxs-lookup"><span data-stu-id="a4504-244">latency</span></span> | <span data-ttu-id="a4504-245">Latence (v milisekundách) požadavků z instance back end, který obsluhuje žádosti.</span><span class="sxs-lookup"><span data-stu-id="a4504-245">Latency (in milliseconds) of requests from the instance to the back end that serves the requests.</span></span> |
|<span data-ttu-id="a4504-246">failedRequestCount</span><span class="sxs-lookup"><span data-stu-id="a4504-246">failedRequestCount</span></span>| <span data-ttu-id="a4504-247">Počet neúspěšných požadavků.</span><span class="sxs-lookup"><span data-stu-id="a4504-247">Number of failed requests.</span></span>|
|<span data-ttu-id="a4504-248">Propustnost</span><span class="sxs-lookup"><span data-stu-id="a4504-248">throughput</span></span>| <span data-ttu-id="a4504-249">Průměrná propustnost od poslední protokolu měřená v bajtech za sekundu.</span><span class="sxs-lookup"><span data-stu-id="a4504-249">Average throughput since the last log, measured in bytes per second.</span></span>|

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
> <span data-ttu-id="a4504-250">Latencí se počítá z čas, kdy je první bajt požadavku HTTP přijal čas odeslání poslední bajt odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="a4504-250">Latency is calculated from the time when the first byte of the HTTP request is received to the time when the last byte of the HTTP response is sent.</span></span> <span data-ttu-id="a4504-251">Jedná se o součet bude čas zpracování aplikační brány a náklady na síť s back-end plus dobu, která back-end potřebná pro zpracování požadavku.</span><span class="sxs-lookup"><span data-stu-id="a4504-251">It's the sum of the Application Gateway processing time plus the network cost to the back end, plus the time that the back end takes to process the request.</span></span>

### <a name="firewall-log"></a><span data-ttu-id="a4504-252">Protokol brány firewall</span><span class="sxs-lookup"><span data-stu-id="a4504-252">Firewall log</span></span>

<span data-ttu-id="a4504-253">Protokol brány firewall se vygeneruje pouze v případě, že jste je povolili pro každý application gateway, jak je podrobně uvedeno v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="a4504-253">The firewall log is generated only if you have enabled it for each application gateway, as detailed in the preceding steps.</span></span> <span data-ttu-id="a4504-254">Tento protokol taky vyžaduje, aby brány firewall webových aplikací je nakonfigurovaný na aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="a4504-254">This log also requires that the web application firewall is configured on an application gateway.</span></span> <span data-ttu-id="a4504-255">Data je uložený v účtu úložiště, které jste zadali při jste povolili protokolování.</span><span class="sxs-lookup"><span data-stu-id="a4504-255">The data is stored in the storage account that you specified when you enabled the logging.</span></span> <span data-ttu-id="a4504-256">Se protokolují tato data:</span><span class="sxs-lookup"><span data-stu-id="a4504-256">The following data is logged:</span></span>


|<span data-ttu-id="a4504-257">Hodnota</span><span class="sxs-lookup"><span data-stu-id="a4504-257">Value</span></span>  |<span data-ttu-id="a4504-258">Popis</span><span class="sxs-lookup"><span data-stu-id="a4504-258">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="a4504-259">identifikátor instanceId</span><span class="sxs-lookup"><span data-stu-id="a4504-259">instanceId</span></span>     | <span data-ttu-id="a4504-260">Instance brány aplikace, které brány firewall data jsou generována.</span><span class="sxs-lookup"><span data-stu-id="a4504-260">Application Gateway instance for which firewall data is being generated.</span></span> <span data-ttu-id="a4504-261">Pro bránu více instancí aplikace je jeden řádek pro každou instanci.</span><span class="sxs-lookup"><span data-stu-id="a4504-261">For a multiple-instance application gateway, there is one row per instance.</span></span>         |
|<span data-ttu-id="a4504-262">Když</span><span class="sxs-lookup"><span data-stu-id="a4504-262">clientIp</span></span>     |   <span data-ttu-id="a4504-263">Původní IP pro požadavek.</span><span class="sxs-lookup"><span data-stu-id="a4504-263">Originating IP for the request.</span></span>      |
|<span data-ttu-id="a4504-264">clientPort</span><span class="sxs-lookup"><span data-stu-id="a4504-264">clientPort</span></span>     |  <span data-ttu-id="a4504-265">Výchozí port pro požadavek.</span><span class="sxs-lookup"><span data-stu-id="a4504-265">Originating port for the request.</span></span>       |
|<span data-ttu-id="a4504-266">requestUri</span><span class="sxs-lookup"><span data-stu-id="a4504-266">requestUri</span></span>     | <span data-ttu-id="a4504-267">Adresa URL přijatý požadavek.</span><span class="sxs-lookup"><span data-stu-id="a4504-267">URL of the received request.</span></span>       |
|<span data-ttu-id="a4504-268">ruleSetType</span><span class="sxs-lookup"><span data-stu-id="a4504-268">ruleSetType</span></span>     | <span data-ttu-id="a4504-269">Typ sady pravidel.</span><span class="sxs-lookup"><span data-stu-id="a4504-269">Rule set type.</span></span> <span data-ttu-id="a4504-270">K dispozici hodnota je OWASP.</span><span class="sxs-lookup"><span data-stu-id="a4504-270">The available value is OWASP.</span></span>        |
|<span data-ttu-id="a4504-271">ruleSetVersion</span><span class="sxs-lookup"><span data-stu-id="a4504-271">ruleSetVersion</span></span>     | <span data-ttu-id="a4504-272">Verze použitá sady pravidel.</span><span class="sxs-lookup"><span data-stu-id="a4504-272">Rule set version used.</span></span> <span data-ttu-id="a4504-273">Dostupné jsou hodnoty 2.2.9 a 3.0.</span><span class="sxs-lookup"><span data-stu-id="a4504-273">Available values are 2.2.9 and 3.0.</span></span>     |
|<span data-ttu-id="a4504-274">RuleId</span><span class="sxs-lookup"><span data-stu-id="a4504-274">ruleId</span></span>     | <span data-ttu-id="a4504-275">ID pravidla spouštěcí události.</span><span class="sxs-lookup"><span data-stu-id="a4504-275">Rule ID of the triggering event.</span></span>        |
|<span data-ttu-id="a4504-276">Zpráva</span><span class="sxs-lookup"><span data-stu-id="a4504-276">message</span></span>     | <span data-ttu-id="a4504-277">Uživatelsky přívětivý zpráva pro aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="a4504-277">User-friendly message for the triggering event.</span></span> <span data-ttu-id="a4504-278">Další podrobnosti najdete v části Podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a4504-278">More details are provided in the details section.</span></span>        |
|<span data-ttu-id="a4504-279">Akce</span><span class="sxs-lookup"><span data-stu-id="a4504-279">action</span></span>     |  <span data-ttu-id="a4504-280">Akce v žádosti.</span><span class="sxs-lookup"><span data-stu-id="a4504-280">Action taken on the request.</span></span> <span data-ttu-id="a4504-281">Dostupné hodnoty jsou blokované a povolené.</span><span class="sxs-lookup"><span data-stu-id="a4504-281">Available values are Blocked and Allowed.</span></span>      |
|<span data-ttu-id="a4504-282">Lokality</span><span class="sxs-lookup"><span data-stu-id="a4504-282">site</span></span>     | <span data-ttu-id="a4504-283">Web, pro které byla vygenerována v protokolu.</span><span class="sxs-lookup"><span data-stu-id="a4504-283">Site for which the log was generated.</span></span> <span data-ttu-id="a4504-284">V současné době pouze globální se má zobrazit, protože pravidla jsou globální.</span><span class="sxs-lookup"><span data-stu-id="a4504-284">Currently, only Global is listed because rules are global.</span></span>|
|<span data-ttu-id="a4504-285">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="a4504-285">details</span></span>     | <span data-ttu-id="a4504-286">Podrobnosti o aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="a4504-286">Details of the triggering event.</span></span>        |
|<span data-ttu-id="a4504-287">details.Message</span><span class="sxs-lookup"><span data-stu-id="a4504-287">details.message</span></span>     | <span data-ttu-id="a4504-288">Popis pravidla.</span><span class="sxs-lookup"><span data-stu-id="a4504-288">Description of the rule.</span></span>        |
|<span data-ttu-id="a4504-289">details.data</span><span class="sxs-lookup"><span data-stu-id="a4504-289">details.data</span></span>     | <span data-ttu-id="a4504-290">Konkrétní data uvedená v požadavek, který odpovídá pravidlo.</span><span class="sxs-lookup"><span data-stu-id="a4504-290">Specific data found in request that matched the rule.</span></span>         |
|<span data-ttu-id="a4504-291">details.File</span><span class="sxs-lookup"><span data-stu-id="a4504-291">details.file</span></span>     | <span data-ttu-id="a4504-292">Konfigurační soubor, který obsahoval pravidlo.</span><span class="sxs-lookup"><span data-stu-id="a4504-292">Configuration file that contained the rule.</span></span>        |
|<span data-ttu-id="a4504-293">details.Line</span><span class="sxs-lookup"><span data-stu-id="a4504-293">details.line</span></span>     | <span data-ttu-id="a4504-294">Číslo řádku v konfiguračním souboru, který spustil událost.</span><span class="sxs-lookup"><span data-stu-id="a4504-294">Line number in the configuration file that triggered the event.</span></span>       |

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

### <a name="view-and-analyze-the-activity-log"></a><span data-ttu-id="a4504-295">Zobrazení a analýza protokolu aktivit</span><span class="sxs-lookup"><span data-stu-id="a4504-295">View and analyze the activity log</span></span>

<span data-ttu-id="a4504-296">Můžete zobrazit a analyzovat data protokolu aktivit pomocí některé z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="a4504-296">You can view and analyze activity log data by using any of the following methods:</span></span>

* <span data-ttu-id="a4504-297">**Nástroje Azure**: načtení informací z protokolu činnosti pomocí prostředí Azure PowerShell, rozhraní příkazového řádku Azure, REST API služby Azure nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a4504-297">**Azure tools**: Retrieve information from the activity log through Azure PowerShell, the Azure CLI, the Azure REST API, or the Azure portal.</span></span> <span data-ttu-id="a4504-298">Podrobné pokyny pro jednotlivé metody jsou podrobně popsané na [operací aktivit s Resource Managerem](../azure-resource-manager/resource-group-audit.md) článku.</span><span class="sxs-lookup"><span data-stu-id="a4504-298">Step-by-step instructions for each method are detailed in the [Activity operations with Resource Manager](../azure-resource-manager/resource-group-audit.md) article.</span></span>
* <span data-ttu-id="a4504-299">**Power BI**: Pokud ještě nemáte [Power BI](https://powerbi.microsoft.com/pricing) účet, můžete zkusit je zdarma.</span><span class="sxs-lookup"><span data-stu-id="a4504-299">**Power BI**: If you don't already have a [Power BI](https://powerbi.microsoft.com/pricing) account, you can try it for free.</span></span> <span data-ttu-id="a4504-300">Pomocí [obsahu protokoly aktivity Azure pack pro Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), můžete analyzovat svá data pomocí předkonfigurované řídicí panely, které můžete použít nebo přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="a4504-300">By using the [Azure Activity Logs content pack for Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), you can analyze your data with preconfigured dashboards that you can use as is or customize.</span></span>

### <a name="view-and-analyze-the-access-performance-and-firewall-logs"></a><span data-ttu-id="a4504-301">Zobrazit a analyzovat přístup, výkonu a protokoly brány firewall</span><span class="sxs-lookup"><span data-stu-id="a4504-301">View and analyze the access, performance, and firewall logs</span></span>

<span data-ttu-id="a4504-302">Azure [analýzy protokolů](../log-analytics/log-analytics-azure-networking-analytics.md) z vašeho účtu úložiště objektů Blob můžete shromažďovat soubory protokolů událostí a čítače.</span><span class="sxs-lookup"><span data-stu-id="a4504-302">Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) can collect the counter and event log files from your Blob storage account.</span></span> <span data-ttu-id="a4504-303">Obsahuje vizualizace a výkonné možnosti vyhledávání k analýze protokolů.</span><span class="sxs-lookup"><span data-stu-id="a4504-303">It includes visualizations and powerful search capabilities to analyze your logs.</span></span>

<span data-ttu-id="a4504-304">Můžete také připojit k účtu úložiště a načítat položky protokolu JSON pro přístup a protokoly výkonu.</span><span class="sxs-lookup"><span data-stu-id="a4504-304">You can also connect to your storage account and retrieve the JSON log entries for access and performance logs.</span></span> <span data-ttu-id="a4504-305">Po stažení soubory JSON, můžete je převést na sdílený svazek clusteru a zobrazit je v aplikaci Excel, Power BI nebo jakýkoli jiný nástroj, vizualizace dat.</span><span class="sxs-lookup"><span data-stu-id="a4504-305">After you download the JSON files, you can convert them to CSV and view them in Excel, Power BI, or any other data-visualization tool.</span></span>

> [!TIP]
> <span data-ttu-id="a4504-306">Pokud jste obeznámeni s Visual Studio a základní koncepty změna hodnoty konstanty a proměnné v jazyce C#, můžete použít [protokolu nástroje Převaděč](https://github.com/Azure-Samples/networking-dotnet-log-converter) dostupné z Githubu.</span><span class="sxs-lookup"><span data-stu-id="a4504-306">If you are familiar with Visual Studio and basic concepts of changing values for constants and variables in C#, you can use the [log converter tools](https://github.com/Azure-Samples/networking-dotnet-log-converter) available from GitHub.</span></span>
> 
> 

## <a name="metrics"></a><span data-ttu-id="a4504-307">Metriky</span><span class="sxs-lookup"><span data-stu-id="a4504-307">Metrics</span></span>

<span data-ttu-id="a4504-308">Metriky jsou funkce u některých prostředků Azure, kde můžete zobrazit čítače výkonu v portálu.</span><span class="sxs-lookup"><span data-stu-id="a4504-308">Metrics are a feature for certain Azure resources where you can view performance counters in the portal.</span></span> <span data-ttu-id="a4504-309">Pro službu Application Gateway jedna metrika je k dispozici nyní.</span><span class="sxs-lookup"><span data-stu-id="a4504-309">For Application Gateway, one metric is available now.</span></span> <span data-ttu-id="a4504-310">Tato metrika je propustnost a zobrazí se na portálu.</span><span class="sxs-lookup"><span data-stu-id="a4504-310">This metric is throughput, and you can see it in the portal.</span></span> <span data-ttu-id="a4504-311">Přejděte do služby application gateway a klikněte na tlačítko **metriky**.</span><span class="sxs-lookup"><span data-stu-id="a4504-311">Browse to an application gateway and click **Metrics**.</span></span> <span data-ttu-id="a4504-312">Chcete-li zobrazit hodnoty, vyberte propustnost v **dostupné metriky** části.</span><span class="sxs-lookup"><span data-stu-id="a4504-312">To view the values, select throughput in the **Available metrics** section.</span></span> <span data-ttu-id="a4504-313">Na následujícím obrázku uvidíte příklad s filtry, které můžete použít k zobrazení dat v různých časových rozsahů.</span><span class="sxs-lookup"><span data-stu-id="a4504-313">In the following image, you can see an example with the filters that you can use to display the data in different time ranges.</span></span>

![Zobrazení metriky s filtry][5]

<span data-ttu-id="a4504-315">Pokud chcete zobrazit aktuální seznam metriky, najdete v části [podporované metriky s Azure monitorování](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="a4504-315">To see a current list of metrics, see [Supported metrics with Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span></span>

### <a name="alert-rules"></a><span data-ttu-id="a4504-316">Pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="a4504-316">Alert rules</span></span>

<span data-ttu-id="a4504-317">Můžete spustit na základě metriky pro prostředek pravidla výstrah.</span><span class="sxs-lookup"><span data-stu-id="a4504-317">You can start alert rules based on metrics for a resource.</span></span> <span data-ttu-id="a4504-318">Výstrahu můžete například volat webhook, jehož nebo e-mailu správce, pokud propustnost aplikační brány je výše, níže nebo na prahovou hodnotu v zadaném období.</span><span class="sxs-lookup"><span data-stu-id="a4504-318">For example, an alert can call a webhook or email an administrator if the throughput of the application gateway is above, below, or at a threshold for a specified period.</span></span>

<span data-ttu-id="a4504-319">Následující příklad vás provede vytvořením pravidlo výstrahy, která odešle e-mail na správce po narušení propustnost a prahové hodnoty:</span><span class="sxs-lookup"><span data-stu-id="a4504-319">The following example walks you through creating an alert rule that sends an email to an administrator after throughput breaches a threshold:</span></span>

1. <span data-ttu-id="a4504-320">Klikněte na tlačítko **přidat metriky upozornění** otevřete **přidat pravidlo** okno.</span><span class="sxs-lookup"><span data-stu-id="a4504-320">Click **Add metric alert** to open the **Add rule** blade.</span></span> <span data-ttu-id="a4504-321">Mohou být využity také toto okno, v okně metriky.</span><span class="sxs-lookup"><span data-stu-id="a4504-321">You can also reach this blade from the metrics blade.</span></span>

   ![Tlačítko "Přidat metriky upozornění"][6]

2. <span data-ttu-id="a4504-323">Na **přidat pravidlo** okno, zadejte název, stav a upozornit části a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a4504-323">On the **Add rule** blade, fill out the name, condition, and notify sections, and click **OK**.</span></span>

   * <span data-ttu-id="a4504-324">V **podmínku** selektor, vyberte jednu ze čtyř hodnot: **větší než**, **větší než nebo rovna**, **menší než**, nebo **Menší než nebo rovna hodnotě**.</span><span class="sxs-lookup"><span data-stu-id="a4504-324">In the **Condition** selector, select one of the four values: **Greater than**, **Greater than or equal**, **Less than**, or **Less than or equal to**.</span></span>

   * <span data-ttu-id="a4504-325">V **období** selektor, vyberte období od 5 minut do 6 hodin.</span><span class="sxs-lookup"><span data-stu-id="a4504-325">In the **Period** selector, select a period from 5 minutes to 6 hours.</span></span>

   * <span data-ttu-id="a4504-326">Pokud vyberete **e-mailu vlastníci, přispěvatelé a čtenáři**, e-mailu, může být dynamické podle uživatelů, kteří mají přístup k prostředku.</span><span class="sxs-lookup"><span data-stu-id="a4504-326">If you select **Email owners, contributors, and readers**, the email can be dynamic based on the users who have access to that resource.</span></span> <span data-ttu-id="a4504-327">Jinak, můžete zadat čárkami oddělený seznam uživatelů v **email(s) další správce** pole.</span><span class="sxs-lookup"><span data-stu-id="a4504-327">Otherwise, you can provide a comma-separated list of users in the **Additional administrator email(s)** box.</span></span>

   ![Přidat pravidlo okno][7]

<span data-ttu-id="a4504-329">Pokud je prahová hodnota nedodržení, dorazí e-mail, který je podobný tomu na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="a4504-329">If the threshold is breached, an email that's similar to the one in the following image arrives:</span></span>

![E-mailu pro porušení prahové hodnoty][8]

<span data-ttu-id="a4504-331">Po vytvoření metriky upozornění se zobrazí seznam výstrah.</span><span class="sxs-lookup"><span data-stu-id="a4504-331">A list of alerts appears after you create a metric alert.</span></span> <span data-ttu-id="a4504-332">Poskytuje přehled o všech pravidla výstrah.</span><span class="sxs-lookup"><span data-stu-id="a4504-332">It provides an overview of all the alert rules.</span></span>

![Seznam výstrah a pravidla][9]

<span data-ttu-id="a4504-334">Další informace o oznámeních výstrah najdete v tématu [dostávat oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="a4504-334">To learn more about alert notifications, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="a4504-335">Bližší informace o webhooky a jak je možné používat s výstrahy, navštivte [konfigurace webhook, jehož na výstrahu Azure metriky](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="a4504-335">To understand more about webhooks and how you can use them with alerts, visit [Configure a webhook on an Azure metric alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4504-336">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a4504-336">Next steps</span></span>

* <span data-ttu-id="a4504-337">Vizualizovat čítač a protokoly událostí pomocí [analýzy protokolů](../log-analytics/log-analytics-azure-networking-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="a4504-337">Visualize counter and event logs by using [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md).</span></span>
* <span data-ttu-id="a4504-338">[Aktivita Azure protokolu s Power BI vizualizovat](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) příspěvku na blogu.</span><span class="sxs-lookup"><span data-stu-id="a4504-338">[Visualize your Azure activity log with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blog post.</span></span>
* <span data-ttu-id="a4504-339">[Zobrazení a analýza protokolů Azure aktivity v Power BI a další](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) příspěvku na blogu.</span><span class="sxs-lookup"><span data-stu-id="a4504-339">[View and analyze Azure activity logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blog post.</span></span>

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
