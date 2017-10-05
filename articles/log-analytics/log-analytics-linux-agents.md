---
redirect_url: /azure/log-analytics/log-analytics-agent-linux
redirect_document_id: TRUE
ROBOTS: NOINDEX
ms.openlocfilehash: 8332bdd39effab8c2ac9a75ca9a1e2510c940719
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-your-linux-computers-to-log-analytics"></a><span data-ttu-id="a6c4e-101">Připojení počítačů Linux k analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="a6c4e-101">Connect your Linux computers to Log Analytics</span></span>
<span data-ttu-id="a6c4e-102">Pomocí analýzy protokolů, můžete shromažďovat a fungovat na informace shromážděné z počítače se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-102">Using Log Analytics, you can collect and act on data generated from Linux computers.</span></span> <span data-ttu-id="a6c4e-103">Přidání data shromážděná z Linux k OMS umožňuje spravovat systémy Linux a kontejner řešení jako Docker, bez ohledu na to, kde jsou počítače umístěny – prakticky odkudkoli.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-103">Adding data collected from Linux to OMS allows you to manage Linux systems and container solutions like Docker, regardless of where your computers are located — virtually anywhere.</span></span> <span data-ttu-id="a6c4e-104">Zdroje dat mohou být umístěné ve vašem datovém centru místní jako fyzické servery, virtuální počítače ve službě hostovaných v cloudu jako Amazon Web Services (AWS) nebo Microsoft Azure nebo i přenosných počítačů na vaše oddělení podpory.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-104">Data sources might reside in your on-premises datacenter as physical servers, virtual computers in a cloud-hosted service like Amazon Web Services (AWS) or Microsoft Azure, or even the laptop on your desk.</span></span> <span data-ttu-id="a6c4e-105">Kromě toho OMS taky shromažďuje data z počítače se systémem Windows podobně, takže podporuje hybridním skutečně IT prostředí.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-105">In addition, OMS also collects data from Windows computers similarly, so it supports a truly hybrid IT environment.</span></span>

<span data-ttu-id="a6c4e-106">Můžete zobrazit a spravovat data ze všech těchto zdrojů s analýzy protokolů v OMS pomocí portálu správy.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-106">You can view and manage data from all of those sources with Log Analytics in OMS with a single management portal.</span></span> <span data-ttu-id="a6c4e-107">Tím se snižuje potřebu pomocí mnoha různými systémy, díky usnadňují využívat a vy můžete exportovat do ať řešení obchodní analýzy nebo systém, který už máte data, která chcete sledovat.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-107">This reduces the need to monitor it using many different systems, makes it easy to consume, and you can export any data you like to whatever business analytics solution or system that you already have.</span></span>

<span data-ttu-id="a6c4e-108">Tento článek je rychlý úvodní příručce, která vám pomůže shromažďovat a spravovat data pro počítače Linux pomocí agenta OMS pro Linux.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-108">This article is a quick start guide that will help you collect and manage data for your Linux computers using the OMS Agent for Linux.</span></span> <span data-ttu-id="a6c4e-109">Další technické podrobnosti jako konfiguraci proxy serveru, informace o CollectD metriky a vlastní zdroje dat JSON najdete tyto informace v [OMS agenta pro Linux přehled](https://github.com/Microsoft/OMS-Agent-for-Linux) a [OMS agenta pro Linux úplné dokumentace](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-109">For more technical details such as proxy server configuration, information about CollectD metrics, and custom JSON data sources, you’ll find that information at [OMS Agent for Linux overview](https://github.com/Microsoft/OMS-Agent-for-Linux) and [OMS Agent for Linux full documentation](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) on GitHub.</span></span>

<span data-ttu-id="a6c4e-110">Z počítače se systémem Linux v současné době můžete shromažďovat následující typy dat:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-110">Currently, you can collect the following types of data from Linux computers:</span></span>

* <span data-ttu-id="a6c4e-111">Metriky výkonu</span><span class="sxs-lookup"><span data-stu-id="a6c4e-111">Performance metrics</span></span>
* <span data-ttu-id="a6c4e-112">Události procesu Syslog</span><span class="sxs-lookup"><span data-stu-id="a6c4e-112">Syslog events</span></span>
* <span data-ttu-id="a6c4e-113">Výstrahy od Nagios a Zabbix</span><span class="sxs-lookup"><span data-stu-id="a6c4e-113">Alerts from Nagios and Zabbix</span></span>
* <span data-ttu-id="a6c4e-114">Metriky výkonu kontejner docker, inventář a protokoly</span><span class="sxs-lookup"><span data-stu-id="a6c4e-114">Docker container performance metrics, inventory and logs</span></span>

## <a name="supported-linux-versions"></a><span data-ttu-id="a6c4e-115">Podporované verze systému Linux</span><span class="sxs-lookup"><span data-stu-id="a6c4e-115">Supported Linux versions</span></span>
<span data-ttu-id="a6c4e-116">Verze x86 a x64 jsou oficiálně podporované pro řadu distribucí systému Linux.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-116">Both x86 and x64 versions are officially supported on a variety of Linux distributions.</span></span> <span data-ttu-id="a6c4e-117">OMS agenta pro Linux mohou však spustit také na dalších distribuce, které nejsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-117">However, the OMS Agent for Linux might also run on other distributions not listed.</span></span>

* <span data-ttu-id="a6c4e-118">Linux Amazon 2012.09 prostřednictvím 2015.09</span><span class="sxs-lookup"><span data-stu-id="a6c4e-118">Amazon Linux 2012.09 through 2015.09</span></span>
* <span data-ttu-id="a6c4e-119">CentOS Linux 5, 6 a 7</span><span class="sxs-lookup"><span data-stu-id="a6c4e-119">CentOS Linux 5, 6, and 7</span></span>
* <span data-ttu-id="a6c4e-120">Oracle Linux 5, 6 a 7</span><span class="sxs-lookup"><span data-stu-id="a6c4e-120">Oracle Linux 5, 6, and 7</span></span>
* <span data-ttu-id="a6c4e-121">Red Hat Enterprise Linux Server 5, 6 a 7</span><span class="sxs-lookup"><span data-stu-id="a6c4e-121">Red Hat Enterprise Linux Server 5, 6 and 7</span></span>
* <span data-ttu-id="a6c4e-122">Debian GNU/Linux 6, 7 a 8</span><span class="sxs-lookup"><span data-stu-id="a6c4e-122">Debian GNU/Linux 6, 7, and 8</span></span>
* <span data-ttu-id="a6c4e-123">Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10</span><span class="sxs-lookup"><span data-stu-id="a6c4e-123">Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10</span></span>
* <span data-ttu-id="a6c4e-124">SUSE Linux Enterprise Server 11 a 12</span><span class="sxs-lookup"><span data-stu-id="a6c4e-124">SUSE Linux Enterprise Server 11 and 12</span></span>

## <a name="oms-agent-for-linux"></a><span data-ttu-id="a6c4e-125">OMS agenta pro Linux</span><span class="sxs-lookup"><span data-stu-id="a6c4e-125">OMS Agent for Linux</span></span>
<span data-ttu-id="a6c4e-126">Agent nástroje Operations Management Suite pro Linux obsahuje více balíčků.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-126">The Operations Management Suite Agent for Linux comprises multiple packages.</span></span> <span data-ttu-id="a6c4e-127">Verze souboru obsahuje následující balíčky k dispozici spuštěním sady prostředí s `--extract`.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-127">The release file contains the following packages, available by running the shell bundle with `--extract`.</span></span>

| <span data-ttu-id="a6c4e-128">**Balíček**</span><span class="sxs-lookup"><span data-stu-id="a6c4e-128">**Package**</span></span> | <span data-ttu-id="a6c4e-129">**Verze**</span><span class="sxs-lookup"><span data-stu-id="a6c4e-129">**Version**</span></span> | <span data-ttu-id="a6c4e-130">**Popis**</span><span class="sxs-lookup"><span data-stu-id="a6c4e-130">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a6c4e-131">omsagent</span><span class="sxs-lookup"><span data-stu-id="a6c4e-131">omsagent</span></span> |<span data-ttu-id="a6c4e-132">1.1.0</span><span class="sxs-lookup"><span data-stu-id="a6c4e-132">1.1.0</span></span> |<span data-ttu-id="a6c4e-133">Agent nástroje Operations Management Suite pro Linux</span><span class="sxs-lookup"><span data-stu-id="a6c4e-133">The Operations Management Suite Agent for Linux</span></span> |
| <span data-ttu-id="a6c4e-134">omsconfig</span><span class="sxs-lookup"><span data-stu-id="a6c4e-134">omsconfig</span></span> |<span data-ttu-id="a6c4e-135">1.1.1</span><span class="sxs-lookup"><span data-stu-id="a6c4e-135">1.1.1</span></span> |<span data-ttu-id="a6c4e-136">Konfigurace agenta pro agenta OMS</span><span class="sxs-lookup"><span data-stu-id="a6c4e-136">Configuration agent for the OMS Agent</span></span> |
| <span data-ttu-id="a6c4e-137">OMI</span><span class="sxs-lookup"><span data-stu-id="a6c4e-137">omi</span></span> |<span data-ttu-id="a6c4e-138">1.0.8.3</span><span class="sxs-lookup"><span data-stu-id="a6c4e-138">1.0.8.3</span></span> |<span data-ttu-id="a6c4e-139">Infrastruktury Open Management Infrastructure (OMI) – prosté Server CIM</span><span class="sxs-lookup"><span data-stu-id="a6c4e-139">Open Management Infrastructure (OMI) -- a lightweight CIM Server</span></span> |
| <span data-ttu-id="a6c4e-140">scx.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-140">scx</span></span> |<span data-ttu-id="a6c4e-141">1.6.2</span><span class="sxs-lookup"><span data-stu-id="a6c4e-141">1.6.2</span></span> |<span data-ttu-id="a6c4e-142">Zprostředkovatelé CIM OMI pro metriku výkonu operačního systému</span><span class="sxs-lookup"><span data-stu-id="a6c4e-142">OMI CIM Providers for operating system performance metrics</span></span> |
| <span data-ttu-id="a6c4e-143">Apache cimprov</span><span class="sxs-lookup"><span data-stu-id="a6c4e-143">apache-cimprov</span></span> |<span data-ttu-id="a6c4e-144">1.0.0</span><span class="sxs-lookup"><span data-stu-id="a6c4e-144">1.0.0</span></span> |<span data-ttu-id="a6c4e-145">Zprostředkovatel pro OMI monitorování výkonu serveru Apache HTTP Server.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-145">Apache HTTP Server performance monitoring provider for OMI.</span></span> <span data-ttu-id="a6c4e-146">Instalovat pouze v případě zjištění serveru Apache HTTP Server.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-146">Only installed if Apache HTTP Server is detected.</span></span> |
| <span data-ttu-id="a6c4e-147">MySQL cimprov</span><span class="sxs-lookup"><span data-stu-id="a6c4e-147">mysql-cimprov</span></span> |<span data-ttu-id="a6c4e-148">1.0.0</span><span class="sxs-lookup"><span data-stu-id="a6c4e-148">1.0.0</span></span> |<span data-ttu-id="a6c4e-149">Výkon serveru MySQL monitorování zprostředkovatele pro OMI.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-149">MySQL Server performance monitoring provider for OMI.</span></span> <span data-ttu-id="a6c4e-150">Instalovat pouze v případě zjištění MySQL nebo MariaDB serveru.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-150">Only installed if MySQL/MariaDB server is detected.</span></span> |
| <span data-ttu-id="a6c4e-151">docker cimprov</span><span class="sxs-lookup"><span data-stu-id="a6c4e-151">docker-cimprov</span></span> |<span data-ttu-id="a6c4e-152">0.1.0</span><span class="sxs-lookup"><span data-stu-id="a6c4e-152">0.1.0</span></span> |<span data-ttu-id="a6c4e-153">Zprostředkovatel docker pro OMI.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-153">Docker provider for OMI.</span></span> <span data-ttu-id="a6c4e-154">Instalovat pouze v případě zjištění Docker.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-154">Only installed if Docker is detected.</span></span> |

### <a name="additional-installation-artifacts"></a><span data-ttu-id="a6c4e-155">Artefakty další instalace</span><span class="sxs-lookup"><span data-stu-id="a6c4e-155">Additional installation artifacts</span></span>
<span data-ttu-id="a6c4e-156">Po instalaci agenta OMS pro balíčky Linux, se použijí následující další konfiguraci systémové změny.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-156">After installing the OMS agent for Linux packages, the following additional system-wide configuration changes are applied.</span></span> <span data-ttu-id="a6c4e-157">Tyto artefakty se odeberou, když omsagent balíček odinstalován.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-157">These artifacts are removed when the omsagent package is uninstalled.</span></span>

* <span data-ttu-id="a6c4e-158">Bez oprávnění uživatele s názvem: `omsagent` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-158">A non-privileged user named: `omsagent` is created.</span></span> <span data-ttu-id="a6c4e-159">Toto je účet, který spouští démon omsagent jako</span><span class="sxs-lookup"><span data-stu-id="a6c4e-159">This is the account the omsagent daemon runs as</span></span>
* <span data-ttu-id="a6c4e-160">Vytvoření souboru sudoers "zahrnout" v /etc/sudoers.d/omsagent to autorizuje omsagent restartovat démoni syslog a omsagent.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-160">A sudoers “include” file is created at /etc/sudoers.d/omsagent This authorizes omsagent to restart the syslog and omsagent daemons.</span></span> <span data-ttu-id="a6c4e-161">Pokud direktivy "zahrnout" sudo nejsou podporovány v nainstalované verzi sudo, tyto položky budou zapsány do /etc/sudoers.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-161">If sudo “include” directives are not supported in the installed version of sudo, these entries will be written to /etc/sudoers.</span></span>
* <span data-ttu-id="a6c4e-162">Konfigurace syslog je upravit tak, aby předávat podmnožinu události agenta.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-162">The syslog configuration is modified to forward a subset of events to the agent.</span></span> <span data-ttu-id="a6c4e-163">Další informace najdete v tématu **konfigurace shromažďování dat** části</span><span class="sxs-lookup"><span data-stu-id="a6c4e-163">For more information, see the **Configuring Data Collection** section below</span></span>

### <a name="linux-data-collection-details"></a><span data-ttu-id="a6c4e-164">Podrobnosti kolekce dat Linux</span><span class="sxs-lookup"><span data-stu-id="a6c4e-164">Linux data collection details</span></span>
<span data-ttu-id="a6c4e-165">Následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-165">The following table shows data collection methods and other details about how data is collected.</span></span>

| <span data-ttu-id="a6c4e-166">Zdroj</span><span class="sxs-lookup"><span data-stu-id="a6c4e-166">source</span></span> | <span data-ttu-id="a6c4e-167">Přímé agenta</span><span class="sxs-lookup"><span data-stu-id="a6c4e-167">Direct Agent</span></span> | <span data-ttu-id="a6c4e-168">Agenta nástroje SCOM</span><span class="sxs-lookup"><span data-stu-id="a6c4e-168">SCOM agent</span></span> | <span data-ttu-id="a6c4e-169">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="a6c4e-169">Azure Storage</span></span> | <span data-ttu-id="a6c4e-170">SCOM vyžaduje?</span><span class="sxs-lookup"><span data-stu-id="a6c4e-170">SCOM required?</span></span> | <span data-ttu-id="a6c4e-171">Data agenta SCOM odeslána prostřednictvím skupiny pro správu</span><span class="sxs-lookup"><span data-stu-id="a6c4e-171">SCOM agent data sent via management group</span></span> | <span data-ttu-id="a6c4e-172">Frekvence kolekce</span><span class="sxs-lookup"><span data-stu-id="a6c4e-172">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="a6c4e-173">Zabbix</span><span class="sxs-lookup"><span data-stu-id="a6c4e-173">Zabbix</span></span> |![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="a6c4e-179">1 minuta</span><span class="sxs-lookup"><span data-stu-id="a6c4e-179">1 minute</span></span> |
| <span data-ttu-id="a6c4e-180">Nagios</span><span class="sxs-lookup"><span data-stu-id="a6c4e-180">Nagios</span></span> |![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="a6c4e-186">v případě přijetí</span><span class="sxs-lookup"><span data-stu-id="a6c4e-186">on arrival</span></span> |
| <span data-ttu-id="a6c4e-187">syslog</span><span class="sxs-lookup"><span data-stu-id="a6c4e-187">syslog</span></span> |![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="a6c4e-193">ze služby Azure storage: 10 minut; z agenta: na přijetí</span><span class="sxs-lookup"><span data-stu-id="a6c4e-193">from Azure storage: 10 minutes; from agent: on arrival</span></span> |
| <span data-ttu-id="a6c4e-194">Čítače výkonu systému Linux</span><span class="sxs-lookup"><span data-stu-id="a6c4e-194">Linux performance counters</span></span> |![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="a6c4e-200">podle plánu, minimálně 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-200">As scheduled, minimum of 10 seconds</span></span> |
| <span data-ttu-id="a6c4e-201">sledování změn</span><span class="sxs-lookup"><span data-stu-id="a6c4e-201">change tracking</span></span> |![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |<span data-ttu-id="a6c4e-207">každou hodinu</span><span class="sxs-lookup"><span data-stu-id="a6c4e-207">hourly</span></span> |

### <a name="package-requirements"></a><span data-ttu-id="a6c4e-208">Požadavky na balíček</span><span class="sxs-lookup"><span data-stu-id="a6c4e-208">Package Requirements</span></span>
| <span data-ttu-id="a6c4e-209">**Požadovaný balíček**</span><span class="sxs-lookup"><span data-stu-id="a6c4e-209">**Required package**</span></span> | <span data-ttu-id="a6c4e-210">**Popis**</span><span class="sxs-lookup"><span data-stu-id="a6c4e-210">**Description**</span></span> | <span data-ttu-id="a6c4e-211">**Minimální verze**</span><span class="sxs-lookup"><span data-stu-id="a6c4e-211">**Minimum version**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a6c4e-212">Glibc</span><span class="sxs-lookup"><span data-stu-id="a6c4e-212">Glibc</span></span> |<span data-ttu-id="a6c4e-213">Knihovna GNU C</span><span class="sxs-lookup"><span data-stu-id="a6c4e-213">GNU C library</span></span> |<span data-ttu-id="a6c4e-214">2.5-12</span><span class="sxs-lookup"><span data-stu-id="a6c4e-214">2.5-12</span></span> |
| <span data-ttu-id="a6c4e-215">OpenSSL</span><span class="sxs-lookup"><span data-stu-id="a6c4e-215">Openssl</span></span> |<span data-ttu-id="a6c4e-216">Knihovny OpenSSL</span><span class="sxs-lookup"><span data-stu-id="a6c4e-216">OpenSSL libraries</span></span> |<span data-ttu-id="a6c4e-217">0.9.8E nebo 1.0</span><span class="sxs-lookup"><span data-stu-id="a6c4e-217">0.9.8e or 1.0</span></span> |
| <span data-ttu-id="a6c4e-218">Curl</span><span class="sxs-lookup"><span data-stu-id="a6c4e-218">Curl</span></span> |<span data-ttu-id="a6c4e-219">cURL webového klienta</span><span class="sxs-lookup"><span data-stu-id="a6c4e-219">cURL web client</span></span> |<span data-ttu-id="a6c4e-220">7.15.5</span><span class="sxs-lookup"><span data-stu-id="a6c4e-220">7.15.5</span></span> |
| <span data-ttu-id="a6c4e-221">Python ctypes</span><span class="sxs-lookup"><span data-stu-id="a6c4e-221">Python-ctypes</span></span> |<span data-ttu-id="a6c4e-222">Funkce knihovny</span><span class="sxs-lookup"><span data-stu-id="a6c4e-222">function libraries</span></span> |<span data-ttu-id="a6c4e-223">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="a6c4e-223">n/a</span></span> |
| <span data-ttu-id="a6c4e-224">PAM</span><span class="sxs-lookup"><span data-stu-id="a6c4e-224">PAM</span></span> |<span data-ttu-id="a6c4e-225">PAM moduly</span><span class="sxs-lookup"><span data-stu-id="a6c4e-225">Pluggable authentication Modules</span></span> |<span data-ttu-id="a6c4e-226">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="a6c4e-226">n/a</span></span> |

> [!NOTE]
> <span data-ttu-id="a6c4e-227">Rsyslog nebo syslog ng jsou požadované ke shromáždění zprávy syslog.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-227">Either rsyslog or syslog-ng are required to collect syslog messages.</span></span> <span data-ttu-id="a6c4e-228">Démon procesu syslog výchozí na verze 5 Red Hat Enterprise Linux a CentOS, Oracle Linux verze (sysklog) není podporována pro shromažďování událostí syslog.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-228">The default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="a6c4e-229">Ke shromažďování dat syslog z této verze těchto distribuce, musí být nainstalovaná a nakonfigurovaná nahradit sysklog démon rsyslog.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-229">To collect syslog data from this version of these distributions, the rsyslog daemon should be installed and configured to replace sysklog.</span></span>
>
>

## <a name="quick-install"></a><span data-ttu-id="a6c4e-230">Rychlé instalace</span><span class="sxs-lookup"><span data-stu-id="a6c4e-230">Quick install</span></span>
<span data-ttu-id="a6c4e-231">Spusťte následující příkazy ke stažení omsagent, ověření kontrolního součtu a pak nainstalovat i integrovaného agenta.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-231">Run the following commands to download the omsagent, validate the checksum, then  install and onboard the agent.</span></span> <span data-ttu-id="a6c4e-232">Příkazy jsou pro 64bitovou verzi.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-232">Commands are for 64-bit.</span></span> <span data-ttu-id="a6c4e-233">ID pracovního prostoru a primární klíč se nacházejí na portálu OMS pod **nastavení** na **připojené zdroje** kartě.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-233">The Workspace ID and Primary Key are found in the OMS portal under **Settings** on the **Connected Sources** tab.</span></span>

![Podrobnosti o pracovním prostoru](./media/log-analytics-linux-agents/oms-direct-agent-primary-key.png)

```
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR OMS WORKSPACE ID> -s <YOUR OMS WORKSPACE PRIMARY KEY>
```

<span data-ttu-id="a6c4e-235">Existuje mnoho různých dalších metod k instalaci agenta a upgradujte ho.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-235">There are a variety of other methods to install the agent and upgrade it.</span></span> <span data-ttu-id="a6c4e-236">Další informace o nich v [kroky pro instalaci agenta OMS pro Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux).</span><span class="sxs-lookup"><span data-stu-id="a6c4e-236">You can read more about them at [Steps to install the OMS Agent for Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux).</span></span>

<span data-ttu-id="a6c4e-237">Můžete také zobrazit [Azure video s návodem](https://www.youtube.com/watch?v=mF1wtHPEzT0).</span><span class="sxs-lookup"><span data-stu-id="a6c4e-237">You can also view the [Azure video walkthrough](https://www.youtube.com/watch?v=mF1wtHPEzT0).</span></span>

## <a name="choose-your-linux-data-collection-method"></a><span data-ttu-id="a6c4e-238">Zvolte metodu kolekce dat Linux</span><span class="sxs-lookup"><span data-stu-id="a6c4e-238">Choose your Linux data collection method</span></span>
<span data-ttu-id="a6c4e-239">Tom, jak provedete typy dat, které chcete shromažďovat závisí na, zda chcete použít na portálu OMS nebo pokud chcete upravit různé konfigurační soubory přímo na klienty Linux.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-239">How you choose the data types you'd like to collect depends on whether you want to use the OMS portal or if you want edit various configuration files directly on your Linux clients.</span></span> <span data-ttu-id="a6c4e-240">Pokud se rozhodnete používat portál, je konfigurace odeslány na všechny klienty Linux automaticky.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-240">If you choose to use the portal, the configuration is sent to all of your Linux clients automatically.</span></span> <span data-ttu-id="a6c4e-241">Pokud potřebujete různé konfigurace pro jiné klienty Linux, musíte upravit soubory klienta jednotlivě – nebo použijte alternativu jako PowerShell DSC, Chef nebo Puppet.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-241">If you need different configurations for different Linux clients, you will need to edit client files individually – or use an alternative like PowerShell DSC, Chef, or Puppet.</span></span>

<span data-ttu-id="a6c4e-242">Můžete zadat události procesu syslog a čítače výkonu, které chcete shromáždit pomocí konfigurační soubory do počítače se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-242">You can specify the syslog events and performance counters that you want to collect using configuration files on the Linux computers.</span></span> <span data-ttu-id="a6c4e-243">*Pokud jste se rozhodli nakonfigurovat shromažďování dat úpravou konfigurační soubory agenta, měli byste zakázat Centralizovaná konfigurace.*</span><span class="sxs-lookup"><span data-stu-id="a6c4e-243">*If you chose to configure data collection by editing agent configuration files, you should disable the centralized configuration.*</span></span>  <span data-ttu-id="a6c4e-244">Pokyny najdete níže ke konfiguraci shromažďování dat v konfiguračních souborech na agenta a také zakázat centrální konfigurace pro všechny agenty OMS pro Linux nebo jednotlivé počítače.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-244">Instructions are provided below to configure data collection in the agent's configuration files as well as to disable central configuration for all OMS Agents for Linux, or individual computers.</span></span>

### <a name="disable-oms-management-for-an-individual-linux-computer"></a><span data-ttu-id="a6c4e-245">Zakázat správu OMS pro jednotlivý počítač Linux</span><span class="sxs-lookup"><span data-stu-id="a6c4e-245">Disable OMS management for an individual Linux computer</span></span>
<span data-ttu-id="a6c4e-246">Centralizované shromažďování dat pro konfigurační data je zakázána pro jednotlivý počítač Linux pomocí skriptu OMS_MetaConfigHelper.py.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-246">Centralized data collection for configuration data is disabled for an individual Linux computer with the OMS_MetaConfigHelper.py script.</span></span> <span data-ttu-id="a6c4e-247">To může být užitečné, pokud podmnožině počítačů by měl mít speciální konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-247">This can be useful if a subset of computers should have a specialized configuration.</span></span>

<span data-ttu-id="a6c4e-248">Postup při zakázání Centralizovaná konfigurace:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-248">To disable centralized configuration:</span></span>

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```

<span data-ttu-id="a6c4e-249">Chcete-li znovu povolit Centralizovaná konfigurace:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-249">To re-enable centralized configuration:</span></span>

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py –enable
```

## <a name="linux-performance-counters"></a><span data-ttu-id="a6c4e-250">Čítače výkonu systému Linux</span><span class="sxs-lookup"><span data-stu-id="a6c4e-250">Linux performance counters</span></span>
<span data-ttu-id="a6c4e-251">Čítače výkonu systému Linux jsou podobná čítačů výkonu systému Windows, jak fungují podobně.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-251">Linux performance counters are similar to Windows performance counters—both operate similarly.</span></span> <span data-ttu-id="a6c4e-252">Přidání a konfigurace je můžete použít následující postupy.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-252">You can use the following procedures to add and configure them.</span></span> <span data-ttu-id="a6c4e-253">Po přidání do OMS, data jsou shromažďována pro ně každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-253">After they are added to OMS, data is collected for them every 30 seconds.</span></span>

### <a name="to-add-a-linux-performance-counter-in-oms"></a><span data-ttu-id="a6c4e-254">Chcete-li přidat čítače výkonu Linux v OMS</span><span class="sxs-lookup"><span data-stu-id="a6c4e-254">To add a Linux performance counter in OMS</span></span>
1. <span data-ttu-id="a6c4e-255">Chcete-li nakonfigurovat OMS agentů pro Linux pomocí portálu OMS, můžete přidat čítače výkonu Linux na stránce nastavení, klikněte na **Data**.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-255">To configure OMS Agents for Linux using the OMS portal, you can add Linux performance counters on the Settings page, click **Data**.</span></span>  
2. <span data-ttu-id="a6c4e-256">Na **nastavení** v části **Data** , klikněte na tlačítko **čítače výkonu Linux** a pak vyberte nebo zadejte název čítače, které chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-256">On the **Settings** page under **Data** , click **Linux performance counters** and then select or type the name of the counter you want to add.</span></span>  
    <span data-ttu-id="a6c4e-257">![data](./media/log-analytics-linux-agents/oms-settings-data01.png)</span><span class="sxs-lookup"><span data-stu-id="a6c4e-257">![data](./media/log-analytics-linux-agents/oms-settings-data01.png)</span></span>
3. <span data-ttu-id="a6c4e-258">Pokud si nejste jisti úplným názvem čítače, pište částečný název a zobrazí se seznam dostupných čítačů.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-258">If you don't know the full name of the counter, you can start typing a partial name and a list of available counters will appear.</span></span> <span data-ttu-id="a6c4e-259">Pokud zjistíte, čítač, který chcete přidat, klikněte na název v seznamu a pak klikněte na ikonu plus přidat čítač.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-259">When you find the counter you want to add, click the name in the list and then click the plus icon to add the counter.</span></span>
4. <span data-ttu-id="a6c4e-260">Po přidání čítač se zobrazí v seznamu čítačů zvýrazněná s barevnou panelu.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-260">After you add the counter, it appears in the list of counters highlighted with a colored bar.</span></span>
5. <span data-ttu-id="a6c4e-261">Ve výchozím nastavení **použít dole uvedenou konfiguraci u mých počítačů** je vybraná možnost.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-261">By default, the **Apply below configuration to my machines** option is selected.</span></span> <span data-ttu-id="a6c4e-262">Pokud chcete zakázat odesílání konfiguračních dat, zrušte zaškrtnutí políčka.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-262">If you want to disable sending configuration data, clear the selection.</span></span>
6. <span data-ttu-id="a6c4e-263">Po dokončení změny čítače výkonu, v dolní části stránky klikněte na tlačítko **Uložit** na provedené změny se dokončí.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-263">When you are done modifying performance counters, at the bottom of the page click **Save** to finalize your changes.</span></span> <span data-ttu-id="a6c4e-264">Změny konfigurace, které jste udělali jsou poté odeslány všechny agenty OMS pro Linux, které jsou registrovány OMS, obvykle během pěti minut.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-264">The configuration changes that you've made are then sent to all the OMS Agents for Linux that are registered with OMS, typically within 5 minutes.</span></span>

### <a name="configure-linux-performance-counters-in-oms"></a><span data-ttu-id="a6c4e-265">Konfigurovat čítačů výkonu systému Linux v OMS</span><span class="sxs-lookup"><span data-stu-id="a6c4e-265">Configure Linux performance counters in OMS</span></span>
<span data-ttu-id="a6c4e-266">Pro čítače výkonu systému Windows můžete konkrétní instance jednotlivých čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-266">For Windows performance counters, you can choose a specific instance for each performance counter.</span></span> <span data-ttu-id="a6c4e-267">Pro čítače výkonu systému Linux, ale jakoukoli instanci čítače, který zvolíte, platí pro všechny podřízené čítače nadřazené čítače.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-267">However, for Linux performance counters, whatever instance of a counter that you choose applies to all child counters of the parent counter.</span></span> <span data-ttu-id="a6c4e-268">V následující tabulce jsou uvedeny běžné instancí, které jsou k dispozici pro Linux a Windows čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-268">The following table shows the common instances available to both Linux and Windows performance counters.</span></span>

| <span data-ttu-id="a6c4e-269">**Název instance**</span><span class="sxs-lookup"><span data-stu-id="a6c4e-269">**Instance name**</span></span> | <span data-ttu-id="a6c4e-270">**Význam**</span><span class="sxs-lookup"><span data-stu-id="a6c4e-270">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="a6c4e-271">\_Celkový počet</span><span class="sxs-lookup"><span data-stu-id="a6c4e-271">\_Total</span></span> |<span data-ttu-id="a6c4e-272">Celkový počet všech instancí</span><span class="sxs-lookup"><span data-stu-id="a6c4e-272">Total of all the instances</span></span> |
| \* |<span data-ttu-id="a6c4e-273">Všechny instance</span><span class="sxs-lookup"><span data-stu-id="a6c4e-273">All instances</span></span> |
| <span data-ttu-id="a6c4e-274">(/ &#124; / var)</span><span class="sxs-lookup"><span data-stu-id="a6c4e-274">(/&#124;/var)</span></span> |<span data-ttu-id="a6c4e-275">Odpovídá instancí s názvem: / nebo /var</span><span class="sxs-lookup"><span data-stu-id="a6c4e-275">Matches instances named: / or /var</span></span> |

<span data-ttu-id="a6c4e-276">Podobně intervalu vzorkování, který zvolíte, čítače nadřazené platí pro všechny jeho podřízené čítače.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-276">Similarly, the sample interval that you choose for a parent counter applies to all its child counters.</span></span> <span data-ttu-id="a6c4e-277">Jinými slovy jsou všechny podřízené intervalů vzorkování čítače a instance svázané společně.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-277">In other words, all the child counter sample intervals and instances are tied together.</span></span>

### <a name="add-and-configure-performance-metrics-with-linux"></a><span data-ttu-id="a6c4e-278">Přidání a konfigurace metriky výkonu operačního systému Linux</span><span class="sxs-lookup"><span data-stu-id="a6c4e-278">Add and configure performance metrics with Linux</span></span>
<span data-ttu-id="a6c4e-279">Metriky výkonu ke shromažďování jsou řízeny konfigurace v/etc/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/conf/omsagent.conf.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-279">Performance metrics to collect are controlled by the configuration in /etc/opt/microsoft/omsagent/&lt;workspace id&gt;/conf/omsagent.conf.</span></span> <span data-ttu-id="a6c4e-280">V tématu [metriky výkonu k dispozici](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) dostupných tříd a metriky pro agenta OMS pro Linux.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-280">See [Available performance metrics](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) for available classes and metrics for the OMS Agent for Linux.</span></span>

<span data-ttu-id="a6c4e-281">Každý objekt nebo kategorie metrik výkonu ke shromažďování by měl být definována v konfiguračním souboru jako jeden `<source>` elementu.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-281">Each object, or category, of performance metrics to collect should be defined in the configuration file as a single `<source>` element.</span></span> <span data-ttu-id="a6c4e-282">Syntaxe následující níže.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-282">The syntax follows the pattern below.</span></span>

```
<source>
  type oms_omi  
  object_name "Processor"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

<span data-ttu-id="a6c4e-283">Konfigurovat parametry tohoto elementu jsou:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-283">The configurable parameters of this element are:</span></span>

* <span data-ttu-id="a6c4e-284">**Objekt\_název**: název objektu pro kolekci.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-284">**Object\_name**: the object name for the collection.</span></span>
* <span data-ttu-id="a6c4e-285">**Instance\_regex**: *regulární výraz* definování instance, které ke shromažďování.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-285">**Instance\_regex**: a *regular expression* defining which instances to collect.</span></span> <span data-ttu-id="a6c4e-286">Hodnota: `.*` Určuje všechny instance.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-286">The value: `.*` specifies all instances.</span></span> <span data-ttu-id="a6c4e-287">Ke shromažďování metrik procesoru pro pouze \_celkový počet instancí, můžete zadat `_Total`.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-287">To collect processor metrics for only the \_Total instance, you could specify `_Total`.</span></span> <span data-ttu-id="a6c4e-288">Ke shromažďování metrik proces pro pouze crond nebo sshd instancí, můžete zadat: `(crond|sshd)`.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-288">To collect process metrics for only the crond or sshd instances, you could specify: `(crond|sshd)`.</span></span>
* <span data-ttu-id="a6c4e-289">**Čítač\_název\_regex**: *regulární výraz* definice, které ke shromažďování čítače (pro objekt).</span><span class="sxs-lookup"><span data-stu-id="a6c4e-289">**Counter\_name\_regex**: a *regular expression* defining which counters (for the object) to collect.</span></span> <span data-ttu-id="a6c4e-290">Chcete-li shromažďovat všechny čítače pro objekt, zadejte: `.*`.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-290">To collect all counters for the object, specify: `.*`.</span></span> <span data-ttu-id="a6c4e-291">Chcete-li shromažďovat pouze čítače místa odkládacího souboru objektu paměti, můžete zadat:`.+Swap.+`</span><span class="sxs-lookup"><span data-stu-id="a6c4e-291">To collect only swap space counters for the memory object, you could specify: `.+Swap.+`</span></span>
* <span data-ttu-id="a6c4e-292">**Interval:**: frekvence, na které se shromažďují objektu čítače.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-292">**Interval:**: the frequency at which the object's counters are collected.</span></span>

<span data-ttu-id="a6c4e-293">Výchozí konfiguraci pro metriku výkonu je:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-293">The default configuration for performance metrics is:</span></span>

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

### <a name="enable-mysql-performance-counters-using-linux-commands"></a><span data-ttu-id="a6c4e-294">Povolit MySQL čítače výkonu pomocí příkazů Linux</span><span class="sxs-lookup"><span data-stu-id="a6c4e-294">Enable MySQL performance counters using Linux commands</span></span>
<span data-ttu-id="a6c4e-295">Pokud Server databáze MySQL nebo MariaDB Server je v počítači nalezen při instalaci sady omsagent, automaticky se nainstaluje zprostředkovatele pro Server databáze MySQL monitorování výkonu.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-295">If MySQL Server or MariaDB Server is detected on the computer when the omsagent bundle is installed, a performance monitoring provider for MySQL Server is automatically installed.</span></span> <span data-ttu-id="a6c4e-296">Tento zprostředkovatel připojí k místnímu serveru MySQL nebo MariaDB vystavit statistiku výkonu.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-296">This provider connects to the local MySQL/MariaDB server to expose performance statistics.</span></span> <span data-ttu-id="a6c4e-297">Budete muset nakonfigurovat přihlašovací údaje uživatele MySQL tak, aby zprostředkovatel můžete přístup k serveru databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-297">You need to configure MySQL user credentials so that the provider can access the MySQL Server.</span></span>

<span data-ttu-id="a6c4e-298">Chcete-li definovat výchozí uživatelského účtu pro server databáze MySQL na místního hostitele, použijte v následujícím příkladu příkaz.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-298">To define a default user account for the MySQL server on localhost, use the following command example.</span></span>

> [!NOTE]
> <span data-ttu-id="a6c4e-299">Soubor s přihlašovacími údaji musí být účet omsagent.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-299">The credentials file must be readable by the omsagent account.</span></span> <span data-ttu-id="a6c4e-300">Doporučuje se spuštěním příkazu mycimprovauth jako omsgent.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-300">Running the mycimprovauth command as omsgent is recommended.</span></span>
>
>

```
sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>

sudo /opt/omi/bin/service_control restart
```


<span data-ttu-id="a6c4e-301">Alternativně můžete zadat požadovaná pověření MySQL v souboru, vytváření souboru: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-301">Alternatively, you can specify the required MySQL credentials in a file, by creating the file: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth.</span></span> <span data-ttu-id="a6c4e-302">Další informace o správě MySQL přihlašovací údaje pro monitorování prostřednictvím soubor mysql ověřování najdete v části [spravovat MySQL monitorování přihlašovací údaje v souboru authentication](#manage-mysql-monitoring-credentials-in-the-authentication-file).</span><span class="sxs-lookup"><span data-stu-id="a6c4e-302">For more information about managing MySQL credentials for monitoring through the mysql-auth file, see [Manage MySQL monitoring credentials in the authentication file](#manage-mysql-monitoring-credentials-in-the-authentication-file).</span></span>

<span data-ttu-id="a6c4e-303">V tématu [databáze oprávnění vyžadovaná pro čítače výkonu databáze MySQL](#database-permissions-required-for-mysql-performance-counters) podrobnosti o oprávnění objektu MySQL uživatel požaduje ke shromažďování dat výkonu serveru MySQL.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-303">See [Database permissions required for MySQL performance counters](#database-permissions-required-for-mysql-performance-counters) for details about object permissions required by the MySQL user to collect MySQL Server performance data.</span></span>

### <a name="enable-apache-http-server-performance-counters-using-linux-commands"></a><span data-ttu-id="a6c4e-304">Povolit čítače výkonu serveru Apache HTTP Server pomocí příkazů Linux</span><span class="sxs-lookup"><span data-stu-id="a6c4e-304">Enable Apache HTTP Server performance counters using Linux commands</span></span>
<span data-ttu-id="a6c4e-305">Pokud serveru Apache HTTP Server je v počítači nalezen, když je nainstalovaná sada omsagent, automaticky se nainstaluje zprostředkovatele pro serveru Apache HTTP Server monitorování výkonu.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-305">If Apache HTTP Server is detected on the computer when the omsagent bundle is installed, a performance monitoring provider for Apache HTTP Server is automatically installed.</span></span> <span data-ttu-id="a6c4e-306">Tento zprostředkovatel spoléhá na platformě Apache "modul", který je nutné načíst do serveru Apache HTTP Server, aby měli přístup na údaje o výkonu.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-306">This provider relies on an Apache "module" that must be loaded into the Apache HTTP Server in order to access performance data.</span></span>

<span data-ttu-id="a6c4e-307">Můžete načíst modul pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-307">You can load the module with the following command:</span></span>

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

<span data-ttu-id="a6c4e-308">Unload modulu Sledování Apache, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-308">To unload the Apache monitoring module, run the following command:</span></span>

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```
### <a name="to-view-performance-data-with-log-analytics"></a><span data-ttu-id="a6c4e-309">Chcete-li zobrazit údaje o výkonu s analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="a6c4e-309">To view performance data with Log Analytics</span></span>
1. <span data-ttu-id="a6c4e-310">Na portálu služby Operations Management Suite klikněte na dlaždici hledání protokolů.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-310">In the Operations Management Suite portal, click the Log Search tile.</span></span>
2. <span data-ttu-id="a6c4e-311">V panelu vyhledávání, zadejte `* (Type=Perf)` k zobrazení všech čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-311">In the search bar, type `* (Type=Perf)` to view all performance counters.</span></span>

<span data-ttu-id="a6c4e-312">Protože OMS taky shromažďuje data čítače výkonu systému Windows, jste měli oboru rozbalovací výsledky vyhledávání na data specifická pro Linux.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-312">Because OMS also collects Windows performance counter data, you should scope-down the search to Linux-specific data.</span></span> <span data-ttu-id="a6c4e-313">Ano v následujícím příkladu by zobrazit údaje o výkonu konkrétní na server Linux příklad s názvem Chorizo21.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-313">So, the following example would show performance data specific to an example Linux server named Chorizo21.</span></span>

```
Type=Perf Computer=chorizo*
```

![Příklad server zobrazen ve výsledcích vyhledávání](./media/log-analytics-linux-agents/oms-perfsearch01.png)

<span data-ttu-id="a6c4e-315">Ve výsledcích, můžete kliknout na **metriky** počítadla, která nebyla shromážděna data pro zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-315">In the results, you can click **Metrics** to view the counters that data was collected for.</span></span> <span data-ttu-id="a6c4e-316">Data v reálném čase je zobrazen jako grafy pro každý čítač.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-316">Real-time data is shown as graphs for each counter.</span></span>

![Průzkumníku metrik](./media/log-analytics-linux-agents/oms-perfmetrics01.png)

## <a name="syslog"></a><span data-ttu-id="a6c4e-318">Syslog</span><span class="sxs-lookup"><span data-stu-id="a6c4e-318">Syslog</span></span>
<span data-ttu-id="a6c4e-319">Syslog je protokol protokolování událostí, podobně jako protokol událostí systému Windows – obě fungují podobně jako při zobrazení v OMS.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-319">Syslog is an event logging protocol similar to Windows Event logs—both operate similarly when displayed in OMS.</span></span>

### <a name="to-add-a-new-linux-syslog-facility-in-oms"></a><span data-ttu-id="a6c4e-320">Chcete-li přidat nového zařízení syslog Linux v OMS</span><span class="sxs-lookup"><span data-stu-id="a6c4e-320">To add a new Linux syslog facility in OMS</span></span>
1. <span data-ttu-id="a6c4e-321">Na **nastavení** v části **Data** , klikněte na **Syslog** a vlevo na ikonu plus, zadejte název mechanismus syslog, který chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-321">On the **Settings** page under **Data** , click **Syslog** and then to the left of the plus icon, type the name of the syslog facility that you want to add.</span></span>
    <span data-ttu-id="a6c4e-322">![Linux syslog](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)</span><span class="sxs-lookup"><span data-stu-id="a6c4e-322">![Linux syslog](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)</span></span>
2. <span data-ttu-id="a6c4e-323">Pokud si nejste jisti úplný název zařízení, pište částečný název a zobrazí se seznam dostupných syslog zařízení.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-323">If you don’t know the full name of the facility, you can start typing a partial name and a list of available syslog facilities will appear.</span></span> <span data-ttu-id="a6c4e-324">Pokud zjistíte, mechanismus syslog, který chcete přidat, klikněte na název v seznamu a pak klikněte na ikonu plus přidat protokolovací mechanismus syslog.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-324">When you find the syslog facility that you want to add, click the name in the list and then click the plus icon to add the syslog facility.</span></span>
3. <span data-ttu-id="a6c4e-325">Po přidání zařízení, zobrazí se v seznamu se zvýrazněnou s barevnou panelu.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-325">After you add the facility, it appears in the list of highlighted with a colored bar.</span></span> <span data-ttu-id="a6c4e-326">V dalším kroku vyberte závažnosti (kategorie syslog zařízení informací), které chcete shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-326">Next, choose the severities (categories of syslog facility information) that you want to collect.</span></span>
4. <span data-ttu-id="a6c4e-327">V dolní části stránky klikněte na tlačítko **Uložit** na provedené změny se dokončí.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-327">At the bottom of the page click **Save** to finalize your changes.</span></span> <span data-ttu-id="a6c4e-328">Změny konfigurace, které jste udělali jsou poté odeslány všechny agenty OMS pro Linux, které jsou registrovány OMS, obvykle během pěti minut.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-328">The configuration changes that you’ve made are then sent to all the OMS Agents for Linux that are registered with OMS, typically within 5 minutes.</span></span>

### <a name="configure-linux-syslog-facilities-in-linux"></a><span data-ttu-id="a6c4e-329">Nakonfigurujte zařízení syslog Linux v systému Linux</span><span class="sxs-lookup"><span data-stu-id="a6c4e-329">Configure Linux syslog facilities in Linux</span></span>
<span data-ttu-id="a6c4e-330">Události procesu Syslog jsou odesílány z démon procesu syslog, například rsyslog nebo syslog ng, místní port, který agent naslouchá na.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-330">Syslog events are sent from the syslog daemon, for example rsyslog or syslog-ng, to a local port that the agent is listening on.</span></span> <span data-ttu-id="a6c4e-331">Ve výchozím nastavení je to port 25224.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-331">By default, port 25224.</span></span> <span data-ttu-id="a6c4e-332">Pokud je agent nainstalován, je použita výchozí konfigurace syslog.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-332">When the agent is installed, a default syslog configuration is applied.</span></span> <span data-ttu-id="a6c4e-333">To se nachází zde:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-333">This is found at:</span></span>

<span data-ttu-id="a6c4e-334">Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf</span><span class="sxs-lookup"><span data-stu-id="a6c4e-334">Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf</span></span>

<span data-ttu-id="a6c4e-335">Syslog ng: /etc/syslog-ng/syslog-ng.conf</span><span class="sxs-lookup"><span data-stu-id="a6c4e-335">Syslog-ng: /etc/syslog-ng/syslog-ng.conf</span></span>

<span data-ttu-id="a6c4e-336">Výchozí konfigurace syslog agenta OMS odesílá události procesu syslog od všech zařízení, se závažností upozornění nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-336">The default OMS agent syslog configuration uploads syslog events from all facilities with a severity of warning or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="a6c4e-337">Pokud chcete upravit konfiguraci syslog, je nutné restartovat démon procesu syslog změny se projeví.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-337">If you edit the syslog configuration, you must restart the syslog daemon for the changes to take effect.</span></span>
>
>

<span data-ttu-id="a6c4e-338">Výchozí syslog konfigurace pro agenta OMS pro Linux pro OMS je:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-338">The default syslog configuration for the OMS Agent for Linux for OMS is:</span></span>

#### <a name="rsyslog"></a><span data-ttu-id="a6c4e-339">rsyslog</span><span class="sxs-lookup"><span data-stu-id="a6c4e-339">Rsyslog</span></span>
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

#### <a name="syslog-ng"></a><span data-ttu-id="a6c4e-340">Syslog ng</span><span class="sxs-lookup"><span data-stu-id="a6c4e-340">Syslog-ng</span></span>
```
#OMS_facility = all
filter f_warning_oms { level(warning); };
destination warning_oms { tcp("127.0.0.1" port(25224)); };
log { source(src); filter(f_warning_oms); destination(warning_oms); };
```

### <a name="to-view-all-syslog-events-with-log-analytics"></a><span data-ttu-id="a6c4e-341">Chcete-li zobrazit všechny události procesu Syslog s analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="a6c4e-341">To view all Syslog events with Log Analytics</span></span>
1. <span data-ttu-id="a6c4e-342">Na portálu služby Operations Management Suite, klikněte **hledání protokolů** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-342">In the Operations Management Suite portal, click the **Log Search** tile.</span></span>
2. <span data-ttu-id="a6c4e-343">V **Správa protokolu** seskupování, vyberte předdefinované syslog vyhledávání a pak vyberte jedno ji spustit.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-343">In the **Log Management** grouping, choose a predefined syslog search and then select one to run it.</span></span>

<span data-ttu-id="a6c4e-344">Tento příklad ukazuje všechny události procesu Syslog.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-344">This example shows all Syslog events.</span></span>

![Ukazuje vyhledávání protokolu události procesu Syslog](./media/log-analytics-linux-agents/oms-linux-syslog.png)

<span data-ttu-id="a6c4e-346">Nyní můžete rozbalit výsledky hledání.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-346">Now you can drill into search results.</span></span>

## <a name="linux-alerts"></a><span data-ttu-id="a6c4e-347">Linux výstrahy</span><span class="sxs-lookup"><span data-stu-id="a6c4e-347">Linux alerts</span></span>
<span data-ttu-id="a6c4e-348">Pokud používáte Nagios nebo Zabbix ke správě vašeho počítače se systémem Linux, může přijímat OMS výstrahy generované z těchto nástrojů.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-348">If you use Nagios or Zabbix to manage your Linux machines, then OMS can receive the alerts generated from those tools.</span></span> <span data-ttu-id="a6c4e-349">Však není aktuálně žádná metoda konfigurace příchozích dat výstrah pomocí portálu OMS.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-349">However, there is currently no method to configure incoming alert data using the OMS portal.</span></span> <span data-ttu-id="a6c4e-350">Místo toho bude muset upravit konfigurační soubor zahájit odesílání výstrahy k OMS.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-350">Instead, you will need to edit a config file to start sending alerts to OMS.</span></span>

### <a name="collect-alerts-from-nagios"></a><span data-ttu-id="a6c4e-351">Shromažďovat výstrahy z Nagios</span><span class="sxs-lookup"><span data-stu-id="a6c4e-351">Collect alerts from Nagios</span></span>
<span data-ttu-id="a6c4e-352">Shromažďovat výstrahy ze serveru Nagios, budete muset provést následující změny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-352">To collect alerts from a Nagios server, you need to make the following configuration changes.</span></span>

1. <span data-ttu-id="a6c4e-353">Udělit **omsagent** přístup pro čtení k souboru protokolu Nagios (tj. /var/log/nagios/nagios.log).</span><span class="sxs-lookup"><span data-stu-id="a6c4e-353">Grant the user **omsagent** read access to the Nagios log file (i.e. /var/log/nagios/nagios.log).</span></span> <span data-ttu-id="a6c4e-354">Za předpokladu, že soubor nagios.log je vlastníkem skupiny **nagios** , můžete přidat uživatele **omsagent** k **nagios** skupiny.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-354">Assuming the nagios.log file is owned by the group **nagios** , you can add the user **omsagent** to the **nagios** group.</span></span>

    ```
    sudo usermod –a -G nagios omsagent
    ```
2. <span data-ttu-id="a6c4e-355">Upravte soubor omsagent.confconfiguration (/ etc/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/conf/omsagent.conf).</span><span class="sxs-lookup"><span data-stu-id="a6c4e-355">Modify the omsagent.confconfiguration file (/etc/opt/microsoft/omsagent/&lt;workspace id&gt;/conf/omsagent.conf).</span></span> <span data-ttu-id="a6c4e-356">Ujistěte se, že následující položky jsou existuje a není komentáři se:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-356">Ensure the following entries are present and not commented out:</span></span>

    ```
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
    ```
3. <span data-ttu-id="a6c4e-357">Restartujte démon omsagent:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-357">Restart the omsagent daemon:</span></span>

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="collect-alerts-from-zabbix"></a><span data-ttu-id="a6c4e-358">Shromažďovat výstrahy z Zabbix</span><span class="sxs-lookup"><span data-stu-id="a6c4e-358">Collect alerts from Zabbix</span></span>
<span data-ttu-id="a6c4e-359">Ke shromažďování výstrah ze serveru Zabbix, budete provádět podobným způsobem jako pro Nagios vyšší, s výjimkou toho, budete muset zadat uživatele a heslo v *text vymažte, pokud*.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-359">To collect alerts from a Zabbix server, you'll perform similar steps to those for Nagios above, except you'll need to specify a user and password in *clear text*.</span></span> <span data-ttu-id="a6c4e-360">Toto není ideální, ale pravděpodobně bude brzy změnit.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-360">This is not ideal, but will likely change soon.</span></span> <span data-ttu-id="a6c4e-361">Chcete-li vyřešit tento problém, doporučujeme vytvořit uživatele a udělit mu oprávnění k monitorování pouze.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-361">To address this issue, we recommend that you create the user and grant it permission to monitor only.</span></span>

<span data-ttu-id="a6c4e-362">Oddílu příklad konfiguračního souboru omsagent.conf (/ etc/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/conf/omsagent.conf) pro Zabbix by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-362">An example section of the omsagent.conf configuration file  (/etc/opt/microsoft/omsagent/&lt;workspace id&gt;/conf/omsagent.conf) for Zabbix should resemble the following:</span></span>

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

### <a name="view-alerts-in-log-analytics-search"></a><span data-ttu-id="a6c4e-363">Zobrazit výstrahy ve vyhledávání analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="a6c4e-363">View alerts in Log Analytics search</span></span>
<span data-ttu-id="a6c4e-364">Po dokončení konfigurace počítače Linux k odesílání upozornění na OMS, můžete zobrazit výstrahy několik jednoduchých protokolu vyhledávací dotazy.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-364">After you've configured your Linux computers to send alerts to OMS, you can use a few simple log search queries to view the alerts.</span></span> <span data-ttu-id="a6c4e-365">Následující příklad dotazu vyhledávání vrací všechny zaznamenané výstrahy, které byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-365">The following search query example returns all the recorded alerts that were generated.</span></span> <span data-ttu-id="a6c4e-366">Například v případě nějaká problém v infrastruktuře IT pak výsledky pro následující ukázkový dotaz může určují, kde může být problém pocházejí.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-366">For example, if some sort of problem occurs in your IT infrastructure, then results for the following example query might indicate where the problem might originate.</span></span> <span data-ttu-id="a6c4e-367">A jste můžete snadno přejít k podrobnostem na výstrahy ve zdrojovém systému pomohou úzké šetření.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-367">And, you can easily drill in to the alerts by source system to help narrow your investigation.</span></span> <span data-ttu-id="a6c4e-368">Výhodou je, že nemusíte nutně přejít na různé systémy správy od začátku – za předpokladu, že vaše výstrahy budou zasílány OMS, můžete spustit existuje.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-368">The benefit is that you don't necessarily have to go to various management systems from the start—provided that your alerts are sent to OMS, you can start there.</span></span>

```
Type=Alert
```

#### <a name="to-view-all-nagios-alerts-with-log-analytics"></a><span data-ttu-id="a6c4e-369">Chcete-li zobrazit všechny výstrahy Nagios s analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="a6c4e-369">To view all Nagios alerts with Log Analytics</span></span>
1. <span data-ttu-id="a6c4e-370">Na portálu služby Operations Management Suite, klikněte **hledání protokolů** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-370">In the Operations Management Suite portal, click the **Log Search** tile.</span></span>
2. <span data-ttu-id="a6c4e-371">Na panelu dotazů zadejte následující vyhledávací dotaz</span><span class="sxs-lookup"><span data-stu-id="a6c4e-371">In the query bar, type the following search query</span></span>

    ```
    Type=Alert SourceSystem=Nagios
    ```
   ![Nagios výstrah zobrazených ve vyhledávání protokolu](./media/log-analytics-linux-agents/oms-linux-nagios-alerts.png)

<span data-ttu-id="a6c4e-373">Až se zobrazí výsledky hledání, můžete přejít do další podrobnosti, jako *AlertState*.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-373">After you see the search results, you can drill into additional details such as *AlertState*.</span></span>

### <a name="to-view-all-zabbix-alerts-with-log-analytics"></a><span data-ttu-id="a6c4e-374">Chcete-li zobrazit všechny výstrahy Zabbix s analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="a6c4e-374">To view all Zabbix alerts with Log Analytics</span></span>
1. <span data-ttu-id="a6c4e-375">Na portálu služby Operations Management Suite, klikněte **hledání protokolů** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-375">In the Operations Management Suite portal, click the **Log Search** tile.</span></span>
2. <span data-ttu-id="a6c4e-376">Na panelu dotazů zadejte následující vyhledávací dotaz</span><span class="sxs-lookup"><span data-stu-id="a6c4e-376">In the query bar, type the following search query</span></span>

    ```
    Type=Alert SourceSystem=Zabbix
    ```
   ![Zabbix výstrah zobrazených ve vyhledávání protokolu](./media/log-analytics-linux-agents/oms-linux-zabbix-alerts.png)

<span data-ttu-id="a6c4e-378">Až se zobrazí výsledky hledání, můžete přejít do další podrobnosti, jako *AlertName*.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-378">After you see the search results, you can drill into additional details such as *AlertName*.</span></span>

## <a name="compatibility-with-system-center-operations-manager"></a><span data-ttu-id="a6c4e-379">Kompatibilita s nástrojem System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="a6c4e-379">Compatibility with System Center Operations Manager</span></span>
<span data-ttu-id="a6c4e-380">OMS agenta pro Linux sdílí binárních souborů agenta s agenta System Center Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-380">The OMS Agent for Linux shares agent binaries with the System Center Operations Manager agent.</span></span> <span data-ttu-id="a6c4e-381">Instalace agenta OMS pro Linux v systému aktuálně spravován nástrojem Operations Manager upgraduje OMI a SCX balíčky v počítači na novější verzi.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-381">Installing the OMS Agent for Linux on a system currently managed by Operations Manager upgrades the OMI and SCX packages on the computer to a newer version.</span></span> <span data-ttu-id="a6c4e-382">Agent OMS pro systémy Linux a System Center 2012 R2 jsou kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-382">The OMS Agent for Linux and System Center 2012 R2 are compatible.</span></span> <span data-ttu-id="a6c4e-383">Ale **System Center 2012 SP1 a starší verze nejsou aktuálně kompatibilní nebo není podporován s agentem OMS pro Linux.**</span><span class="sxs-lookup"><span data-stu-id="a6c4e-383">However, **System Center 2012 SP1 and earlier versions are currently not compatible or supported with the OMS Agent for Linux.**</span></span>

> [!NOTE]
> <span data-ttu-id="a6c4e-384">Pokud je Agent OMS pro Linux nainstalován do počítače, který není aktuálně spravována nástrojem Operations Manager, a budete později chtít spravovat počítač s nástrojem Operations Manager, je třeba změnit konfiguraci OMI před zjištění počítače.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-384">If the OMS Agent for Linux is installed to a computer that is not currently managed by Operations Manager, and you later want to manage the computer with Operations Manager, you must modify the OMI configuration before you discover the computer.</span></span> <span data-ttu-id="a6c4e-385">**Tento krok není nutný, pokud je nainstalován agent nástroje Operations Manager před OMS agenta pro Linux.**</span><span class="sxs-lookup"><span data-stu-id="a6c4e-385">**This step is not needed if the Operations Manager agent is installed before the OMS Agent for Linux.**</span></span>
>
>

### <a name="to-enable-the-oms-agent-for-linux-to-communicate-with-operations-manager"></a><span data-ttu-id="a6c4e-386">Chcete-li povolit agenta OMS pro Linux ke komunikaci s nástrojem Operations Manager</span><span class="sxs-lookup"><span data-stu-id="a6c4e-386">To enable the OMS Agent for Linux to communicate with Operations Manager</span></span>
1. <span data-ttu-id="a6c4e-387">Upravit soubor /etc/opt/omi/conf/omiserver.conf</span><span class="sxs-lookup"><span data-stu-id="a6c4e-387">Edit the file /etc/opt/omi/conf/omiserver.conf</span></span>
2. <span data-ttu-id="a6c4e-388">Ujistěte se, že na začátek řádku s **httpsport =** definuje port 1270.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-388">Ensure that the line beginning with **httpsport=** defines the port 1270.</span></span> <span data-ttu-id="a6c4e-389">Například`httpsport=1270`</span><span class="sxs-lookup"><span data-stu-id="a6c4e-389">Such as `httpsport=1270`</span></span>
3. <span data-ttu-id="a6c4e-390">Restartujte OMI server:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-390">Restart the OMI server:</span></span>

    ```
    sudo /opt/omi/bin/service_control restart
    ```

## <a name="database-permissions-required-for-mysql-performance-counters"></a><span data-ttu-id="a6c4e-391">Databáze oprávnění vyžadovaná pro čítače výkonu databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="a6c4e-391">Database permissions required for MySQL performance counters</span></span>
<span data-ttu-id="a6c4e-392">Pokud chcete udělit oprávnění pro uživatele monitorování MySQL, poskytující uživatel musí mít oprávnění k 'GRANT option, a také udělení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-392">To grant permissions to a MySQL monitoring user, the granting user must have the 'GRANT option' privilege as well as the privilege being granted.</span></span>

<span data-ttu-id="a6c4e-393">Aby mohl uživatel MySQL vrátit data výkonu uživatele bude potřebovat přístup k následující dotazy:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-393">In order for the MySQL User to return performance data the user will need access to the following queries:</span></span>

```
SHOW GLOBAL STATUS;
SHOW GLOBAL VARIABLES:
```

<span data-ttu-id="a6c4e-394">Kromě těchto dotazů MySQL uživatel vyžaduje vyberte přístup k následující výchozí tabulky:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-394">In addition to these queries the MySQL user requires SELECT access to the following default tables:</span></span>

* <span data-ttu-id="a6c4e-395">INFORMATION_SCHEMA</span><span class="sxs-lookup"><span data-stu-id="a6c4e-395">information_schema</span></span>
* <span data-ttu-id="a6c4e-396">MySQL</span><span class="sxs-lookup"><span data-stu-id="a6c4e-396">mysql</span></span>

<span data-ttu-id="a6c4e-397">Tato oprávnění lze udělit spuštěním následujících příkazů grant.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-397">These privileges can be granted by running the following grant commands.</span></span>

```
GRANT SELECT ON information_schema.* TO ‘monuser’@’localhost’;
GRANT SELECT ON mysql.* TO ‘monuser’@’localhost’;
```

## <a name="manage-mysql-monitoring-credentials-in-the-authentication-file"></a><span data-ttu-id="a6c4e-398">Spravovat MySQL monitorování přihlašovací údaje v souboru ověřování</span><span class="sxs-lookup"><span data-stu-id="a6c4e-398">Manage MySQL monitoring credentials in the authentication file</span></span>
<span data-ttu-id="a6c4e-399">V následujících částech vám pomohou spravovat MySQL přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-399">The following sections help you manage MySQL credentials.</span></span>

### <a name="configure-the-mysql-omi-provider"></a><span data-ttu-id="a6c4e-400">Konfigurace zprostředkovatele MySQL OMI</span><span class="sxs-lookup"><span data-stu-id="a6c4e-400">Configure the MySQL OMI provider</span></span>
<span data-ttu-id="a6c4e-401">Zprostředkovatel MySQL OMI vyžaduje předkonfigurované uživatele MySQL a nainstalovat MySQL klientské knihovny chcete-li se dotazovat informací stavu nebo výkonu z instance databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-401">The MySQL OMI provider requires a preconfigured MySQL user and installed MySQL client libraries in order to query the performance/health information from the MySQL instance.</span></span>

### <a name="mysql-omi-authentication-file"></a><span data-ttu-id="a6c4e-402">Soubor pro ověření MySQL OMI</span><span class="sxs-lookup"><span data-stu-id="a6c4e-402">MySQL OMI authentication file</span></span>
<span data-ttu-id="a6c4e-403">Určit, jaké vazby adresy a portu MySQL instance naslouchá na a jaké přihlašovací údaje se má použít ke shromažďování metrik používá zprostředkovatel MySQL OMI soubor ověřování.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-403">MySQL OMI provider uses an authentication file to determine what bind-address and port the MySQL instance is listening on and what credentials to use to gather metrics.</span></span> <span data-ttu-id="a6c4e-404">Během instalace MySQL OMI zprostředkovatele vyhledá MySQL my.cnf konfigurační soubory (výchozí umístění) pro vazbu adresy a portu a částečně nastavit MySQL OMI soubor pro ověření.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-404">During installation the MySQL OMI provider will scan MySQL my.cnf configuration files (default locations) for bind-address and port and partially set the MySQL OMI authentication file.</span></span>

<span data-ttu-id="a6c4e-405">K dokončení monitorování instance serveru MySQL, přidá soubor předem vygenerovaná ověřování MySQL OMI ve správném adresáři.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-405">To complete monitoring of a MySQL server instance, add a pre-generated MySQL OMI authentication file into the correct directory.</span></span>

### <a name="authentication-file-format"></a><span data-ttu-id="a6c4e-406">Formát souboru ověřování</span><span class="sxs-lookup"><span data-stu-id="a6c4e-406">Authentication file format</span></span>
<span data-ttu-id="a6c4e-407">MySQL OMI ověřování soubor je textový soubor, který obsahuje informace o:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-407">The MySQL OMI authentication file is a text file that contains information about:</span></span>

* <span data-ttu-id="a6c4e-408">Port</span><span class="sxs-lookup"><span data-stu-id="a6c4e-408">Port</span></span>
* <span data-ttu-id="a6c4e-409">Vazby – adresy</span><span class="sxs-lookup"><span data-stu-id="a6c4e-409">Bind-Address</span></span>
* <span data-ttu-id="a6c4e-410">Uživatelské jméno MySQL</span><span class="sxs-lookup"><span data-stu-id="a6c4e-410">MySQL username</span></span>
* <span data-ttu-id="a6c4e-411">Heslo s kódováním base64</span><span class="sxs-lookup"><span data-stu-id="a6c4e-411">Base64 encoded password</span></span>

<span data-ttu-id="a6c4e-412">Soubor databáze MySQL OMI ověřování jenom uděluje oprávnění pro čtení a zápis Linux uživateli, který ji vygenerovala.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-412">The MySQL OMI authentication file only grants privileges for read/write to the Linux user that generated it.</span></span>

```
[Port]=[Bind-Address], [username], [Base64 encoded Password]
(Port)=(Bind-Address), (username), (Base64 encoded Password)
(Port)=(Bind-Address), (username), (Base64 encoded Password)
AutoUpdate=[true|false]
```

<span data-ttu-id="a6c4e-413">Výchozí soubor pro ověření MySQL OMI obsahuje výchozí instance a číslo portu v závislosti na tom, jaké informace je k dispozici a Analyzovaná z konfiguračního souboru nalezen MySQL.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-413">A default MySQL OMI authentication file contains a default instance and a port number depending on what information is available and parsed from the found MySQL configuration file.</span></span>

<span data-ttu-id="a6c4e-414">Výchozí instance je prostředkem, který chcete-li více instancí databáze MySQL na jednom hostiteli systému Linux jednodušší správu a je označený jako instance s portu 0.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-414">The default instance is a means to make managing multiple MySQL instances on one Linux host easier, and is denoted by the instance with port 0.</span></span> <span data-ttu-id="a6c4e-415">Všechny přidané instance zdědí vlastnosti z instance výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-415">All added instances will inherit properties set from the default instance.</span></span> <span data-ttu-id="a6c4e-416">Například pokud je přidána instance databáze MySQL naslouchání na portu '3308', vazby adresu výchozí instance, uživatelské jméno a heslo kódováním Base64 použije sledovat instance naslouchá na 3308 a zkuste to.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-416">For example, if MySQL instance listening on port '3308' is added, the default instance's bind-address, username, and Base64 encoded password will be used to try and monitor the instance listening on 3308.</span></span> <span data-ttu-id="a6c4e-417">Pokud instanci na 3308 je vázaný na jinou adresu a používá stejný MySQL dvojici uživatelské jméno a heslo je potřeba jenom respecification adresy vazby a zdědí vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-417">If the instance on 3308 is binded to another address and uses the same MySQL username and password pair only the respecification of the bind-address is needed and the other properties will be inherited.</span></span>

<span data-ttu-id="a6c4e-418">Příklady souboru ověřování vypadat takto.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-418">Examples of the authentication file resemble the following.</span></span>

<span data-ttu-id="a6c4e-419">Výchozí instance a instanci s portem 3308:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-419">Default instance and instance with port 3308:</span></span>

```
0=127.0.0.1, myuser, cnBwdA==3308=, ,AutoUpdate=true
```

<span data-ttu-id="a6c4e-420">Výchozí instance a instance s port 3308 + různých Base 64 kódovaný heslo:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-420">Default instance and instance with port 3308 + different Base 64 encoded password:</span></span>

```
0=127.0.0.1, myuser, cnBwdA==3308=127.0.1.1, , AutoUpdate=true
```


| <span data-ttu-id="a6c4e-421">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="a6c4e-421">**Property**</span></span> | <span data-ttu-id="a6c4e-422">**Popis**</span><span class="sxs-lookup"><span data-stu-id="a6c4e-422">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="a6c4e-423">Port</span><span class="sxs-lookup"><span data-stu-id="a6c4e-423">Port</span></span> |<span data-ttu-id="a6c4e-424">Port představuje aktuální port, který naslouchá instance databáze MySQL na.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-424">Port represents the current port the MySQL instance is listening on.</span></span>  <span data-ttu-id="a6c4e-425">Port 0 znamená, že následující vlastnosti se používají pro výchozí instanci.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-425">The port 0 implies that the properties following are used for default instance.</span></span> |
| <span data-ttu-id="a6c4e-426">Vazby – adresy</span><span class="sxs-lookup"><span data-stu-id="a6c4e-426">Bind-Address</span></span> |<span data-ttu-id="a6c4e-427">Adresa vazby je aktuální vazby MySQL-adresa</span><span class="sxs-lookup"><span data-stu-id="a6c4e-427">the Bind Address is the current MySQL bind-address</span></span> |
| <span data-ttu-id="a6c4e-428">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="a6c4e-428">username</span></span> |<span data-ttu-id="a6c4e-429">Toto uživatelské jméno uživatele databáze MySQL, které chcete použít k monitorování instance serveru MySQL.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-429">This the username of the MySQL user you wish to use to monitor the MySQL server instance.</span></span> |
| <span data-ttu-id="a6c4e-430">Kódováním base64, pomocí hesla</span><span class="sxs-lookup"><span data-stu-id="a6c4e-430">Base64 encoded Password</span></span> |<span data-ttu-id="a6c4e-431">Jedná se o heslo uživatele monitorování MySQL kódovaný jako Base64.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-431">This is the password of the MySQL monitoring user encoded in Base64.</span></span> |
| <span data-ttu-id="a6c4e-432">Pomocí funkce Automatické aktualizace</span><span class="sxs-lookup"><span data-stu-id="a6c4e-432">AutoUpdate</span></span> |<span data-ttu-id="a6c4e-433">Když upgraduje se zprostředkovatel OMI MySQL bude zprostředkovatele Prohledat znovu na změny v souboru my.cnf a přepsat soubor MySQL OMI ověřování.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-433">When the MySQL OMI Provider is upgraded the provider will rescan for changes in the my.cnf file and overwrite the MySQL OMI Authentication file.</span></span> <span data-ttu-id="a6c4e-434">Tento příznak nastavte na hodnotu PRAVDA nebo NEPRAVDA v závislosti na požadované aktualizace do souboru MySQL OMI ověřování.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-434">Set this flag to true or false depending on required updates to the MySQL OMI authentication file.</span></span> |

#### <a name="authentication-file-location"></a><span data-ttu-id="a6c4e-435">Umístění souboru ověřování</span><span class="sxs-lookup"><span data-stu-id="a6c4e-435">Authentication file location</span></span>
<span data-ttu-id="a6c4e-436">Soubor databáze MySQL OMI ověřování by měl nachází v následujícím umístění a s názvem "mysql ověření":</span><span class="sxs-lookup"><span data-stu-id="a6c4e-436">The MySQL OMI Authentication File should be located in the following location and named "mysql-auth":</span></span>

<span data-ttu-id="a6c4e-437">/var/OPT/Microsoft/MySQL-cimprov/auth/omsagent/MySQL-auth</span><span class="sxs-lookup"><span data-stu-id="a6c4e-437">/var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth</span></span>

<span data-ttu-id="a6c4e-438">Soubor (a ověřování nebo omsagent directory) by měl být vlastněných uživatelem omsagent.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-438">The file (and auth/omsagent directory) should be owned by the omsagent user.</span></span>

## <a name="agent-logs"></a><span data-ttu-id="a6c4e-439">Protokoly agenta</span><span class="sxs-lookup"><span data-stu-id="a6c4e-439">Agent logs</span></span>
<span data-ttu-id="a6c4e-440">Protokoly pro OMS agenta pro Linux je na:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-440">The logs for the OMS Agent for Linux is at:</span></span>

<span data-ttu-id="a6c4e-441">/ var/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/log/</span><span class="sxs-lookup"><span data-stu-id="a6c4e-441">/var/opt/microsoft/omsagent/&lt;workspace id&gt;/log/</span></span>

<span data-ttu-id="a6c4e-442">Protokoly pro OMS agenta pro Linux programu omsconfig (Konfigurace agenta) je na:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-442">The logs for the OMS Agent for Linux for omsconfig (agent configuration) program is at:</span></span>

<span data-ttu-id="a6c4e-443">/ var/opt/microsoft/omsconfig nebo protokolu nebo</span><span class="sxs-lookup"><span data-stu-id="a6c4e-443">/var/opt/microsoft/omsconfig/log/</span></span>

<span data-ttu-id="a6c4e-444">Protokoly pro součásti infrastruktury OMI a SCX (které poskytují data metriky výkonu) je na:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-444">Logs for the OMI and SCX components (which provide performance metrics data) is at:</span></span>

<span data-ttu-id="a6c4e-445">/ var/opt/omi/log/a /var/opt/microsoft/scx/log</span><span class="sxs-lookup"><span data-stu-id="a6c4e-445">/var/opt/omi/log/ and /var/opt/microsoft/scx/log</span></span>

## <a name="troubleshooting-the-oms-agent-for-linux"></a><span data-ttu-id="a6c4e-446">Řešení potíží s agentem OMS pro Linux</span><span class="sxs-lookup"><span data-stu-id="a6c4e-446">Troubleshooting the OMS Agent for Linux</span></span>
<span data-ttu-id="a6c4e-447">Použijte následující informace pro diagnostiku a řešení běžných problémů.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-447">Use the following information to diagnose and troubleshoot common issues.</span></span>

<span data-ttu-id="a6c4e-448">Pokud žádné informace o odstraňování potíží v této části vám pomůže, můžete taky v následujících zdrojích informací vyřešit váš problém.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-448">If none of the troubleshooting information in this section helps you, you can also use the following resources to help resolve your problem.</span></span>

* <span data-ttu-id="a6c4e-449">Zákazníci s Premier support může přihlásit případu podpory prostřednictvím [úrovně Premier](https://premier.microsoft.com/)</span><span class="sxs-lookup"><span data-stu-id="a6c4e-449">Customers with Premier support can log a support case via [Premier](https://premier.microsoft.com/)</span></span>
* <span data-ttu-id="a6c4e-450">Zákazníci s smlouvy podporu systému Azure se můžou přihlásit případů podpory [portálu Azure](https://manage.windowsazure.com/?getsupport=true)</span><span class="sxs-lookup"><span data-stu-id="a6c4e-450">Customers with Azure support agreements can log support cases in the [Azure portal](https://manage.windowsazure.com/?getsupport=true)</span></span>
* <span data-ttu-id="a6c4e-451">Soubor [potíže Githubu](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)</span><span class="sxs-lookup"><span data-stu-id="a6c4e-451">File a [GitHub Issue](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)</span></span>
* <span data-ttu-id="a6c4e-452">Fóru pro zpětnou vazbu pro návrhy a pro vytvoření sestavy chyb [http://aka.ms/opinsightsfeedback](http://aka.ms/opinsightsfeedback)</span><span class="sxs-lookup"><span data-stu-id="a6c4e-452">Feedback forum for ideas and to create a bug report [http://aka.ms/opinsightsfeedback](http://aka.ms/opinsightsfeedback)</span></span>

### <a name="important-log-locations"></a><span data-ttu-id="a6c4e-453">Umístění důležité protokolů</span><span class="sxs-lookup"><span data-stu-id="a6c4e-453">Important log locations</span></span>
| <span data-ttu-id="a6c4e-454">File</span><span class="sxs-lookup"><span data-stu-id="a6c4e-454">File</span></span> | <span data-ttu-id="a6c4e-455">Cesta</span><span class="sxs-lookup"><span data-stu-id="a6c4e-455">Path</span></span> |
| --- | --- |
| <span data-ttu-id="a6c4e-456">Agent OMS pro soubor protokolu systému Linux</span><span class="sxs-lookup"><span data-stu-id="a6c4e-456">OMS Agent for Linux Log File</span></span> |`/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log ` |
| <span data-ttu-id="a6c4e-457">V souboru protokolu konfigurace agenta OMS</span><span class="sxs-lookup"><span data-stu-id="a6c4e-457">OMS Agent Configuration Log File</span></span> |`/var/opt/microsoft/omsconfig/omsconfig.log` |

### <a name="important-configuration-files"></a><span data-ttu-id="a6c4e-458">Důležité konfigurační soubory</span><span class="sxs-lookup"><span data-stu-id="a6c4e-458">Important configuration files</span></span>
| <span data-ttu-id="a6c4e-459">Catergory</span><span class="sxs-lookup"><span data-stu-id="a6c4e-459">Catergory</span></span> | <span data-ttu-id="a6c4e-460">Umístění souboru</span><span class="sxs-lookup"><span data-stu-id="a6c4e-460">File Location</span></span> |
| --- | --- |
| <span data-ttu-id="a6c4e-461">Syslog</span><span class="sxs-lookup"><span data-stu-id="a6c4e-461">Syslog</span></span> |<span data-ttu-id="a6c4e-462">`/etc/syslog-ng/syslog-ng.conf`nebo `/etc/rsyslog.conf` nebo`/etc/rsyslog.d/95-omsagent.conf`</span><span class="sxs-lookup"><span data-stu-id="a6c4e-462">`/etc/syslog-ng/syslog-ng.conf` or `/etc/rsyslog.conf` or `/etc/rsyslog.d/95-omsagent.conf`</span></span> |
| <span data-ttu-id="a6c4e-463">Výkon, Nagios, Zabbix, OMS výstup a obecné agenta</span><span class="sxs-lookup"><span data-stu-id="a6c4e-463">Performance, Nagios, Zabbix, OMS output and general agent</span></span> |`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` |
| <span data-ttu-id="a6c4e-464">Další konfigurace</span><span class="sxs-lookup"><span data-stu-id="a6c4e-464">Additional configurations</span></span> |`/etc/opt/microsoft/omsagent/<workspace id>/omsagent.d/*.conf` |

> [!NOTE]
> <span data-ttu-id="a6c4e-465">Úpravy konfigurační soubory pro čítače výkonu a syslog jsou přepsány, pokud je povoleno nastavení portálu OMS.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-465">Editing configuration files for performance counters and syslog are overwritten if OMS Portal Configuration is enabled.</span></span> <span data-ttu-id="a6c4e-466">Můžete zakázat konfigurací na portálu OMS (pro všechny uzly) nebo pro jeden uzly spuštěním následující:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-466">You can disable configuration in the OMS Portal (for all nodes) or for single nodes by running the following:</span></span>
>
>

```
sudo su omsagent -c /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```


### <a name="enable-debug-logging"></a><span data-ttu-id="a6c4e-467">Povolit protokolování ladění</span><span class="sxs-lookup"><span data-stu-id="a6c4e-467">Enable debug logging</span></span>
<span data-ttu-id="a6c4e-468">Pokud chcete povolit protokolování ladění, můžete použít modul plug-in výstup OMS a podrobný výstup.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-468">To enable debug logging, you can use the OMS output plugin and verbose output.</span></span>

#### <a name="oms-output-plugin"></a><span data-ttu-id="a6c4e-469">Modul plug-in výstup OMS</span><span class="sxs-lookup"><span data-stu-id="a6c4e-469">OMS output plugin</span></span>
<span data-ttu-id="a6c4e-470">FluentD umožňuje modulu plug-in k určení úrovně protokolování pro úrovně jiný protokol pro vstupy a výstupy.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-470">FluentD allows the plugin to specify logging levels for different log levels for inputs and outputs.</span></span> <span data-ttu-id="a6c4e-471">Zadejte úroveň jiný protokol pro výstup OMS, upravte konfiguraci obecné agenta v `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` souboru.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-471">To specify a different log level for OMS output, edit the general agent configuration in the `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` file.</span></span>

<span data-ttu-id="a6c4e-472">V dolní části konfiguračního souboru, změnit `log_level` vlastnost z `info` k `debug`.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-472">Near the bottom of the configuration file, change the `log_level` property from `info` to `debug`.</span></span>

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

<span data-ttu-id="a6c4e-473">Protokolování ladění vám umožní zobrazit dávkové nahrávání ke službě OMS oddělených typ, počet datových položek a čas potřebný k odeslání.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-473">Debug logging allows you to see batched uploads to the OMS Service separated by type, number of data items, and time taken to send.</span></span>

<span data-ttu-id="a6c4e-474">*Příklad povoleno ladění protokolu:*</span><span class="sxs-lookup"><span data-stu-id="a6c4e-474">*Example debug enabled log:*</span></span>

```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

#### <a name="verbose-output"></a><span data-ttu-id="a6c4e-475">Podrobný výstup</span><span class="sxs-lookup"><span data-stu-id="a6c4e-475">Verbose output</span></span>
<span data-ttu-id="a6c4e-476">Místo použití modulu plug-in výstup OMS, také výstup můžete položky dat přímo na `stdout`, který se zobrazí v OMS agenta pro Linux souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-476">Instead of using the OMS output plugin, you can also output data items directly to `stdout`, which is visible in the OMS Agent for Linux log file.</span></span>

<span data-ttu-id="a6c4e-477">V konfiguračním souboru obecné agenta OMS na `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, okomentujte OMS výstup přidáním modulů plug-in `#` před každý řádek.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-477">In the OMS general agent configuration file at `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, comment-out the OMS output plugin by adding a `#` in front of each line.</span></span>

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

<span data-ttu-id="a6c4e-478">Níže tento modul plug-in výstup odebrat komentář v následující části odebráním `#` symbol na začátku každého řádku.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-478">Below the output plugin, remove the comment in the following section by removing the `#` symbol at the beginning of each line.</span></span>

```
<match **>
  type stdout
</match>
```

### <a name="forwarded-syslog-messages-do-not-appear-in-the-log"></a><span data-ttu-id="a6c4e-479">Přesměrovaná zprávy Syslog se nezobrazí v protokolu</span><span class="sxs-lookup"><span data-stu-id="a6c4e-479">Forwarded Syslog messages do not appear in the log</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="a6c4e-480">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="a6c4e-480">Probable causes</span></span>
* <span data-ttu-id="a6c4e-481">Použitá na Linux server konfigurace neumožňuje kolekce odeslaných zařízení a/nebo protokolu úrovně</span><span class="sxs-lookup"><span data-stu-id="a6c4e-481">The configuration applied to the Linux server does not allow collection of the sent facilities and/or log levels</span></span>
* <span data-ttu-id="a6c4e-482">Syslog není předávaná správně na Linux server</span><span class="sxs-lookup"><span data-stu-id="a6c4e-482">Syslog is not being forwarded correctly to the Linux server</span></span>
* <span data-ttu-id="a6c4e-483">Počet zpráv předávaná za sekundu, je příliš velké pro základní konfiguraci agenta OMS pro Linux pro zpracování</span><span class="sxs-lookup"><span data-stu-id="a6c4e-483">The number of messages being forwarded per second are too large for the base configuration of the OMS Agent for Linux to handle</span></span>

#### <a name="resolutions"></a><span data-ttu-id="a6c4e-484">Řešení</span><span class="sxs-lookup"><span data-stu-id="a6c4e-484">Resolutions</span></span>
* <span data-ttu-id="a6c4e-485">Ověřte, že konfigurace na portálu OMS Syslog všech zařízení a úrovně správné protokolu</span><span class="sxs-lookup"><span data-stu-id="a6c4e-485">Verify that the configuration in the OMS Portal for Syslog has all the facilities and the correct log levels</span></span>
  * <span data-ttu-id="a6c4e-486">**Portál OMS > Nastavení > Data > Syslog**</span><span class="sxs-lookup"><span data-stu-id="a6c4e-486">**OMS Portal > Settings > Data > Syslog**</span></span>
* <span data-ttu-id="a6c4e-487">Ověřte, že nativní syslog zasílání zpráv démoni (`rsyslog`, `syslog-ng`) mohou přijímat přesměrované zprávy</span><span class="sxs-lookup"><span data-stu-id="a6c4e-487">Verify that native syslog messaging daemons (`rsyslog`, `syslog-ng`) are able to receive the forwarded messages</span></span>
* <span data-ttu-id="a6c4e-488">Zkontrolujte nastavení brány firewall na serveru Syslog zajistit, že zprávy nejsou blokovány.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-488">Check firewall settings on the Syslog server to ensure that messages are not being blocked</span></span>
* <span data-ttu-id="a6c4e-489">Simulovat zprávu Syslog pomocí OMS `logger` příkaz – například:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-489">Simulate a Syslog message to OMS using the `logger` command - for example:</span></span>
  * `logger -p local0.err "This is my test message"`

### <a name="problems-connecting-to-oms-when-using-a-proxy"></a><span data-ttu-id="a6c4e-490">Potíže s připojením k OMS při použití serveru proxy</span><span class="sxs-lookup"><span data-stu-id="a6c4e-490">Problems connecting to OMS when using a proxy</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="a6c4e-491">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="a6c4e-491">Probable causes</span></span>
* <span data-ttu-id="a6c4e-492">Proxy server zadaný při instalaci a konfiguraci agenta je nesprávný</span><span class="sxs-lookup"><span data-stu-id="a6c4e-492">The proxy specified when installing and configuring the agent is incorrect</span></span>
* <span data-ttu-id="a6c4e-493">Koncové body služby OMS nejsou whitelistested ve vašem datovém centru</span><span class="sxs-lookup"><span data-stu-id="a6c4e-493">The OMS Service endpoints are not whitelistested in your datacenter</span></span>

#### <a name="resolutions"></a><span data-ttu-id="a6c4e-494">Řešení</span><span class="sxs-lookup"><span data-stu-id="a6c4e-494">Resolutions</span></span>
* <span data-ttu-id="a6c4e-495">Znovu nainstalujte agenta OMS pro Linux pomocí následujícího příkazu s parametrem `-v` povolena.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-495">Reinstall the OMS Agent for Linux using the following command with the option `-v` enabled.</span></span> <span data-ttu-id="a6c4e-496">To umožňuje podrobný výstup agenta připojení prostřednictvím proxy serveru pro službu OMS.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-496">This allows verbose output of the agent connecting through the proxy to the OMS Service.</span></span>
  * `/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`
  * <span data-ttu-id="a6c4e-497">Přečtěte si dokumentaci k proxy serveru OMS na [konfigurace agenta pro použití s HTTP proxy server](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#configuring-the-agent-for-use-with-an-http-proxy-server)</span><span class="sxs-lookup"><span data-stu-id="a6c4e-497">Review the documentation for OMS proxy at [Configuring the agent for use with an HTTP proxy server](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#configuring-the-agent-for-use-with-an-http-proxy-server)</span></span>
* <span data-ttu-id="a6c4e-498">Ověřte, zda jsou následující koncové body služby OMS seznam povolených adres</span><span class="sxs-lookup"><span data-stu-id="a6c4e-498">Verify that the following OMS Service endpoints are whitelisted</span></span>

| <span data-ttu-id="a6c4e-499">Prostředek agenta</span><span class="sxs-lookup"><span data-stu-id="a6c4e-499">Agent Resource</span></span> | <span data-ttu-id="a6c4e-500">Porty</span><span class="sxs-lookup"><span data-stu-id="a6c4e-500">Ports</span></span> |
| --- | --- |
| <span data-ttu-id="a6c4e-501">&#42;. Ods.opinsights.Azure.com</span><span class="sxs-lookup"><span data-stu-id="a6c4e-501">&#42;.ods.opinsights.azure.com</span></span> |<span data-ttu-id="a6c4e-502">Port 443</span><span class="sxs-lookup"><span data-stu-id="a6c4e-502">Port 443</span></span> |
| <span data-ttu-id="a6c4e-503">&#42;. OMS.opinsights.Azure.com</span><span class="sxs-lookup"><span data-stu-id="a6c4e-503">&#42;.oms.opinsights.azure.com</span></span> |<span data-ttu-id="a6c4e-504">Port 443</span><span class="sxs-lookup"><span data-stu-id="a6c4e-504">Port 443</span></span> |
| <span data-ttu-id="a6c4e-505">ods.systemcenteradvisor.com</span><span class="sxs-lookup"><span data-stu-id="a6c4e-505">ods.systemcenteradvisor.com</span></span> |<span data-ttu-id="a6c4e-506">Port 443</span><span class="sxs-lookup"><span data-stu-id="a6c4e-506">Port 443</span></span> |
| <span data-ttu-id="a6c4e-507">&#42;.blob.core.windows.net/</span><span class="sxs-lookup"><span data-stu-id="a6c4e-507">&#42;.blob.core.windows.net/</span></span> |<span data-ttu-id="a6c4e-508">Port 443</span><span class="sxs-lookup"><span data-stu-id="a6c4e-508">Port 443</span></span> |

### <a name="a-403-error-is-displayed-when-onboarding"></a><span data-ttu-id="a6c4e-509">Zobrazí se Chyba 403 při připojování</span><span class="sxs-lookup"><span data-stu-id="a6c4e-509">A 403 error is displayed when onboarding</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="a6c4e-510">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="a6c4e-510">Probable causes</span></span>
* <span data-ttu-id="a6c4e-511">Datum a čas, jsou nesprávná na Linux Server</span><span class="sxs-lookup"><span data-stu-id="a6c4e-511">The date and time are incorrect on Linux Server</span></span>
* <span data-ttu-id="a6c4e-512">ID pracovního prostoru a používá klíč pracovního prostoru jsou nesprávné</span><span class="sxs-lookup"><span data-stu-id="a6c4e-512">The Workspace ID and Workspace Key used are incorrect</span></span>

#### <a name="resolution"></a><span data-ttu-id="a6c4e-513">Řešení</span><span class="sxs-lookup"><span data-stu-id="a6c4e-513">Resolution</span></span>
* <span data-ttu-id="a6c4e-514">Ověřte časový server Linux pomocí `date` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-514">Verify the time on your Linux server with the `date` command.</span></span> <span data-ttu-id="a6c4e-515">Pokud data jsou větší nebo menší než 15 minut od aktuálního času, registrace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-515">If the data is greater than or less than 15 minutes from the current time, then onboarding fails.</span></span> <span data-ttu-id="a6c4e-516">Chcete-li vyřešit tento problém, aktualizujte datum a časové pásmo serveru Linux.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-516">To correct this, update the date and/or timezone of your Linux server.</span></span>
* <span data-ttu-id="a6c4e-517">Nejnovější verzi agenta OMS pro Linux vás upozorní, pokud je časový rozdíl je příčinou selhání registrace</span><span class="sxs-lookup"><span data-stu-id="a6c4e-517">The latest version of the OMS Agent for Linux notifies you if a time difference is causing onboarding failure</span></span>
* <span data-ttu-id="a6c4e-518">Opětovná zařadit pomocí správné ID pracovního prostoru a klíč pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-518">Re-onboard using the correct Workspace ID and Workspace Key.</span></span> <span data-ttu-id="a6c4e-519">V tématu [registrace pomocí příkazového řádku](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) Další informace.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-519">See  [Onboarding using the command line](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) for more information.</span></span>

### <a name="a-500-error-or-404-error-appears-in-the-log-file-after-onboarding"></a><span data-ttu-id="a6c4e-520">500 Chyba nebo chybu 404 se zobrazí v souboru protokolu za registrace</span><span class="sxs-lookup"><span data-stu-id="a6c4e-520">A 500 error or 404 error appears in the log file after onboarding</span></span>
<span data-ttu-id="a6c4e-521">Jde o známý problém, který spadá první nahrávání dat Linux do pracovního prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-521">This is a known issue that occurs during the first upload of Linux data into an OMS workspace.</span></span> <span data-ttu-id="a6c4e-522">To nemá vliv odesílat data nebo jiné problémy.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-522">This does not affect data being sent or other problems.</span></span> <span data-ttu-id="a6c4e-523">Můžete ignorovat chyby, když se od začátku registrace.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-523">You can ignore the errors when initially onboarding.</span></span>

### <a name="nagios-data-does-not-appear-in-the-oms-portal"></a><span data-ttu-id="a6c4e-524">Nagios data nezobrazí na portálu OMS</span><span class="sxs-lookup"><span data-stu-id="a6c4e-524">Nagios data does not appear in the OMS Portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="a6c4e-525">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="a6c4e-525">Probable causes</span></span>
* <span data-ttu-id="a6c4e-526">Omsagent uživatel nemá oprávnění ke čtení ze souboru protokolu Nagios</span><span class="sxs-lookup"><span data-stu-id="a6c4e-526">The omsagent user does not have permissions to read from the Nagios log file</span></span>
* <span data-ttu-id="a6c4e-527">Nagios zdroje a filtru oddílů jsou stále vloženy do komentáře v souboru omsagent.conf</span><span class="sxs-lookup"><span data-stu-id="a6c4e-527">The Nagios source and filter sections are still commented in the omsagent.conf file</span></span>

#### <a name="resolutions"></a><span data-ttu-id="a6c4e-528">Řešení</span><span class="sxs-lookup"><span data-stu-id="a6c4e-528">Resolutions</span></span>
* <span data-ttu-id="a6c4e-529">Aby bylo možné číst ze souboru Nagios přidejte omsagent uživatele.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-529">Add the omsagent user in order to read from the Nagios file.</span></span> <span data-ttu-id="a6c4e-530">V tématu [Nagios výstrahy](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts) Další informace.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-530">See [Nagios alerts](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts) for more information.</span></span>
* <span data-ttu-id="a6c4e-531">V OMS agenta pro Linux obecné konfiguračního souboru v `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, ujistěte se, že **i** Nagios zdroje a filtru oddílů mít komentáře odebrána, podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-531">In the OMS Agent for Linux general configuration file at `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, ensure that **both** the Nagios source and filter sections have comments removed, similar to the following example.</span></span>

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


### <a name="linux-data-doesnt-appear-in-the-oms-portal"></a><span data-ttu-id="a6c4e-532">Linux data se nezobrazí na portálu OMS</span><span class="sxs-lookup"><span data-stu-id="a6c4e-532">Linux data doesn't appear in the OMS Portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="a6c4e-533">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="a6c4e-533">Probable causes</span></span>
* <span data-ttu-id="a6c4e-534">Připojování ke službě OMS se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="a6c4e-534">Onboarding to the OMS Service failed</span></span>
* <span data-ttu-id="a6c4e-535">Připojení ke službě OMS blokováno.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-535">Connection to the OMS Service is blocked</span></span>
* <span data-ttu-id="a6c4e-536">Agent OMS pro Linux data je zálohovaná</span><span class="sxs-lookup"><span data-stu-id="a6c4e-536">The OMS Agent for Linux data is backed-up</span></span>

#### <a name="resolutions"></a><span data-ttu-id="a6c4e-537">Řešení</span><span class="sxs-lookup"><span data-stu-id="a6c4e-537">Resolutions</span></span>
* <span data-ttu-id="a6c4e-538">Ověřte, že registrace ke službě OMS byla úspěšná, jestli `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` existuje.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-538">Verify that onboarding to the OMS Service was successful by verifying that the `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` exists.</span></span>
* <span data-ttu-id="a6c4e-539">Opětovná zařadit omsadmin.sh příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-539">Re-onboard using the omsadmin.sh command line.</span></span> <span data-ttu-id="a6c4e-540">V tématu [registrace pomocí příkazového řádku](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) Další informace.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-540">See [Onboarding using the command line](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) for more information.</span></span>
* <span data-ttu-id="a6c4e-541">Pokud používáte proxy server, používejte proxy server výše uvedené kroky řešení potíží</span><span class="sxs-lookup"><span data-stu-id="a6c4e-541">If using a proxy, use the proxy troubleshooting steps above</span></span>
* <span data-ttu-id="a6c4e-542">V některých případech při OMS agenta pro Linux nemůže komunikovat se službou OMS data na agentovi je zálohovaná na velikost vyrovnávací paměti úplné 50 MB.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-542">In some cases, when the OMS Agent for Linux cannot communicate with the OMS Service, data on the Agent is backed-up to the full buffer size of 50 MB.</span></span> <span data-ttu-id="a6c4e-543">Restartujte agenta OMS pro Linux spuštěním `/opt/microsoft/omsagent/bin/service_control restart` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-543">Restart the OMS Agent for Linux by running the `/opt/microsoft/omsagent/bin/service_control restart` command.</span></span>
  >[AZURE.NOTE] <span data-ttu-id="a6c4e-544">Tento problém vyřešen v 1.1.0-28 verze agenta nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-544">This issue is fixed in Agent version 1.1.0-28 and later.</span></span>

### <a name="syslog-linux-performance-counter-configuration-is-not-applied-in-the-oms-portal"></a><span data-ttu-id="a6c4e-545">Konfigurace čítače výkonu Syslog Linux není použita na portálu OMS</span><span class="sxs-lookup"><span data-stu-id="a6c4e-545">Syslog Linux performance counter configuration is not applied in the OMS portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="a6c4e-546">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="a6c4e-546">Probable causes</span></span>
* <span data-ttu-id="a6c4e-547">Konfigurace agenta v OMS agenta pro Linux nebyl načíst nejnovější konfigurace z portálu OMS.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-547">The configuration agent in the OMS Agent for Linux has not retrieved the latest configuration from the OMS portal.</span></span>
* <span data-ttu-id="a6c4e-548">Upravené nastavení na portálu nebyly použity.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-548">The revised settings in the portal were not applied</span></span>

#### <a name="resolutions"></a><span data-ttu-id="a6c4e-549">Řešení</span><span class="sxs-lookup"><span data-stu-id="a6c4e-549">Resolutions</span></span>
<span data-ttu-id="a6c4e-550">`omsconfig`je agent konfigurace v OMS agenta pro Linux, který načte změny konfigurace portálu OMS každých 5 minut.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-550">`omsconfig` is the configuration agent in the OMS Agent for Linux that retrieves OMS portal configuration changes every 5 minutes.</span></span> <span data-ttu-id="a6c4e-551">Tato konfigurace se potom použije OMS agenta pro Linux konfigurační soubory umístěné na `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-551">This configuration is then applied to the OMS Agent for Linux configuration files located at `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.</span></span>

* <span data-ttu-id="a6c4e-552">V některých případech nemusí být schopni komunikovat se službou konfigurace portálu, což vede k nejnovější konfigurace nebudou použity OMS agenta pro Linux konfigurace agenta.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-552">In some cases, the OMS Agent for Linux configuration agent might not be able to communicate with the portal configuration service resulting in latest configuration not being applied.</span></span>
* <span data-ttu-id="a6c4e-553">Ověřte, zda `omsconfig` je agent nainstalovaný následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-553">Verify that the `omsconfig` agent is installed with the following:</span></span>

  * <span data-ttu-id="a6c4e-554">`dpkg --list omsconfig` nebo `rpm -qi omsconfig`</span><span class="sxs-lookup"><span data-stu-id="a6c4e-554">`dpkg --list omsconfig` or `rpm -qi omsconfig`</span></span>
  * <span data-ttu-id="a6c4e-555">Pokud nainstalovaná není, znovu nainstalujte nejnovější verzi agenta OMS pro Linux</span><span class="sxs-lookup"><span data-stu-id="a6c4e-555">If not installed, reinstall the latest version of the OMS Agent for Linux</span></span>
* <span data-ttu-id="a6c4e-556">Ověřte, zda `omsconfig` agent může komunikovat se službou OMS</span><span class="sxs-lookup"><span data-stu-id="a6c4e-556">Verify that the `omsconfig` agent can communicate with the OMS service</span></span>

  * <span data-ttu-id="a6c4e-557">Spustit `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` příkaz</span><span class="sxs-lookup"><span data-stu-id="a6c4e-557">Run the `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` command</span></span>
    * <span data-ttu-id="a6c4e-558">Výše uvedeného příkazu vrátí konfiguraci tohoto agenta načte z portálu, včetně nastavení Syslog, čítače výkonu systému Linux a vlastní protokoly</span><span class="sxs-lookup"><span data-stu-id="a6c4e-558">The command above returns the configuration that agent retrieves from the portal, including Syslog settings, Linux performance counters, and custom logs</span></span>
    * <span data-ttu-id="a6c4e-559">Pokud se výše uvedeného příkazu nezdaří, spusťte `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-559">If the command above fails, run the `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` command.</span></span> <span data-ttu-id="a6c4e-560">Tento příkaz zajistí omsconfig agenta pro komunikaci se službou OMS načíst nejnovější konfigurací.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-560">This command forces the omsconfig agent to communicate with the OMS service to retrieve the latest configuration.</span></span>

### <a name="custom-linux-log-data-does-not-appear-in-the-oms-portal"></a><span data-ttu-id="a6c4e-561">Vlastní data protokolu Linux nezobrazí na portálu OMS</span><span class="sxs-lookup"><span data-stu-id="a6c4e-561">Custom Linux log data does not appear in the OMS Portal</span></span>
#### <a name="probable-causes"></a><span data-ttu-id="a6c4e-562">Možných příčin</span><span class="sxs-lookup"><span data-stu-id="a6c4e-562">Probable causes</span></span>
* <span data-ttu-id="a6c4e-563">Registrace k službě OMS se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="a6c4e-563">Onboarding to OMS Service failed</span></span>
* <span data-ttu-id="a6c4e-564">**Platí následující konfigurace pro servery se systémem Linux** nebylo vybráno nastavení</span><span class="sxs-lookup"><span data-stu-id="a6c4e-564">The **Apply the following configuration to my Linux Servers** setting has not been selected</span></span>
* <span data-ttu-id="a6c4e-565">omsconfig nebyl zachyceny nejnovější vlastní protokol z portálu.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-565">omsconfig has not picked up the latest custom log from the portal</span></span>
* <span data-ttu-id="a6c4e-566">`omsagent` Použití nemá přístup k vlastní protokol z důvodu problému oprávnění nebo `omsagent` nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-566">The `omsagent` use is unable to access the custom log due to a permissions problem or `omsagent` was not found.</span></span> <span data-ttu-id="a6c4e-567">V takovém případě se zobrazí následující výstup:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-567">In this case, you'll see the following output:</span></span>
  * `[DATETIME] [warn]: file not found. Continuing without tailing it.`
  * `[DATETIME] [error]: file not accessible by omsagent.`
* <span data-ttu-id="a6c4e-568">Jedná se o známý problém s časování, která byla opravena v OMS agenta pro Linux verze 1.1.0-217</span><span class="sxs-lookup"><span data-stu-id="a6c4e-568">This is a known issue with the Race Condition that was fixed in the OMS Agent for Linux version 1.1.0-217</span></span>

#### <a name="resolutions"></a><span data-ttu-id="a6c4e-569">Řešení</span><span class="sxs-lookup"><span data-stu-id="a6c4e-569">Resolutions</span></span>
* <span data-ttu-id="a6c4e-570">Ověřte, že jste úspěšně zařazený, nemá tak, že určíte jestli `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` soubor existuje.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-570">Verify that you've successfully onboarded, by determining whether the `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` file exists.</span></span>
  * <span data-ttu-id="a6c4e-571">Pokud potřebné, zařadit znovu pomocí příkazového řádku omsadmin.sh.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-571">If needed, onboard again using the omsadmin.sh command line.</span></span> <span data-ttu-id="a6c4e-572">V tématu [registrace pomocí příkazového řádku](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) Další informace.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-572">See [Onboarding using the command line](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) for more information.</span></span>
* <span data-ttu-id="a6c4e-573">Na portálu OMS pod **nastavení** na **Data** ověřte, že **platí následující konfigurace pro servery se systémem Linux** je vybráno nastavení</span><span class="sxs-lookup"><span data-stu-id="a6c4e-573">In the OMS Portal, under **Settings** on the **Data** tab, ensure that the **Apply the following configuration to my Linux Servers** setting is selected</span></span>  
  <span data-ttu-id="a6c4e-574">![použít konfiguraci](./media/log-analytics-linux-agents/customloglinuxenabled.png)</span><span class="sxs-lookup"><span data-stu-id="a6c4e-574">![apply configuration](./media/log-analytics-linux-agents/customloglinuxenabled.png)</span></span>
* <span data-ttu-id="a6c4e-575">Ověřte, zda `omsconfig` agent může komunikovat se službou OMS</span><span class="sxs-lookup"><span data-stu-id="a6c4e-575">Verify that the `omsconfig` agent can communicate with the OMS service</span></span>

  * <span data-ttu-id="a6c4e-576">Spustit `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` příkaz</span><span class="sxs-lookup"><span data-stu-id="a6c4e-576">Run the `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` command</span></span>
  * <span data-ttu-id="a6c4e-577">Výše uvedeného příkazu vrátí konfiguraci tohoto agenta načte z portálu, včetně nastavení Syslog, čítače výkonu systému Linux a vlastní protokoly</span><span class="sxs-lookup"><span data-stu-id="a6c4e-577">The command above returns the configuration that agent retrieves from the Portal, including Syslog settings, Linux performance counters, and custom Logs</span></span>
  * <span data-ttu-id="a6c4e-578">Pokud se výše uvedeného příkazu nezdaří, spusťte `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-578">If the command above fails, run the `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` command.</span></span> <span data-ttu-id="a6c4e-579">Tento příkaz zajistí omsconfig agenta pro komunikaci se službou OMS a načíst nejnovější konfigurací.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-579">This command forces the omsconfig agent to communicate with OMS service and retrieve the latest configuration.</span></span>

<span data-ttu-id="a6c4e-580">Místo OMS agenta pro Linux uživatel, který spouští jako privilegovaných uživatelů `root`, Agent OMS pro Linux běží jako `omsagent` uživatele.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-580">Instead of the OMS Agent for Linux user running as a privileged user `root`, the OMS Agent for Linux runs as the `omsagent` user.</span></span> <span data-ttu-id="a6c4e-581">Ve většině případů musí mít udělen výslovná oprávnění uživateli, aby bylo možné číst určité soubory.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-581">In most cases, explicit permission must be granted to the user in order to read certain files.</span></span>

<span data-ttu-id="a6c4e-582">Udělení oprávnění ke `omsagent` uživatele, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-582">To grant permission to `omsagent` user, run the following commands:</span></span>

1. <span data-ttu-id="a6c4e-583">Přidat `omsagent` uživatele na konkrétní skupinu s`sudo usermod -a -G <GROUPNAME> <USERNAME>`</span><span class="sxs-lookup"><span data-stu-id="a6c4e-583">Add the `omsagent` user to a specific group with `sudo usermod -a -G <GROUPNAME> <USERNAME>`</span></span>
2. <span data-ttu-id="a6c4e-584">Udělte universal oprávnění ke čtení pro požadovaný soubor s`sudo chmod -R ugo+rw <FILE DIRECTORY>`</span><span class="sxs-lookup"><span data-stu-id="a6c4e-584">Grant universal read access to the required file with `sudo chmod -R ugo+rw <FILE DIRECTORY>`</span></span>

<span data-ttu-id="a6c4e-585">Je známý problém s časování, která byla opravena v OMS agenta pro Linux verze 1.1.0-217.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-585">There is a known issue with the Race Condition that was fixed in the OMS Agent for Linux version 1.1.0-217.</span></span> <span data-ttu-id="a6c4e-586">Po aktualizaci na nejnovější verzi agenta, spusťte následující příkaz, abyste získali nejnovější verzi modulu plug-in výstup:</span><span class="sxs-lookup"><span data-stu-id="a6c4e-586">After updating to the latest agent, run the following command to get the latest version of the output plugin:</span></span>

```
sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf
```

## <a name="known-limitations"></a><span data-ttu-id="a6c4e-587">Známá omezení</span><span class="sxs-lookup"><span data-stu-id="a6c4e-587">Known limitations</span></span>
<span data-ttu-id="a6c4e-588">Zkontrolujte v následujících částech Další informace o aktuální omezení OMS agenta pro Linux.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-588">Review the following sections to learn about current limitations of the OMS Agent for Linux.</span></span>

### <a name="azure-diagnostics"></a><span data-ttu-id="a6c4e-589">Diagnostika Azure</span><span class="sxs-lookup"><span data-stu-id="a6c4e-589">Azure Diagnostics</span></span>
<span data-ttu-id="a6c4e-590">Pro virtuální počítače Linux běží v Azure mohou být vyžadovány další kroky umožňuje shromažďování dat pomocí diagnostiky Azure a Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-590">For Linux virtual machines running in Azure, additional steps may be required to allow data collection by Azure Diagnostics and Operations Management Suite.</span></span> <span data-ttu-id="a6c4e-591">**Verze 2.2** rozšíření diagnostiky pro Linux je vyžadována pro kompatibilitu s agentem OMS pro Linux.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-591">**Version 2.2** of the Diagnostics Extension for Linux is required for compatibility with the OMS Agent for Linux.</span></span>

<span data-ttu-id="a6c4e-592">Další informace o instalaci a konfiguraci rozšíření diagnostiky pro Linux najdete v tématu [pomocí příkazu příkazového řádku Azure CLI můžete povolit rozšíření diagnostiky Linux](../virtual-machines/linux/classic/diagnostic-extension-v2.md#use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension).</span><span class="sxs-lookup"><span data-stu-id="a6c4e-592">For more information on installing and configuring the Diagnostic Extension for Linux, see [Use the Azure CLI command to enable Linux Diagnostic Extension](../virtual-machines/linux/classic/diagnostic-extension-v2.md#use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension).</span></span>

<span data-ttu-id="a6c4e-593">**Upgrade rozšíření diagnostiky Linux 2.0 na 2.2 rozhraní příkazového řádku Azure ASM:**</span><span class="sxs-lookup"><span data-stu-id="a6c4e-593">**Upgrading the Linux Diagnostics Extension from 2.0 to 2.2 Azure CLI ASM:**</span></span>

```
azure vm extension set -u <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.0
azure vm extension set <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

<span data-ttu-id="a6c4e-594">**ARM**</span><span class="sxs-lookup"><span data-stu-id="a6c4e-594">**ARM**</span></span>

```
azure vm extension set -u <resource-group> <vm-name> Microsoft.Insights.VMDiagnosticsSettings Microsoft.OSTCExtensions 2.0
azure vm extension set <resource-group> <vm-name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

<span data-ttu-id="a6c4e-595">Tyto příklady příkaz odkazovat na soubor s názvem PrivateConfig.json.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-595">These command examples reference a file named PrivateConfig.json.</span></span> <span data-ttu-id="a6c4e-596">Formát souboru by měl vypadat podobně jako následující příklad.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-596">The format of that file should resemble the following sample.</span></span>

```
    {
    "storageAccountName":"the storage account to receive data",
    "storageAccountKey":"the key of the account"
    }
```

### <a name="sysklog-is-not-supported"></a><span data-ttu-id="a6c4e-597">Sysklog není podporován.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-597">Sysklog is not supported</span></span>
<span data-ttu-id="a6c4e-598">Rsyslog nebo syslog ng jsou požadované ke shromáždění zprávy syslog.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-598">Either rsyslog or syslog-ng are required to collect syslog messages.</span></span> <span data-ttu-id="a6c4e-599">Démon procesu syslog výchozí na verze 5 Red Hat Enterprise Linux a CentOS, Oracle Linux verze (sysklog) není podporována pro shromažďování událostí syslog.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-599">The default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="a6c4e-600">Ke shromažďování dat syslog z této verze těchto distribuce, musí být nainstalovaná a nakonfigurovaná nahradit sysklog démon rsyslog.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-600">To collect syslog data from this version of these distributions, the rsyslog daemon should be installed and configured to replace sysklog.</span></span> <span data-ttu-id="a6c4e-601">Další informace o nahrazení sysklog rsyslog najdete v tématu [nainstalovat nově vytvořený rsyslog RPM](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM).</span><span class="sxs-lookup"><span data-stu-id="a6c4e-601">For more information on replacing sysklog with rsyslog, see [Install the newly built rsyslog RPM](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a6c4e-602">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a6c4e-602">Next Steps</span></span>
* <span data-ttu-id="a6c4e-603">Článek [Přidání řešení Log Analytics z galerie řešení](log-analytics-add-solutions.md) popisuje přidání funkcí a shromažďování dat.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-603">[Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md) to add functionality and gather data.</span></span>
* <span data-ttu-id="a6c4e-604">Chcete-li zobrazit podrobné informace shromážděné řešeními, seznamte se s [vyhledáváním protokolů](log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="a6c4e-604">Get familiar with [log searches](log-analytics-log-searches.md) to view detailed information gathered by solutions.</span></span>
* <span data-ttu-id="a6c4e-605">Pomocí [řídicích panelů](log-analytics-dashboards.md) můžete ukládat a zobrazovat vlastní vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="a6c4e-605">Use [dashboards](log-analytics-dashboards.md) to save and display your own custom searches.</span></span>
