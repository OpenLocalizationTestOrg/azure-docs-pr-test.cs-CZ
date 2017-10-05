---
title: "Shromažďovat a analyzovat zprávy Syslog v OMS Log Analytics | Microsoft Docs"
description: "Syslog je protokol protokolování událostí, které je běžné Linux. Tento článek popisuje, jak nakonfigurovat kolekce zprávy Syslog v analýzy protokolů a podrobnosti záznamů, které vytvoří v úložišti OMS."
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
ms.openlocfilehash: 7513f405d5c7c05a8e6e2b7b0e6313f23a319c84
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="syslog-data-sources-in-log-analytics"></a><span data-ttu-id="3196b-104">Syslog zdroje dat v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="3196b-104">Syslog data sources in Log Analytics</span></span>
<span data-ttu-id="3196b-105">Syslog je protokol protokolování událostí, které je běžné Linux.</span><span class="sxs-lookup"><span data-stu-id="3196b-105">Syslog is an event logging protocol that is common to Linux.</span></span>  <span data-ttu-id="3196b-106">Aplikace bude odesílat zprávy, které mohou být uloženy v místním počítači nebo doručit do kolekce Syslog.</span><span class="sxs-lookup"><span data-stu-id="3196b-106">Applications will send messages that may be stored on the local machine or delivered to a Syslog collector.</span></span>  <span data-ttu-id="3196b-107">Pokud je nainstalován Agent OMS pro Linux, nakonfiguruje místní démon procesu Syslog předávání zpráv do agenta.</span><span class="sxs-lookup"><span data-stu-id="3196b-107">When the OMS Agent for Linux is installed, it configures the local Syslog daemon to forward messages to the agent.</span></span>  <span data-ttu-id="3196b-108">Agent pak odešle zprávu k analýze protokolů, které se vytvoří odpovídající záznam v úložišti OMS.</span><span class="sxs-lookup"><span data-stu-id="3196b-108">The agent then sends the message to Log Analytics where a corresponding record is created in the OMS repository.</span></span>  

> [!NOTE]
> <span data-ttu-id="3196b-109">Analýzy protokolů podporuje kolekce zprávy odeslané rsyslog nebo syslog ng, kde je rsyslog démon výchozí.</span><span class="sxs-lookup"><span data-stu-id="3196b-109">Log Analytics supports collection of messages sent by rsyslog or syslog-ng, where rsyslog is the default daemon.</span></span> <span data-ttu-id="3196b-110">Démon procesu syslog výchozí na verze 5 Red Hat Enterprise Linux a CentOS, Oracle Linux verze (sysklog) není podporována pro shromažďování událostí syslog.</span><span class="sxs-lookup"><span data-stu-id="3196b-110">The default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="3196b-111">Ke shromažďování dat syslog z této verze těchto distribuce [rsyslog démon](http://rsyslog.com) musí být nainstalovaná a nakonfigurovaná nahradit sysklog.</span><span class="sxs-lookup"><span data-stu-id="3196b-111">To collect syslog data from this version of these distributions, the [rsyslog daemon](http://rsyslog.com) should be installed and configured to replace sysklog.</span></span>
>
>

![Kolekce Syslog](media/log-analytics-data-sources-syslog/overview.png)

## <a name="configuring-syslog"></a><span data-ttu-id="3196b-113">Konfigurace procesu Syslog</span><span class="sxs-lookup"><span data-stu-id="3196b-113">Configuring Syslog</span></span>
<span data-ttu-id="3196b-114">OMS agenta pro Linux bude pouze shromažďovat události se zařízením a závažnosti, které jsou určené v konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="3196b-114">The OMS Agent for Linux will only collect events with the facilities and severities that are specified in its configuration.</span></span>  <span data-ttu-id="3196b-115">Můžete nakonfigurovat Syslog prostřednictvím portálu OMS nebo Správa konfiguračních souborů na agenty systému Linux.</span><span class="sxs-lookup"><span data-stu-id="3196b-115">You can configure Syslog through the OMS portal or by managing configuration files on your Linux agents.</span></span>

### <a name="configure-syslog-in-the-oms-portal"></a><span data-ttu-id="3196b-116">Konfigurovat Syslog na portálu OMS</span><span class="sxs-lookup"><span data-stu-id="3196b-116">Configure Syslog in the OMS portal</span></span>
<span data-ttu-id="3196b-117">Konfigurace Syslog z [nabídce Data v nastavení analýzy protokolů](log-analytics-data-sources.md#configuring-data-sources).</span><span class="sxs-lookup"><span data-stu-id="3196b-117">Configure Syslog from the [Data menu in Log Analytics Settings](log-analytics-data-sources.md#configuring-data-sources).</span></span>  <span data-ttu-id="3196b-118">Tato konfigurace se doručí do konfiguračního souboru na každého agenta systému Linux.</span><span class="sxs-lookup"><span data-stu-id="3196b-118">This configuration is delivered to the configuration file on each Linux agent.</span></span>

<span data-ttu-id="3196b-119">Můžete přidat nové zařízení zadáním v jeho název a kliknutím na tlačítko  **+** .</span><span class="sxs-lookup"><span data-stu-id="3196b-119">You can add a new facility by typing in its name and clicking **+**.</span></span>  <span data-ttu-id="3196b-120">Pro každé zařízení budou shromažďovány pouze zprávy s vybranou závažnosti.</span><span class="sxs-lookup"><span data-stu-id="3196b-120">For each facility, only messages with the selected severities will be collected.</span></span>  <span data-ttu-id="3196b-121">Zkontrolujte závažnosti pro konkrétní zařízení, které chcete shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="3196b-121">Check the severities for the particular facility that you want to collect.</span></span>  <span data-ttu-id="3196b-122">Nelze poskytnout žádná další kritéria filtru zpráv.</span><span class="sxs-lookup"><span data-stu-id="3196b-122">You cannot provide any additional criteria to filter messages.</span></span>

![Konfigurace procesu Syslog](media/log-analytics-data-sources-syslog/configure.png)

<span data-ttu-id="3196b-124">Ve výchozím nastavení všechny změny konfigurace automaticky odesílají na všechny agenty.</span><span class="sxs-lookup"><span data-stu-id="3196b-124">By default, all configuration changes are automatically pushed to all agents.</span></span>  <span data-ttu-id="3196b-125">Pokud chcete ručně nakonfigurovat Syslog na každého agenta systému Linux, poté zrušte zaškrtnutí políčka *použít dole uvedenou konfiguraci u mých Linuxových počítačů*.</span><span class="sxs-lookup"><span data-stu-id="3196b-125">If you want to configure Syslog manually on each Linux agent, then uncheck the box *Apply below configuration to my Linux machines*.</span></span>

### <a name="configure-syslog-on-linux-agent"></a><span data-ttu-id="3196b-126">Konfigurace Syslog na agenta systému Linux</span><span class="sxs-lookup"><span data-stu-id="3196b-126">Configure Syslog on Linux agent</span></span>
<span data-ttu-id="3196b-127">Když [Linux klienta je nainstalován OMS agent](log-analytics-linux-agents.md), nainstaluje výchozí syslog konfiguračního souboru, který definuje zařízením a závažnost zpráv, které byly shromážděny.</span><span class="sxs-lookup"><span data-stu-id="3196b-127">When the [OMS agent is installed on a Linux client](log-analytics-linux-agents.md), it installs a default syslog configuration file that defines the facility and severity of the messages that are collected.</span></span>  <span data-ttu-id="3196b-128">Můžete upravit tento soubor a změňte konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="3196b-128">You can modify this file to change the configuration.</span></span>  <span data-ttu-id="3196b-129">Konfigurační soubor se liší v závislosti na tom, který se klient nainstaloval démon procesu Syslog.</span><span class="sxs-lookup"><span data-stu-id="3196b-129">The configuration file is different depending on the Syslog daemon that the client has installed.</span></span>

> [!NOTE]
> <span data-ttu-id="3196b-130">Pokud chcete upravit konfiguraci syslog, je nutné restartovat démon procesu syslog změny se projeví.</span><span class="sxs-lookup"><span data-stu-id="3196b-130">If you edit the syslog configuration, you must restart the syslog daemon for the changes to take effect.</span></span>
>
>

#### <a name="rsyslog"></a><span data-ttu-id="3196b-131">rsyslog</span><span class="sxs-lookup"><span data-stu-id="3196b-131">rsyslog</span></span>
<span data-ttu-id="3196b-132">Konfigurační soubor pro rsyslog se nachází v **/etc/rsyslog.d/95-omsagent.conf**.</span><span class="sxs-lookup"><span data-stu-id="3196b-132">The configuration file for rsyslog is located at **/etc/rsyslog.d/95-omsagent.conf**.</span></span>  <span data-ttu-id="3196b-133">Níže jsou uvedeny výchozí obsah.</span><span class="sxs-lookup"><span data-stu-id="3196b-133">Its default contents are shown below.</span></span>  <span data-ttu-id="3196b-134">To shromažďuje syslog zpráv odeslaných z místního agenta pro všechna zařízení s úrovní varování nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="3196b-134">This collects syslog messages sent from the local agent for all facilities with a level of warning or higher.</span></span>

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

<span data-ttu-id="3196b-135">Budovy můžete odebrat odstraněním příslušném oddílu konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="3196b-135">You can remove a facility by removing its section of the configuration file.</span></span>  <span data-ttu-id="3196b-136">Můžete omezit závažnosti, které se shromažďují pro konkrétní zařízení úpravou položky tohoto zařízení.</span><span class="sxs-lookup"><span data-stu-id="3196b-136">You can limit the severities that are collected for a particular facility by modifying that facility's entry.</span></span>  <span data-ttu-id="3196b-137">Chcete-li omezit uživatele zařízení pro zprávy o závažnosti chyby nebo vyšší je by upravit daného řádku konfiguračního souboru pro následující:</span><span class="sxs-lookup"><span data-stu-id="3196b-137">For example, to limit the user facility to messages with a severity of error or higher you would modify that line of the configuration file to the following:</span></span>

    user.error    @127.0.0.1:25224


#### <a name="syslog-ng"></a><span data-ttu-id="3196b-138">Syslog ng</span><span class="sxs-lookup"><span data-stu-id="3196b-138">syslog-ng</span></span>
<span data-ttu-id="3196b-139">Konfigurační soubor pro syslog ng je umístění v **/etc/syslog-ng/syslog-ng.conf**.</span><span class="sxs-lookup"><span data-stu-id="3196b-139">The configuration file for syslog-ng is location at **/etc/syslog-ng/syslog-ng.conf**.</span></span>  <span data-ttu-id="3196b-140">Níže jsou uvedeny výchozí obsah.</span><span class="sxs-lookup"><span data-stu-id="3196b-140">Its default contents are shown below.</span></span>  <span data-ttu-id="3196b-141">To shromažďuje syslog zpráv odeslaných z místního agenta pro všechna zařízení a všechny závažnosti.</span><span class="sxs-lookup"><span data-stu-id="3196b-141">This collects syslog messages sent from the local agent for all facilities and all severities.</span></span>   

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

<span data-ttu-id="3196b-142">Budovy můžete odebrat odstraněním příslušném oddílu konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="3196b-142">You can remove a facility by removing its section of the configuration file.</span></span>  <span data-ttu-id="3196b-143">Můžete omezit závažnosti, které se shromažďují pro konkrétní zařízení jejich vyjmutím ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="3196b-143">You can limit the severities that are collected for a particular facility by removing them from its list.</span></span>  <span data-ttu-id="3196b-144">Chcete-li omezit uživatele zařízení pro právě upozornění a kritickou zprávy, je by upravit tento oddíl konfiguračního souboru pro následující:</span><span class="sxs-lookup"><span data-stu-id="3196b-144">For example, to limit the user facility to just alert and critical messages, you would modify that section of the configuration file to the following:</span></span>

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="collecting-data-from-additional-syslog-ports"></a><span data-ttu-id="3196b-145">Shromažďování dat z další porty Syslog</span><span class="sxs-lookup"><span data-stu-id="3196b-145">Collecting data from additional Syslog ports</span></span>
<span data-ttu-id="3196b-146">Zprávy Syslog v místním klientovi na portu 25224 naslouchá OMS agent.</span><span class="sxs-lookup"><span data-stu-id="3196b-146">The OMS agent listens for Syslog messages on the local client on port 25224.</span></span>  <span data-ttu-id="3196b-147">Pokud je nainstalován agent nástroje, použít výchozí konfigurace syslog a najít v následujícím umístění:</span><span class="sxs-lookup"><span data-stu-id="3196b-147">When the agent is installed, a default syslog configuration is applied and found in the following location:</span></span>

* <span data-ttu-id="3196b-148">Rsyslog:`/etc/rsyslog.d/95-omsagent.conf`</span><span class="sxs-lookup"><span data-stu-id="3196b-148">Rsyslog: `/etc/rsyslog.d/95-omsagent.conf`</span></span>
* <span data-ttu-id="3196b-149">Syslog-ng:`/etc/syslog-ng/syslog-ng.conf`</span><span class="sxs-lookup"><span data-stu-id="3196b-149">Syslog-ng: `/etc/syslog-ng/syslog-ng.conf`</span></span>

<span data-ttu-id="3196b-150">Číslo portu můžete změnit tak, že vytvoříte dvě konfigurační soubory: soubor konfigurace FluentD a soubor rsyslog nebo syslog ng v závislosti na instalaci démon procesu Syslog.</span><span class="sxs-lookup"><span data-stu-id="3196b-150">You can change the port number by creating two configuration files: a FluentD config file and a rsyslog-or-syslog-ng file depending on the Syslog daemon you have installed.</span></span>  

* <span data-ttu-id="3196b-151">Soubor konfigurace FluentD by měl být nový soubor umístěný ve: `/etc/opt/microsoft/omsagent/conf/omsagent.d` a nahraďte hodnotu v **port** položka se vaše vlastní číslo portu.</span><span class="sxs-lookup"><span data-stu-id="3196b-151">The FluentD config file should be a new file located in: `/etc/opt/microsoft/omsagent/conf/omsagent.d` and replace the value in the **port** entry with your custom port number.</span></span>

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

* <span data-ttu-id="3196b-152">Pro rsyslog, měli vytvořit nový soubor konfigurace umístěný v: `/etc/rsyslog.d/` a nahraďte hodnotu % SYSLOG_PORT % vaše vlastní číslo portu.</span><span class="sxs-lookup"><span data-stu-id="3196b-152">For rsyslog, you should create a new configuration file located in: `/etc/rsyslog.d/` and replace the value %SYSLOG_PORT% with your custom port number.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="3196b-153">Pokud změníte tuto hodnotu v konfiguračním souboru `95-omsagent.conf`, pokud agent použije výchozí konfiguraci bude přepsán.</span><span class="sxs-lookup"><span data-stu-id="3196b-153">If you modify this value in the configuration file `95-omsagent.conf`, it will be overwritten when the agent applies a default configuration.</span></span>
    >

        # OMS Syslog collection for workspace %WORKSPACE_ID%
        kern.warning              @127.0.0.1:%SYSLOG_PORT%
        user.warning              @127.0.0.1:%SYSLOG_PORT%
        daemon.warning            @127.0.0.1:%SYSLOG_PORT%
        auth.warning              @127.0.0.1:%SYSLOG_PORT%

* <span data-ttu-id="3196b-154">Konfigurace syslog ng by měl být upraven tak, že zkopírujete následující příklad konfigurace a přidání vlastní změny nastavení na konec syslog ng.conf konfigurační soubor umístěný ve `/etc/syslog-ng/`.</span><span class="sxs-lookup"><span data-stu-id="3196b-154">The syslog-ng config should be modified by copying the example configuration shown below and adding the custom modified settings to the end of the syslog-ng.conf configuration file located in `/etc/syslog-ng/`.</span></span>  <span data-ttu-id="3196b-155">Proveďte **není** použít výchozí štítek **% WORKSPACE_ID % _oms** nebo **% WORKSPACE_ID_OMS**, definovat vlastní popisek k lepšímu odlišení změny.</span><span class="sxs-lookup"><span data-stu-id="3196b-155">Do **not** use the default label **%WORKSPACE_ID%_oms** or **%WORKSPACE_ID_OMS**, define a custom label to help distinguish your changes.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="3196b-156">Pokud změníte výchozí hodnoty v konfiguračním souboru, budou při agent použije výchozí konfigurace přepsány.</span><span class="sxs-lookup"><span data-stu-id="3196b-156">If you modify the default values in the configuration file, they will be overwritten when the agent applies a default configuration.</span></span>
    >

        filter f_custom_filter { level(warning) and facility(auth; };
        destination d_custom_dest { udp("127.0.0.1" port(%SYSLOG_PORT%)); };
        log { source(s_src); filter(f_custom_filter); destination(d_custom_dest); };

<span data-ttu-id="3196b-157">Po dokončení změny, Syslog a OMS agent služba musí restartovat, aby se že projevily změny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3196b-157">After completing the changes, the Syslog and the OMS agent service needs to be restarted to ensure the configuration changes take effect.</span></span>   

## <a name="syslog-record-properties"></a><span data-ttu-id="3196b-158">Vlastnosti záznamu Syslog</span><span class="sxs-lookup"><span data-stu-id="3196b-158">Syslog record properties</span></span>
<span data-ttu-id="3196b-159">Zaznamenává Syslog mít typ **Syslog** a mít vlastnosti v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="3196b-159">Syslog records have a type of **Syslog** and have the properties in the following table.</span></span>

| <span data-ttu-id="3196b-160">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3196b-160">Property</span></span> | <span data-ttu-id="3196b-161">Popis</span><span class="sxs-lookup"><span data-stu-id="3196b-161">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="3196b-162">Počítač</span><span class="sxs-lookup"><span data-stu-id="3196b-162">Computer</span></span> |<span data-ttu-id="3196b-163">Počítač, který událost nebyla shromážděna z.</span><span class="sxs-lookup"><span data-stu-id="3196b-163">Computer that the event was collected from.</span></span> |
| <span data-ttu-id="3196b-164">Zařízení</span><span class="sxs-lookup"><span data-stu-id="3196b-164">Facility</span></span> |<span data-ttu-id="3196b-165">Definuje část systému, který vygeneroval zprávu.</span><span class="sxs-lookup"><span data-stu-id="3196b-165">Defines the part of the system that generated the message.</span></span> |
| <span data-ttu-id="3196b-166">HostIP</span><span class="sxs-lookup"><span data-stu-id="3196b-166">HostIP</span></span> |<span data-ttu-id="3196b-167">IP adresa systému odeslání zprávy.</span><span class="sxs-lookup"><span data-stu-id="3196b-167">IP address of the system sending the message.</span></span> |
| <span data-ttu-id="3196b-168">Název hostitele</span><span class="sxs-lookup"><span data-stu-id="3196b-168">HostName</span></span> |<span data-ttu-id="3196b-169">Název systému odeslání zprávy.</span><span class="sxs-lookup"><span data-stu-id="3196b-169">Name of the system sending the message.</span></span> |
| <span data-ttu-id="3196b-170">Úroveň závažnosti</span><span class="sxs-lookup"><span data-stu-id="3196b-170">SeverityLevel</span></span> |<span data-ttu-id="3196b-171">Úroveň závažnosti události.</span><span class="sxs-lookup"><span data-stu-id="3196b-171">Severity level of the event.</span></span> |
| <span data-ttu-id="3196b-172">SyslogMessage</span><span class="sxs-lookup"><span data-stu-id="3196b-172">SyslogMessage</span></span> |<span data-ttu-id="3196b-173">Text zprávy.</span><span class="sxs-lookup"><span data-stu-id="3196b-173">Text of the message.</span></span> |
| <span data-ttu-id="3196b-174">ID procesu</span><span class="sxs-lookup"><span data-stu-id="3196b-174">ProcessID</span></span> |<span data-ttu-id="3196b-175">ID procesu, který vygeneroval zprávu.</span><span class="sxs-lookup"><span data-stu-id="3196b-175">ID of the process that generated the message.</span></span> |
| <span data-ttu-id="3196b-176">eventTime</span><span class="sxs-lookup"><span data-stu-id="3196b-176">EventTime</span></span> |<span data-ttu-id="3196b-177">Datum a čas, která byla vygenerována událost.</span><span class="sxs-lookup"><span data-stu-id="3196b-177">Date and time that the event was generated.</span></span> |

## <a name="log-queries-with-syslog-records"></a><span data-ttu-id="3196b-178">Dotazy protokolu se záznamy Syslog</span><span class="sxs-lookup"><span data-stu-id="3196b-178">Log queries with Syslog records</span></span>
<span data-ttu-id="3196b-179">Následující tabulka obsahuje různé příklady dotazů protokolu, která načítají záznamy Syslog.</span><span class="sxs-lookup"><span data-stu-id="3196b-179">The following table provides different examples of log queries that retrieve Syslog records.</span></span>

| <span data-ttu-id="3196b-180">Dotaz</span><span class="sxs-lookup"><span data-stu-id="3196b-180">Query</span></span> | <span data-ttu-id="3196b-181">Popis</span><span class="sxs-lookup"><span data-stu-id="3196b-181">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="3196b-182">Typ = Syslog</span><span class="sxs-lookup"><span data-stu-id="3196b-182">Type=Syslog</span></span> |<span data-ttu-id="3196b-183">Všechny systémové protokoly.</span><span class="sxs-lookup"><span data-stu-id="3196b-183">All Syslogs.</span></span> |
| <span data-ttu-id="3196b-184">Typ = Syslog úroveň závažnosti chyby =</span><span class="sxs-lookup"><span data-stu-id="3196b-184">Type=Syslog SeverityLevel=error</span></span> |<span data-ttu-id="3196b-185">Všechny záznamy Syslog s závažnosti chyby.</span><span class="sxs-lookup"><span data-stu-id="3196b-185">All Syslog records with severity of error.</span></span> |
| <span data-ttu-id="3196b-186">Typ = Syslog &#124; míra count() počítačem.</span><span class="sxs-lookup"><span data-stu-id="3196b-186">Type=Syslog &#124; measure count() by Computer</span></span> |<span data-ttu-id="3196b-187">Počet Syslog záznamy počítačem.</span><span class="sxs-lookup"><span data-stu-id="3196b-187">Count of Syslog records by computer.</span></span> |
| <span data-ttu-id="3196b-188">Typ = Syslog &#124; míra count() podle zařízení</span><span class="sxs-lookup"><span data-stu-id="3196b-188">Type=Syslog &#124; measure count() by Facility</span></span> |<span data-ttu-id="3196b-189">Počet Syslog záznamy podle budovy.</span><span class="sxs-lookup"><span data-stu-id="3196b-189">Count of Syslog records by facility.</span></span> |

>[!NOTE]
> <span data-ttu-id="3196b-190">Pokud byl váš pracovní prostor upgradován na [nový dotazovací jazyk Log Analytics](log-analytics-log-search-upgrade.md), výše uvedené dotazy se změní na následující.</span><span class="sxs-lookup"><span data-stu-id="3196b-190">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then the above queries would change to the following.</span></span>

> | <span data-ttu-id="3196b-191">Dotaz</span><span class="sxs-lookup"><span data-stu-id="3196b-191">Query</span></span> | <span data-ttu-id="3196b-192">Popis</span><span class="sxs-lookup"><span data-stu-id="3196b-192">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="3196b-193">Syslog</span><span class="sxs-lookup"><span data-stu-id="3196b-193">Syslog</span></span> |<span data-ttu-id="3196b-194">Všechny systémové protokoly.</span><span class="sxs-lookup"><span data-stu-id="3196b-194">All Syslogs.</span></span> |
| <span data-ttu-id="3196b-195">Syslog &#124; kde úroveň závažnosti == "error.</span><span class="sxs-lookup"><span data-stu-id="3196b-195">Syslog &#124; where SeverityLevel == "error"</span></span> |<span data-ttu-id="3196b-196">Všechny záznamy Syslog s závažnosti chyby.</span><span class="sxs-lookup"><span data-stu-id="3196b-196">All Syslog records with severity of error.</span></span> |
| <span data-ttu-id="3196b-197">Syslog &#124; shrnout AggregatedValue = count() počítačem.</span><span class="sxs-lookup"><span data-stu-id="3196b-197">Syslog &#124; summarize AggregatedValue = count() by Computer</span></span> |<span data-ttu-id="3196b-198">Počet Syslog záznamy počítačem.</span><span class="sxs-lookup"><span data-stu-id="3196b-198">Count of Syslog records by computer.</span></span> |
| <span data-ttu-id="3196b-199">Syslog &#124; shrnout AggregatedValue = count() podle zařízení</span><span class="sxs-lookup"><span data-stu-id="3196b-199">Syslog &#124; summarize AggregatedValue = count() by Facility</span></span> |<span data-ttu-id="3196b-200">Počet Syslog záznamy podle budovy.</span><span class="sxs-lookup"><span data-stu-id="3196b-200">Count of Syslog records by facility.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3196b-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3196b-201">Next steps</span></span>
* <span data-ttu-id="3196b-202">Další informace o [protokolu hledání](log-analytics-log-searches.md) analyzovat data shromážděná ze zdrojů dat a řešení.</span><span class="sxs-lookup"><span data-stu-id="3196b-202">Learn about [log searches](log-analytics-log-searches.md) to analyze the data collected from data sources and solutions.</span></span>
* <span data-ttu-id="3196b-203">Použití [vlastní pole](log-analytics-custom-fields.md) k analýze dat z syslog záznamů do jednotlivých polí.</span><span class="sxs-lookup"><span data-stu-id="3196b-203">Use [Custom Fields](log-analytics-custom-fields.md) to parse data from syslog records into individual fields.</span></span>
* <span data-ttu-id="3196b-204">[Konfigurace agentů Linux](log-analytics-linux-agents.md) ke shromažďování dalších typů dat.</span><span class="sxs-lookup"><span data-stu-id="3196b-204">[Configure Linux agents](log-analytics-linux-agents.md) to collect other types of data.</span></span>
