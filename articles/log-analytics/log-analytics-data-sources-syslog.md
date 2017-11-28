---
title: "aaaCollect a analyzovat zprávy Syslog v OMS Log Analytics | Microsoft Docs"
description: "Syslog je protokol protokolování událostí, který je běžné tooLinux. Tento článek popisuje, jak tooconfigure kolekce zprávy Syslog v analýzy protokolů a podrobnosti záznamů hello vytvoří v úložišti OMS hello."
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
ms.date: 07/12/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 8bfa0bca3f2f18287d1352c98bbaa2a70e41e276
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="syslog-data-sources-in-log-analytics"></a><span data-ttu-id="2c66d-104">Syslog zdroje dat v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="2c66d-104">Syslog data sources in Log Analytics</span></span>
<span data-ttu-id="2c66d-105">Syslog je protokol protokolování událostí, který je běžné tooLinux.</span><span class="sxs-lookup"><span data-stu-id="2c66d-105">Syslog is an event logging protocol that is common tooLinux.</span></span>  <span data-ttu-id="2c66d-106">Aplikace bude odesílat zprávy, které může být uložené na místním počítači hello nebo doručit tooa Syslog kolekce.</span><span class="sxs-lookup"><span data-stu-id="2c66d-106">Applications will send messages that may be stored on hello local machine or delivered tooa Syslog collector.</span></span>  <span data-ttu-id="2c66d-107">Pokud je nainstalovaná hello OMS agenta pro Linux, nakonfiguruje hello místní Syslog démon tooforward zprávy toohello agenta.</span><span class="sxs-lookup"><span data-stu-id="2c66d-107">When hello OMS Agent for Linux is installed, it configures hello local Syslog daemon tooforward messages toohello agent.</span></span>  <span data-ttu-id="2c66d-108">Hello agent pak odešle zpráva tooLog hello Analytics kde se vytvoří odpovídající záznam v úložišti OMS hello.</span><span class="sxs-lookup"><span data-stu-id="2c66d-108">hello agent then sends hello message tooLog Analytics where a corresponding record is created in hello OMS repository.</span></span>  

> [!NOTE]
> <span data-ttu-id="2c66d-109">Analýzy protokolů podporuje kolekce zprávy odeslané rsyslog nebo syslog ng, kde je rsyslog démon výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="2c66d-109">Log Analytics supports collection of messages sent by rsyslog or syslog-ng, where rsyslog is hello default daemon.</span></span> <span data-ttu-id="2c66d-110">Démon procesu syslog výchozí Hello na verze 5 Red Hat Enterprise Linux a CentOS, Oracle Linux verze (sysklog) není podporována pro shromažďování událostí syslog.</span><span class="sxs-lookup"><span data-stu-id="2c66d-110">hello default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="2c66d-111">hello toocollect syslog data z této verze těchto distribuce [rsyslog démon](http://rsyslog.com) by měl být nainstalován a nakonfigurován tooreplace sysklog.</span><span class="sxs-lookup"><span data-stu-id="2c66d-111">toocollect syslog data from this version of these distributions, hello [rsyslog daemon](http://rsyslog.com) should be installed and configured tooreplace sysklog.</span></span>
>
>

![Kolekce Syslog](media/log-analytics-data-sources-syslog/overview.png)

## <a name="configuring-syslog"></a><span data-ttu-id="2c66d-113">Konfigurace procesu Syslog</span><span class="sxs-lookup"><span data-stu-id="2c66d-113">Configuring Syslog</span></span>
<span data-ttu-id="2c66d-114">Hello OMS agenta pro Linux bude shromažďovat jenom události s hello zařízení a závažnosti, které jsou určené v konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2c66d-114">hello OMS Agent for Linux will only collect events with hello facilities and severities that are specified in its configuration.</span></span>  <span data-ttu-id="2c66d-115">Syslog můžete nakonfigurovat přes portál OMS hello nebo Správa konfiguračních souborů na agenty systému Linux.</span><span class="sxs-lookup"><span data-stu-id="2c66d-115">You can configure Syslog through hello OMS portal or by managing configuration files on your Linux agents.</span></span>

### <a name="configure-syslog-in-hello-oms-portal"></a><span data-ttu-id="2c66d-116">Konfigurace Syslog na portálu OMS hello</span><span class="sxs-lookup"><span data-stu-id="2c66d-116">Configure Syslog in hello OMS portal</span></span>
<span data-ttu-id="2c66d-117">Nakonfigurovat Syslog z hello [nabídce Data v nastavení analýzy protokolů](log-analytics-data-sources.md#configuring-data-sources).</span><span class="sxs-lookup"><span data-stu-id="2c66d-117">Configure Syslog from hello [Data menu in Log Analytics Settings](log-analytics-data-sources.md#configuring-data-sources).</span></span>  <span data-ttu-id="2c66d-118">Tato konfigurace se doručí toohello konfigurační soubor na každého agenta systému Linux.</span><span class="sxs-lookup"><span data-stu-id="2c66d-118">This configuration is delivered toohello configuration file on each Linux agent.</span></span>

<span data-ttu-id="2c66d-119">Můžete přidat nové zařízení zadáním v jeho název a kliknutím na tlačítko  **+** .</span><span class="sxs-lookup"><span data-stu-id="2c66d-119">You can add a new facility by typing in its name and clicking **+**.</span></span>  <span data-ttu-id="2c66d-120">Pro každé zařízení budou shromažďovány pouze zprávy s hello vybrané závažnosti.</span><span class="sxs-lookup"><span data-stu-id="2c66d-120">For each facility, only messages with hello selected severities will be collected.</span></span>  <span data-ttu-id="2c66d-121">Zkontrolujte hello závažnosti pro hello konkrétní zařízení, které chcete toocollect.</span><span class="sxs-lookup"><span data-stu-id="2c66d-121">Check hello severities for hello particular facility that you want toocollect.</span></span>  <span data-ttu-id="2c66d-122">Žádné další kritéria nemůžete zadat toofilter zprávy.</span><span class="sxs-lookup"><span data-stu-id="2c66d-122">You cannot provide any additional criteria toofilter messages.</span></span>

![Konfigurace procesu Syslog](media/log-analytics-data-sources-syslog/configure.png)

<span data-ttu-id="2c66d-124">Standardně jsou všechny změny konfigurace automaticky instaluje tooall agenty.</span><span class="sxs-lookup"><span data-stu-id="2c66d-124">By default, all configuration changes are automatically pushed tooall agents.</span></span>  <span data-ttu-id="2c66d-125">Pokud chcete tooconfigure Syslog ručně na každý agenta systému Linux, poté zrušte zaškrtnutí políčka hello *použít následující konfigurace počítačů se systémem Linux toomy*.</span><span class="sxs-lookup"><span data-stu-id="2c66d-125">If you want tooconfigure Syslog manually on each Linux agent, then uncheck hello box *Apply below configuration toomy Linux machines*.</span></span>

### <a name="configure-syslog-on-linux-agent"></a><span data-ttu-id="2c66d-126">Konfigurace Syslog na agenta systému Linux</span><span class="sxs-lookup"><span data-stu-id="2c66d-126">Configure Syslog on Linux agent</span></span>
<span data-ttu-id="2c66d-127">Když hello [Linux klienta je nainstalován OMS agent](log-analytics-linux-agents.md), nainstaluje výchozí syslog konfiguračního souboru, který definuje hello zařízení a závažnost hello zprávy, které se shromažďují.</span><span class="sxs-lookup"><span data-stu-id="2c66d-127">When hello [OMS agent is installed on a Linux client](log-analytics-linux-agents.md), it installs a default syslog configuration file that defines hello facility and severity of hello messages that are collected.</span></span>  <span data-ttu-id="2c66d-128">Tato konfigurace hello toochange soubor můžete upravit.</span><span class="sxs-lookup"><span data-stu-id="2c66d-128">You can modify this file toochange hello configuration.</span></span>  <span data-ttu-id="2c66d-129">Hello konfigurační soubor se liší v závislosti na hello Syslog nainstaloval démon procesu, který hello klienta.</span><span class="sxs-lookup"><span data-stu-id="2c66d-129">hello configuration file is different depending on hello Syslog daemon that hello client has installed.</span></span>

> [!NOTE]
> <span data-ttu-id="2c66d-130">Pokud chcete upravit konfiguraci hello syslog, je nutné restartovat hello démon procesu syslog pro hello změny tootake vliv.</span><span class="sxs-lookup"><span data-stu-id="2c66d-130">If you edit hello syslog configuration, you must restart hello syslog daemon for hello changes tootake effect.</span></span>
>
>

#### <a name="rsyslog"></a><span data-ttu-id="2c66d-131">rsyslog</span><span class="sxs-lookup"><span data-stu-id="2c66d-131">rsyslog</span></span>
<span data-ttu-id="2c66d-132">Hello konfiguračního souboru pro rsyslog se nachází v **/etc/rsyslog.d/95-omsagent.conf**.</span><span class="sxs-lookup"><span data-stu-id="2c66d-132">hello configuration file for rsyslog is located at **/etc/rsyslog.d/95-omsagent.conf**.</span></span>  <span data-ttu-id="2c66d-133">Níže jsou uvedeny výchozí obsah.</span><span class="sxs-lookup"><span data-stu-id="2c66d-133">Its default contents are shown below.</span></span>  <span data-ttu-id="2c66d-134">To shromažďuje syslog zpráv odeslaných z místního agenta hello pro všechna zařízení s úrovní varování nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="2c66d-134">This collects syslog messages sent from hello local agent for all facilities with a level of warning or higher.</span></span>

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

<span data-ttu-id="2c66d-135">Budovy můžete odebrat odstraněním jeho části hello konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="2c66d-135">You can remove a facility by removing its section of hello configuration file.</span></span>  <span data-ttu-id="2c66d-136">Můžete omezit hello závažnosti, které se shromažďují pro konkrétní zařízení úpravou položky tohoto zařízení.</span><span class="sxs-lookup"><span data-stu-id="2c66d-136">You can limit hello severities that are collected for a particular facility by modifying that facility's entry.</span></span>  <span data-ttu-id="2c66d-137">Například toolimit hello uživatele zařízení toomessages s závažnosti chyby nebo vyšší byste upravili daného řádku hello konfigurační soubor toohello následující:</span><span class="sxs-lookup"><span data-stu-id="2c66d-137">For example, toolimit hello user facility toomessages with a severity of error or higher you would modify that line of hello configuration file toohello following:</span></span>

    user.error    @127.0.0.1:25224


#### <a name="syslog-ng"></a><span data-ttu-id="2c66d-138">Syslog ng</span><span class="sxs-lookup"><span data-stu-id="2c66d-138">syslog-ng</span></span>
<span data-ttu-id="2c66d-139">Hello konfigurační soubor pro syslog ng je umístění v **/etc/syslog-ng/syslog-ng.conf**.</span><span class="sxs-lookup"><span data-stu-id="2c66d-139">hello configuration file for syslog-ng is location at **/etc/syslog-ng/syslog-ng.conf**.</span></span>  <span data-ttu-id="2c66d-140">Níže jsou uvedeny výchozí obsah.</span><span class="sxs-lookup"><span data-stu-id="2c66d-140">Its default contents are shown below.</span></span>  <span data-ttu-id="2c66d-141">To shromažďuje zpráv odeslaných z místního agenta hello všech zařízení a všechny závažnosti.</span><span class="sxs-lookup"><span data-stu-id="2c66d-141">This collects syslog messages sent from hello local agent for all facilities and all severities.</span></span>   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };

    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };

    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };

    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };

    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };

    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };

    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

<span data-ttu-id="2c66d-142">Budovy můžete odebrat odstraněním jeho části hello konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="2c66d-142">You can remove a facility by removing its section of hello configuration file.</span></span>  <span data-ttu-id="2c66d-143">Můžete omezit hello závažnosti, které se shromažďují pro konkrétní zařízení jejich vyjmutím ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="2c66d-143">You can limit hello severities that are collected for a particular facility by removing them from its list.</span></span>  <span data-ttu-id="2c66d-144">Například toolimit hello uživatele zařízení toojust zprávy upozornění a kritickou byste upravili této části hello konfigurační soubor toohello následující:</span><span class="sxs-lookup"><span data-stu-id="2c66d-144">For example, toolimit hello user facility toojust alert and critical messages, you would modify that section of hello configuration file toohello following:</span></span>

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="collecting-data-from-additional-syslog-ports"></a><span data-ttu-id="2c66d-145">Shromažďování dat z další porty Syslog</span><span class="sxs-lookup"><span data-stu-id="2c66d-145">Collecting data from additional Syslog ports</span></span>
<span data-ttu-id="2c66d-146">Hello OMS agent přijímají zprávy Syslog v místním klientovi hello na portu 25224.</span><span class="sxs-lookup"><span data-stu-id="2c66d-146">hello OMS agent listens for Syslog messages on hello local client on port 25224.</span></span>  <span data-ttu-id="2c66d-147">Pokud je nainstalován hello agent, použít výchozí konfigurace syslog a najít v hello následující umístění:</span><span class="sxs-lookup"><span data-stu-id="2c66d-147">When hello agent is installed, a default syslog configuration is applied and found in hello following location:</span></span>

* <span data-ttu-id="2c66d-148">Rsyslog:`/etc/rsyslog.d/95-omsagent.conf`</span><span class="sxs-lookup"><span data-stu-id="2c66d-148">Rsyslog: `/etc/rsyslog.d/95-omsagent.conf`</span></span>
* <span data-ttu-id="2c66d-149">Syslog-ng:`/etc/syslog-ng/syslog-ng.conf`</span><span class="sxs-lookup"><span data-stu-id="2c66d-149">Syslog-ng: `/etc/syslog-ng/syslog-ng.conf`</span></span>

<span data-ttu-id="2c66d-150">Číslo portu hello můžete změnit tak, že vytvoříte dvě konfigurační soubory: soubor konfigurace FluentD a soubor rsyslog nebo syslog ng v závislosti na instalaci hello démon procesu Syslog.</span><span class="sxs-lookup"><span data-stu-id="2c66d-150">You can change hello port number by creating two configuration files: a FluentD config file and a rsyslog-or-syslog-ng file depending on hello Syslog daemon you have installed.</span></span>  

* <span data-ttu-id="2c66d-151">Hello FluentD konfigurační soubor by měl být nový soubor umístěný ve: `/etc/opt/microsoft/omsagent/conf/omsagent.d` a nahraďte hodnotu hello v hello **port** položka se vaše vlastní číslo portu.</span><span class="sxs-lookup"><span data-stu-id="2c66d-151">hello FluentD config file should be a new file located in: `/etc/opt/microsoft/omsagent/conf/omsagent.d` and replace hello value in hello **port** entry with your custom port number.</span></span>

        <source>
          type syslog
          port %SYSLOG_PORT%
          bind 127.0.0.1
          protocol_type udp
          tag oms.syslog
        </source>
        <filter oms.syslog.**>
          type filter_syslog
        </filter>

* <span data-ttu-id="2c66d-152">Pro rsyslog, měli vytvořit nový soubor konfigurace umístěný v: `/etc/rsyslog.d/` a hello hodnota % SYSLOG_PORT % nahraďte váš vlastní číslo portu.</span><span class="sxs-lookup"><span data-stu-id="2c66d-152">For rsyslog, you should create a new configuration file located in: `/etc/rsyslog.d/` and replace hello value %SYSLOG_PORT% with your custom port number.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="2c66d-153">Pokud změníte tuto hodnotu v konfiguračním souboru hello `95-omsagent.conf`, kdy hello agent platí výchozí konfiguraci bude přepsán.</span><span class="sxs-lookup"><span data-stu-id="2c66d-153">If you modify this value in hello configuration file `95-omsagent.conf`, it will be overwritten when hello agent applies a default configuration.</span></span>
    >

        # OMS Syslog collection for workspace %WORKSPACE_ID%
        kern.warning              @127.0.0.1:%SYSLOG_PORT%
        user.warning              @127.0.0.1:%SYSLOG_PORT%
        daemon.warning            @127.0.0.1:%SYSLOG_PORT%
        auth.warning              @127.0.0.1:%SYSLOG_PORT%

* <span data-ttu-id="2c66d-154">Hello syslog ng konfigurace by měl být upraven tak, že zkopírujete následující hello příklad konfigurace a přidání hello vlastní upravené nastavení toohello konec hello syslog ng.conf konfigurační soubor umístěný ve `/etc/syslog-ng/`.</span><span class="sxs-lookup"><span data-stu-id="2c66d-154">hello syslog-ng config should be modified by copying hello example configuration shown below and adding hello custom modified settings toohello end of hello syslog-ng.conf configuration file located in `/etc/syslog-ng/`.</span></span>  <span data-ttu-id="2c66d-155">Proveďte **není** použít výchozí štítek hello **% WORKSPACE_ID % _oms** nebo **% WORKSPACE_ID_OMS**, definovat vlastní popisek toohelp rozlišit změny.</span><span class="sxs-lookup"><span data-stu-id="2c66d-155">Do **not** use hello default label **%WORKSPACE_ID%_oms** or **%WORKSPACE_ID_OMS**, define a custom label toohelp distinguish your changes.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="2c66d-156">Pokud změníte výchozí hodnoty hello v konfiguračním souboru hello, budou při hello agent použije výchozí konfigurace přepsány.</span><span class="sxs-lookup"><span data-stu-id="2c66d-156">If you modify hello default values in hello configuration file, they will be overwritten when hello agent applies a default configuration.</span></span>
    >

        filter f_custom_filter { level(warning) and facility(auth; };
        destination d_custom_dest { udp("127.0.0.1" port(%SYSLOG_PORT%)); };
        log { source(s_src); filter(f_custom_filter); destination(d_custom_dest); };

<span data-ttu-id="2c66d-157">Po dokončení změny hello, hello Syslog a hello služba agent OMS musí toobe restartovat tooensure hello konfigurace změny vstoupí v platnost.</span><span class="sxs-lookup"><span data-stu-id="2c66d-157">After completing hello changes, hello Syslog and hello OMS agent service needs toobe restarted tooensure hello configuration changes take effect.</span></span>   

## <a name="syslog-record-properties"></a><span data-ttu-id="2c66d-158">Vlastnosti záznamu Syslog</span><span class="sxs-lookup"><span data-stu-id="2c66d-158">Syslog record properties</span></span>
<span data-ttu-id="2c66d-159">Zaznamenává Syslog mít typ **Syslog** a mít hello vlastnosti v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="2c66d-159">Syslog records have a type of **Syslog** and have hello properties in hello following table.</span></span>

| <span data-ttu-id="2c66d-160">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2c66d-160">Property</span></span> | <span data-ttu-id="2c66d-161">Popis</span><span class="sxs-lookup"><span data-stu-id="2c66d-161">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2c66d-162">Počítač</span><span class="sxs-lookup"><span data-stu-id="2c66d-162">Computer</span></span> |<span data-ttu-id="2c66d-163">Počítač, který hello událost nebyla shromážděna z.</span><span class="sxs-lookup"><span data-stu-id="2c66d-163">Computer that hello event was collected from.</span></span> |
| <span data-ttu-id="2c66d-164">Zařízení</span><span class="sxs-lookup"><span data-stu-id="2c66d-164">Facility</span></span> |<span data-ttu-id="2c66d-165">Definuje hello součást hello systému, který vygeneroval zprávu hello.</span><span class="sxs-lookup"><span data-stu-id="2c66d-165">Defines hello part of hello system that generated hello message.</span></span> |
| <span data-ttu-id="2c66d-166">HostIP</span><span class="sxs-lookup"><span data-stu-id="2c66d-166">HostIP</span></span> |<span data-ttu-id="2c66d-167">IP adresa systému hello odesílání zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="2c66d-167">IP address of hello system sending hello message.</span></span> |
| <span data-ttu-id="2c66d-168">Název hostitele</span><span class="sxs-lookup"><span data-stu-id="2c66d-168">HostName</span></span> |<span data-ttu-id="2c66d-169">Název systému hello odesílání zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="2c66d-169">Name of hello system sending hello message.</span></span> |
| <span data-ttu-id="2c66d-170">Úroveň závažnosti</span><span class="sxs-lookup"><span data-stu-id="2c66d-170">SeverityLevel</span></span> |<span data-ttu-id="2c66d-171">Úroveň závažnosti události hello.</span><span class="sxs-lookup"><span data-stu-id="2c66d-171">Severity level of hello event.</span></span> |
| <span data-ttu-id="2c66d-172">SyslogMessage</span><span class="sxs-lookup"><span data-stu-id="2c66d-172">SyslogMessage</span></span> |<span data-ttu-id="2c66d-173">Text zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="2c66d-173">Text of hello message.</span></span> |
| <span data-ttu-id="2c66d-174">ID procesu</span><span class="sxs-lookup"><span data-stu-id="2c66d-174">ProcessID</span></span> |<span data-ttu-id="2c66d-175">ID procesu hello generovaný uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="2c66d-175">ID of hello process that generated hello message.</span></span> |
| <span data-ttu-id="2c66d-176">eventTime</span><span class="sxs-lookup"><span data-stu-id="2c66d-176">EventTime</span></span> |<span data-ttu-id="2c66d-177">Datum a čas, který hello událostí byl vygenerován.</span><span class="sxs-lookup"><span data-stu-id="2c66d-177">Date and time that hello event was generated.</span></span> |

## <a name="log-queries-with-syslog-records"></a><span data-ttu-id="2c66d-178">Dotazy protokolu se záznamy Syslog</span><span class="sxs-lookup"><span data-stu-id="2c66d-178">Log queries with Syslog records</span></span>
<span data-ttu-id="2c66d-179">Hello následující tabulka obsahuje různé příklady dotazů protokolu, která načítají záznamy Syslog.</span><span class="sxs-lookup"><span data-stu-id="2c66d-179">hello following table provides different examples of log queries that retrieve Syslog records.</span></span>

| <span data-ttu-id="2c66d-180">Dotaz</span><span class="sxs-lookup"><span data-stu-id="2c66d-180">Query</span></span> | <span data-ttu-id="2c66d-181">Popis</span><span class="sxs-lookup"><span data-stu-id="2c66d-181">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2c66d-182">Typ = Syslog</span><span class="sxs-lookup"><span data-stu-id="2c66d-182">Type=Syslog</span></span> |<span data-ttu-id="2c66d-183">Všechny systémové protokoly.</span><span class="sxs-lookup"><span data-stu-id="2c66d-183">All Syslogs.</span></span> |
| <span data-ttu-id="2c66d-184">Typ = Syslog úroveň závažnosti chyby =</span><span class="sxs-lookup"><span data-stu-id="2c66d-184">Type=Syslog SeverityLevel=error</span></span> |<span data-ttu-id="2c66d-185">Všechny záznamy Syslog s závažnosti chyby.</span><span class="sxs-lookup"><span data-stu-id="2c66d-185">All Syslog records with severity of error.</span></span> |
| <span data-ttu-id="2c66d-186">Typ = Syslog &#124; míra count() počítačem.</span><span class="sxs-lookup"><span data-stu-id="2c66d-186">Type=Syslog &#124; measure count() by Computer</span></span> |<span data-ttu-id="2c66d-187">Počet Syslog záznamy počítačem.</span><span class="sxs-lookup"><span data-stu-id="2c66d-187">Count of Syslog records by computer.</span></span> |
| <span data-ttu-id="2c66d-188">Typ = Syslog &#124; míra count() podle zařízení</span><span class="sxs-lookup"><span data-stu-id="2c66d-188">Type=Syslog &#124; measure count() by Facility</span></span> |<span data-ttu-id="2c66d-189">Počet Syslog záznamy podle budovy.</span><span class="sxs-lookup"><span data-stu-id="2c66d-189">Count of Syslog records by facility.</span></span> |

>[!NOTE]
> <span data-ttu-id="2c66d-190">Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak změní následující toohello hello výše dotazy.</span><span class="sxs-lookup"><span data-stu-id="2c66d-190">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then hello above queries would change toohello following.</span></span>

> | <span data-ttu-id="2c66d-191">Dotaz</span><span class="sxs-lookup"><span data-stu-id="2c66d-191">Query</span></span> | <span data-ttu-id="2c66d-192">Popis</span><span class="sxs-lookup"><span data-stu-id="2c66d-192">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2c66d-193">Syslog</span><span class="sxs-lookup"><span data-stu-id="2c66d-193">Syslog</span></span> |<span data-ttu-id="2c66d-194">Všechny systémové protokoly.</span><span class="sxs-lookup"><span data-stu-id="2c66d-194">All Syslogs.</span></span> |
| <span data-ttu-id="2c66d-195">Syslog &#124; kde úroveň závažnosti == "error.</span><span class="sxs-lookup"><span data-stu-id="2c66d-195">Syslog &#124; where SeverityLevel == "error"</span></span> |<span data-ttu-id="2c66d-196">Všechny záznamy Syslog s závažnosti chyby.</span><span class="sxs-lookup"><span data-stu-id="2c66d-196">All Syslog records with severity of error.</span></span> |
| <span data-ttu-id="2c66d-197">Syslog &#124; shrnout AggregatedValue = count() počítačem.</span><span class="sxs-lookup"><span data-stu-id="2c66d-197">Syslog &#124; summarize AggregatedValue = count() by Computer</span></span> |<span data-ttu-id="2c66d-198">Počet Syslog záznamy počítačem.</span><span class="sxs-lookup"><span data-stu-id="2c66d-198">Count of Syslog records by computer.</span></span> |
| <span data-ttu-id="2c66d-199">Syslog &#124; shrnout AggregatedValue = count() podle zařízení</span><span class="sxs-lookup"><span data-stu-id="2c66d-199">Syslog &#124; summarize AggregatedValue = count() by Facility</span></span> |<span data-ttu-id="2c66d-200">Počet Syslog záznamy podle budovy.</span><span class="sxs-lookup"><span data-stu-id="2c66d-200">Count of Syslog records by facility.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2c66d-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c66d-201">Next steps</span></span>
* <span data-ttu-id="2c66d-202">Další informace o [protokolu hledání](log-analytics-log-searches.md) tooanalyze hello data budou shromažďovat ze zdroje dat a řešení.</span><span class="sxs-lookup"><span data-stu-id="2c66d-202">Learn about [log searches](log-analytics-log-searches.md) tooanalyze hello data collected from data sources and solutions.</span></span>
* <span data-ttu-id="2c66d-203">Použití [vlastní pole](log-analytics-custom-fields.md) tooparse data z syslog záznamů do jednotlivých polí.</span><span class="sxs-lookup"><span data-stu-id="2c66d-203">Use [Custom Fields](log-analytics-custom-fields.md) tooparse data from syslog records into individual fields.</span></span>
* <span data-ttu-id="2c66d-204">[Konfigurace agentů Linux](log-analytics-linux-agents.md) toocollect jiné datové typy.</span><span class="sxs-lookup"><span data-stu-id="2c66d-204">[Configure Linux agents](log-analytics-linux-agents.md) toocollect other types of data.</span></span>
