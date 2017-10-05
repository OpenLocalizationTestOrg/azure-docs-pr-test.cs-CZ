---
title: "Shromažďovat výstrahy Nagios a Zabbix v OMS Log Analytics | Microsoft Docs"
description: "Nagios a Zabbix jsou nástroje pro sledování s otevřeným zdrojem. Můžete shromáždit výstrahy z těchto nástrojů do analýzy protokolů, chcete-li analyzovat, společně s výstrahy z jiných zdrojů.  Tento článek popisuje postup konfigurace agenta OMS pro Linux ke shromažďování výstrah z těchto systémů."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 0b64c32e1031e704d50aab0b38eaea41e27d134b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-oms-agent-for-linux"></a><span data-ttu-id="4a66b-105">Shromažďovat výstrahy z Nagios a Zabbix v analýzy protokolů z OMS agenta pro Linux</span><span class="sxs-lookup"><span data-stu-id="4a66b-105">Collect alerts from Nagios and Zabbix in Log Analytics from OMS Agent for Linux</span></span> 
<span data-ttu-id="4a66b-106">[Nagios](https://www.nagios.org/) a [Zabbix](http://www.zabbix.com/) jsou nástroje pro sledování s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="4a66b-106">[Nagios](https://www.nagios.org/) and [Zabbix](http://www.zabbix.com/) are open source monitoring tools.</span></span>  <span data-ttu-id="4a66b-107">Výstrahy můžete shromáždit z těchto nástrojů do analýzy protokolů, aby bylo možné analyzovat, spolu s [výstrahy z jiných zdrojů](log-analytics-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="4a66b-107">You can collect alerts from these tools into Log Analytics in order to analyze them along with [alerts from other sources](log-analytics-alerts.md).</span></span>  <span data-ttu-id="4a66b-108">Tento článek popisuje postup konfigurace agenta OMS pro Linux ke shromažďování výstrah z těchto systémů.</span><span class="sxs-lookup"><span data-stu-id="4a66b-108">This article describes how to configure the OMS Agent for Linux to collect alerts from these systems.</span></span>
 
## <a name="configure-alert-collection"></a><span data-ttu-id="4a66b-109">Konfigurace shromažďování výstrah</span><span class="sxs-lookup"><span data-stu-id="4a66b-109">Configure alert collection</span></span>

### <a name="configuring-nagios-alert-collection"></a><span data-ttu-id="4a66b-110">Konfigurace shromažďování výstrah Nagios</span><span class="sxs-lookup"><span data-stu-id="4a66b-110">Configuring Nagios alert collection</span></span>
<span data-ttu-id="4a66b-111">Proveďte následující kroky na serveru Nagios ke shromažďování výstrah.</span><span class="sxs-lookup"><span data-stu-id="4a66b-111">Perform the following steps on the Nagios server to collect alerts.</span></span>

1. <span data-ttu-id="4a66b-112">Udělit **omsagent** přístup pro čtení do souboru protokolu Nagios (tj. `/var/log/nagios/nagios.log`).</span><span class="sxs-lookup"><span data-stu-id="4a66b-112">Grant the user **omsagent** read access to the Nagios log file (i.e. `/var/log/nagios/nagios.log`).</span></span> <span data-ttu-id="4a66b-113">Za předpokladu, že soubor nagios.log je vlastníkem skupiny `nagios`, můžete přidat uživatele **omsagent** k **nagios** skupiny.</span><span class="sxs-lookup"><span data-stu-id="4a66b-113">Assuming the nagios.log file is owned by the group `nagios`, you can add the user **omsagent** to the **nagios** group.</span></span> 

    <span data-ttu-id="4a66b-114">sudo "usermod" - a -G nagios omsagent</span><span class="sxs-lookup"><span data-stu-id="4a66b-114">sudo usermod -a -G nagios omsagent</span></span>

2.  <span data-ttu-id="4a66b-115">Upravte konfigurační soubor. na (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span><span class="sxs-lookup"><span data-stu-id="4a66b-115">Modify the configuration file at (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span></span> <span data-ttu-id="4a66b-116">Ujistěte se, že následující položky jsou existuje a není komentáři se:</span><span class="sxs-lookup"><span data-stu-id="4a66b-116">Ensure the following entries are present and not commented out:</span></span>  

        <source>  
          type tail  
          #Update path to point to your nagios.log  
          path /var/log/nagios/nagios.log  
          format none  
          tag oms.nagios  
        </source>  
      
        <filter oms.nagios>  
          type filter_nagios_log  
        </filter>  

3. <span data-ttu-id="4a66b-117">Restartujte démon omsagent</span><span class="sxs-lookup"><span data-stu-id="4a66b-117">Restart the omsagent daemon</span></span>

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a><span data-ttu-id="4a66b-118">Konfigurace shromažďování výstrah Zabbix</span><span class="sxs-lookup"><span data-stu-id="4a66b-118">Configuring Zabbix alert collection</span></span>
<span data-ttu-id="4a66b-119">Shromažďovat výstrahy ze serveru Zabbix, budete muset zadat uživatele a heslo v *text vymažte, pokud*.</span><span class="sxs-lookup"><span data-stu-id="4a66b-119">To collect alerts from a Zabbix server, you need to specify a user and password in *clear text*.</span></span> <span data-ttu-id="4a66b-120">Toto není ideální, ale doporučujeme vytvořit uživateli a udělte oprávnění ke sledování onlu.</span><span class="sxs-lookup"><span data-stu-id="4a66b-120">This is not ideal, but we recommend that you create the user and grant permissions to monitor onlu.</span></span>

<span data-ttu-id="4a66b-121">Proveďte následující kroky na serveru Nagios ke shromažďování výstrah.</span><span class="sxs-lookup"><span data-stu-id="4a66b-121">Perform the following steps on the Nagios server to collect alerts.</span></span>

1. <span data-ttu-id="4a66b-122">Upravte konfigurační soubor. na (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span><span class="sxs-lookup"><span data-stu-id="4a66b-122">Modify the configuration file at (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span></span> <span data-ttu-id="4a66b-123">Zkontrolujte, zda že následující položky jsou existuje a není komentáři limitu.</span><span class="sxs-lookup"><span data-stu-id="4a66b-123">Ensure the following entries are present and not commented out.</span></span>  <span data-ttu-id="4a66b-124">Změňte hodnoty pro vaše prostředí Zabbix uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="4a66b-124">Change the user name and password to values for your Zabbix environment.</span></span>

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. <span data-ttu-id="4a66b-125">Restartujte démon omsagent</span><span class="sxs-lookup"><span data-stu-id="4a66b-125">Restart the omsagent daemon</span></span>

    <span data-ttu-id="4a66b-126">sudo dílet /opt/microsoft/omsagent/bin/service_control restartovat</span><span class="sxs-lookup"><span data-stu-id="4a66b-126">sudo sh /opt/microsoft/omsagent/bin/service_control restart</span></span>


## <a name="alert-records"></a><span data-ttu-id="4a66b-127">Výstrahy záznamů</span><span class="sxs-lookup"><span data-stu-id="4a66b-127">Alert records</span></span>
<span data-ttu-id="4a66b-128">Výstrahy záznamy můžete načíst z Nagios a Zabbix pomocí [protokolu hledání](log-analytics-log-searches.md) v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="4a66b-128">You can retrieve alert records from Nagios and Zabbix using [log searches](log-analytics-log-searches.md) in Log Analytics.</span></span>

### <a name="nagios-alert-records"></a><span data-ttu-id="4a66b-129">Výstraha Nagios záznamů</span><span class="sxs-lookup"><span data-stu-id="4a66b-129">Nagios Alert records</span></span>

<span data-ttu-id="4a66b-130">Výstrahy mají záznamy shromažďují Nagios **typ** z **výstrahy** a **SourceSystem** z **Nagios**.</span><span class="sxs-lookup"><span data-stu-id="4a66b-130">Alert records collected by Nagios have a **Type** of **Alert** and a **SourceSystem** of **Nagios**.</span></span>  <span data-ttu-id="4a66b-131">V následující tabulce mají vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4a66b-131">They have the properties in the following table.</span></span>

| <span data-ttu-id="4a66b-132">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4a66b-132">Property</span></span> | <span data-ttu-id="4a66b-133">Popis</span><span class="sxs-lookup"><span data-stu-id="4a66b-133">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="4a66b-134">Typ</span><span class="sxs-lookup"><span data-stu-id="4a66b-134">Type</span></span> |<span data-ttu-id="4a66b-135">*Výstrahy*</span><span class="sxs-lookup"><span data-stu-id="4a66b-135">*Alert*</span></span> |
| <span data-ttu-id="4a66b-136">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="4a66b-136">SourceSystem</span></span> |<span data-ttu-id="4a66b-137">*Nagios*</span><span class="sxs-lookup"><span data-stu-id="4a66b-137">*Nagios*</span></span> |
| <span data-ttu-id="4a66b-138">AlertName</span><span class="sxs-lookup"><span data-stu-id="4a66b-138">AlertName</span></span> |<span data-ttu-id="4a66b-139">Název výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4a66b-139">Name of the alert.</span></span> |
| <span data-ttu-id="4a66b-140">AlertDescription</span><span class="sxs-lookup"><span data-stu-id="4a66b-140">AlertDescription</span></span> | <span data-ttu-id="4a66b-141">Popis výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4a66b-141">Description of the alert.</span></span> |
| <span data-ttu-id="4a66b-142">AlertState</span><span class="sxs-lookup"><span data-stu-id="4a66b-142">AlertState</span></span> | <span data-ttu-id="4a66b-143">Stav služby nebo hostitele.</span><span class="sxs-lookup"><span data-stu-id="4a66b-143">Status of the service or host.</span></span><br><br><span data-ttu-id="4a66b-144">OK</span><span class="sxs-lookup"><span data-stu-id="4a66b-144">OK</span></span><br><span data-ttu-id="4a66b-145">UPOZORNĚNÍ</span><span class="sxs-lookup"><span data-stu-id="4a66b-145">WARNING</span></span><br><span data-ttu-id="4a66b-146">NAHORU</span><span class="sxs-lookup"><span data-stu-id="4a66b-146">UP</span></span><br><span data-ttu-id="4a66b-147">DOLŮ</span><span class="sxs-lookup"><span data-stu-id="4a66b-147">DOWN</span></span> |
| <span data-ttu-id="4a66b-148">Název hostitele</span><span class="sxs-lookup"><span data-stu-id="4a66b-148">HostName</span></span> | <span data-ttu-id="4a66b-149">Název hostitele, který vytvořili výstrahu.</span><span class="sxs-lookup"><span data-stu-id="4a66b-149">Name of the host that created the alert.</span></span> |
| <span data-ttu-id="4a66b-150">PriorityNumber</span><span class="sxs-lookup"><span data-stu-id="4a66b-150">PriorityNumber</span></span> | <span data-ttu-id="4a66b-151">Úroveň priority výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4a66b-151">Priority level of the alert.</span></span> |
| <span data-ttu-id="4a66b-152">StateType</span><span class="sxs-lookup"><span data-stu-id="4a66b-152">StateType</span></span> | <span data-ttu-id="4a66b-153">Typ stav výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4a66b-153">The type of state of the alert.</span></span><br><br><span data-ttu-id="4a66b-154">Konfigurace SOFT - problém, který nebyl opakovaným zkontrolováním.</span><span class="sxs-lookup"><span data-stu-id="4a66b-154">SOFT - Issue that has not been rechecked.</span></span><br><span data-ttu-id="4a66b-155">PEVNÝ - problém, který byl znovu zkontrolují zadaného počtu opakování.</span><span class="sxs-lookup"><span data-stu-id="4a66b-155">HARD - Issue that has been rechecked a specified number of times.</span></span>  |
| <span data-ttu-id="4a66b-156">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="4a66b-156">TimeGenerated</span></span> |<span data-ttu-id="4a66b-157">Datum a čas vytvoření výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4a66b-157">Date and time the alert was created.</span></span> |


### <a name="zabbix-alert-records"></a><span data-ttu-id="4a66b-158">Výstrahy záznamy Zabbix</span><span class="sxs-lookup"><span data-stu-id="4a66b-158">Zabbix alert records</span></span>
<span data-ttu-id="4a66b-159">Výstrahy mají záznamy shromažďují Zabbix **typ** z **výstrahy** a **SourceSystem** z **Zabbix**.</span><span class="sxs-lookup"><span data-stu-id="4a66b-159">Alert records collected by Zabbix have a **Type** of **Alert** and a **SourceSystem** of **Zabbix**.</span></span>  <span data-ttu-id="4a66b-160">V následující tabulce mají vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4a66b-160">They have the properties in the following table.</span></span>

| <span data-ttu-id="4a66b-161">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4a66b-161">Property</span></span> | <span data-ttu-id="4a66b-162">Popis</span><span class="sxs-lookup"><span data-stu-id="4a66b-162">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="4a66b-163">Typ</span><span class="sxs-lookup"><span data-stu-id="4a66b-163">Type</span></span> |<span data-ttu-id="4a66b-164">*Výstrahy*</span><span class="sxs-lookup"><span data-stu-id="4a66b-164">*Alert*</span></span> |
| <span data-ttu-id="4a66b-165">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="4a66b-165">SourceSystem</span></span> |<span data-ttu-id="4a66b-166">*Zabbix*</span><span class="sxs-lookup"><span data-stu-id="4a66b-166">*Zabbix*</span></span> |
| <span data-ttu-id="4a66b-167">AlertName</span><span class="sxs-lookup"><span data-stu-id="4a66b-167">AlertName</span></span> | <span data-ttu-id="4a66b-168">Název výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4a66b-168">Name of the alert.</span></span> |
| <span data-ttu-id="4a66b-169">AlertPriority</span><span class="sxs-lookup"><span data-stu-id="4a66b-169">AlertPriority</span></span> | <span data-ttu-id="4a66b-170">Závažnost výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4a66b-170">Severity of the alert.</span></span><br><br><span data-ttu-id="4a66b-171">není klasifikovaný</span><span class="sxs-lookup"><span data-stu-id="4a66b-171">not classified</span></span><br><span data-ttu-id="4a66b-172">Informace o</span><span class="sxs-lookup"><span data-stu-id="4a66b-172">information</span></span><br><span data-ttu-id="4a66b-173">Upozornění</span><span class="sxs-lookup"><span data-stu-id="4a66b-173">warning</span></span><br><span data-ttu-id="4a66b-174">Průměr</span><span class="sxs-lookup"><span data-stu-id="4a66b-174">average</span></span><br><span data-ttu-id="4a66b-175">Vysoká</span><span class="sxs-lookup"><span data-stu-id="4a66b-175">high</span></span><br><span data-ttu-id="4a66b-176">po havárii</span><span class="sxs-lookup"><span data-stu-id="4a66b-176">disaster</span></span>  |
| <span data-ttu-id="4a66b-177">AlertState</span><span class="sxs-lookup"><span data-stu-id="4a66b-177">AlertState</span></span> | <span data-ttu-id="4a66b-178">Stav výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4a66b-178">State of the alert.</span></span><br><br><span data-ttu-id="4a66b-179">0 – stav je aktuální.</span><span class="sxs-lookup"><span data-stu-id="4a66b-179">0 - State is up to date.</span></span><br><span data-ttu-id="4a66b-180">1 - stav není znám.</span><span class="sxs-lookup"><span data-stu-id="4a66b-180">1 - State is unknown.</span></span>  |
| <span data-ttu-id="4a66b-181">AlertTypeNumber</span><span class="sxs-lookup"><span data-stu-id="4a66b-181">AlertTypeNumber</span></span> | <span data-ttu-id="4a66b-182">Určuje, zda výstraha může vygenerovat více událostí problém.</span><span class="sxs-lookup"><span data-stu-id="4a66b-182">Specifies whether alert can generate multiple problem events.</span></span><br><br><span data-ttu-id="4a66b-183">0 – stav je aktuální.</span><span class="sxs-lookup"><span data-stu-id="4a66b-183">0 - State is up to date.</span></span><br><span data-ttu-id="4a66b-184">1 - stav není znám.</span><span class="sxs-lookup"><span data-stu-id="4a66b-184">1 - State is unknown.</span></span>    |
| <span data-ttu-id="4a66b-185">Komentáře</span><span class="sxs-lookup"><span data-stu-id="4a66b-185">Comments</span></span> | <span data-ttu-id="4a66b-186">Další poznámky pro výstrahu.</span><span class="sxs-lookup"><span data-stu-id="4a66b-186">Additional comments for alert.</span></span> |
| <span data-ttu-id="4a66b-187">Název hostitele</span><span class="sxs-lookup"><span data-stu-id="4a66b-187">HostName</span></span> | <span data-ttu-id="4a66b-188">Název hostitele, který vytvořili výstrahu.</span><span class="sxs-lookup"><span data-stu-id="4a66b-188">Name of the host that created the alert.</span></span> |
| <span data-ttu-id="4a66b-189">PriorityNumber</span><span class="sxs-lookup"><span data-stu-id="4a66b-189">PriorityNumber</span></span> | <span data-ttu-id="4a66b-190">Hodnota označující závažnost výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4a66b-190">Value indicating severity of the alert.</span></span><br><br><span data-ttu-id="4a66b-191">0 – nezařazených</span><span class="sxs-lookup"><span data-stu-id="4a66b-191">0 - not classified</span></span><br><span data-ttu-id="4a66b-192">1 - informace</span><span class="sxs-lookup"><span data-stu-id="4a66b-192">1 - information</span></span><br><span data-ttu-id="4a66b-193">2 – upozornění</span><span class="sxs-lookup"><span data-stu-id="4a66b-193">2 - warning</span></span><br><span data-ttu-id="4a66b-194">3 – průměr</span><span class="sxs-lookup"><span data-stu-id="4a66b-194">3 - average</span></span><br><span data-ttu-id="4a66b-195">4 – vysoká</span><span class="sxs-lookup"><span data-stu-id="4a66b-195">4 - high</span></span><br><span data-ttu-id="4a66b-196">5 - po havárii</span><span class="sxs-lookup"><span data-stu-id="4a66b-196">5 - disaster</span></span> |
| <span data-ttu-id="4a66b-197">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="4a66b-197">TimeGenerated</span></span> |<span data-ttu-id="4a66b-198">Datum a čas vytvoření výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4a66b-198">Date and time the alert was created.</span></span> |
| <span data-ttu-id="4a66b-199">TimeLastModified</span><span class="sxs-lookup"><span data-stu-id="4a66b-199">TimeLastModified</span></span> |<span data-ttu-id="4a66b-200">Datum a čas, kdy byl naposledy změněn stav výstrahy.</span><span class="sxs-lookup"><span data-stu-id="4a66b-200">Date and time the state of the alert was last changed.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="4a66b-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4a66b-201">Next steps</span></span>
* <span data-ttu-id="4a66b-202">Další informace o [výstrahy](log-analytics-alerts.md) v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="4a66b-202">Learn about [alerts](log-analytics-alerts.md) in Log Analytics.</span></span>
* <span data-ttu-id="4a66b-203">Další informace o [protokolu hledání](log-analytics-log-searches.md) analyzovat data shromážděná ze zdrojů dat a řešení.</span><span class="sxs-lookup"><span data-stu-id="4a66b-203">Learn about [log searches](log-analytics-log-searches.md) to analyze the data collected from data sources and solutions.</span></span> 
