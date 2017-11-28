---
redirect_url: /azure/log-analytics/log-analytics-agent-linux
redirect_document_id: True
ROBOTS: NOINDEX
ms.openlocfilehash: 8b526144cd565f6750368e12970f008e66cc2023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-linux-computers-toolog-analytics"></a><span data-ttu-id="7b069-101">Připojení počítače tooLog vaše Linux Analytics</span><span class="sxs-lookup"><span data-stu-id="7b069-101">Connect your Linux computers tooLog Analytics</span></span>
<span data-ttu-id="7b069-102">Pomocí analýzy protokolů, můžete shromažďovat a fungovat na informace shromážděné z počítače se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="7b069-102">Using Log Analytics, you can collect and act on data generated from Linux computers.</span></span> <span data-ttu-id="7b069-103">Přidání data shromážděná z Linux tooOMS umožňuje toomanage systémy Linux a kontejner řešení jako Docker, bez ohledu na to, kde jsou počítače umístěny – prakticky odkudkoli.</span><span class="sxs-lookup"><span data-stu-id="7b069-103">Adding data collected from Linux tooOMS allows you toomanage Linux systems and container solutions like Docker, regardless of where your computers are located — virtually anywhere.</span></span> <span data-ttu-id="7b069-104">Zdroje dat může nacházet ve vašem datovém centru místní jako fyzické servery, virtuální počítače ve službě hostovaných v cloudu jako Amazon Web Services (AWS) nebo Microsoft Azure, nebo i hello přenosných počítačů na vaše oddělení podpory.</span><span class="sxs-lookup"><span data-stu-id="7b069-104">Data sources might reside in your on-premises datacenter as physical servers, virtual computers in a cloud-hosted service like Amazon Web Services (AWS) or Microsoft Azure, or even hello laptop on your desk.</span></span> <span data-ttu-id="7b069-105">Kromě toho OMS taky shromažďuje data z počítače se systémem Windows podobně, takže podporuje hybridním skutečně IT prostředí.</span><span class="sxs-lookup"><span data-stu-id="7b069-105">In addition, OMS also collects data from Windows computers similarly, so it supports a truly hybrid IT environment.</span></span>

<span data-ttu-id="7b069-106">Můžete zobrazit a spravovat data ze všech těchto zdrojů s analýzy protokolů v OMS pomocí portálu správy.</span><span class="sxs-lookup"><span data-stu-id="7b069-106">You can view and manage data from all of those sources with Log Analytics in OMS with a single management portal.</span></span> <span data-ttu-id="7b069-107">To snižuje nutnost hello toomonitor pomocí jinými systémy, umožňuje snadno tooconsume, a můžete exportovat žádná data, jako řešení toowhatever obchodní analýzy nebo systém, který už máte.</span><span class="sxs-lookup"><span data-stu-id="7b069-107">This reduces hello need toomonitor it using many different systems, makes it easy tooconsume, and you can export any data you like toowhatever business analytics solution or system that you already have.</span></span>

<span data-ttu-id="7b069-108">Tento článek je úvodní příručce, která vám pomůže shromažďovat a spravovat data pro počítače Linux pomocí hello OMS agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="7b069-108">This article is a quick start guide that will help you collect and manage data for your Linux computers using hello OMS Agent for Linux.</span></span> <span data-ttu-id="7b069-109">Další technické podrobnosti jako konfiguraci proxy serveru, informace o CollectD metriky a vlastní zdroje dat JSON najdete tyto informace v [OMS agenta pro Linux přehled](https://github.com/Microsoft/OMS-Agent-for-Linux) a [OMS agenta pro Linux úplné dokumentace](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="7b069-109">For more technical details such as proxy server configuration, information about CollectD metrics, and custom JSON data sources, you’ll find that information at [OMS Agent for Linux overview](https://github.com/Microsoft/OMS-Agent-for-Linux) and [OMS Agent for Linux full documentation](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) on GitHub.</span></span>

<span data-ttu-id="7b069-110">V současné době můžete shromáždit hello následujících typů dat z počítače se systémem Linux:</span><span class="sxs-lookup"><span data-stu-id="7b069-110">Currently, you can collect hello following types of data from Linux computers:</span></span>

* <span data-ttu-id="7b069-111">Metriky výkonu</span><span class="sxs-lookup"><span data-stu-id="7b069-111">Performance metrics</span></span>
* <span data-ttu-id="7b069-112">Události procesu Syslog</span><span class="sxs-lookup"><span data-stu-id="7b069-112">Syslog events</span></span>
* <span data-ttu-id="7b069-113">Výstrahy od Nagios a Zabbix</span><span class="sxs-lookup"><span data-stu-id="7b069-113">Alerts from Nagios and Zabbix</span></span>
* <span data-ttu-id="7b069-114">Metriky výkonu kontejner docker, inventář a protokoly</span><span class="sxs-lookup"><span data-stu-id="7b069-114">Docker container performance metrics, inventory and logs</span></span>

## <a name="supported-linux-versions"></a><span data-ttu-id="7b069-115">Podporované verze systému Linux</span><span class="sxs-lookup"><span data-stu-id="7b069-115">Supported Linux versions</span></span>
<span data-ttu-id="7b069-116">Verze x86 a x64 jsou oficiálně podporované pro řadu distribucí systému Linux.</span><span class="sxs-lookup"><span data-stu-id="7b069-116">Both x86 and x64 versions are officially supported on a variety of Linux distributions.</span></span> <span data-ttu-id="7b069-117">Hello OMS agenta pro Linux mohou však spustit také na dalších distribuce, které nejsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="7b069-117">However, hello OMS Agent for Linux might also run on other distributions not listed.</span></span>

* <span data-ttu-id="7b069-118">Linux Amazon 2012.09 prostřednictvím 2015.09</span><span class="sxs-lookup"><span data-stu-id="7b069-118">Amazon Linux 2012.09 through 2015.09</span></span>
* <span data-ttu-id="7b069-119">CentOS Linux 5, 6 a 7</span><span class="sxs-lookup"><span data-stu-id="7b069-119">CentOS Linux 5, 6, and 7</span></span>
* <span data-ttu-id="7b069-120">Oracle Linux 5, 6 a 7</span><span class="sxs-lookup"><span data-stu-id="7b069-120">Oracle Linux 5, 6, and 7</span></span>
* <span data-ttu-id="7b069-121">Red Hat Enterprise Linux Server 5, 6 a 7</span><span class="sxs-lookup"><span data-stu-id="7b069-121">Red Hat Enterprise Linux Server 5, 6 and 7</span></span>
* <span data-ttu-id="7b069-122">Debian GNU/Linux 6, 7 a 8</span><span class="sxs-lookup"><span data-stu-id="7b069-122">Debian GNU/Linux 6, 7, and 8</span></span>
* <span data-ttu-id="7b069-123">Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10</span><span class="sxs-lookup"><span data-stu-id="7b069-123">Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10</span></span>
* <span data-ttu-id="7b069-124">SUSE Linux Enterprise Server 11 a 12</span><span class="sxs-lookup"><span data-stu-id="7b069-124">SUSE Linux Enterprise Server 11 and 12</span></span>

## <a name="oms-agent-for-linux"></a><span data-ttu-id="7b069-125">OMS agenta pro Linux</span><span class="sxs-lookup"><span data-stu-id="7b069-125">OMS Agent for Linux</span></span>
<span data-ttu-id="7b069-126">Hello Operations Management Suite agenta pro Linux obsahuje více balíčků.</span><span class="sxs-lookup"><span data-stu-id="7b069-126">hello Operations Management Suite Agent for Linux comprises multiple packages.</span></span> <span data-ttu-id="7b069-127">Hello verze souboru obsahuje následující balíčky, které jsou k dispozici ve spuštěné sadě hello prostředí s hello `--extract`.</span><span class="sxs-lookup"><span data-stu-id="7b069-127">hello release file contains hello following packages, available by running hello shell bundle with `--extract`.</span></span>

| <span data-ttu-id="7b069-128">**Balíček**</span><span class="sxs-lookup"><span data-stu-id="7b069-128">**Package**</span></span> | <span data-ttu-id="7b069-129">**Verze**</span><span class="sxs-lookup"><span data-stu-id="7b069-129">**Version**</span></span> | <span data-ttu-id="7b069-130">**Popis**</span><span class="sxs-lookup"><span data-stu-id="7b069-130">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7b069-131">omsagent</span><span class="sxs-lookup"><span data-stu-id="7b069-131">omsagent</span></span> |<span data-ttu-id="7b069-132">1.1.0</span><span class="sxs-lookup"><span data-stu-id="7b069-132">1.1.0</span></span> |<span data-ttu-id="7b069-133">Hello Operations Management Suite agenta pro Linux</span><span class="sxs-lookup"><span data-stu-id="7b069-133">hello Operations Management Suite Agent for Linux</span></span> |
| <span data-ttu-id="7b069-134">omsconfig</span><span class="sxs-lookup"><span data-stu-id="7b069-134">omsconfig</span></span> |<span data-ttu-id="7b069-135">1.1.1</span><span class="sxs-lookup"><span data-stu-id="7b069-135">1.1.1</span></span> |<span data-ttu-id="7b069-136">Konfigurace agenta pro hello agenta OMS</span><span class="sxs-lookup"><span data-stu-id="7b069-136">Configuration agent for hello OMS Agent</span></span> |
| <span data-ttu-id="7b069-137">OMI</span><span class="sxs-lookup"><span data-stu-id="7b069-137">omi</span></span> |<span data-ttu-id="7b069-138">1.0.8.3</span><span class="sxs-lookup"><span data-stu-id="7b069-138">1.0.8.3</span></span> |<span data-ttu-id="7b069-139">Infrastruktury Open Management Infrastructure (OMI) – prosté Server CIM</span><span class="sxs-lookup"><span data-stu-id="7b069-139">Open Management Infrastructure (OMI) -- a lightweight CIM Server</span></span> |
| <span data-ttu-id="7b069-140">scx.</span><span class="sxs-lookup"><span data-stu-id="7b069-140">scx</span></span> |<span data-ttu-id="7b069-141">1.6.2</span><span class="sxs-lookup"><span data-stu-id="7b069-141">1.6.2</span></span> |<span data-ttu-id="7b069-142">Zprostředkovatelé CIM OMI pro metriku výkonu operačního systému</span><span class="sxs-lookup"><span data-stu-id="7b069-142">OMI CIM Providers for operating system performance metrics</span></span> |
| <span data-ttu-id="7b069-143">Apache cimprov</span><span class="sxs-lookup"><span data-stu-id="7b069-143">apache-cimprov</span></span> |<span data-ttu-id="7b069-144">1.0.0</span><span class="sxs-lookup"><span data-stu-id="7b069-144">1.0.0</span></span> |<span data-ttu-id="7b069-145">Zprostředkovatel pro OMI monitorování výkonu serveru Apache HTTP Server.</span><span class="sxs-lookup"><span data-stu-id="7b069-145">Apache HTTP Server performance monitoring provider for OMI.</span></span> <span data-ttu-id="7b069-146">Instalovat pouze v případě zjištění serveru Apache HTTP Server.</span><span class="sxs-lookup"><span data-stu-id="7b069-146">Only installed if Apache HTTP Server is detected.</span></span> |
| <span data-ttu-id="7b069-147">MySQL cimprov</span><span class="sxs-lookup"><span data-stu-id="7b069-147">mysql-cimprov</span></span> |<span data-ttu-id="7b069-148">1.0.0</span><span class="sxs-lookup"><span data-stu-id="7b069-148">1.0.0</span></span> |<span data-ttu-id="7b069-149">Výkon serveru MySQL monitorování zprostředkovatele pro OMI.</span><span class="sxs-lookup"><span data-stu-id="7b069-149">MySQL Server performance monitoring provider for OMI.</span></span> <span data-ttu-id="7b069-150">Instalovat pouze v případě zjištění MySQL nebo MariaDB serveru.</span><span class="sxs-lookup"><span data-stu-id="7b069-150">Only installed if MySQL/MariaDB server is detected.</span></span> |
| <span data-ttu-id="7b069-151">docker cimprov</span><span class="sxs-lookup"><span data-stu-id="7b069-151">docker-cimprov</span></span> |<span data-ttu-id="7b069-152">0.1.0</span><span class="sxs-lookup"><span data-stu-id="7b069-152">0.1.0</span></span> |<span data-ttu-id="7b069-153">Zprostředkovatel docker pro OMI.</span><span class="sxs-lookup"><span data-stu-id="7b069-153">Docker provider for OMI.</span></span> <span data-ttu-id="7b069-154">Instalovat pouze v případě zjištění Docker.</span><span class="sxs-lookup"><span data-stu-id="7b069-154">Only installed if Docker is detected.</span></span> |

### <a name="additional-installation-artifacts"></a><span data-ttu-id="7b069-155">Artefakty další instalace</span><span class="sxs-lookup"><span data-stu-id="7b069-155">Additional installation artifacts</span></span>
<span data-ttu-id="7b069-156">Po instalaci agenta OMS hello pro balíčky Linux, hello následující další systémové změny konfigurace se použijí.</span><span class="sxs-lookup"><span data-stu-id="7b069-156">After installing hello OMS agent for Linux packages, hello following additional system-wide configuration changes are applied.</span></span> <span data-ttu-id="7b069-157">Tyto artefakty se odeberou, když hello omsagent balíček odinstalován.</span><span class="sxs-lookup"><span data-stu-id="7b069-157">These artifacts are removed when hello omsagent package is uninstalled.</span></span>

* <span data-ttu-id="7b069-158">Bez oprávnění uživatele s názvem: `omsagent` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="7b069-158">A non-privileged user named: `omsagent` is created.</span></span> <span data-ttu-id="7b069-159">Toto je hello účet hello omsagent démon běží jako</span><span class="sxs-lookup"><span data-stu-id="7b069-159">This is hello account hello omsagent daemon runs as</span></span>
* <span data-ttu-id="7b069-160">Vytvoření souboru sudoers "zahrnout" v /etc/sudoers.d/omsagent to autorizuje omsagent toorestart hello syslog a omsagent démoni.</span><span class="sxs-lookup"><span data-stu-id="7b069-160">A sudoers “include” file is created at /etc/sudoers.d/omsagent This authorizes omsagent toorestart hello syslog and omsagent daemons.</span></span> <span data-ttu-id="7b069-161">Pokud direktivy "zahrnout" sudo nejsou podporovány v hello nainstalované verzi sudo, tyto položky budou zapsány příliš/etc/sudoers.</span><span class="sxs-lookup"><span data-stu-id="7b069-161">If sudo “include” directives are not supported in hello installed version of sudo, these entries will be written too/etc/sudoers.</span></span>
* <span data-ttu-id="7b069-162">Konfigurace syslog Hello je upravený tooforward podmnožinu události toohello agenta.</span><span class="sxs-lookup"><span data-stu-id="7b069-162">hello syslog configuration is modified tooforward a subset of events toohello agent.</span></span> <span data-ttu-id="7b069-163">Další informace najdete v tématu hello **konfigurace shromažďování dat** části</span><span class="sxs-lookup"><span data-stu-id="7b069-163">For more information, see hello **Configuring Data Collection** section below</span></span>

### <a name="linux-data-collection-details"></a><span data-ttu-id="7b069-164">Podrobnosti kolekce dat Linux</span><span class="sxs-lookup"><span data-stu-id="7b069-164">Linux data collection details</span></span>
<span data-ttu-id="7b069-165">Hello následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují.</span><span class="sxs-lookup"><span data-stu-id="7b069-165">hello following table shows data collection methods and other details about how data is collected.</span></span>

| <span data-ttu-id="7b069-166">Zdroj</span><span class="sxs-lookup"><span data-stu-id="7b069-166">source</span></span> | <span data-ttu-id="7b069-167">Přímé agenta</span><span class="sxs-lookup"><span data-stu-id="7b069-167">Direct Agent</span></span> | <span data-ttu-id="7b069-168">Agenta nástroje SCOM</span><span class="sxs-lookup"><span data-stu-id="7b069-168">SCOM agent</span></span> | <span data-ttu-id="7b069-169">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="7b069-169">Azure Storage</span></span> | <span data-ttu-id="7b069-170">SCOM vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="7b069-170">SCOM required?</span></span> | <span data-ttu-id="7b069-171">Data agenta SCOM odeslána prostřednictvím skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="7b069-171">SCOM agent data sent via management group</span></span> | <span data-ttu-id="7b069-172">Frekvence kolekce</span><span class="sxs-lookup"><span data-stu-id="7b069-172">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="7b069-173">Zabbix</span><span class="sxs-lookup"><span data-stu-id="7b069-173">Zabbix</span></span> |![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="7b069-179">1 minuta</span><span class="sxs-lookup"><span data-stu-id="7b069-179">1 minute</span></span> |
| <span data-ttu-id="7b069-180">Nagios</span><span class="sxs-lookup"><span data-stu-id="7b069-180">Nagios</span></span> |![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="7b069-186">v případě přijetí</span><span class="sxs-lookup"><span data-stu-id="7b069-186">on arrival</span></span> |
| <span data-ttu-id="7b069-187">syslog</span><span class="sxs-lookup"><span data-stu-id="7b069-187">syslog</span></span> |![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="7b069-193">ze služby Azure storage: 10 minut; z agenta: na přijetí</span><span class="sxs-lookup"><span data-stu-id="7b069-193">from Azure storage: 10 minutes; from agent: on arrival</span></span> |
| <span data-ttu-id="7b069-194">Čítače výkonu systému Linux</span><span class="sxs-lookup"><span data-stu-id="7b069-194">Linux performance counters</span></span> |![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="7b069-200">podle plánu, minimálně 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="7b069-200">As scheduled, minimum of 10 seconds</span></span> |
| <span data-ttu-id="7b069-201">sledování změn</span><span class="sxs-lookup"><span data-stu-id="7b069-201">change tracking</span></span> |![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="7b069-207">každou hodinu</span><span class="sxs-lookup"><span data-stu-id="7b069-207">hourly</span></span> |

### <a name="package-requirements"></a><span data-ttu-id="7b069-208">Požadavky na balíček</span><span class="sxs-lookup"><span data-stu-id="7b069-208">Package Requirements</span></span>
| <span data-ttu-id="7b069-209">**Požadovaný balíček**</span><span class="sxs-lookup"><span data-stu-id="7b069-209">**Required package**</span></span> | <span data-ttu-id="7b069-210">**Popis**</span><span class="sxs-lookup"><span data-stu-id="7b069-210">**Description**</span></span> | <span data-ttu-id="7b069-211">**Minimální verze**</span><span class="sxs-lookup"><span data-stu-id="7b069-211">**Minimum version**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7b069-212">Glibc</span><span class="sxs-lookup"><span data-stu-id="7b069-212">Glibc</span></span> |<span data-ttu-id="7b069-213">Knihovna GNU C</span><span class="sxs-lookup"><span data-stu-id="7b069-213">GNU C library</span></span> |<span data-ttu-id="7b069-214">2.5-12</span><span class="sxs-lookup"><span data-stu-id="7b069-214">2.5-12</span></span> |
| <span data-ttu-id="7b069-215">OpenSSL</span><span class="sxs-lookup"><span data-stu-id="7b069-215">Openssl</span></span> |<span data-ttu-id="7b069-216">Knihovny OpenSSL</span><span class="sxs-lookup"><span data-stu-id="7b069-216">OpenSSL libraries</span></span> |<span data-ttu-id="7b069-217">0.9.8E nebo 1.0</span><span class="sxs-lookup"><span data-stu-id="7b069-217">0.9.8e or 1.0</span></span> |
| <span data-ttu-id="7b069-218">Curl</span><span class="sxs-lookup"><span data-stu-id="7b069-218">Curl</span></span> |<span data-ttu-id="7b069-219">cURL webového klienta</span><span class="sxs-lookup"><span data-stu-id="7b069-219">cURL web client</span></span> |<span data-ttu-id="7b069-220">7.15.5</span><span class="sxs-lookup"><span data-stu-id="7b069-220">7.15.5</span></span> |
| <span data-ttu-id="7b069-221">Python ctypes</span><span class="sxs-lookup"><span data-stu-id="7b069-221">Python-ctypes</span></span> |<span data-ttu-id="7b069-222">Funkce knihovny</span><span class="sxs-lookup"><span data-stu-id="7b069-222">function libraries</span></span> |<span data-ttu-id="7b069-223">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="7b069-223">n/a</span></span> |
| <span data-ttu-id="7b069-224">PAM</span><span class="sxs-lookup"><span data-stu-id="7b069-224">PAM</span></span> |<span data-ttu-id="7b069-225">PAM moduly</span><span class="sxs-lookup"><span data-stu-id="7b069-225">Pluggable authentication Modules</span></span> |<span data-ttu-id="7b069-226">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="7b069-226">n/a</span></span> |

> [!NOTE]
> <span data-ttu-id="7b069-227">Rsyslog nebo syslog ng jsou požadované toocollect zprávy syslog.</span><span class="sxs-lookup"><span data-stu-id="7b069-227">Either rsyslog or syslog-ng are required toocollect syslog messages.</span></span> <span data-ttu-id="7b069-228">Démon procesu syslog výchozí Hello na verze 5 Red Hat Enterprise Linux a CentOS, Oracle Linux verze (sysklog) není podporována pro shromažďování událostí syslog.</span><span class="sxs-lookup"><span data-stu-id="7b069-228">hello default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="7b069-229">toocollect syslog data z této verze těchto distribuce hello rsyslog démon by měl být nainstalován a nakonfigurován tooreplace sysklog.</span><span class="sxs-lookup"><span data-stu-id="7b069-229">toocollect syslog data from this version of these distributions, hello rsyslog daemon should be installed and configured tooreplace sysklog.</span></span>
>
>

## <a name="quick-install"></a><span data-ttu-id="7b069-230">Rychlé instalace</span><span class="sxs-lookup"><span data-stu-id="7b069-230">Quick install</span></span>
<span data-ttu-id="7b069-231">Spusťte následující příkazy toodownload hello omsagent hello, ověření kontrolního součtu hello, pak instalace a zařadit hello agenta.</span><span class="sxs-lookup"><span data-stu-id="7b069-231">Run hello following commands toodownload hello omsagent, validate hello checksum, then  install and onboard hello agent.</span></span> <span data-ttu-id="7b069-232">Příkazy jsou pro 64bitovou verzi.</span><span class="sxs-lookup"><span data-stu-id="7b069-232">Commands are for 64-bit.</span></span> <span data-ttu-id="7b069-233">Hello ID pracovního prostoru a primární klíč se nacházejí na portálu OMS hello pod **nastavení** na hello **připojené zdroje** kartě.</span><span class="sxs-lookup"><span data-stu-id="7b069-233">hello Workspace ID and Primary Key are found in hello OMS portal under **Settings** on hello **Connected Sources** tab.</span></span>

![Podrobnosti o pracovním prostoru](./media/log-analytics-linux-agents/oms-direct-agent-primary-key.png)

```
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR OMS WORKSPACE ID> -s <YOUR OMS WORKSPACE PRIMARY KEY>
```

<span data-ttu-id="7b069-235">Existují různé další metody tooinstall hello agenta a upgradujte ho.</span><span class="sxs-lookup"><span data-stu-id="7b069-235">There are a variety of other methods tooinstall hello agent and upgrade it.</span></span> <span data-ttu-id="7b069-236">Další informace o nich v [kroky tooinstall hello OMS agenta pro Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux).</span><span class="sxs-lookup"><span data-stu-id="7b069-236">You can read more about them at [Steps tooinstall hello OMS Agent for Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux).</span></span>

<span data-ttu-id="7b069-237">Můžete také zobrazit hello [Azure video s návodem](https://www.youtube.com/watch?v=mF1wtHPEzT0).</span><span class="sxs-lookup"><span data-stu-id="7b069-237">You can also view hello [Azure video walkthrough](https://www.youtube.com/watch?v=mF1wtHPEzT0).</span></span>

## <a name="choose-your-linux-data-collection-method"></a><span data-ttu-id="7b069-238">Zvolte metodu kolekce dat Linux</span><span class="sxs-lookup"><span data-stu-id="7b069-238">Choose your Linux data collection method</span></span>
<span data-ttu-id="7b069-239">Jak vybrat hello datové typy, které byste jako toocollect závisí na tom, jestli se má portál OMS hello toouse, nebo pokud chcete upravit různé konfigurační soubory přímo na klienty Linux.</span><span class="sxs-lookup"><span data-stu-id="7b069-239">How you choose hello data types you'd like toocollect depends on whether you want toouse hello OMS portal or if you want edit various configuration files directly on your Linux clients.</span></span> <span data-ttu-id="7b069-240">Pokud si zvolíte toouse hello portál, konfigurace hello je odeslána tooall vaši klienti Linux.</span><span class="sxs-lookup"><span data-stu-id="7b069-240">If you choose toouse hello portal, hello configuration is sent tooall of your Linux clients automatically.</span></span> <span data-ttu-id="7b069-241">Pokud potřebujete různé konfigurace pro jiné klienty Linux, budete potřebovat soubory klienta tooedit jednotlivě – nebo použijete alternativu jako PowerShell DSC, Chef nebo Puppet.</span><span class="sxs-lookup"><span data-stu-id="7b069-241">If you need different configurations for different Linux clients, you will need tooedit client files individually – or use an alternative like PowerShell DSC, Chef, or Puppet.</span></span>

<span data-ttu-id="7b069-242">Můžete zadat hello syslog události a čítače výkonu, které chcete toocollect pomocí konfigurační soubory do počítače se systémem Linux hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-242">You can specify hello syslog events and performance counters that you want toocollect using configuration files on hello Linux computers.</span></span> <span data-ttu-id="7b069-243">*Pokud jste zvolili tooconfigure shromažďování dat úpravou konfigurační soubory agenta, měli byste zakázat hello Centralizovaná konfigurace.*</span><span class="sxs-lookup"><span data-stu-id="7b069-243">*If you chose tooconfigure data collection by editing agent configuration files, you should disable hello centralized configuration.*</span></span>  <span data-ttu-id="7b069-244">Pokyny najdete níže shromažďování dat tooconfigure hello agenta konfigurační soubory a také toodisable centrální konfigurace pro všechny agenty OMS pro Linux nebo jednotlivé počítače.</span><span class="sxs-lookup"><span data-stu-id="7b069-244">Instructions are provided below tooconfigure data collection in hello agent's configuration files as well as toodisable central configuration for all OMS Agents for Linux, or individual computers.</span></span>

### <a name="disable-oms-management-for-an-individual-linux-computer"></a><span data-ttu-id="7b069-245">Zakázat správu OMS pro jednotlivý počítač Linux</span><span class="sxs-lookup"><span data-stu-id="7b069-245">Disable OMS management for an individual Linux computer</span></span>
<span data-ttu-id="7b069-246">Centralizované shromažďování dat pro konfigurační data je zakázána pro jednotlivý počítač Linux s hello OMS_MetaConfigHelper.py skriptu.</span><span class="sxs-lookup"><span data-stu-id="7b069-246">Centralized data collection for configuration data is disabled for an individual Linux computer with hello OMS_MetaConfigHelper.py script.</span></span> <span data-ttu-id="7b069-247">To může být užitečné, pokud podmnožině počítačů by měl mít speciální konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7b069-247">This can be useful if a subset of computers should have a specialized configuration.</span></span>

<span data-ttu-id="7b069-248">Centralizovaná konfigurace toodisable:</span><span class="sxs-lookup"><span data-stu-id="7b069-248">toodisable centralized configuration:</span></span>

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```

<span data-ttu-id="7b069-249">Povolit toore Centralizovaná konfigurace:</span><span class="sxs-lookup"><span data-stu-id="7b069-249">toore-enable centralized configuration:</span></span>

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py –enable
```

## <a name="linux-performance-counters"></a><span data-ttu-id="7b069-250">Čítače výkonu systému Linux</span><span class="sxs-lookup"><span data-stu-id="7b069-250">Linux performance counters</span></span>
<span data-ttu-id="7b069-251">Čítače výkonu systému Linux jsou podobné čítače výkonu tooWindows – jak fungují podobně.</span><span class="sxs-lookup"><span data-stu-id="7b069-251">Linux performance counters are similar tooWindows performance counters—both operate similarly.</span></span> <span data-ttu-id="7b069-252">Můžete použít následující postupy tooadd hello a je nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="7b069-252">You can use hello following procedures tooadd and configure them.</span></span> <span data-ttu-id="7b069-253">Po přidání tooOMS data jsou shromažďována pro ně každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="7b069-253">After they are added tooOMS, data is collected for them every 30 seconds.</span></span>

### <a name="tooadd-a-linux-performance-counter-in-oms"></a><span data-ttu-id="7b069-254">tooadd Linux čítače výkonu v OMS</span><span class="sxs-lookup"><span data-stu-id="7b069-254">tooadd a Linux performance counter in OMS</span></span>
1. <span data-ttu-id="7b069-255">tooconfigure OMS agentů pro Linux pomocí portálu OMS hello, můžete přidat čítače výkonu Linux na stránce nastavení hello, klikněte na tlačítko **Data**.</span><span class="sxs-lookup"><span data-stu-id="7b069-255">tooconfigure OMS Agents for Linux using hello OMS portal, you can add Linux performance counters on hello Settings page, click **Data**.</span></span>  
2. <span data-ttu-id="7b069-256">Na hello **nastavení** v části **Data** , klikněte na tlačítko **čítače výkonu Linux** a pak vyberte nebo zadejte hello název čítače hello chcete tooadd.</span><span class="sxs-lookup"><span data-stu-id="7b069-256">On hello **Settings** page under **Data** , click **Linux performance counters** and then select or type hello name of hello counter you want tooadd.</span></span>  
    <span data-ttu-id="7b069-257">![data](./media/log-analytics-linux-agents/oms-settings-data01.png)</span><span class="sxs-lookup"><span data-stu-id="7b069-257">![data](./media/log-analytics-linux-agents/oms-settings-data01.png)</span></span>
3. <span data-ttu-id="7b069-258">Pokud si nejste jisti hello úplný název čítače hello, pište částečný název a zobrazí se seznam dostupných čítačů.</span><span class="sxs-lookup"><span data-stu-id="7b069-258">If you don't know hello full name of hello counter, you can start typing a partial name and a list of available counters will appear.</span></span> <span data-ttu-id="7b069-259">Když najít hello čítače můžete tooadd, klikněte na název hello hello seznamu a potom klikněte na hello plus ikonu tooadd hello čítače.</span><span class="sxs-lookup"><span data-stu-id="7b069-259">When you find hello counter you want tooadd, click hello name in hello list and then click hello plus icon tooadd hello counter.</span></span>
4. <span data-ttu-id="7b069-260">Po přidání hello čítač se zobrazí v seznamu hello čítačů zvýrazněná s barevnou panelu.</span><span class="sxs-lookup"><span data-stu-id="7b069-260">After you add hello counter, it appears in hello list of counters highlighted with a colored bar.</span></span>
5. <span data-ttu-id="7b069-261">Ve výchozím nastavení, hello **použít níže uvedených počítačích toomy konfigurace** je vybraná možnost.</span><span class="sxs-lookup"><span data-stu-id="7b069-261">By default, hello **Apply below configuration toomy machines** option is selected.</span></span> <span data-ttu-id="7b069-262">Pokud chcete toodisable odesílání konfiguračních dat, zrušte výběr hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-262">If you want toodisable sending configuration data, clear hello selection.</span></span>
6. <span data-ttu-id="7b069-263">Po dokončení změny čítače výkonu, v dolní části hello hello stránky klikněte na tlačítko **Uložit** toofinalize změny.</span><span class="sxs-lookup"><span data-stu-id="7b069-263">When you are done modifying performance counters, at hello bottom of hello page click **Save** toofinalize your changes.</span></span> <span data-ttu-id="7b069-264">Hello změny konfigurace, které jste udělali se pak odešlou tooall hello OMS agentů pro Linux, které jsou registrovány OMS, obvykle během pěti minut.</span><span class="sxs-lookup"><span data-stu-id="7b069-264">hello configuration changes that you've made are then sent tooall hello OMS Agents for Linux that are registered with OMS, typically within 5 minutes.</span></span>

### <a name="configure-linux-performance-counters-in-oms"></a><span data-ttu-id="7b069-265">Konfigurovat čítačů výkonu systému Linux v OMS</span><span class="sxs-lookup"><span data-stu-id="7b069-265">Configure Linux performance counters in OMS</span></span>
<span data-ttu-id="7b069-266">Pro čítače výkonu systému Windows můžete konkrétní instance jednotlivých čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="7b069-266">For Windows performance counters, you can choose a specific instance for each performance counter.</span></span> <span data-ttu-id="7b069-267">Pro čítače výkonu systému Linux, platí jakoukoli instanci čítače, který zvolíte, čítače podřízené tooall hello nadřazené čítače.</span><span class="sxs-lookup"><span data-stu-id="7b069-267">However, for Linux performance counters, whatever instance of a counter that you choose applies tooall child counters of hello parent counter.</span></span> <span data-ttu-id="7b069-268">Hello následující tabulka zobrazuje hello běžné instance čítače výkonu k dispozici tooboth Linux a Windows.</span><span class="sxs-lookup"><span data-stu-id="7b069-268">hello following table shows hello common instances available tooboth Linux and Windows performance counters.</span></span>

| <span data-ttu-id="7b069-269">**Název instance**</span><span class="sxs-lookup"><span data-stu-id="7b069-269">**Instance name**</span></span> | <span data-ttu-id="7b069-270">**Význam**</span><span class="sxs-lookup"><span data-stu-id="7b069-270">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="7b069-271">\_Celkový počet</span><span class="sxs-lookup"><span data-stu-id="7b069-271">\_Total</span></span> |<span data-ttu-id="7b069-272">Celkový počet všech instancí hello</span><span class="sxs-lookup"><span data-stu-id="7b069-272">Total of all hello instances</span></span> |
| \* |<span data-ttu-id="7b069-273">Všechny instance</span><span class="sxs-lookup"><span data-stu-id="7b069-273">All instances</span></span> |
| <span data-ttu-id="7b069-274">(/ &#124; / var)</span><span class="sxs-lookup"><span data-stu-id="7b069-274">(/&#124;/var)</span></span> |<span data-ttu-id="7b069-275">Odpovídá instancí s názvem: / nebo /var</span><span class="sxs-lookup"><span data-stu-id="7b069-275">Matches instances named: / or /var</span></span> |

<span data-ttu-id="7b069-276">Podobně hello ukázka interval, který vyberete pro nadřazené čítač platí tooall jeho podřízených čítače.</span><span class="sxs-lookup"><span data-stu-id="7b069-276">Similarly, hello sample interval that you choose for a parent counter applies tooall its child counters.</span></span> <span data-ttu-id="7b069-277">Jinými slovy jsou všechny intervalů vzorkování čítač podřízené hello a instance svázané společně.</span><span class="sxs-lookup"><span data-stu-id="7b069-277">In other words, all hello child counter sample intervals and instances are tied together.</span></span>

### <a name="add-and-configure-performance-metrics-with-linux"></a><span data-ttu-id="7b069-278">Přidání a konfigurace metriky výkonu operačního systému Linux</span><span class="sxs-lookup"><span data-stu-id="7b069-278">Add and configure performance metrics with Linux</span></span>
<span data-ttu-id="7b069-279">Toocollect metriky výkonu jsou řízeny hello konfigurace v/etc/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/conf/omsagent.conf.</span><span class="sxs-lookup"><span data-stu-id="7b069-279">Performance metrics toocollect are controlled by hello configuration in /etc/opt/microsoft/omsagent/&lt;workspace id&gt;/conf/omsagent.conf.</span></span> <span data-ttu-id="7b069-280">V tématu [metriky výkonu k dispozici](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) dostupných tříd a metriky pro hello OMS agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="7b069-280">See [Available performance metrics](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) for available classes and metrics for hello OMS Agent for Linux.</span></span>

<span data-ttu-id="7b069-281">Každý objekt nebo kategorii toocollect metriky výkonu by měla být definována v konfiguračním souboru hello jako jeden `<source>` elementu.</span><span class="sxs-lookup"><span data-stu-id="7b069-281">Each object, or category, of performance metrics toocollect should be defined in hello configuration file as a single `<source>` element.</span></span> <span data-ttu-id="7b069-282">Syntaxe Hello následující hello níže.</span><span class="sxs-lookup"><span data-stu-id="7b069-282">hello syntax follows hello pattern below.</span></span>

```
<source>
  type oms_omi  
  object_name "Processor"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

<span data-ttu-id="7b069-283">Konfigurovat parametry Hello tohoto elementu jsou:</span><span class="sxs-lookup"><span data-stu-id="7b069-283">hello configurable parameters of this element are:</span></span>

* <span data-ttu-id="7b069-284">**Objekt\_název**: název objektu hello hello kolekce.</span><span class="sxs-lookup"><span data-stu-id="7b069-284">**Object\_name**: hello object name for hello collection.</span></span>
* <span data-ttu-id="7b069-285">**Instance\_regex**: *regulární výraz* definování které toocollect instance.</span><span class="sxs-lookup"><span data-stu-id="7b069-285">**Instance\_regex**: a *regular expression* defining which instances toocollect.</span></span> <span data-ttu-id="7b069-286">Hello hodnota: `.*` Určuje všechny instance.</span><span class="sxs-lookup"><span data-stu-id="7b069-286">hello value: `.*` specifies all instances.</span></span> <span data-ttu-id="7b069-287">toocollect procesoru metriky pro pouze hello \_celkový počet instancí, můžete zadat `_Total`.</span><span class="sxs-lookup"><span data-stu-id="7b069-287">toocollect processor metrics for only hello \_Total instance, you could specify `_Total`.</span></span> <span data-ttu-id="7b069-288">toocollect proces metriky pro pouze hello crond nebo sshd instancí, můžete zadat: `(crond|sshd)`.</span><span class="sxs-lookup"><span data-stu-id="7b069-288">toocollect process metrics for only hello crond or sshd instances, you could specify: `(crond|sshd)`.</span></span>
* <span data-ttu-id="7b069-289">**Čítač\_název\_regex**: *regulární výraz* definování které toocollect čítače (pro objekt hello).</span><span class="sxs-lookup"><span data-stu-id="7b069-289">**Counter\_name\_regex**: a *regular expression* defining which counters (for hello object) toocollect.</span></span> <span data-ttu-id="7b069-290">Zadejte všechny čítače pro objekt hello toocollect: `.*`.</span><span class="sxs-lookup"><span data-stu-id="7b069-290">toocollect all counters for hello object, specify: `.*`.</span></span> <span data-ttu-id="7b069-291">toocollect pouze odkládacího prostoru čítače hello paměti objektu, můžete zadat:`.+Swap.+`</span><span class="sxs-lookup"><span data-stu-id="7b069-291">toocollect only swap space counters for hello memory object, you could specify: `.+Swap.+`</span></span>
* <span data-ttu-id="7b069-292">**Interval:**: hello frekvence, na které hello se shromažďují objektu čítače.</span><span class="sxs-lookup"><span data-stu-id="7b069-292">**Interval:**: hello frequency at which hello object's counters are collected.</span></span>

<span data-ttu-id="7b069-293">Hello výchozí konfiguraci pro metriku výkonu je:</span><span class="sxs-lookup"><span data-stu-id="7b069-293">hello default configuration for performance metrics is:</span></span>

```
<source>
  type oms_omi
  object_name "Physical Disk"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Logical Disk"
  instance_regex ".*
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Processor"
  instance_regex ".*
  counter_name_regex ".*"
  interval 30s
</source>

<source>
  type oms_omi
  object_name "Memory"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

### <a name="enable-mysql-performance-counters-using-linux-commands"></a><span data-ttu-id="7b069-294">Povolit MySQL čítače výkonu pomocí příkazů Linux</span><span class="sxs-lookup"><span data-stu-id="7b069-294">Enable MySQL performance counters using Linux commands</span></span>
<span data-ttu-id="7b069-295">Pokud Server databáze MySQL nebo MariaDB Server v počítači se zjistí hello při instalaci sady omsagent hello, automaticky se nainstaluje zprostředkovatele pro Server databáze MySQL monitorování výkonu.</span><span class="sxs-lookup"><span data-stu-id="7b069-295">If MySQL Server or MariaDB Server is detected on hello computer when hello omsagent bundle is installed, a performance monitoring provider for MySQL Server is automatically installed.</span></span> <span data-ttu-id="7b069-296">Tento zprostředkovatel připojí toohello místní databáze MySQL nebo MariaDB tooexpose Statistika výkonu serveru.</span><span class="sxs-lookup"><span data-stu-id="7b069-296">This provider connects toohello local MySQL/MariaDB server tooexpose performance statistics.</span></span> <span data-ttu-id="7b069-297">Budete potřebovat přihlašovací údaje uživatele tooconfigure MySQL, aby hello zprostředkovatele přístup hello MySQL serveru.</span><span class="sxs-lookup"><span data-stu-id="7b069-297">You need tooconfigure MySQL user credentials so that hello provider can access hello MySQL Server.</span></span>

<span data-ttu-id="7b069-298">toodefine výchozího uživatele účtu pro hello MySQL server na localhost, hello použijte následující příkaz Ukázka.</span><span class="sxs-lookup"><span data-stu-id="7b069-298">toodefine a default user account for hello MySQL server on localhost, use hello following command example.</span></span>

> [!NOTE]
> <span data-ttu-id="7b069-299">soubor s přihlašovacími údaji Hello musí být přečíst hello omsagent účtu.</span><span class="sxs-lookup"><span data-stu-id="7b069-299">hello credentials file must be readable by hello omsagent account.</span></span> <span data-ttu-id="7b069-300">Spuštění příkazu mycimprovauth hello jako omsgent se doporučuje.</span><span class="sxs-lookup"><span data-stu-id="7b069-300">Running hello mycimprovauth command as omsgent is recommended.</span></span>
>
>

```
sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>

sudo /opt/omi/bin/service_control restart
```


<span data-ttu-id="7b069-301">Alternativně můžete zadat hello vyžadují přihlašovací údaje databáze MySQL do souboru, vytváření souboru hello: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth. Další informace o správě MySQL přihlašovací údaje pro monitorování prostřednictvím hello mysql auth souboru, najdete v části [spravovat MySQL monitorování přihlašovací údaje v souboru authentication hello](#manage-mysql-monitoring-credentials-in-the-authentication-file).</span><span class="sxs-lookup"><span data-stu-id="7b069-301">Alternatively, you can specify hello required MySQL credentials in a file, by creating hello file: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth. For more information about managing MySQL credentials for monitoring through hello mysql-auth file, see [Manage MySQL monitoring credentials in hello authentication file](#manage-mysql-monitoring-credentials-in-the-authentication-file).</span></span>

<span data-ttu-id="7b069-302">V tématu [databáze oprávnění vyžadovaná pro čítače výkonu databáze MySQL](#database-permissions-required-for-mysql-performance-counters) podrobnosti o objektu oprávněních hello MySQL uživatele toocollect data o výkonu serveru MySQL.</span><span class="sxs-lookup"><span data-stu-id="7b069-302">See [Database permissions required for MySQL performance counters](#database-permissions-required-for-mysql-performance-counters) for details about object permissions required by hello MySQL user toocollect MySQL Server performance data.</span></span>

### <a name="enable-apache-http-server-performance-counters-using-linux-commands"></a><span data-ttu-id="7b069-303">Povolit čítače výkonu serveru Apache HTTP Server pomocí příkazů Linux</span><span class="sxs-lookup"><span data-stu-id="7b069-303">Enable Apache HTTP Server performance counters using Linux commands</span></span>
<span data-ttu-id="7b069-304">Pokud v počítači hello se zjistí serveru Apache HTTP Server, pokud je nainstalovaná sada omsagent hello, automaticky se nainstaluje zprostředkovatele pro serveru Apache HTTP Server monitorování výkonu.</span><span class="sxs-lookup"><span data-stu-id="7b069-304">If Apache HTTP Server is detected on hello computer when hello omsagent bundle is installed, a performance monitoring provider for Apache HTTP Server is automatically installed.</span></span> <span data-ttu-id="7b069-305">Tento zprostředkovatel spoléhá na platformě Apache "modul", který je nutné načíst do hello serveru Apache HTTP Server v datech o výkonu tooaccess pořadí.</span><span class="sxs-lookup"><span data-stu-id="7b069-305">This provider relies on an Apache "module" that must be loaded into hello Apache HTTP Server in order tooaccess performance data.</span></span>

<span data-ttu-id="7b069-306">Modul hello můžete načíst pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7b069-306">You can load hello module with hello following command:</span></span>

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

<span data-ttu-id="7b069-307">toounload hello Apache modulu sledování, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="7b069-307">toounload hello Apache monitoring module, run hello following command:</span></span>

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```
### <a name="tooview-performance-data-with-log-analytics"></a><span data-ttu-id="7b069-308">data výkonu tooview s analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="7b069-308">tooview performance data with Log Analytics</span></span>
1. <span data-ttu-id="7b069-309">V hello portál Operations Management Suite klikněte na dlaždici hledání protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-309">In hello Operations Management Suite portal, click hello Log Search tile.</span></span>
2. <span data-ttu-id="7b069-310">V panelu vyhledávání hello, zadejte `* (Type=Perf)` tooview všech čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="7b069-310">In hello search bar, type `* (Type=Perf)` tooview all performance counters.</span></span>

<span data-ttu-id="7b069-311">Protože OMS taky shromažďuje data čítače výkonu systému Windows, jste měli oboru rozbalovací hello tooLinux specifických dat hledání.</span><span class="sxs-lookup"><span data-stu-id="7b069-311">Because OMS also collects Windows performance counter data, you should scope-down hello search tooLinux-specific data.</span></span> <span data-ttu-id="7b069-312">Ano hello následujícím příkladu by zobrazit výkon dat konkrétní tooan příklad Linux serveru s názvem Chorizo21.</span><span class="sxs-lookup"><span data-stu-id="7b069-312">So, hello following example would show performance data specific tooan example Linux server named Chorizo21.</span></span>

```
Type=Perf Computer=chorizo*
```

![Příklad server zobrazen ve výsledcích vyhledávání](./media/log-analytics-linux-agents/oms-perfsearch01.png)

<span data-ttu-id="7b069-314">Ve výsledcích hello, můžete kliknout na **metriky** tooview hello čítače, které pro nebyla shromážděna data.</span><span class="sxs-lookup"><span data-stu-id="7b069-314">In hello results, you can click **Metrics** tooview hello counters that data was collected for.</span></span> <span data-ttu-id="7b069-315">Data v reálném čase je zobrazen jako grafy pro každý čítač.</span><span class="sxs-lookup"><span data-stu-id="7b069-315">Real-time data is shown as graphs for each counter.</span></span>

![Průzkumníku metrik](./media/log-analytics-linux-agents/oms-perfmetrics01.png)

## <a name="syslog"></a><span data-ttu-id="7b069-317">Syslog</span><span class="sxs-lookup"><span data-stu-id="7b069-317">Syslog</span></span>
<span data-ttu-id="7b069-318">Syslog je událost protokolování protokol podobné tooWindows protokoly událostí – obě fungují podobně jako při zobrazení v OMS.</span><span class="sxs-lookup"><span data-stu-id="7b069-318">Syslog is an event logging protocol similar tooWindows Event logs—both operate similarly when displayed in OMS.</span></span>

### <a name="tooadd-a-new-linux-syslog-facility-in-oms"></a><span data-ttu-id="7b069-319">tooadd nového zařízení syslog Linux v OMS</span><span class="sxs-lookup"><span data-stu-id="7b069-319">tooadd a new Linux syslog facility in OMS</span></span>
1. <span data-ttu-id="7b069-320">Na hello **nastavení** v části **Data** , klikněte na tlačítko **Syslog** a toohello vlevo hello a ikonu, zadejte název hello hello mechanismus syslog, které chcete tooadd.</span><span class="sxs-lookup"><span data-stu-id="7b069-320">On hello **Settings** page under **Data** , click **Syslog** and then toohello left of hello plus icon, type hello name of hello syslog facility that you want tooadd.</span></span>
    <span data-ttu-id="7b069-321">![Linux syslog](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)</span><span class="sxs-lookup"><span data-stu-id="7b069-321">![Linux syslog](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)</span></span>
2. <span data-ttu-id="7b069-322">Pokud si nejste jisti hello úplný název hello zařízení, pište částečný název a zobrazí se seznam dostupných syslog zařízení.</span><span class="sxs-lookup"><span data-stu-id="7b069-322">If you don’t know hello full name of hello facility, you can start typing a partial name and a list of available syslog facilities will appear.</span></span> <span data-ttu-id="7b069-323">Zjistíte, když chcete tooadd, klikněte na název hello v seznamu hello a pak klikněte na hello plus ikonu tooadd hello protokolovací mechanismus syslog hello protokolovací mechanismus syslog.</span><span class="sxs-lookup"><span data-stu-id="7b069-323">When you find hello syslog facility that you want tooadd, click hello name in hello list and then click hello plus icon tooadd hello syslog facility.</span></span>
3. <span data-ttu-id="7b069-324">Po přidání hello zařízení, zobrazí se v seznamu hello zvýrazněná s barevnou panelu.</span><span class="sxs-lookup"><span data-stu-id="7b069-324">After you add hello facility, it appears in hello list of highlighted with a colored bar.</span></span> <span data-ttu-id="7b069-325">V dalším kroku vyberte hello závažnosti (kategorie syslog zařízení informací), které chcete toocollect.</span><span class="sxs-lookup"><span data-stu-id="7b069-325">Next, choose hello severities (categories of syslog facility information) that you want toocollect.</span></span>
4. <span data-ttu-id="7b069-326">V dolní části hello hello stránky klikněte na tlačítko **Uložit** toofinalize změny.</span><span class="sxs-lookup"><span data-stu-id="7b069-326">At hello bottom of hello page click **Save** toofinalize your changes.</span></span> <span data-ttu-id="7b069-327">Hello změny konfigurace, které jste udělali se pak odešlou tooall hello OMS agentů pro Linux, které jsou registrovány OMS, obvykle během pěti minut.</span><span class="sxs-lookup"><span data-stu-id="7b069-327">hello configuration changes that you’ve made are then sent tooall hello OMS Agents for Linux that are registered with OMS, typically within 5 minutes.</span></span>

### <a name="configure-linux-syslog-facilities-in-linux"></a><span data-ttu-id="7b069-328">Nakonfigurujte zařízení syslog Linux v systému Linux</span><span class="sxs-lookup"><span data-stu-id="7b069-328">Configure Linux syslog facilities in Linux</span></span>
<span data-ttu-id="7b069-329">Události procesu Syslog jsou odesílány z hello démon procesu syslog, například rsyslog nebo syslog ng, místního portu tooa tohoto agenta hello naslouchá na.</span><span class="sxs-lookup"><span data-stu-id="7b069-329">Syslog events are sent from hello syslog daemon, for example rsyslog or syslog-ng, tooa local port that hello agent is listening on.</span></span> <span data-ttu-id="7b069-330">Ve výchozím nastavení je to port 25224.</span><span class="sxs-lookup"><span data-stu-id="7b069-330">By default, port 25224.</span></span> <span data-ttu-id="7b069-331">Pokud je nainstalován hello agent, je použita výchozí konfigurace syslog.</span><span class="sxs-lookup"><span data-stu-id="7b069-331">When hello agent is installed, a default syslog configuration is applied.</span></span> <span data-ttu-id="7b069-332">To se nachází zde:</span><span class="sxs-lookup"><span data-stu-id="7b069-332">This is found at:</span></span>

<span data-ttu-id="7b069-333">Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf</span><span class="sxs-lookup"><span data-stu-id="7b069-333">Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf</span></span>

<span data-ttu-id="7b069-334">Syslog ng: /etc/syslog-ng/syslog-ng.conf</span><span class="sxs-lookup"><span data-stu-id="7b069-334">Syslog-ng: /etc/syslog-ng/syslog-ng.conf</span></span>

<span data-ttu-id="7b069-335">Konfigurace syslog agenta OMS výchozí Hello odesílá události procesu syslog od všech zařízení, se závažností upozornění nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="7b069-335">hello default OMS agent syslog configuration uploads syslog events from all facilities with a severity of warning or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="7b069-336">Pokud chcete upravit konfiguraci hello syslog, je nutné restartovat hello démon procesu syslog pro hello změny tootake vliv.</span><span class="sxs-lookup"><span data-stu-id="7b069-336">If you edit hello syslog configuration, you must restart hello syslog daemon for hello changes tootake effect.</span></span>
>
>

<span data-ttu-id="7b069-337">Hello výchozí syslog pro hello OMS agenta pro Linux pro OMS je konfigurace:</span><span class="sxs-lookup"><span data-stu-id="7b069-337">hello default syslog configuration for hello OMS Agent for Linux for OMS is:</span></span>

#### <a name="rsyslog"></a><span data-ttu-id="7b069-338">rsyslog</span><span class="sxs-lookup"><span data-stu-id="7b069-338">Rsyslog</span></span>
```
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
```

#### <a name="syslog-ng"></a><span data-ttu-id="7b069-339">Syslog ng</span><span class="sxs-lookup"><span data-stu-id="7b069-339">Syslog-ng</span></span>
```
#OMS_facility = all
filter f_warning_oms { level(warning); };
destination warning_oms { tcp("127.0.0.1" port(25224)); };
log { source(src); filter(f_warning_oms); destination(warning_oms); };
```

### <a name="tooview-all-syslog-events-with-log-analytics"></a><span data-ttu-id="7b069-340">tooview všechny události procesu Syslog s analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="7b069-340">tooview all Syslog events with Log Analytics</span></span>
1. <span data-ttu-id="7b069-341">Hello Operations Management Suite portálu, klikněte na tlačítko hello **hledání protokolů** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="7b069-341">In hello Operations Management Suite portal, click hello **Log Search** tile.</span></span>
2. <span data-ttu-id="7b069-342">V hello **Správa protokolu** seskupování, vyberte předdefinované syslog vyhledávání a pak vyberte jeden toorun ho.</span><span class="sxs-lookup"><span data-stu-id="7b069-342">In hello **Log Management** grouping, choose a predefined syslog search and then select one toorun it.</span></span>

<span data-ttu-id="7b069-343">Tento příklad ukazuje všechny události procesu Syslog.</span><span class="sxs-lookup"><span data-stu-id="7b069-343">This example shows all Syslog events.</span></span>

![Ukazuje vyhledávání protokolu události procesu Syslog](./media/log-analytics-linux-agents/oms-linux-syslog.png)

<span data-ttu-id="7b069-345">Nyní můžete rozbalit výsledky hledání.</span><span class="sxs-lookup"><span data-stu-id="7b069-345">Now you can drill into search results.</span></span>

## <a name="linux-alerts"></a><span data-ttu-id="7b069-346">Linux výstrahy</span><span class="sxs-lookup"><span data-stu-id="7b069-346">Linux alerts</span></span>
<span data-ttu-id="7b069-347">Pokud používáte Nagios nebo Zabbix toomanage počítače se systémem Linux a pak OMS může přijímat hello výstrahy generované z těchto nástrojů.</span><span class="sxs-lookup"><span data-stu-id="7b069-347">If you use Nagios or Zabbix toomanage your Linux machines, then OMS can receive hello alerts generated from those tools.</span></span> <span data-ttu-id="7b069-348">Je však aktuálně žádná metoda tooconfigure příchozích dat výstrah pomocí portálu OMS hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-348">However, there is currently no method tooconfigure incoming alert data using hello OMS portal.</span></span> <span data-ttu-id="7b069-349">Místo toho je nutné tooedit konfigurační soubor toostart odesílání výstrah tooOMS.</span><span class="sxs-lookup"><span data-stu-id="7b069-349">Instead, you will need tooedit a config file toostart sending alerts tooOMS.</span></span>

### <a name="collect-alerts-from-nagios"></a><span data-ttu-id="7b069-350">Shromažďovat výstrahy z Nagios</span><span class="sxs-lookup"><span data-stu-id="7b069-350">Collect alerts from Nagios</span></span>
<span data-ttu-id="7b069-351">toocollect výstrahy ze serveru Nagios, musíte toomake hello následující změny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7b069-351">toocollect alerts from a Nagios server, you need toomake hello following configuration changes.</span></span>

1. <span data-ttu-id="7b069-352">Udělení hello uživatele **omsagent** soubor protokolu Nagios toohello přístup pro čtení (tj. /var/log/nagios/nagios.log).</span><span class="sxs-lookup"><span data-stu-id="7b069-352">Grant hello user **omsagent** read access toohello Nagios log file (i.e. /var/log/nagios/nagios.log).</span></span> <span data-ttu-id="7b069-353">Za předpokladu, že soubor nagios.log hello je vlastníkem skupiny hello **nagios** , můžete přidat uživatele hello **omsagent** toohello **nagios** skupiny.</span><span class="sxs-lookup"><span data-stu-id="7b069-353">Assuming hello nagios.log file is owned by hello group **nagios** , you can add hello user **omsagent** toohello **nagios** group.</span></span>

    ```
    sudo usermod –a -G nagios omsagent
    ```
2. <span data-ttu-id="7b069-354">Upravte soubor omsagent.confconfiguration hello (/ etc/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/conf/omsagent.conf).</span><span class="sxs-lookup"><span data-stu-id="7b069-354">Modify hello omsagent.confconfiguration file (/etc/opt/microsoft/omsagent/&lt;workspace id&gt;/conf/omsagent.conf).</span></span> <span data-ttu-id="7b069-355">Zajistěte, aby hello následující položky jsou existuje a není komentáři se:</span><span class="sxs-lookup"><span data-stu-id="7b069-355">Ensure hello following entries are present and not commented out:</span></span>

    ```
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
    ```
3. <span data-ttu-id="7b069-356">Restartujte démon omsagent hello:</span><span class="sxs-lookup"><span data-stu-id="7b069-356">Restart hello omsagent daemon:</span></span>

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="collect-alerts-from-zabbix"></a><span data-ttu-id="7b069-357">Shromažďovat výstrahy z Zabbix</span><span class="sxs-lookup"><span data-stu-id="7b069-357">Collect alerts from Zabbix</span></span>
<span data-ttu-id="7b069-358">toocollect výstrahy ze serveru Zabbix, budete provádět podobné toothose kroky pro Nagios výše, s výjimkou budete potřebovat toospecify uživatele a heslo v *text vymažte, pokud*.</span><span class="sxs-lookup"><span data-stu-id="7b069-358">toocollect alerts from a Zabbix server, you'll perform similar steps toothose for Nagios above, except you'll need toospecify a user and password in *clear text*.</span></span> <span data-ttu-id="7b069-359">Toto není ideální, ale pravděpodobně bude brzy změnit.</span><span class="sxs-lookup"><span data-stu-id="7b069-359">This is not ideal, but will likely change soon.</span></span> <span data-ttu-id="7b069-360">tooaddress tento problém, doporučujeme vytvořit hello uživatele a udělit mu oprávnění toomonitor pouze.</span><span class="sxs-lookup"><span data-stu-id="7b069-360">tooaddress this issue, we recommend that you create hello user and grant it permission toomonitor only.</span></span>

<span data-ttu-id="7b069-361">Na příkladu část hello omsagent.conf konfiguračního souboru (/ etc/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/conf/omsagent.conf) pro Zabbix by měla vypadat přibližně hello následující:</span><span class="sxs-lookup"><span data-stu-id="7b069-361">An example section of hello omsagent.conf configuration file  (/etc/opt/microsoft/omsagent/&lt;workspace id&gt;/conf/omsagent.conf) for Zabbix should resemble hello following:</span></span>

```
<source>
  type zabbix_alerts
  run_interval 1m
  tag oms.zabbix
  zabbix_url http://localhost/zabbix/api_jsonrpc.php
  zabbix_username Admin
  zabbix_password zabbix
</source>

```

### <a name="view-alerts-in-log-analytics-search"></a><span data-ttu-id="7b069-362">Zobrazit výstrahy ve vyhledávání analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="7b069-362">View alerts in Log Analytics search</span></span>
<span data-ttu-id="7b069-363">Po dokončení konfigurace vaší tooOMS Linux počítače toosend výstrahy, můžete použít několik jednoduchých protokolu vyhledávací dotazy tooview hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="7b069-363">After you've configured your Linux computers toosend alerts tooOMS, you can use a few simple log search queries tooview hello alerts.</span></span> <span data-ttu-id="7b069-364">Hello následující příklad vyhledávací dotaz vrátí všechny hello zaznamenávají výstrahy, které byly vygenerovány.</span><span class="sxs-lookup"><span data-stu-id="7b069-364">hello following search query example returns all hello recorded alerts that were generated.</span></span> <span data-ttu-id="7b069-365">Například pokud nějaká problému dojde v infrastruktuře IT, pak výsledky pro hello následující ukázka, že dotaz může znamenat, kde může být problém hello pocházejí.</span><span class="sxs-lookup"><span data-stu-id="7b069-365">For example, if some sort of problem occurs in your IT infrastructure, then results for hello following example query might indicate where hello problem might originate.</span></span> <span data-ttu-id="7b069-366">A můžete snadno zobrazit další podrobnosti v toohello výstrahy ve zdrojovém systému toohelp úzké šetření.</span><span class="sxs-lookup"><span data-stu-id="7b069-366">And, you can easily drill in toohello alerts by source system toohelp narrow your investigation.</span></span> <span data-ttu-id="7b069-367">Hello výhodou je, že nemáte nutně systémy správy toovarious toogo od začátku hello – za předpokladu, že vaše výstrahy se posílají tooOMS, můžete spustit existuje.</span><span class="sxs-lookup"><span data-stu-id="7b069-367">hello benefit is that you don't necessarily have toogo toovarious management systems from hello start—provided that your alerts are sent tooOMS, you can start there.</span></span>

```
Type=Alert
```

#### <a name="tooview-all-nagios-alerts-with-log-analytics"></a><span data-ttu-id="7b069-368">tooview Nagios všechny výstrahy s analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="7b069-368">tooview all Nagios alerts with Log Analytics</span></span>
1. <span data-ttu-id="7b069-369">Hello Operations Management Suite portálu, klikněte na tlačítko hello **hledání protokolů** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="7b069-369">In hello Operations Management Suite portal, click hello **Log Search** tile.</span></span>
2. <span data-ttu-id="7b069-370">V panelu hello dotazu zadejte hello následující vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="7b069-370">In hello query bar, type hello following search query</span></span>

    ```
    Type=Alert SourceSystem=Nagios
    ```
   ![Nagios výstrah zobrazených ve vyhledávání protokolu](./media/log-analytics-linux-agents/oms-linux-nagios-alerts.png)

<span data-ttu-id="7b069-372">Až se zobrazí výsledky hledání hello, můžete přejít do další podrobnosti, jako *AlertState*.</span><span class="sxs-lookup"><span data-stu-id="7b069-372">After you see hello search results, you can drill into additional details such as *AlertState*.</span></span>

### <a name="tooview-all-zabbix-alerts-with-log-analytics"></a><span data-ttu-id="7b069-373">tooview všechny výstrahy Zabbix s analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="7b069-373">tooview all Zabbix alerts with Log Analytics</span></span>
1. <span data-ttu-id="7b069-374">Hello Operations Management Suite portálu, klikněte na tlačítko hello **hledání protokolů** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="7b069-374">In hello Operations Management Suite portal, click hello **Log Search** tile.</span></span>
2. <span data-ttu-id="7b069-375">V panelu hello dotazu zadejte hello následující vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="7b069-375">In hello query bar, type hello following search query</span></span>

    ```
    Type=Alert SourceSystem=Zabbix
    ```
   ![Zabbix výstrah zobrazených ve vyhledávání protokolu](./media/log-analytics-linux-agents/oms-linux-zabbix-alerts.png)

<span data-ttu-id="7b069-377">Až se zobrazí výsledky hledání hello, můžete přejít do další podrobnosti, jako *AlertName*.</span><span class="sxs-lookup"><span data-stu-id="7b069-377">After you see hello search results, you can drill into additional details such as *AlertName*.</span></span>

## <a name="compatibility-with-system-center-operations-manager"></a><span data-ttu-id="7b069-378">Kompatibilita s nástrojem System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="7b069-378">Compatibility with System Center Operations Manager</span></span>
<span data-ttu-id="7b069-379">Hello OMS agenta pro Linux sdílí binárních souborů agenta s hello agenta System Center Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="7b069-379">hello OMS Agent for Linux shares agent binaries with hello System Center Operations Manager agent.</span></span> <span data-ttu-id="7b069-380">Instalace hello OMS agenta pro Linux v systému aktuálně spravován nástrojem Operations Manager upgrady hello OMI a SCX balíčků na novější verzi tooa hello počítače.</span><span class="sxs-lookup"><span data-stu-id="7b069-380">Installing hello OMS Agent for Linux on a system currently managed by Operations Manager upgrades hello OMI and SCX packages on hello computer tooa newer version.</span></span> <span data-ttu-id="7b069-381">Hello OMS agenta pro Linux a System Center 2012 R2 jsou kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="7b069-381">hello OMS Agent for Linux and System Center 2012 R2 are compatible.</span></span> <span data-ttu-id="7b069-382">Ale **System Center 2012 SP1 a starší verze nejsou aktuálně kompatibilní nebo není podporován s hello OMS agenta pro Linux.**</span><span class="sxs-lookup"><span data-stu-id="7b069-382">However, **System Center 2012 SP1 and earlier versions are currently not compatible or supported with hello OMS Agent for Linux.**</span></span>

> [!NOTE]
> <span data-ttu-id="7b069-383">Pokud je nainstalované tooa počítač, který není aktuálně spravována nástrojem Operations Manager hello OMS agenta pro Linux a chcete později toomanage hello počítač s nástrojem Operations Manager, je třeba změnit konfiguraci OMI hello před zjistit počítač se hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-383">If hello OMS Agent for Linux is installed tooa computer that is not currently managed by Operations Manager, and you later want toomanage hello computer with Operations Manager, you must modify hello OMI configuration before you discover hello computer.</span></span> <span data-ttu-id="7b069-384">**Tento krok není nutný, pokud je nainstalován agent nástroje Operations Manager hello před hello OMS agenta pro Linux.**</span><span class="sxs-lookup"><span data-stu-id="7b069-384">**This step is not needed if hello Operations Manager agent is installed before hello OMS Agent for Linux.**</span></span>
>
>

### <a name="tooenable-hello-oms-agent-for-linux-toocommunicate-with-operations-manager"></a><span data-ttu-id="7b069-385">tooenable hello OMS agenta pro Linux toocommunicate s nástrojem Operations Manager</span><span class="sxs-lookup"><span data-stu-id="7b069-385">tooenable hello OMS Agent for Linux toocommunicate with Operations Manager</span></span>
1. <span data-ttu-id="7b069-386">Upravit soubor /etc/opt/omi/conf/omiserver.conf hello</span><span class="sxs-lookup"><span data-stu-id="7b069-386">Edit hello file /etc/opt/omi/conf/omiserver.conf</span></span>
2. <span data-ttu-id="7b069-387">Ujistěte se, že hello řádek začínající **httpsport =** definuje hello port 1270.</span><span class="sxs-lookup"><span data-stu-id="7b069-387">Ensure that hello line beginning with **httpsport=** defines hello port 1270.</span></span> <span data-ttu-id="7b069-388">Například`httpsport=1270`</span><span class="sxs-lookup"><span data-stu-id="7b069-388">Such as `httpsport=1270`</span></span>
3. <span data-ttu-id="7b069-389">Restartujte hello OMI server:</span><span class="sxs-lookup"><span data-stu-id="7b069-389">Restart hello OMI server:</span></span>

    ```
    sudo /opt/omi/bin/service_control restart
    ```

## <a name="database-permissions-required-for-mysql-performance-counters"></a><span data-ttu-id="7b069-390">Databáze oprávnění vyžadovaná pro čítače výkonu databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="7b069-390">Database permissions required for MySQL performance counters</span></span>
<span data-ttu-id="7b069-391">toogrant oprávnění tooa MySQL uživatele monitorování, hello poskytující uživatel musí mít oprávnění "Udělit možnost" hello, jakož i udělení oprávnění hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-391">toogrant permissions tooa MySQL monitoring user, hello granting user must have hello 'GRANT option' privilege as well as hello privilege being granted.</span></span>

<span data-ttu-id="7b069-392">Aby hello MySQL uživatele tooreturn výkonu dat hello uživatel bude potřebovat přístup k toohello následující dotazy:</span><span class="sxs-lookup"><span data-stu-id="7b069-392">In order for hello MySQL User tooreturn performance data hello user will need access toohello following queries:</span></span>

```
SHOW GLOBAL STATUS;
SHOW GLOBAL VARIABLES:
```

<span data-ttu-id="7b069-393">Kromě toho toothese dotazy hello MySQL uživatel vyžaduje následující výchozí tabulky toohello vyberte přístup:</span><span class="sxs-lookup"><span data-stu-id="7b069-393">In addition toothese queries hello MySQL user requires SELECT access toohello following default tables:</span></span>

* <span data-ttu-id="7b069-394">INFORMATION_SCHEMA</span><span class="sxs-lookup"><span data-stu-id="7b069-394">information_schema</span></span>
* <span data-ttu-id="7b069-395">MySQL</span><span class="sxs-lookup"><span data-stu-id="7b069-395">mysql</span></span>

<span data-ttu-id="7b069-396">Tato oprávnění lze udělit tak, že spustíte následující příkazy grant hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-396">These privileges can be granted by running hello following grant commands.</span></span>

```
GRANT SELECT ON information_schema.* too‘monuser’@’localhost’;
GRANT SELECT ON mysql.* too‘monuser’@’localhost’;
```

## <a name="manage-mysql-monitoring-credentials-in-hello-authentication-file"></a><span data-ttu-id="7b069-397">Spravovat MySQL monitorování přihlašovací údaje v souboru authentication hello</span><span class="sxs-lookup"><span data-stu-id="7b069-397">Manage MySQL monitoring credentials in hello authentication file</span></span>
<span data-ttu-id="7b069-398">Hello následující části snadněji tak můžete spravovat MySQL přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="7b069-398">hello following sections help you manage MySQL credentials.</span></span>

### <a name="configure-hello-mysql-omi-provider"></a><span data-ttu-id="7b069-399">Konfigurace hello MySQL OMI zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="7b069-399">Configure hello MySQL OMI provider</span></span>
<span data-ttu-id="7b069-400">Hello MySQL OMI zprostředkovatele vyžaduje předkonfigurované uživatele MySQL a nainstalovat MySQL klientské knihovny v pořadí tooquery hello stavu nebo výkonu informace z instance databáze MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-400">hello MySQL OMI provider requires a preconfigured MySQL user and installed MySQL client libraries in order tooquery hello performance/health information from hello MySQL instance.</span></span>

### <a name="mysql-omi-authentication-file"></a><span data-ttu-id="7b069-401">Soubor pro ověření MySQL OMI</span><span class="sxs-lookup"><span data-stu-id="7b069-401">MySQL OMI authentication file</span></span>
<span data-ttu-id="7b069-402">MySQL OMI zprostředkovatele používá k ověření souboru toodetermine instance jaké vazby adresu a port, MySQL hello naslouchá na a jaké přihlašovací údaje toouse toogather metriky.</span><span class="sxs-lookup"><span data-stu-id="7b069-402">MySQL OMI provider uses an authentication file toodetermine what bind-address and port hello MySQL instance is listening on and what credentials toouse toogather metrics.</span></span> <span data-ttu-id="7b069-403">Během instalace hello MySQL OMI zprostředkovatele vyhledá MySQL my.cnf konfigurační soubory (výchozí umístění) vazby adresy a portu a částečně sadu hello soubor pro ověření MySQL OMI.</span><span class="sxs-lookup"><span data-stu-id="7b069-403">During installation hello MySQL OMI provider will scan MySQL my.cnf configuration files (default locations) for bind-address and port and partially set hello MySQL OMI authentication file.</span></span>

<span data-ttu-id="7b069-404">toocomplete monitorování instance serveru databáze MySQL, přidá soubor předem vygenerovaná ověřování MySQL OMI do správného adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-404">toocomplete monitoring of a MySQL server instance, add a pre-generated MySQL OMI authentication file into hello correct directory.</span></span>

### <a name="authentication-file-format"></a><span data-ttu-id="7b069-405">Formát souboru ověřování</span><span class="sxs-lookup"><span data-stu-id="7b069-405">Authentication file format</span></span>
<span data-ttu-id="7b069-406">Hello MySQL OMI ověřování soubor je textový soubor, který obsahuje informace o:</span><span class="sxs-lookup"><span data-stu-id="7b069-406">hello MySQL OMI authentication file is a text file that contains information about:</span></span>

* <span data-ttu-id="7b069-407">Port</span><span class="sxs-lookup"><span data-stu-id="7b069-407">Port</span></span>
* <span data-ttu-id="7b069-408">Vazby – adresy</span><span class="sxs-lookup"><span data-stu-id="7b069-408">Bind-Address</span></span>
* <span data-ttu-id="7b069-409">Uživatelské jméno MySQL</span><span class="sxs-lookup"><span data-stu-id="7b069-409">MySQL username</span></span>
* <span data-ttu-id="7b069-410">Heslo s kódováním base64</span><span class="sxs-lookup"><span data-stu-id="7b069-410">Base64 encoded password</span></span>

<span data-ttu-id="7b069-411">Hello souboru MySQL OMI authentication pouze uděluje oprávnění pro čtení/zápisu toohello Linux uživatele, která ji vygenerovala.</span><span class="sxs-lookup"><span data-stu-id="7b069-411">hello MySQL OMI authentication file only grants privileges for read/write toohello Linux user that generated it.</span></span>

```
[Port]=[Bind-Address], [username], [Base64 encoded Password]
(Port)=(Bind-Address), (username), (Base64 encoded Password)
(Port)=(Bind-Address), (username), (Base64 encoded Password)
AutoUpdate=[true|false]
```

<span data-ttu-id="7b069-412">Výchozí soubor pro ověření MySQL OMI obsahuje výchozí instance a číslo portu v závislosti na tom, jaké informace je k dispozici a Analyzovaná z hello nalezen konfigurační soubor MySQL.</span><span class="sxs-lookup"><span data-stu-id="7b069-412">A default MySQL OMI authentication file contains a default instance and a port number depending on what information is available and parsed from hello found MySQL configuration file.</span></span>

<span data-ttu-id="7b069-413">Hello výchozí instance je znamená toomake, Správa více instancí databáze MySQL na jednom hostiteli systému Linux jednodušší a je označený jako instance hello s portu 0.</span><span class="sxs-lookup"><span data-stu-id="7b069-413">hello default instance is a means toomake managing multiple MySQL instances on one Linux host easier, and is denoted by hello instance with port 0.</span></span> <span data-ttu-id="7b069-414">Všechny instance přidané zdědí vlastnosti nastavit od hello výchozí instanci.</span><span class="sxs-lookup"><span data-stu-id="7b069-414">All added instances will inherit properties set from hello default instance.</span></span> <span data-ttu-id="7b069-415">Například pokud je přidána instance databáze MySQL naslouchání na portu '3308', vazby adresu hello výchozí instance, uživatelské jméno a heslo kódováním Base64 bude použité tootry a monitorovat naslouchání na 3308 instance hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-415">For example, if MySQL instance listening on port '3308' is added, hello default instance's bind-address, username, and Base64 encoded password will be used tootry and monitor hello instance listening on 3308.</span></span> <span data-ttu-id="7b069-416">Pokud instance hello na 3308 je vázaný tooanother adresu a hello používá stejné uživatelské jméno MySQL a pár heslo pouze hello respecification hello je vyžadována adresa vazby a hello ostatní vlastnosti zdědí.</span><span class="sxs-lookup"><span data-stu-id="7b069-416">If hello instance on 3308 is binded tooanother address and uses hello same MySQL username and password pair only hello respecification of hello bind-address is needed and hello other properties will be inherited.</span></span>

<span data-ttu-id="7b069-417">Příklady hello ověřování souboru vypadat podobně jako následující hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-417">Examples of hello authentication file resemble hello following.</span></span>

<span data-ttu-id="7b069-418">Výchozí instance a instanci s portem 3308:</span><span class="sxs-lookup"><span data-stu-id="7b069-418">Default instance and instance with port 3308:</span></span>

```
0=127.0.0.1, myuser, cnBwdA==3308=, ,AutoUpdate=true
```

<span data-ttu-id="7b069-419">Výchozí instance a instance s port 3308 + různých Base 64 kódovaný heslo:</span><span class="sxs-lookup"><span data-stu-id="7b069-419">Default instance and instance with port 3308 + different Base 64 encoded password:</span></span>

```
0=127.0.0.1, myuser, cnBwdA==3308=127.0.1.1, , AutoUpdate=true
```


| <span data-ttu-id="7b069-420">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="7b069-420">**Property**</span></span> | <span data-ttu-id="7b069-421">**Popis**</span><span class="sxs-lookup"><span data-stu-id="7b069-421">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="7b069-422">Port</span><span class="sxs-lookup"><span data-stu-id="7b069-422">Port</span></span> |<span data-ttu-id="7b069-423">Port představuje hello aktuální port hello instance naslouchá na MySQL.</span><span class="sxs-lookup"><span data-stu-id="7b069-423">Port represents hello current port hello MySQL instance is listening on.</span></span>  <span data-ttu-id="7b069-424">Hello port 0 znamená, že následující vlastnosti hello jsou pro výchozí instanci.</span><span class="sxs-lookup"><span data-stu-id="7b069-424">hello port 0 implies that hello properties following are used for default instance.</span></span> |
| <span data-ttu-id="7b069-425">Vazby – adresy</span><span class="sxs-lookup"><span data-stu-id="7b069-425">Bind-Address</span></span> |<span data-ttu-id="7b069-426">Hello vazby adresa je hello aktuální MySQL vazby adresa</span><span class="sxs-lookup"><span data-stu-id="7b069-426">hello Bind Address is hello current MySQL bind-address</span></span> |
| <span data-ttu-id="7b069-427">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="7b069-427">username</span></span> |<span data-ttu-id="7b069-428">Toto uživatelské jméno hello hello MySQL uživatele chcete instanci serveru toouse toomonitor hello MySQL.</span><span class="sxs-lookup"><span data-stu-id="7b069-428">This hello username of hello MySQL user you wish toouse toomonitor hello MySQL server instance.</span></span> |
| <span data-ttu-id="7b069-429">Kódováním base64, pomocí hesla</span><span class="sxs-lookup"><span data-stu-id="7b069-429">Base64 encoded Password</span></span> |<span data-ttu-id="7b069-430">Toto je heslo hello hello MySQL monitorování uživatele kódovaný jako Base64.</span><span class="sxs-lookup"><span data-stu-id="7b069-430">This is hello password of hello MySQL monitoring user encoded in Base64.</span></span> |
| <span data-ttu-id="7b069-431">Pomocí funkce Automatické aktualizace</span><span class="sxs-lookup"><span data-stu-id="7b069-431">AutoUpdate</span></span> |<span data-ttu-id="7b069-432">Upgradován hello MySQL OMI zprostředkovatele hello zprostředkovatele bude znovu zkontrolujte změny v souboru my.cnf hello a přepsat soubor hello MySQL OMI ověřování.</span><span class="sxs-lookup"><span data-stu-id="7b069-432">When hello MySQL OMI Provider is upgraded hello provider will rescan for changes in hello my.cnf file and overwrite hello MySQL OMI Authentication file.</span></span> <span data-ttu-id="7b069-433">Nastavte tento příznak tootrue nebo NEPRAVDA v závislosti na požadované aktualizace toohello MySQL OMI soubor pro ověření.</span><span class="sxs-lookup"><span data-stu-id="7b069-433">Set this flag tootrue or false depending on required updates toohello MySQL OMI authentication file.</span></span> |

#### <a name="authentication-file-location"></a><span data-ttu-id="7b069-434">Umístění souboru ověřování</span><span class="sxs-lookup"><span data-stu-id="7b069-434">Authentication file location</span></span>
<span data-ttu-id="7b069-435">Hello MySQL OMI ověřování souboru by mělo být umístěný v hello následující umístění a s názvem "mysql ověření":</span><span class="sxs-lookup"><span data-stu-id="7b069-435">hello MySQL OMI Authentication File should be located in hello following location and named "mysql-auth":</span></span>

<span data-ttu-id="7b069-436">/var/OPT/Microsoft/MySQL-cimprov/auth/omsagent/MySQL-auth</span><span class="sxs-lookup"><span data-stu-id="7b069-436">/var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth</span></span>

<span data-ttu-id="7b069-437">Hello souboru (a ověřování nebo omsagent directory) by měl být vlastněných uživatelem omsagent hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-437">hello file (and auth/omsagent directory) should be owned by hello omsagent user.</span></span>

## <a name="agent-logs"></a><span data-ttu-id="7b069-438">Protokoly agenta</span><span class="sxs-lookup"><span data-stu-id="7b069-438">Agent logs</span></span>
<span data-ttu-id="7b069-439">Hello protokoly pro hello OMS agenta pro Linux je na:</span><span class="sxs-lookup"><span data-stu-id="7b069-439">hello logs for hello OMS Agent for Linux is at:</span></span>

<span data-ttu-id="7b069-440">/ var/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/log/</span><span class="sxs-lookup"><span data-stu-id="7b069-440">/var/opt/microsoft/omsagent/&lt;workspace id&gt;/log/</span></span>

<span data-ttu-id="7b069-441">Hello protokoly pro hello OMS agenta pro Linux programu omsconfig (Konfigurace agenta) je na:</span><span class="sxs-lookup"><span data-stu-id="7b069-441">hello logs for hello OMS Agent for Linux for omsconfig (agent configuration) program is at:</span></span>

<span data-ttu-id="7b069-442">/ var/opt/microsoft/omsconfig nebo protokolu nebo</span><span class="sxs-lookup"><span data-stu-id="7b069-442">/var/opt/microsoft/omsconfig/log/</span></span>

<span data-ttu-id="7b069-443">Protokoly pro součásti pro OMI a SCX hello (které poskytují data metriky výkonu) je na:</span><span class="sxs-lookup"><span data-stu-id="7b069-443">Logs for hello OMI and SCX components (which provide performance metrics data) is at:</span></span>

<span data-ttu-id="7b069-444">/ var/opt/omi/log/a /var/opt/microsoft/scx/log</span><span class="sxs-lookup"><span data-stu-id="7b069-444">/var/opt/omi/log/ and /var/opt/microsoft/scx/log</span></span>

## <a name="troubleshooting-hello-oms-agent-for-linux"></a><span data-ttu-id="7b069-445">Řešení potíží s hello OMS agenta pro Linux</span><span class="sxs-lookup"><span data-stu-id="7b069-445">Troubleshooting hello OMS Agent for Linux</span></span>
<span data-ttu-id="7b069-446">Použijte následující informace toodiagnose hello a řešení běžných problémů.</span><span class="sxs-lookup"><span data-stu-id="7b069-446">Use hello following information toodiagnose and troubleshoot common issues.</span></span>

<span data-ttu-id="7b069-447">Pokud žádná z hello řešení potíží s informace v této části vám pomůže, můžete taky hello následující prostředky toohelp vyřešit váš problém.</span><span class="sxs-lookup"><span data-stu-id="7b069-447">If none of hello troubleshooting information in this section helps you, you can also use hello following resources toohelp resolve your problem.</span></span>

* <span data-ttu-id="7b069-448">Zákazníci s Premier support může přihlásit případu podpory prostřednictvím [úrovně Premier](https://premier.microsoft.com/)</span><span class="sxs-lookup"><span data-stu-id="7b069-448">Customers with Premier support can log a support case via [Premier](https://premier.microsoft.com/)</span></span>
* <span data-ttu-id="7b069-449">Zákazníci s smlouvy podporu Azure se můžou přihlásit případů podpory v hello [portálu Azure](https://manage.windowsazure.com/?getsupport=true)</span><span class="sxs-lookup"><span data-stu-id="7b069-449">Customers with Azure support agreements can log support cases in hello [Azure portal](https://manage.windowsazure.com/?getsupport=true)</span></span>
* <span data-ttu-id="7b069-450">Soubor [potíže Githubu](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)</span><span class="sxs-lookup"><span data-stu-id="7b069-450">File a [GitHub Issue](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)</span></span>
* <span data-ttu-id="7b069-451">Fóru pro zpětnou vazbu pro návrhy a toocreate sestavy chyb [http://aka.ms/opinsightsfeedback](http://aka.ms/opinsightsfeedback)</span><span class="sxs-lookup"><span data-stu-id="7b069-451">Feedback forum for ideas and toocreate a bug report [http://aka.ms/opinsightsfeedback](http://aka.ms/opinsightsfeedback)</span></span>

### <a name="important-log-locations"></a><span data-ttu-id="7b069-452">Umístění důležité protokolů</span><span class="sxs-lookup"><span data-stu-id="7b069-452">Important log locations</span></span>
| <span data-ttu-id="7b069-453">File</span><span class="sxs-lookup"><span data-stu-id="7b069-453">File</span></span> | <span data-ttu-id="7b069-454">Cesta</span><span class="sxs-lookup"><span data-stu-id="7b069-454">Path</span></span> |
| --- | --- |
| <span data-ttu-id="7b069-455">Agent OMS pro soubor protokolu systému Linux</span><span class="sxs-lookup"><span data-stu-id="7b069-455">OMS Agent for Linux Log File</span></span> |`/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log ` |
| <span data-ttu-id="7b069-456">V souboru protokolu konfigurace agenta OMS</span><span class="sxs-lookup"><span data-stu-id="7b069-456">OMS Agent Configuration Log File</span></span> |`/var/opt/microsoft/omsconfig/omsconfig.log` |

### <a name="important-configuration-files"></a><span data-ttu-id="7b069-457">Důležité konfigurační soubory</span><span class="sxs-lookup"><span data-stu-id="7b069-457">Important configuration files</span></span>
| <span data-ttu-id="7b069-458">Catergory</span><span class="sxs-lookup"><span data-stu-id="7b069-458">Catergory</span></span> | <span data-ttu-id="7b069-459">Umístění souboru</span><span class="sxs-lookup"><span data-stu-id="7b069-459">File Location</span></span> |
| --- | --- |
| <span data-ttu-id="7b069-460">Syslog</span><span class="sxs-lookup"><span data-stu-id="7b069-460">Syslog</span></span> |<span data-ttu-id="7b069-461">`/etc/syslog-ng/syslog-ng.conf`nebo `/etc/rsyslog.conf` nebo`/etc/rsyslog.d/95-omsagent.conf`</span><span class="sxs-lookup"><span data-stu-id="7b069-461">`/etc/syslog-ng/syslog-ng.conf` or `/etc/rsyslog.conf` or `/etc/rsyslog.d/95-omsagent.conf`</span></span> |
| <span data-ttu-id="7b069-462">Výkon, Nagios, Zabbix, OMS výstup a obecné agenta</span><span class="sxs-lookup"><span data-stu-id="7b069-462">Performance, Nagios, Zabbix, OMS output and general agent</span></span> |`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` |
| <span data-ttu-id="7b069-463">Další konfigurace</span><span class="sxs-lookup"><span data-stu-id="7b069-463">Additional configurations</span></span> |`/etc/opt/microsoft/omsagent/<workspace id>/omsagent.d/*.conf` |

> [!NOTE]
> <span data-ttu-id="7b069-464">Úpravy konfigurační soubory pro čítače výkonu a syslog jsou přepsány, pokud je povoleno nastavení portálu OMS.</span><span class="sxs-lookup"><span data-stu-id="7b069-464">Editing configuration files for performance counters and syslog are overwritten if OMS Portal Configuration is enabled.</span></span> <span data-ttu-id="7b069-465">Můžete zakázat konfiguraci v hello portálu OMS (pro všechny uzly) nebo pro jeden uzly spuštěním hello následující:</span><span class="sxs-lookup"><span data-stu-id="7b069-465">You can disable configuration in hello OMS Portal (for all nodes) or for single nodes by running hello following:</span></span>
>
>

```
sudo su omsagent -c /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```


### <a name="enable-debug-logging"></a><span data-ttu-id="7b069-466">Povolit protokolování ladění</span><span class="sxs-lookup"><span data-stu-id="7b069-466">Enable debug logging</span></span>
<span data-ttu-id="7b069-467">ladění tooenable protokolování, můžete použít modul plug-in výstup hello OMS a podrobný výstup.</span><span class="sxs-lookup"><span data-stu-id="7b069-467">tooenable debug logging, you can use hello OMS output plugin and verbose output.</span></span>

#### <a name="oms-output-plugin"></a><span data-ttu-id="7b069-468">Modul plug-in výstup OMS</span><span class="sxs-lookup"><span data-stu-id="7b069-468">OMS output plugin</span></span>
<span data-ttu-id="7b069-469">FluentD umožňuje hello modul plug-in toospecify úrovně protokolování pro úrovně jiný protokol pro vstupy a výstupy.</span><span class="sxs-lookup"><span data-stu-id="7b069-469">FluentD allows hello plugin toospecify logging levels for different log levels for inputs and outputs.</span></span> <span data-ttu-id="7b069-470">toospecify úrovní jiný protokol pro výstup OMS upravit konfiguraci hello obecné agenta v hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` souboru.</span><span class="sxs-lookup"><span data-stu-id="7b069-470">toospecify a different log level for OMS output, edit hello general agent configuration in hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` file.</span></span>

<span data-ttu-id="7b069-471">Téměř hello dolní části hello konfigurační soubor, změňte hello `log_level` vlastnost z `info` příliš`debug`.</span><span class="sxs-lookup"><span data-stu-id="7b069-471">Near hello bottom of hello configuration file, change hello `log_level` property from `info` too`debug`.</span></span>

 ```
 <match oms.** docker.**>
  type out_oms
  log_level debug
  num_threads 5
  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
 ```

<span data-ttu-id="7b069-472">Protokolování ladění vám umožní toosee zpracovat v dávce nahrávání toohello OMS služby oddělených typ, počet datových položek a doba trvání toosend.</span><span class="sxs-lookup"><span data-stu-id="7b069-472">Debug logging allows you toosee batched uploads toohello OMS Service separated by type, number of data items, and time taken toosend.</span></span>

<span data-ttu-id="7b069-473">*Příklad povoleno ladění protokolu:*</span><span class="sxs-lookup"><span data-stu-id="7b069-473">*Example debug enabled log:*</span></span>

```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

#### <a name="verbose-output"></a><span data-ttu-id="7b069-474">Podrobný výstup</span><span class="sxs-lookup"><span data-stu-id="7b069-474">Verbose output</span></span>
<span data-ttu-id="7b069-475">Místo použití modulu plug-in výstup hello OMS, také výstup můžete položky dat přímo příliš`stdout`, který se zobrazí v hello agenta OMS pro soubor protokolu systému Linux.</span><span class="sxs-lookup"><span data-stu-id="7b069-475">Instead of using hello OMS output plugin, you can also output data items directly too`stdout`, which is visible in hello OMS Agent for Linux log file.</span></span>

<span data-ttu-id="7b069-476">V souboru konfigurace obecné agenta hello OMS na `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, okomentujte hello OMS výstup přidáním modulů plug-in `#` před každý řádek.</span><span class="sxs-lookup"><span data-stu-id="7b069-476">In hello OMS general agent configuration file at `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, comment-out hello OMS output plugin by adding a `#` in front of each line.</span></span>

```
#<match oms.** docker.**>
#  type out_oms
#  log_level info
#  num_threads 5
#  buffer_chunk_limit 5m
#  buffer_type file
#  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
#  buffer_queue_limit 10
#  flush_interval 20s
#  retry_limit 10
#  retry_wait 30s
#</match>
```

<span data-ttu-id="7b069-477">Níže hello výstup modulu plug-in, odeberte hello komentář v následující části odebráním hello hello `#` symbol na hello začátku každého řádku.</span><span class="sxs-lookup"><span data-stu-id="7b069-477">Below hello output plugin, remove hello comment in hello following section by removing hello `#` symbol at hello beginning of each line.</span></span>

```
<match **>
  type stdout
</match>
```

### <a name="forwarded-syslog-messages-do-not-appear-in-hello-log"></a><span data-ttu-id="7b069-478">Přesměrovaná zprávy Syslog se nezobrazí v protokolu hello</span><span class="sxs-lookup"><span data-stu-id="7b069-478">Forwarded Syslog messages do not appear in hello log</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="7b069-479">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="7b069-479">Probable causes</span></span>
* <span data-ttu-id="7b069-480">Hello použitá konfigurace toohello Linux serveru povolit kolekce zařízení hello odeslat nebo protokolování úrovně</span><span class="sxs-lookup"><span data-stu-id="7b069-480">hello configuration applied toohello Linux server does not allow collection of hello sent facilities and/or log levels</span></span>
* <span data-ttu-id="7b069-481">Syslog není předávaná správně toohello Linux server</span><span class="sxs-lookup"><span data-stu-id="7b069-481">Syslog is not being forwarded correctly toohello Linux server</span></span>
* <span data-ttu-id="7b069-482">jsou příliš velké vzhledem k základní konfiguraci hello hello OMS agenta pro Linux toohandle Hello počet zpráv předávaná za sekundu</span><span class="sxs-lookup"><span data-stu-id="7b069-482">hello number of messages being forwarded per second are too large for hello base configuration of hello OMS Agent for Linux toohandle</span></span>

#### <a name="resolutions"></a><span data-ttu-id="7b069-483">Řešení</span><span class="sxs-lookup"><span data-stu-id="7b069-483">Resolutions</span></span>
* <span data-ttu-id="7b069-484">Ověřte konfiguraci tohoto hello v hello portálu OMS, pro Syslog má všechny hello zařízení a úrovně správné protokolu hello</span><span class="sxs-lookup"><span data-stu-id="7b069-484">Verify that hello configuration in hello OMS Portal for Syslog has all hello facilities and hello correct log levels</span></span>
  * <span data-ttu-id="7b069-485">**Portál OMS > Nastavení > Data > Syslog**</span><span class="sxs-lookup"><span data-stu-id="7b069-485">**OMS Portal > Settings > Data > Syslog**</span></span>
* <span data-ttu-id="7b069-486">Ověřte, že nativní syslog zasílání zpráv démoni (`rsyslog`, `syslog-ng`) jsou možné tooreceive hello předávat zprávy</span><span class="sxs-lookup"><span data-stu-id="7b069-486">Verify that native syslog messaging daemons (`rsyslog`, `syslog-ng`) are able tooreceive hello forwarded messages</span></span>
* <span data-ttu-id="7b069-487">Zkontrolujte nastavení brány firewall na tooensure serveru Syslog hello nejsou blokovány zprávy</span><span class="sxs-lookup"><span data-stu-id="7b069-487">Check firewall settings on hello Syslog server tooensure that messages are not being blocked</span></span>
* <span data-ttu-id="7b069-488">Simulovat tooOMS zprávy Syslog pomocí hello `logger` příkaz – například:</span><span class="sxs-lookup"><span data-stu-id="7b069-488">Simulate a Syslog message tooOMS using hello `logger` command - for example:</span></span>
  * `logger -p local0.err "This is my test message"`

### <a name="problems-connecting-toooms-when-using-a-proxy"></a><span data-ttu-id="7b069-489">Potíže s připojením tooOMS při použití serveru proxy</span><span class="sxs-lookup"><span data-stu-id="7b069-489">Problems connecting tooOMS when using a proxy</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="7b069-490">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="7b069-490">Probable causes</span></span>
* <span data-ttu-id="7b069-491">Hello proxy zadaný při instalaci a konfiguraci agenta hello je nesprávný</span><span class="sxs-lookup"><span data-stu-id="7b069-491">hello proxy specified when installing and configuring hello agent is incorrect</span></span>
* <span data-ttu-id="7b069-492">Koncové body služby OMS Hello nejsou whitelistested ve vašem datovém centru</span><span class="sxs-lookup"><span data-stu-id="7b069-492">hello OMS Service endpoints are not whitelistested in your datacenter</span></span>

#### <a name="resolutions"></a><span data-ttu-id="7b069-493">Řešení</span><span class="sxs-lookup"><span data-stu-id="7b069-493">Resolutions</span></span>
* <span data-ttu-id="7b069-494">Přeinstalujte hello OMS agenta pro Linux pomocí hello následující příkaz s možností hello `-v` povolena.</span><span class="sxs-lookup"><span data-stu-id="7b069-494">Reinstall hello OMS Agent for Linux using hello following command with hello option `-v` enabled.</span></span> <span data-ttu-id="7b069-495">To umožňuje podrobný výstup hello agenta připojení prostřednictvím hello proxy toohello služby OMS.</span><span class="sxs-lookup"><span data-stu-id="7b069-495">This allows verbose output of hello agent connecting through hello proxy toohello OMS Service.</span></span>
  * `/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`
  * <span data-ttu-id="7b069-496">Přečtěte si dokumentaci k hello proxy OMS na [konfigurace hello agenta pro použití se HTTP proxy server](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#configuring-the-agent-for-use-with-an-http-proxy-server)</span><span class="sxs-lookup"><span data-stu-id="7b069-496">Review hello documentation for OMS proxy at [Configuring hello agent for use with an HTTP proxy server](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#configuring-the-agent-for-use-with-an-http-proxy-server)</span></span>
* <span data-ttu-id="7b069-497">Ověřte, že hello následující koncové body služby OMS jsou seznam povolených adres</span><span class="sxs-lookup"><span data-stu-id="7b069-497">Verify that hello following OMS Service endpoints are whitelisted</span></span>

| <span data-ttu-id="7b069-498">Prostředek agenta</span><span class="sxs-lookup"><span data-stu-id="7b069-498">Agent Resource</span></span> | <span data-ttu-id="7b069-499">Porty</span><span class="sxs-lookup"><span data-stu-id="7b069-499">Ports</span></span> |
| --- | --- |
| <span data-ttu-id="7b069-500">&#42;. Ods.opinsights.Azure.com</span><span class="sxs-lookup"><span data-stu-id="7b069-500">&#42;.ods.opinsights.azure.com</span></span> |<span data-ttu-id="7b069-501">Port 443</span><span class="sxs-lookup"><span data-stu-id="7b069-501">Port 443</span></span> |
| <span data-ttu-id="7b069-502">&#42;. OMS.opinsights.Azure.com</span><span class="sxs-lookup"><span data-stu-id="7b069-502">&#42;.oms.opinsights.azure.com</span></span> |<span data-ttu-id="7b069-503">Port 443</span><span class="sxs-lookup"><span data-stu-id="7b069-503">Port 443</span></span> |
| <span data-ttu-id="7b069-504">ods.systemcenteradvisor.com</span><span class="sxs-lookup"><span data-stu-id="7b069-504">ods.systemcenteradvisor.com</span></span> |<span data-ttu-id="7b069-505">Port 443</span><span class="sxs-lookup"><span data-stu-id="7b069-505">Port 443</span></span> |
| <span data-ttu-id="7b069-506">&#42;.blob.core.windows.net/</span><span class="sxs-lookup"><span data-stu-id="7b069-506">&#42;.blob.core.windows.net/</span></span> |<span data-ttu-id="7b069-507">Port 443</span><span class="sxs-lookup"><span data-stu-id="7b069-507">Port 443</span></span> |

### <a name="a-403-error-is-displayed-when-onboarding"></a><span data-ttu-id="7b069-508">Zobrazí se Chyba 403 při připojování</span><span class="sxs-lookup"><span data-stu-id="7b069-508">A 403 error is displayed when onboarding</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="7b069-509">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="7b069-509">Probable causes</span></span>
* <span data-ttu-id="7b069-510">Hello datum a čas jsou nesprávná na Linux Server</span><span class="sxs-lookup"><span data-stu-id="7b069-510">hello date and time are incorrect on Linux Server</span></span>
* <span data-ttu-id="7b069-511">Hello ID pracovního prostoru a používá klíč pracovního prostoru jsou nesprávné</span><span class="sxs-lookup"><span data-stu-id="7b069-511">hello Workspace ID and Workspace Key used are incorrect</span></span>

#### <a name="resolution"></a><span data-ttu-id="7b069-512">Řešení</span><span class="sxs-lookup"><span data-stu-id="7b069-512">Resolution</span></span>
* <span data-ttu-id="7b069-513">Ověřte hello čas na serveru Linux s hello `date` příkaz.</span><span class="sxs-lookup"><span data-stu-id="7b069-513">Verify hello time on your Linux server with hello `date` command.</span></span> <span data-ttu-id="7b069-514">Pokud hello dat je větší nebo menší než 15 minut od hello aktuální čas, registrace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="7b069-514">If hello data is greater than or less than 15 minutes from hello current time, then onboarding fails.</span></span> <span data-ttu-id="7b069-515">toocorrect, aktualizovat hello datum a časové pásmo serveru Linux.</span><span class="sxs-lookup"><span data-stu-id="7b069-515">toocorrect this, update hello date and/or timezone of your Linux server.</span></span>
* <span data-ttu-id="7b069-516">nejnovější verzi hello OMS agenta pro Linux Hello vás upozorní, pokud je časový rozdíl je příčinou selhání registrace</span><span class="sxs-lookup"><span data-stu-id="7b069-516">hello latest version of hello OMS Agent for Linux notifies you if a time difference is causing onboarding failure</span></span>
* <span data-ttu-id="7b069-517">Znovu zařadit pomocí hello správné ID pracovního prostoru a klíč pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="7b069-517">Re-onboard using hello correct Workspace ID and Workspace Key.</span></span> <span data-ttu-id="7b069-518">V tématu [registrace pomocí příkazového řádku hello](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) Další informace.</span><span class="sxs-lookup"><span data-stu-id="7b069-518">See  [Onboarding using hello command line](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) for more information.</span></span>

### <a name="a-500-error-or-404-error-appears-in-hello-log-file-after-onboarding"></a><span data-ttu-id="7b069-519">500 Chyba nebo chybu 404 se zobrazí v souboru protokolu hello po registraci</span><span class="sxs-lookup"><span data-stu-id="7b069-519">A 500 error or 404 error appears in hello log file after onboarding</span></span>
<span data-ttu-id="7b069-520">Jde o známý problém, který spadá hello první nahrávání dat Linux do pracovního prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="7b069-520">This is a known issue that occurs during hello first upload of Linux data into an OMS workspace.</span></span> <span data-ttu-id="7b069-521">To nemá vliv odesílat data nebo jiné problémy.</span><span class="sxs-lookup"><span data-stu-id="7b069-521">This does not affect data being sent or other problems.</span></span> <span data-ttu-id="7b069-522">Můžete ignorovat hello chyby, když se od začátku registrace.</span><span class="sxs-lookup"><span data-stu-id="7b069-522">You can ignore hello errors when initially onboarding.</span></span>

### <a name="nagios-data-does-not-appear-in-hello-oms-portal"></a><span data-ttu-id="7b069-523">Nagios data se nezobrazují v hello portálu OMS</span><span class="sxs-lookup"><span data-stu-id="7b069-523">Nagios data does not appear in hello OMS Portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="7b069-524">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="7b069-524">Probable causes</span></span>
* <span data-ttu-id="7b069-525">Hello omsagent uživatel nemá oprávnění tooread ze souboru protokolu Nagios hello</span><span class="sxs-lookup"><span data-stu-id="7b069-525">hello omsagent user does not have permissions tooread from hello Nagios log file</span></span>
* <span data-ttu-id="7b069-526">Hello Nagios zdroje a filtru oddílů jsou stále vloženy do komentáře v souboru omsagent.conf hello</span><span class="sxs-lookup"><span data-stu-id="7b069-526">hello Nagios source and filter sections are still commented in hello omsagent.conf file</span></span>

#### <a name="resolutions"></a><span data-ttu-id="7b069-527">Řešení</span><span class="sxs-lookup"><span data-stu-id="7b069-527">Resolutions</span></span>
* <span data-ttu-id="7b069-528">Přidáte uživatele omsagent hello v pořadí tooread ze souboru Nagios hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-528">Add hello omsagent user in order tooread from hello Nagios file.</span></span> <span data-ttu-id="7b069-529">V tématu [Nagios výstrahy](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts) Další informace.</span><span class="sxs-lookup"><span data-stu-id="7b069-529">See [Nagios alerts](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts) for more information.</span></span>
* <span data-ttu-id="7b069-530">V hello OMS agenta pro Linux obecné konfiguračního souboru v `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, ujistěte se, že **i** hello Nagios zdroje a filtr sekce mají komentáře k odebrání, podobně jako toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="7b069-530">In hello OMS Agent for Linux general configuration file at `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, ensure that **both** hello Nagios source and filter sections have comments removed, similar toohello following example.</span></span>

```
<source>
  type tail
  path /var/log/nagios/nagios.log
  format none
  tag oms.nagios
</source>

<filter oms.nagios>
  type filter_nagios_log
</filter>
```


### <a name="linux-data-doesnt-appear-in-hello-oms-portal"></a><span data-ttu-id="7b069-531">Linux data nezobrazí v hello portálu OMS</span><span class="sxs-lookup"><span data-stu-id="7b069-531">Linux data doesn't appear in hello OMS Portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="7b069-532">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="7b069-532">Probable causes</span></span>
* <span data-ttu-id="7b069-533">Registrace toohello OMS služby se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="7b069-533">Onboarding toohello OMS Service failed</span></span>
* <span data-ttu-id="7b069-534">Připojení toohello OMS služby je blokovaný.</span><span class="sxs-lookup"><span data-stu-id="7b069-534">Connection toohello OMS Service is blocked</span></span>
* <span data-ttu-id="7b069-535">je Hello OMS agenta pro Linux data zálohovaná</span><span class="sxs-lookup"><span data-stu-id="7b069-535">hello OMS Agent for Linux data is backed-up</span></span>

#### <a name="resolutions"></a><span data-ttu-id="7b069-536">Řešení</span><span class="sxs-lookup"><span data-stu-id="7b069-536">Resolutions</span></span>
* <span data-ttu-id="7b069-537">Ověřte, že registrace toohello OMS služby bylo úspěšné kontrolou tohoto hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` existuje.</span><span class="sxs-lookup"><span data-stu-id="7b069-537">Verify that onboarding toohello OMS Service was successful by verifying that hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` exists.</span></span>
* <span data-ttu-id="7b069-538">Znovu zařadit pomocí hello omsadmin.sh příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="7b069-538">Re-onboard using hello omsadmin.sh command line.</span></span> <span data-ttu-id="7b069-539">V tématu [registrace pomocí příkazového řádku hello](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) Další informace.</span><span class="sxs-lookup"><span data-stu-id="7b069-539">See [Onboarding using hello command line](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) for more information.</span></span>
* <span data-ttu-id="7b069-540">Pokud používáte proxy server, používejte hello proxy pro řešení potíží výše</span><span class="sxs-lookup"><span data-stu-id="7b069-540">If using a proxy, use hello proxy troubleshooting steps above</span></span>
* <span data-ttu-id="7b069-541">V některých případech při hello OMS agenta pro Linux nemůže komunikovat s hello OMS služby data na hello agenta je zálohovaná toohello úplné vyrovnávací paměti velikost 50 MB.</span><span class="sxs-lookup"><span data-stu-id="7b069-541">In some cases, when hello OMS Agent for Linux cannot communicate with hello OMS Service, data on hello Agent is backed-up toohello full buffer size of 50 MB.</span></span> <span data-ttu-id="7b069-542">Restartujte hello OMS agenta pro Linux spuštěním hello `/opt/microsoft/omsagent/bin/service_control restart` příkaz.</span><span class="sxs-lookup"><span data-stu-id="7b069-542">Restart hello OMS Agent for Linux by running hello `/opt/microsoft/omsagent/bin/service_control restart` command.</span></span>
  >[AZURE.NOTE] <span data-ttu-id="7b069-543">Tento problém vyřešen v 1.1.0-28 verze agenta nebo novější.</span><span class="sxs-lookup"><span data-stu-id="7b069-543">This issue is fixed in Agent version 1.1.0-28 and later.</span></span>

### <a name="syslog-linux-performance-counter-configuration-is-not-applied-in-hello-oms-portal"></a><span data-ttu-id="7b069-544">Konfigurace čítače výkonu Syslog Linux není použita na portálu OMS hello</span><span class="sxs-lookup"><span data-stu-id="7b069-544">Syslog Linux performance counter configuration is not applied in hello OMS portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="7b069-545">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="7b069-545">Probable causes</span></span>
* <span data-ttu-id="7b069-546">Hello konfiguraci agenta v hello OMS agenta pro Linux nebyla načtena hello nejnovější konfigurace z portálu OMS hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-546">hello configuration agent in hello OMS Agent for Linux has not retrieved hello latest configuration from hello OMS portal.</span></span>
* <span data-ttu-id="7b069-547">Hello revidovaný nastavení portálu hello nebyly použity</span><span class="sxs-lookup"><span data-stu-id="7b069-547">hello revised settings in hello portal were not applied</span></span>

#### <a name="resolutions"></a><span data-ttu-id="7b069-548">Řešení</span><span class="sxs-lookup"><span data-stu-id="7b069-548">Resolutions</span></span>
<span data-ttu-id="7b069-549">`omsconfig`je hello konfiguraci agenta v hello OMS agenta pro Linux, který načte změny konfigurace portálu OMS každých 5 minut.</span><span class="sxs-lookup"><span data-stu-id="7b069-549">`omsconfig` is hello configuration agent in hello OMS Agent for Linux that retrieves OMS portal configuration changes every 5 minutes.</span></span> <span data-ttu-id="7b069-550">Tato konfigurace je pak použité toohello OMS agenta pro Linux konfigurační soubory umístěné v `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.</span><span class="sxs-lookup"><span data-stu-id="7b069-550">This configuration is then applied toohello OMS Agent for Linux configuration files located at `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.</span></span>

* <span data-ttu-id="7b069-551">V některých případech hello OMS agenta pro Linux konfigurace agenta nemusí být možné toocommunicate službou hello konfigurace portálu, což vede k nejnovější konfigurace nebudou použity.</span><span class="sxs-lookup"><span data-stu-id="7b069-551">In some cases, hello OMS Agent for Linux configuration agent might not be able toocommunicate with hello portal configuration service resulting in latest configuration not being applied.</span></span>
* <span data-ttu-id="7b069-552">Ověřte, že hello `omsconfig` je agent nainstalovaný s hello následující:</span><span class="sxs-lookup"><span data-stu-id="7b069-552">Verify that hello `omsconfig` agent is installed with hello following:</span></span>

  * <span data-ttu-id="7b069-553">`dpkg --list omsconfig` nebo `rpm -qi omsconfig`</span><span class="sxs-lookup"><span data-stu-id="7b069-553">`dpkg --list omsconfig` or `rpm -qi omsconfig`</span></span>
  * <span data-ttu-id="7b069-554">Pokud nainstalovaná není, znovu nainstalujte nejnovější verzi hello agenta OMS hello pro Linux</span><span class="sxs-lookup"><span data-stu-id="7b069-554">If not installed, reinstall hello latest version of hello OMS Agent for Linux</span></span>
* <span data-ttu-id="7b069-555">Ověřte, že hello `omsconfig` agent může komunikovat s hello OMS služby</span><span class="sxs-lookup"><span data-stu-id="7b069-555">Verify that hello `omsconfig` agent can communicate with hello OMS service</span></span>

  * <span data-ttu-id="7b069-556">Spustit hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` příkaz</span><span class="sxs-lookup"><span data-stu-id="7b069-556">Run hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` command</span></span>
    * <span data-ttu-id="7b069-557">Hello nahoře na příkaz vrátí hello konfiguraci tohoto agenta načte z hello portálu, včetně nastavení Syslog, čítače výkonu systému Linux a vlastní protokoly</span><span class="sxs-lookup"><span data-stu-id="7b069-557">hello command above returns hello configuration that agent retrieves from hello portal, including Syslog settings, Linux performance counters, and custom logs</span></span>
    * <span data-ttu-id="7b069-558">Pokud se výše hello příkazu nezdaří, spusťte hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` příkaz.</span><span class="sxs-lookup"><span data-stu-id="7b069-558">If hello command above fails, run hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` command.</span></span> <span data-ttu-id="7b069-559">Tento příkaz zajistí hello omsconfig agenta toocommunicate s hello OMS služby tooretrieve hello nejnovější konfigurací.</span><span class="sxs-lookup"><span data-stu-id="7b069-559">This command forces hello omsconfig agent toocommunicate with hello OMS service tooretrieve hello latest configuration.</span></span>

### <a name="custom-linux-log-data-does-not-appear-in-hello-oms-portal"></a><span data-ttu-id="7b069-560">Vlastní data protokolu Linux nejsou uvedené v hello portálu OMS</span><span class="sxs-lookup"><span data-stu-id="7b069-560">Custom Linux log data does not appear in hello OMS Portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="7b069-561">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="7b069-561">Probable causes</span></span>
* <span data-ttu-id="7b069-562">Registrace tooOMS služby se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="7b069-562">Onboarding tooOMS Service failed</span></span>
* <span data-ttu-id="7b069-563">Hello **hello použít následující konfigurace toomy servery se systémem Linux** nebylo vybráno nastavení</span><span class="sxs-lookup"><span data-stu-id="7b069-563">hello **Apply hello following configuration toomy Linux Servers** setting has not been selected</span></span>
* <span data-ttu-id="7b069-564">omsconfig nebyl zachyceny nejnovější vlastní protokol hello z portálu hello</span><span class="sxs-lookup"><span data-stu-id="7b069-564">omsconfig has not picked up hello latest custom log from hello portal</span></span>
* <span data-ttu-id="7b069-565">Hello `omsagent` použití je nelze tooaccess hello vlastního protokolu z důvodu problému oprávnění tooa nebo `omsagent` nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="7b069-565">hello `omsagent` use is unable tooaccess hello custom log due tooa permissions problem or `omsagent` was not found.</span></span> <span data-ttu-id="7b069-566">V takovém případě se zobrazí hello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="7b069-566">In this case, you'll see hello following output:</span></span>
  * `[DATETIME] [warn]: file not found. Continuing without tailing it.`
  * `[DATETIME] [error]: file not accessible by omsagent.`
* <span data-ttu-id="7b069-567">Jedná se o známý problém s hello časování, která byla opravena v hello OMS agenta pro Linux verze 1.1.0-217</span><span class="sxs-lookup"><span data-stu-id="7b069-567">This is a known issue with hello Race Condition that was fixed in hello OMS Agent for Linux version 1.1.0-217</span></span>

#### <a name="resolutions"></a><span data-ttu-id="7b069-568">Řešení</span><span class="sxs-lookup"><span data-stu-id="7b069-568">Resolutions</span></span>
* <span data-ttu-id="7b069-569">Ověřte, že jste úspěšně zařazený, nemá tak, že určíte, zda hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` soubor existuje.</span><span class="sxs-lookup"><span data-stu-id="7b069-569">Verify that you've successfully onboarded, by determining whether hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` file exists.</span></span>
  * <span data-ttu-id="7b069-570">Pokud potřebné, zařadit znovu s použitím hello omsadmin.sh příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="7b069-570">If needed, onboard again using hello omsadmin.sh command line.</span></span> <span data-ttu-id="7b069-571">V tématu [registrace pomocí příkazového řádku hello](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) Další informace.</span><span class="sxs-lookup"><span data-stu-id="7b069-571">See [Onboarding using hello command line](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) for more information.</span></span>
* <span data-ttu-id="7b069-572">V hello portálu OMS, v části **nastavení** na hello **Data** kartě, ujistěte se, že hello **hello použít následující konfigurace toomy servery se systémem Linux** je vybráno nastavení</span><span class="sxs-lookup"><span data-stu-id="7b069-572">In hello OMS Portal, under **Settings** on hello **Data** tab, ensure that hello **Apply hello following configuration toomy Linux Servers** setting is selected</span></span>  
  <span data-ttu-id="7b069-573">![použít konfiguraci](./media/log-analytics-linux-agents/customloglinuxenabled.png)</span><span class="sxs-lookup"><span data-stu-id="7b069-573">![apply configuration](./media/log-analytics-linux-agents/customloglinuxenabled.png)</span></span>
* <span data-ttu-id="7b069-574">Ověřte, že hello `omsconfig` agent může komunikovat s hello OMS služby</span><span class="sxs-lookup"><span data-stu-id="7b069-574">Verify that hello `omsconfig` agent can communicate with hello OMS service</span></span>

  * <span data-ttu-id="7b069-575">Spustit hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` příkaz</span><span class="sxs-lookup"><span data-stu-id="7b069-575">Run hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` command</span></span>
  * <span data-ttu-id="7b069-576">Hello nahoře na příkaz vrátí hello konfiguraci tohoto agenta načte z portálu, včetně nastavení Syslog, čítače výkonu systému Linux a vlastní protokoly hello</span><span class="sxs-lookup"><span data-stu-id="7b069-576">hello command above returns hello configuration that agent retrieves from hello Portal, including Syslog settings, Linux performance counters, and custom Logs</span></span>
  * <span data-ttu-id="7b069-577">Pokud se výše hello příkazu nezdaří, spusťte hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` příkaz.</span><span class="sxs-lookup"><span data-stu-id="7b069-577">If hello command above fails, run hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` command.</span></span> <span data-ttu-id="7b069-578">Tento příkaz vynutí hello omsconfig agenta toocommunicate službou OMS a načíst nejnovější konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-578">This command forces hello omsconfig agent toocommunicate with OMS service and retrieve hello latest configuration.</span></span>

<span data-ttu-id="7b069-579">Místo hello OMS agenta pro Linux uživatel, který spouští jako privilegovaných uživatelů `root`, hello OMS agenta pro Linux běží jako hello `omsagent` uživatele.</span><span class="sxs-lookup"><span data-stu-id="7b069-579">Instead of hello OMS Agent for Linux user running as a privileged user `root`, hello OMS Agent for Linux runs as hello `omsagent` user.</span></span> <span data-ttu-id="7b069-580">Ve většině případů musí být výslovná oprávnění udělená toohello uživateli v pořadí tooread určité soubory.</span><span class="sxs-lookup"><span data-stu-id="7b069-580">In most cases, explicit permission must be granted toohello user in order tooread certain files.</span></span>

<span data-ttu-id="7b069-581">oprávnění toogrant příliš`omsagent` uživatele, spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="7b069-581">toogrant permission too`omsagent` user, run hello following commands:</span></span>

1. <span data-ttu-id="7b069-582">Přidat hello `omsagent` konkrétní skupiny uživatelů tooa s`sudo usermod -a -G <GROUPNAME> <USERNAME>`</span><span class="sxs-lookup"><span data-stu-id="7b069-582">Add hello `omsagent` user tooa specific group with `sudo usermod -a -G <GROUPNAME> <USERNAME>`</span></span>
2. <span data-ttu-id="7b069-583">Požadovaný soubor toohello grant univerzální přístup pro čtení s`sudo chmod -R ugo+rw <FILE DIRECTORY>`</span><span class="sxs-lookup"><span data-stu-id="7b069-583">Grant universal read access toohello required file with `sudo chmod -R ugo+rw <FILE DIRECTORY>`</span></span>

<span data-ttu-id="7b069-584">Je známý problém s hello časování, která byla opravena v hello OMS agenta pro Linux verze 1.1.0-217.</span><span class="sxs-lookup"><span data-stu-id="7b069-584">There is a known issue with hello Race Condition that was fixed in hello OMS Agent for Linux version 1.1.0-217.</span></span> <span data-ttu-id="7b069-585">Po aktualizaci toohello nejnovější verze agenta, spusťte následující příkaz tooget hello nejnovější verzi modulů plug-in výstup hello hello:</span><span class="sxs-lookup"><span data-stu-id="7b069-585">After updating toohello latest agent, run hello following command tooget hello latest version of hello output plugin:</span></span>

```
sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf
```

## <a name="known-limitations"></a><span data-ttu-id="7b069-586">Známá omezení</span><span class="sxs-lookup"><span data-stu-id="7b069-586">Known limitations</span></span>
<span data-ttu-id="7b069-587">Zkontrolujte následující oddíly toolearn o aktuálních omezeních hello OMS agenta pro Linux hello.</span><span class="sxs-lookup"><span data-stu-id="7b069-587">Review hello following sections toolearn about current limitations of hello OMS Agent for Linux.</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="7b069-588">Diagnostika Azure</span><span class="sxs-lookup"><span data-stu-id="7b069-588">Azure Diagnostics</span></span>
<span data-ttu-id="7b069-589">Pro virtuální počítače Linux běží v Azure mohou být další kroky požadované tooallow shromažďování dat pomocí diagnostiky Azure a Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="7b069-589">For Linux virtual machines running in Azure, additional steps may be required tooallow data collection by Azure Diagnostics and Operations Management Suite.</span></span> <span data-ttu-id="7b069-590">**Verze 2.2** hello rozšíření diagnostiky pro Linux je vyžadována pro kompatibilitu s hello OMS agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="7b069-590">**Version 2.2** of hello Diagnostics Extension for Linux is required for compatibility with hello OMS Agent for Linux.</span></span>

<span data-ttu-id="7b069-591">Další informace o instalaci a konfiguraci hello rozšíření diagnostiky pro Linux najdete v tématu [použít hello rozhraní příkazového řádku Azure příkaz tooenable rozšíření diagnostiky Linux](../virtual-machines/linux/classic/diagnostic-extension-v2.md#use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension).</span><span class="sxs-lookup"><span data-stu-id="7b069-591">For more information on installing and configuring hello Diagnostic Extension for Linux, see [Use hello Azure CLI command tooenable Linux Diagnostic Extension](../virtual-machines/linux/classic/diagnostic-extension-v2.md#use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension).</span></span>

<span data-ttu-id="7b069-592">**Upgrade hello rozšíření diagnostiky Linux z 2.0 too2.2 Azure CLI ASM:**</span><span class="sxs-lookup"><span data-stu-id="7b069-592">**Upgrading hello Linux Diagnostics Extension from 2.0 too2.2 Azure CLI ASM:**</span></span>

```
azure vm extension set -u <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.0
azure vm extension set <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

<span data-ttu-id="7b069-593">**ARM**</span><span class="sxs-lookup"><span data-stu-id="7b069-593">**ARM**</span></span>

```
azure vm extension set -u <resource-group> <vm-name> Microsoft.Insights.VMDiagnosticsSettings Microsoft.OSTCExtensions 2.0
azure vm extension set <resource-group> <vm-name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

<span data-ttu-id="7b069-594">Tyto příklady příkaz odkazovat na soubor s názvem PrivateConfig.json.</span><span class="sxs-lookup"><span data-stu-id="7b069-594">These command examples reference a file named PrivateConfig.json.</span></span> <span data-ttu-id="7b069-595">Formát Hello tohoto souboru by měla vypadat přibližně hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="7b069-595">hello format of that file should resemble hello following sample.</span></span>

```
    {
    "storageAccountName":"hello storage account tooreceive data",
    "storageAccountKey":"hello key of hello account"
    }
```

### <a name="sysklog-is-not-supported"></a><span data-ttu-id="7b069-596">Sysklog není podporován.</span><span class="sxs-lookup"><span data-stu-id="7b069-596">Sysklog is not supported</span></span>
<span data-ttu-id="7b069-597">Rsyslog nebo syslog ng jsou požadované toocollect zprávy syslog.</span><span class="sxs-lookup"><span data-stu-id="7b069-597">Either rsyslog or syslog-ng are required toocollect syslog messages.</span></span> <span data-ttu-id="7b069-598">Démon procesu syslog výchozí Hello na verze 5 Red Hat Enterprise Linux a CentOS, Oracle Linux verze (sysklog) není podporována pro shromažďování událostí syslog.</span><span class="sxs-lookup"><span data-stu-id="7b069-598">hello default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="7b069-599">toocollect syslog data z této verze těchto distribuce hello rsyslog démon by měl být nainstalován a nakonfigurován tooreplace sysklog.</span><span class="sxs-lookup"><span data-stu-id="7b069-599">toocollect syslog data from this version of these distributions, hello rsyslog daemon should be installed and configured tooreplace sysklog.</span></span> <span data-ttu-id="7b069-600">Další informace o nahrazení sysklog rsyslog najdete v tématu [nainstalovat hello nově vytvořené rsyslog RPM](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM).</span><span class="sxs-lookup"><span data-stu-id="7b069-600">For more information on replacing sysklog with rsyslog, see [Install hello newly built rsyslog RPM](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7b069-601">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7b069-601">Next Steps</span></span>
* <span data-ttu-id="7b069-602">[Přidat řešení pro analýzu protokolu z hello řešení Galerie](log-analytics-add-solutions.md) tooadd funkce a shromáždění dat.</span><span class="sxs-lookup"><span data-stu-id="7b069-602">[Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md) tooadd functionality and gather data.</span></span>
* <span data-ttu-id="7b069-603">Seznamte se s [protokolu hledání](log-analytics-log-searches.md) tooview podrobné informace shromážděné řešení.</span><span class="sxs-lookup"><span data-stu-id="7b069-603">Get familiar with [log searches](log-analytics-log-searches.md) tooview detailed information gathered by solutions.</span></span>
* <span data-ttu-id="7b069-604">Použití [řídicí panely](log-analytics-dashboards.md) toosave a zobrazit prohledává vlastní vlastní.</span><span class="sxs-lookup"><span data-stu-id="7b069-604">Use [dashboards](log-analytics-dashboards.md) toosave and display your own custom searches.</span></span>
