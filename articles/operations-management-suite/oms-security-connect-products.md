---
title: "aaaConnecting toohello zabezpečení produkty Operations Management Suite (OMS) zabezpečení a Audit řešení | Microsoft Docs"
description: "Tento dokument pomůže vám tooconnect vaše tooOperations produkty zabezpečení Management Suite zabezpečení a Audit řešení pomocí běžný formát událostí."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 46eee484-e078-4bad-8c89-c88a3508f6aa
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: yurid
ms.openlocfilehash: 0f4b372d0379987c4e249628a3c8d52733be65c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-your-security-products-toohello-operations-management-suite-oms-security-and-audit-solution"></a><span data-ttu-id="fc380-103">Připojení zabezpečení produkty toohello Operations Management Suite (OMS) zabezpečení a Audit řešení</span><span class="sxs-lookup"><span data-stu-id="fc380-103">Connecting your security products toohello Operations Management Suite (OMS) Security and Audit Solution</span></span> 
<span data-ttu-id="fc380-104">Tento dokument vám připojit zabezpečovací produkty do hello OMS zabezpečení a Audit řešení pomůže.</span><span class="sxs-lookup"><span data-stu-id="fc380-104">This document helps you connect your security products into hello OMS Security and Audit Solution.</span></span> <span data-ttu-id="fc380-105">jsou podporovány Hello následující zdroje:</span><span class="sxs-lookup"><span data-stu-id="fc380-105">hello following sources are supported:</span></span>

- <span data-ttu-id="fc380-106">Události CEF (Common Event Format)</span><span class="sxs-lookup"><span data-stu-id="fc380-106">Common Event Format (CEF) events</span></span>
- <span data-ttu-id="fc380-107">Události Cisco ASA</span><span class="sxs-lookup"><span data-stu-id="fc380-107">Cisco ASA events</span></span>


## <a name="what-is-cef"></a><span data-ttu-id="fc380-108">Co je CEF?</span><span class="sxs-lookup"><span data-stu-id="fc380-108">What is CEF?</span></span>
<span data-ttu-id="fc380-109">Běžný formát událostí (CEF) je standardní formát nad zprávy Syslog, používá mnoho zabezpečení dodavatelé tooallow událostí vzájemná funkční spolupráce mezi různými platformami.</span><span class="sxs-lookup"><span data-stu-id="fc380-109">Common Event Format (CEF) is an industry standard format on top of Syslog messages, used by many security vendors tooallow event interoperability among different platforms.</span></span> <span data-ttu-id="fc380-110">OMS zabezpečení a Audit řešení podporovat přijímání dat pomocí CEF, což vám umožní tooconnect zabezpečovací produkty OMS zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fc380-110">OMS Security and Audit Solution support data ingestion using CEF, which enables you tooconnect your security products with OMS Security.</span></span> 

<span data-ttu-id="fc380-111">Připojením vašeho tooOMS zdroje dat, jsou možné tootake výhod hello následující možnosti, které jsou součástí této platformě:</span><span class="sxs-lookup"><span data-stu-id="fc380-111">By connecting your data source tooOMS, you are able tootake advantage of hello following capabilities that are part of this platform:</span></span>

- <span data-ttu-id="fc380-112">Hledání a korelace</span><span class="sxs-lookup"><span data-stu-id="fc380-112">Search & Correlation</span></span>
- <span data-ttu-id="fc380-113">Auditování</span><span class="sxs-lookup"><span data-stu-id="fc380-113">Auditing</span></span>
- <span data-ttu-id="fc380-114">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="fc380-114">Alert</span></span>
- <span data-ttu-id="fc380-115">Analýza hrozeb</span><span class="sxs-lookup"><span data-stu-id="fc380-115">Threat Intelligence</span></span>
- <span data-ttu-id="fc380-116">Významné problémy</span><span class="sxs-lookup"><span data-stu-id="fc380-116">Notable Issues</span></span>

## <a name="collection-of-security-solution-logs"></a><span data-ttu-id="fc380-117">Shromažďování protokolů řešení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="fc380-117">Collection of security solution logs</span></span>

<span data-ttu-id="fc380-118">Zabezpečení OMS podporuje shromažďování protokolů pomocí formátu CEF přes Syslogy a protokoly [Cisco ASA](https://blogs.technet.microsoft.com/msoms/2016/08/25/add-your-cisco-asa-logs-to-oms-security/).</span><span class="sxs-lookup"><span data-stu-id="fc380-118">OMS Security supports collection of logs using CEF over Syslogs and [Cisco ASA](https://blogs.technet.microsoft.com/msoms/2016/08/25/add-your-cisco-asa-logs-to-oms-security/) logs.</span></span> <span data-ttu-id="fc380-119">V tomto příkladu je počítač se systémem Linux spuštěn démon procesu syslog ng hello zdroj (počítač, který generuje protokoly hello) a cíl hello je OMS zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fc380-119">In this example, hello source (computer that generates hello logs) is a Linux computer running syslog-ng daemon and hello target is OMS Security.</span></span> <span data-ttu-id="fc380-120">počítač se systémem Linux tooprepare hello budete potřebovat tooperform hello následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="fc380-120">tooprepare hello Linux computer you will need tooperform hello following tasks:</span></span>

- <span data-ttu-id="fc380-121">Stáhněte si hello OMS agenta pro Linux, verze 1.2.0-25 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="fc380-121">Download hello OMS Agent for Linux, version 1.2.0-25 or above.</span></span>
- <span data-ttu-id="fc380-122">Postupujte podle tématu hello **Stručná příručka nainstalovat** z [v tomto článku](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux) tooinstall a zařadit hello agenta tooyour prostoru.</span><span class="sxs-lookup"><span data-stu-id="fc380-122">Follow hello section **Quick Install Guide** from [this article](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux) tooinstall and onboard hello agent tooyour workspace.</span></span>

<span data-ttu-id="fc380-123">Hello agent je obvykle nainstalován na jiný počítač než hello jsou generovány, na které hello protokoly jeden.</span><span class="sxs-lookup"><span data-stu-id="fc380-123">Typically, hello agent is installed on a different computer from hello one on which hello logs are generated.</span></span> <span data-ttu-id="fc380-124">Počítač agenta předávání hello protokoly toohello bude obvykle vyžadují hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fc380-124">Forwarding hello logs toohello agent machine will usually require hello following steps:</span></span>

- <span data-ttu-id="fc380-125">Nakonfigurujte hello protokolování produktu nebo počítač tooforward hello požadované události toohello démon procesu syslog (rsyslog nebo syslog ng) v počítači agenta hello.</span><span class="sxs-lookup"><span data-stu-id="fc380-125">Configure hello logging product/machine tooforward hello required events toohello syslog daemon (rsyslog or syslog-ng) on hello agent machine.</span></span>
- <span data-ttu-id="fc380-126">Povolte hello démon procesu syslog na hello agenta počítač tooreceive zpráv ze vzdáleného systému.</span><span class="sxs-lookup"><span data-stu-id="fc380-126">Enable hello syslog daemon on hello agent machine tooreceive messages from a remote system.</span></span>

<span data-ttu-id="fc380-127">V počítači agenta hello třeba hello události toobe odeslaný port UDP toolocal démon procesu syslog hello 25226.</span><span class="sxs-lookup"><span data-stu-id="fc380-127">On hello agent machine, hello events need toobe sent from hello syslog daemon toolocal UDP port 25226.</span></span> <span data-ttu-id="fc380-128">Příchozí události na tomto portu naslouchá Hello agenta.</span><span class="sxs-lookup"><span data-stu-id="fc380-128">hello agent is listening for incoming events on this port.</span></span> <span data-ttu-id="fc380-129">Hello tady je příklad konfigurace pro odesílání všechny události z hello místní systém toohello agenta (nastavení můžete upravit toofit hello konfiguraci vaší místní):</span><span class="sxs-lookup"><span data-stu-id="fc380-129">hello following is an example configuration for sending all events from hello local system toohello agent (you can modify hello configuration toofit your local settings):</span></span>

1. <span data-ttu-id="fc380-130">Hello otevřete okno terminálu a přejděte toohello directory */etc/syslog-ng /*</span><span class="sxs-lookup"><span data-stu-id="fc380-130">Open hello terminal window, and go toohello directory */etc/syslog-ng/*</span></span> 
2. <span data-ttu-id="fc380-131">Vytvořte nový soubor *zabezpečení config-omsagent.conf* a přidejte hello následující obsah: OMS_facility = local4</span><span class="sxs-lookup"><span data-stu-id="fc380-131">Create a new file *security-config-omsagent.conf* and add hello following content:  OMS_facility = local4</span></span>
    
    <span data-ttu-id="fc380-132">filter f_local4_oms { facility(local4); };</span><span class="sxs-lookup"><span data-stu-id="fc380-132">filter f_local4_oms { facility(local4); };</span></span>

    <span data-ttu-id="fc380-133">destination security_oms { tcp("127.0.0.1" port(25226)); };</span><span class="sxs-lookup"><span data-stu-id="fc380-133">destination security_oms { tcp("127.0.0.1" port(25226)); };</span></span>

    <span data-ttu-id="fc380-134">log { source(src); filter(f_local4_oms); destination(security_oms); };</span><span class="sxs-lookup"><span data-stu-id="fc380-134">log { source(src); filter(f_local4_oms); destination(security_oms); };</span></span>
    
3. <span data-ttu-id="fc380-135">Stáhněte si soubor hello *security_events.conf* a umístěte */etc/opt/microsoft/omsagent/conf/omsagent.d/* v počítači agenta OMS hello.</span><span class="sxs-lookup"><span data-stu-id="fc380-135">Download hello file *security_events.conf* and place at */etc/opt/microsoft/omsagent/conf/omsagent.d/* in hello OMS Agent computer.</span></span>
4. <span data-ttu-id="fc380-136">Zadejte příkaz hello níže démon procesu syslog hello toorestart: *pro syslog-ng spustit:*</span><span class="sxs-lookup"><span data-stu-id="fc380-136">Type hello command below toorestart hello syslog daemon:  *For syslog-ng run:*</span></span>
    
    ```
    sudo service rsyslog restart
    ```

    <span data-ttu-id="fc380-137">*Pro rsyslog spusťte:*</span><span class="sxs-lookup"><span data-stu-id="fc380-137">*For rsyslog run:*</span></span>
    
    ```
    /etc/init.d/syslog-ng restart
    ```
5. <span data-ttu-id="fc380-138">Zadejte příkaz hello níže toorestart hello agenta OMS:</span><span class="sxs-lookup"><span data-stu-id="fc380-138">Type hello command below toorestart hello OMS Agent:</span></span>

    <span data-ttu-id="fc380-139">*Pro syslog-ng spusťte:*</span><span class="sxs-lookup"><span data-stu-id="fc380-139">*For syslog-ng run:*</span></span>
    
    ```
    sudo service omsagent restart
    ```

    <span data-ttu-id="fc380-140">*Pro rsyslog spusťte:*</span><span class="sxs-lookup"><span data-stu-id="fc380-140">*For rsyslog run:*</span></span>
    
    ```
    systemctl restart omsagent
    ```
6. <span data-ttu-id="fc380-141">Zadejte následující příkaz hello a zkontrolujte tooconfirm hello výsledek, že nejsou žádné chyby v protokolu agenta OMS hello:</span><span class="sxs-lookup"><span data-stu-id="fc380-141">Type hello command below and review hello result tooconfirm that there are no errors in hello OMS Agent log:</span></span>

    ``` 
    tail /var/opt/microsoft/omsagent/log/omsagent.log
    ```

## <a name="reviewing-collected-security-events"></a><span data-ttu-id="fc380-142">Kontrola shromážděných událostí zabezpečení</span><span class="sxs-lookup"><span data-stu-id="fc380-142">Reviewing collected security events</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

<span data-ttu-id="fc380-143">Po hello konfigurace, začne událostí zabezpečení hello toobe požita OMS zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fc380-143">After hello configuration is over, hello security event will start toobe ingested by OMS Security.</span></span> <span data-ttu-id="fc380-144">toovisualize těchto události, otevřete hello protokolu vyhledávání, zadejte příkaz hello *typ = CommonSecurityLog* v hello pole hledání a stiskněte klávesu ENTER.</span><span class="sxs-lookup"><span data-stu-id="fc380-144">toovisualize those events, open hello Log Search, type hello command *Type=CommonSecurityLog* in hello search field and press ENTER.</span></span> <span data-ttu-id="fc380-145">Hello následující příklad ukazuje hello výsledek tohoto příkazu, Všimněte si, že v tomto případě OMS zabezpečení již požity protokolů zabezpečení od více dodavatelů:</span><span class="sxs-lookup"><span data-stu-id="fc380-145">hello following example shows hello result of this command, notice that in this case OMS Security already ingested security logs from multiple vendors:</span></span>
   
![Vyhodnocování standardních hodnot v řešení Zabezpečení a audit v OMS](./media/oms-security-connect-products/oms-security-connect-products-fig1.png)

<span data-ttu-id="fc380-147">Můžete upřesnit tohoto hledání pro jeden jednoho dodavatele, například toovisualize online Cisco protokoly, typ: *typ = CommonSecurityLog DeviceVendor = Cisco*.</span><span class="sxs-lookup"><span data-stu-id="fc380-147">You can refine this search for one single vendor, for example, toovisualize online Cisco logs, type: *Type=CommonSecurityLog DeviceVendor=Cisco*.</span></span> <span data-ttu-id="fc380-148">Hello "CommonSecurityLog" má předdefinovaná pole pro všechny hlavičky CEF včetně základní extensios hello při jakékoli jiné rozšíření, zda je "Rozšíření vlastních", nebo Ne, budou vložena do pole "Přepište".</span><span class="sxs-lookup"><span data-stu-id="fc380-148">hello “CommonSecurityLog” has predefined fields for any CEF header including hello basic extensios, while any other extension whether it’s “Custom Extension” or not, will be inserted into "AdditionalExtensions" field.</span></span> <span data-ttu-id="fc380-149">Můžete použít hello vlastní pole funkce tooget vyhrazené pole z něj.</span><span class="sxs-lookup"><span data-stu-id="fc380-149">You can use hello Custom Fields feature tooget dedicated fields from it.</span></span> 

### <a name="accessing-computers-missing-baseline-assessment"></a><span data-ttu-id="fc380-150">Přístup k počítačům, u nichž nedošlo k vyhodnocení standardních hodnot</span><span class="sxs-lookup"><span data-stu-id="fc380-150">Accessing computers missing baseline assessment</span></span>
<span data-ttu-id="fc380-151">OMS podporuje profil základní člen hello domény na Windows Server 2008 R2 až tooWindows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="fc380-151">OMS supports hello domain member baseline profile on Windows Server 2008 R2 up tooWindows Server 2012 R2.</span></span> <span data-ttu-id="fc380-152">Standardní hodnoty systému Windows Server 2016 ještě nejsou dokončené, ale budou přidány okamžitě po publikování.</span><span class="sxs-lookup"><span data-stu-id="fc380-152">Windows Server 2016 baseline isn’t final yet and will be added as soon as it is published.</span></span> <span data-ttu-id="fc380-153">Všechny ostatní operační systémy, zkontrolovat pomocí OMS zabezpečení a Audit hodnocení směrného plánu se zobrazí pod hello **počítače s chybějícími směrného plánu vyhodnocení** části.</span><span class="sxs-lookup"><span data-stu-id="fc380-153">All other operating systems scanned via OMS Security and Audit baseline assessment appear under hello **Computers missing baseline assessment** section.</span></span>

## <a name="see-also"></a><span data-ttu-id="fc380-154">Viz také</span><span class="sxs-lookup"><span data-stu-id="fc380-154">See also</span></span>
<span data-ttu-id="fc380-155">V tomto dokumentu jste se naučili jak tooconnect vaše řešení tooOMS CEF.</span><span class="sxs-lookup"><span data-stu-id="fc380-155">In this document, you learned how tooconnect your CEF solution tooOMS.</span></span> <span data-ttu-id="fc380-156">Další informace o zabezpečení OMS toolearn najdete hello následující články:</span><span class="sxs-lookup"><span data-stu-id="fc380-156">toolearn more about OMS Security, see hello following articles:</span></span>

* [<span data-ttu-id="fc380-157">Přehled Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="fc380-157">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="fc380-158">Monitorování a výstrahy tooSecurity odpovídá v Operations Management Suite zabezpečení a Audit řešení</span><span class="sxs-lookup"><span data-stu-id="fc380-158">Monitoring and Responding tooSecurity Alerts in Operations Management Suite Security and Audit Solution</span></span>](oms-security-responding-alerts.md)
* [<span data-ttu-id="fc380-159">Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="fc380-159">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

