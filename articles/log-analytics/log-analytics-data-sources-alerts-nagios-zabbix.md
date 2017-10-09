---
title: "aaaCollect Nagios a Zabbix výstrahy v OMS Log Analytics | Microsoft Docs"
description: "Nagios a Zabbix jsou nástroje pro sledování s otevřeným zdrojem. Vám může shromažďování výstrahy z těchto nástrojů do analýzy protokolů v pořadí tooanalyze je spolu s výstrahy z jiných zdrojů.  Tento článek popisuje, jak tooconfigure hello OMS agenta pro Linux toocollect výstrahy z těchto systémů."
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
ms.openlocfilehash: 23e2252e4fed8bc87baec063694a8472ca84220d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-oms-agent-for-linux"></a><span data-ttu-id="b0022-105">Shromažďovat výstrahy z Nagios a Zabbix v analýzy protokolů z OMS agenta pro Linux</span><span class="sxs-lookup"><span data-stu-id="b0022-105">Collect alerts from Nagios and Zabbix in Log Analytics from OMS Agent for Linux</span></span> 
<span data-ttu-id="b0022-106">[Nagios](https://www.nagios.org/) a [Zabbix](http://www.zabbix.com/) jsou nástroje pro sledování s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="b0022-106">[Nagios](https://www.nagios.org/) and [Zabbix](http://www.zabbix.com/) are open source monitoring tools.</span></span>  <span data-ttu-id="b0022-107">Vám může shromáždit výstrahy z těchto nástrojů do analýzy protokolů v tooanalyze pořadí je spolu s [výstrahy z jiných zdrojů](log-analytics-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="b0022-107">You can collect alerts from these tools into Log Analytics in order tooanalyze them along with [alerts from other sources](log-analytics-alerts.md).</span></span>  <span data-ttu-id="b0022-108">Tento článek popisuje, jak tooconfigure hello OMS agenta pro Linux toocollect výstrahy z těchto systémů.</span><span class="sxs-lookup"><span data-stu-id="b0022-108">This article describes how tooconfigure hello OMS Agent for Linux toocollect alerts from these systems.</span></span>
 
## <a name="configure-alert-collection"></a><span data-ttu-id="b0022-109">Konfigurace shromažďování výstrah</span><span class="sxs-lookup"><span data-stu-id="b0022-109">Configure alert collection</span></span>

### <a name="configuring-nagios-alert-collection"></a><span data-ttu-id="b0022-110">Konfigurace shromažďování výstrah Nagios</span><span class="sxs-lookup"><span data-stu-id="b0022-110">Configuring Nagios alert collection</span></span>
<span data-ttu-id="b0022-111">Proveďte následující kroky na výstrahy toocollect serveru Nagios hello hello.</span><span class="sxs-lookup"><span data-stu-id="b0022-111">Perform hello following steps on hello Nagios server toocollect alerts.</span></span>

1. <span data-ttu-id="b0022-112">Udělení hello uživatele **omsagent** soubor protokolu Nagios toohello přístup pro čtení (tj. `/var/log/nagios/nagios.log`).</span><span class="sxs-lookup"><span data-stu-id="b0022-112">Grant hello user **omsagent** read access toohello Nagios log file (i.e. `/var/log/nagios/nagios.log`).</span></span> <span data-ttu-id="b0022-113">Za předpokladu, že soubor nagios.log hello je vlastníkem skupiny hello `nagios`, můžete přidat uživatele hello **omsagent** toohello **nagios** skupiny.</span><span class="sxs-lookup"><span data-stu-id="b0022-113">Assuming hello nagios.log file is owned by hello group `nagios`, you can add hello user **omsagent** toohello **nagios** group.</span></span> 

    <span data-ttu-id="b0022-114">sudo "usermod" - a -G nagios omsagent</span><span class="sxs-lookup"><span data-stu-id="b0022-114">sudo usermod -a -G nagios omsagent</span></span>

2.  <span data-ttu-id="b0022-115">Upravte konfigurační soubor hello na (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span><span class="sxs-lookup"><span data-stu-id="b0022-115">Modify hello configuration file at (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span></span> <span data-ttu-id="b0022-116">Zajistěte, aby hello následující položky jsou existuje a není komentáři se:</span><span class="sxs-lookup"><span data-stu-id="b0022-116">Ensure hello following entries are present and not commented out:</span></span>  

        <source>  
          type tail  
          #Update path toopoint tooyour nagios.log  
          path /var/log/nagios/nagios.log  
          format none  
          tag oms.nagios  
        </source>  
      
        <filter oms.nagios>  
          type filter_nagios_log  
        </filter>  

3. <span data-ttu-id="b0022-117">Restartujte démon omsagent hello</span><span class="sxs-lookup"><span data-stu-id="b0022-117">Restart hello omsagent daemon</span></span>

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a><span data-ttu-id="b0022-118">Konfigurace shromažďování výstrah Zabbix</span><span class="sxs-lookup"><span data-stu-id="b0022-118">Configuring Zabbix alert collection</span></span>
<span data-ttu-id="b0022-119">toocollect výstrahy ze serveru Zabbix, budete potřebovat toospecify uživatele a heslo v *text vymažte, pokud*.</span><span class="sxs-lookup"><span data-stu-id="b0022-119">toocollect alerts from a Zabbix server, you need toospecify a user and password in *clear text*.</span></span> <span data-ttu-id="b0022-120">Toto není ideální, ale doporučujeme vytvořit hello uživatele a udělit oprávnění toomonitor onlu.</span><span class="sxs-lookup"><span data-stu-id="b0022-120">This is not ideal, but we recommend that you create hello user and grant permissions toomonitor onlu.</span></span>

<span data-ttu-id="b0022-121">Proveďte následující kroky na výstrahy toocollect serveru Nagios hello hello.</span><span class="sxs-lookup"><span data-stu-id="b0022-121">Perform hello following steps on hello Nagios server toocollect alerts.</span></span>

1. <span data-ttu-id="b0022-122">Upravte konfigurační soubor hello na (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span><span class="sxs-lookup"><span data-stu-id="b0022-122">Modify hello configuration file at (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span></span> <span data-ttu-id="b0022-123">Zkontrolujte, zda hello následující položky jsou existuje a není komentáři limitu.  Změnit toovalues jméno a heslo uživatele hello prostředí Zabbix.</span><span class="sxs-lookup"><span data-stu-id="b0022-123">Ensure hello following entries are present and not commented out.  Change hello user name and password toovalues for your Zabbix environment.</span></span>

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. <span data-ttu-id="b0022-124">Restartujte démon omsagent hello</span><span class="sxs-lookup"><span data-stu-id="b0022-124">Restart hello omsagent daemon</span></span>

    <span data-ttu-id="b0022-125">sudo dílet /opt/microsoft/omsagent/bin/service_control restartovat</span><span class="sxs-lookup"><span data-stu-id="b0022-125">sudo sh /opt/microsoft/omsagent/bin/service_control restart</span></span>


## <a name="alert-records"></a><span data-ttu-id="b0022-126">Výstrahy záznamů</span><span class="sxs-lookup"><span data-stu-id="b0022-126">Alert records</span></span>
<span data-ttu-id="b0022-127">Výstrahy záznamy můžete načíst z Nagios a Zabbix pomocí [protokolu hledání](log-analytics-log-searches.md) v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="b0022-127">You can retrieve alert records from Nagios and Zabbix using [log searches](log-analytics-log-searches.md) in Log Analytics.</span></span>

### <a name="nagios-alert-records"></a><span data-ttu-id="b0022-128">Výstraha Nagios záznamů</span><span class="sxs-lookup"><span data-stu-id="b0022-128">Nagios Alert records</span></span>

<span data-ttu-id="b0022-129">Výstrahy mají záznamy shromažďují Nagios **typ** z **výstrahy** a **SourceSystem** z **Nagios**.</span><span class="sxs-lookup"><span data-stu-id="b0022-129">Alert records collected by Nagios have a **Type** of **Alert** and a **SourceSystem** of **Nagios**.</span></span>  <span data-ttu-id="b0022-130">V následující tabulce hello mají vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="b0022-130">They have hello properties in hello following table.</span></span>

| <span data-ttu-id="b0022-131">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b0022-131">Property</span></span> | <span data-ttu-id="b0022-132">Popis</span><span class="sxs-lookup"><span data-stu-id="b0022-132">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b0022-133">Typ</span><span class="sxs-lookup"><span data-stu-id="b0022-133">Type</span></span> |<span data-ttu-id="b0022-134">*Výstrahy*</span><span class="sxs-lookup"><span data-stu-id="b0022-134">*Alert*</span></span> |
| <span data-ttu-id="b0022-135">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="b0022-135">SourceSystem</span></span> |<span data-ttu-id="b0022-136">*Nagios*</span><span class="sxs-lookup"><span data-stu-id="b0022-136">*Nagios*</span></span> |
| <span data-ttu-id="b0022-137">AlertName</span><span class="sxs-lookup"><span data-stu-id="b0022-137">AlertName</span></span> |<span data-ttu-id="b0022-138">Název výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="b0022-138">Name of hello alert.</span></span> |
| <span data-ttu-id="b0022-139">AlertDescription</span><span class="sxs-lookup"><span data-stu-id="b0022-139">AlertDescription</span></span> | <span data-ttu-id="b0022-140">Popis výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="b0022-140">Description of hello alert.</span></span> |
| <span data-ttu-id="b0022-141">AlertState</span><span class="sxs-lookup"><span data-stu-id="b0022-141">AlertState</span></span> | <span data-ttu-id="b0022-142">Stav služby hello nebo hostitele.</span><span class="sxs-lookup"><span data-stu-id="b0022-142">Status of hello service or host.</span></span><br><br><span data-ttu-id="b0022-143">OK</span><span class="sxs-lookup"><span data-stu-id="b0022-143">OK</span></span><br><span data-ttu-id="b0022-144">UPOZORNĚNÍ</span><span class="sxs-lookup"><span data-stu-id="b0022-144">WARNING</span></span><br><span data-ttu-id="b0022-145">NAHORU</span><span class="sxs-lookup"><span data-stu-id="b0022-145">UP</span></span><br><span data-ttu-id="b0022-146">DOLŮ</span><span class="sxs-lookup"><span data-stu-id="b0022-146">DOWN</span></span> |
| <span data-ttu-id="b0022-147">Název hostitele</span><span class="sxs-lookup"><span data-stu-id="b0022-147">HostName</span></span> | <span data-ttu-id="b0022-148">Název hostitele hello, který vytvořili výstrahu hello.</span><span class="sxs-lookup"><span data-stu-id="b0022-148">Name of hello host that created hello alert.</span></span> |
| <span data-ttu-id="b0022-149">PriorityNumber</span><span class="sxs-lookup"><span data-stu-id="b0022-149">PriorityNumber</span></span> | <span data-ttu-id="b0022-150">Úroveň priority výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="b0022-150">Priority level of hello alert.</span></span> |
| <span data-ttu-id="b0022-151">StateType</span><span class="sxs-lookup"><span data-stu-id="b0022-151">StateType</span></span> | <span data-ttu-id="b0022-152">Typ Hello stavu hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="b0022-152">hello type of state of hello alert.</span></span><br><br><span data-ttu-id="b0022-153">Konfigurace SOFT - problém, který nebyl opakovaným zkontrolováním.</span><span class="sxs-lookup"><span data-stu-id="b0022-153">SOFT - Issue that has not been rechecked.</span></span><br><span data-ttu-id="b0022-154">PEVNÝ - problém, který byl znovu zkontrolují zadaného počtu opakování.</span><span class="sxs-lookup"><span data-stu-id="b0022-154">HARD - Issue that has been rechecked a specified number of times.</span></span>  |
| <span data-ttu-id="b0022-155">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="b0022-155">TimeGenerated</span></span> |<span data-ttu-id="b0022-156">Datum a čas hello výstraha byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="b0022-156">Date and time hello alert was created.</span></span> |


### <a name="zabbix-alert-records"></a><span data-ttu-id="b0022-157">Výstrahy záznamy Zabbix</span><span class="sxs-lookup"><span data-stu-id="b0022-157">Zabbix alert records</span></span>
<span data-ttu-id="b0022-158">Výstrahy mají záznamy shromažďují Zabbix **typ** z **výstrahy** a **SourceSystem** z **Zabbix**.</span><span class="sxs-lookup"><span data-stu-id="b0022-158">Alert records collected by Zabbix have a **Type** of **Alert** and a **SourceSystem** of **Zabbix**.</span></span>  <span data-ttu-id="b0022-159">V následující tabulce hello mají vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="b0022-159">They have hello properties in hello following table.</span></span>

| <span data-ttu-id="b0022-160">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="b0022-160">Property</span></span> | <span data-ttu-id="b0022-161">Popis</span><span class="sxs-lookup"><span data-stu-id="b0022-161">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b0022-162">Typ</span><span class="sxs-lookup"><span data-stu-id="b0022-162">Type</span></span> |<span data-ttu-id="b0022-163">*Výstrahy*</span><span class="sxs-lookup"><span data-stu-id="b0022-163">*Alert*</span></span> |
| <span data-ttu-id="b0022-164">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="b0022-164">SourceSystem</span></span> |<span data-ttu-id="b0022-165">*Zabbix*</span><span class="sxs-lookup"><span data-stu-id="b0022-165">*Zabbix*</span></span> |
| <span data-ttu-id="b0022-166">AlertName</span><span class="sxs-lookup"><span data-stu-id="b0022-166">AlertName</span></span> | <span data-ttu-id="b0022-167">Název výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="b0022-167">Name of hello alert.</span></span> |
| <span data-ttu-id="b0022-168">AlertPriority</span><span class="sxs-lookup"><span data-stu-id="b0022-168">AlertPriority</span></span> | <span data-ttu-id="b0022-169">Závažnost výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="b0022-169">Severity of hello alert.</span></span><br><br><span data-ttu-id="b0022-170">není klasifikovaný</span><span class="sxs-lookup"><span data-stu-id="b0022-170">not classified</span></span><br><span data-ttu-id="b0022-171">Informace o</span><span class="sxs-lookup"><span data-stu-id="b0022-171">information</span></span><br><span data-ttu-id="b0022-172">Upozornění</span><span class="sxs-lookup"><span data-stu-id="b0022-172">warning</span></span><br><span data-ttu-id="b0022-173">Průměr</span><span class="sxs-lookup"><span data-stu-id="b0022-173">average</span></span><br><span data-ttu-id="b0022-174">Vysoká</span><span class="sxs-lookup"><span data-stu-id="b0022-174">high</span></span><br><span data-ttu-id="b0022-175">po havárii</span><span class="sxs-lookup"><span data-stu-id="b0022-175">disaster</span></span>  |
| <span data-ttu-id="b0022-176">AlertState</span><span class="sxs-lookup"><span data-stu-id="b0022-176">AlertState</span></span> | <span data-ttu-id="b0022-177">Stav výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="b0022-177">State of hello alert.</span></span><br><br><span data-ttu-id="b0022-178">0 – stav je až toodate.</span><span class="sxs-lookup"><span data-stu-id="b0022-178">0 - State is up toodate.</span></span><br><span data-ttu-id="b0022-179">1 - stav není znám.</span><span class="sxs-lookup"><span data-stu-id="b0022-179">1 - State is unknown.</span></span>  |
| <span data-ttu-id="b0022-180">AlertTypeNumber</span><span class="sxs-lookup"><span data-stu-id="b0022-180">AlertTypeNumber</span></span> | <span data-ttu-id="b0022-181">Určuje, zda výstraha může vygenerovat více událostí problém.</span><span class="sxs-lookup"><span data-stu-id="b0022-181">Specifies whether alert can generate multiple problem events.</span></span><br><br><span data-ttu-id="b0022-182">0 – stav je až toodate.</span><span class="sxs-lookup"><span data-stu-id="b0022-182">0 - State is up toodate.</span></span><br><span data-ttu-id="b0022-183">1 - stav není znám.</span><span class="sxs-lookup"><span data-stu-id="b0022-183">1 - State is unknown.</span></span>    |
| <span data-ttu-id="b0022-184">Komentáře</span><span class="sxs-lookup"><span data-stu-id="b0022-184">Comments</span></span> | <span data-ttu-id="b0022-185">Další poznámky pro výstrahu.</span><span class="sxs-lookup"><span data-stu-id="b0022-185">Additional comments for alert.</span></span> |
| <span data-ttu-id="b0022-186">Název hostitele</span><span class="sxs-lookup"><span data-stu-id="b0022-186">HostName</span></span> | <span data-ttu-id="b0022-187">Název hostitele hello, který vytvořili výstrahu hello.</span><span class="sxs-lookup"><span data-stu-id="b0022-187">Name of hello host that created hello alert.</span></span> |
| <span data-ttu-id="b0022-188">PriorityNumber</span><span class="sxs-lookup"><span data-stu-id="b0022-188">PriorityNumber</span></span> | <span data-ttu-id="b0022-189">Hodnota označující závažnost výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="b0022-189">Value indicating severity of hello alert.</span></span><br><br><span data-ttu-id="b0022-190">0 – nezařazených</span><span class="sxs-lookup"><span data-stu-id="b0022-190">0 - not classified</span></span><br><span data-ttu-id="b0022-191">1 - informace</span><span class="sxs-lookup"><span data-stu-id="b0022-191">1 - information</span></span><br><span data-ttu-id="b0022-192">2 – upozornění</span><span class="sxs-lookup"><span data-stu-id="b0022-192">2 - warning</span></span><br><span data-ttu-id="b0022-193">3 – průměr</span><span class="sxs-lookup"><span data-stu-id="b0022-193">3 - average</span></span><br><span data-ttu-id="b0022-194">4 – vysoká</span><span class="sxs-lookup"><span data-stu-id="b0022-194">4 - high</span></span><br><span data-ttu-id="b0022-195">5 - po havárii</span><span class="sxs-lookup"><span data-stu-id="b0022-195">5 - disaster</span></span> |
| <span data-ttu-id="b0022-196">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="b0022-196">TimeGenerated</span></span> |<span data-ttu-id="b0022-197">Datum a čas hello výstraha byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="b0022-197">Date and time hello alert was created.</span></span> |
| <span data-ttu-id="b0022-198">TimeLastModified</span><span class="sxs-lookup"><span data-stu-id="b0022-198">TimeLastModified</span></span> |<span data-ttu-id="b0022-199">Datum a čas hello stav výstrahy hello byl naposled změněn.</span><span class="sxs-lookup"><span data-stu-id="b0022-199">Date and time hello state of hello alert was last changed.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="b0022-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b0022-200">Next steps</span></span>
* <span data-ttu-id="b0022-201">Další informace o [výstrahy](log-analytics-alerts.md) v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="b0022-201">Learn about [alerts](log-analytics-alerts.md) in Log Analytics.</span></span>
* <span data-ttu-id="b0022-202">Další informace o [protokolu hledání](log-analytics-log-searches.md) tooanalyze hello data budou shromažďovat ze zdroje dat a řešení.</span><span class="sxs-lookup"><span data-stu-id="b0022-202">Learn about [log searches](log-analytics-log-searches.md) tooanalyze hello data collected from data sources and solutions.</span></span> 
