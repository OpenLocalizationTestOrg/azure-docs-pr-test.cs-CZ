---
title: "Připojení počítače se systémem Windows k analýze protokolů Azure | Microsoft Docs"
description: "Tento článek popisuje kroky pro počítače se systémem Windows ve vaší místní infrastruktuře připojení ke službě Analýza protokolů pomocí přizpůsobená verze z monitorování agenta MMA (Microsoft)."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 932f7b8c-485c-40c1-98e3-7d4c560876d2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 48a0eaeb10d406d551c9e5870edde06809bd7544
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-windows-computers-to-the-log-analytics-service-in-azure"></a><span data-ttu-id="48be8-103">Připojení počítače se systémem Windows do služby analýzy protokolů Azure</span><span class="sxs-lookup"><span data-stu-id="48be8-103">Connect Windows computers to the Log Analytics service in Azure</span></span>

<span data-ttu-id="48be8-104">Tento článek popisuje kroky pro připojení počítače se systémem Windows ve vaší místní infrastruktuře k pracovním prostorům, OMS pomocí přizpůsobená verze z monitorování agenta MMA (Microsoft).</span><span class="sxs-lookup"><span data-stu-id="48be8-104">This article shows the steps to connect Windows computers in your on-premises infrastructure to OMS workspaces by using a customized version of the Microsoft Monitoring Agent (MMA).</span></span> <span data-ttu-id="48be8-105">Musíte nainstalovat a připojit agentů pro všechny počítače, které chcete, aby pro ně k odesílání dat do služby analýzy protokolů a prohlížení a provádění akcí na těchto datech zařadit do provozu.</span><span class="sxs-lookup"><span data-stu-id="48be8-105">You need to install and connect agents for all of the computers that you want to onboard in order for them to send data to the Log Analytics service and to view and act on that data.</span></span> <span data-ttu-id="48be8-106">Každý agent můžete zprávu několik pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="48be8-106">Each agent can report to multiple workspaces.</span></span>

<span data-ttu-id="48be8-107">Můžete nainstalovat agenty pomocí instalačního programu, příkazového řádku nebo pomocí požadovaného stavu konfigurace (DSC) ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="48be8-107">You can install agents using Setup, command line, or with Desired State Configuration (DSC) in Azure Automation.</span></span>  

>[!NOTE]
<span data-ttu-id="48be8-108">Pro virtuální počítače běžící v Azure, můžete zjednodušit instalace pomocí [rozšíření virtuálního počítače](log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="48be8-108">For virtual machines running in Azure, you can simplify installation by using the [virtual machine extension](log-analytics-azure-vm-extension.md).</span></span>

<span data-ttu-id="48be8-109">Agent na počítačích s připojením k Internetu pomocí připojení k Internetu k odesílání dat do OMS.</span><span class="sxs-lookup"><span data-stu-id="48be8-109">On computers with Internet connectivity, the agent uses the connection to the Internet to send data to OMS.</span></span> <span data-ttu-id="48be8-110">Pro počítače, které nemají připojení k Internetu, můžete použít proxy server nebo [OMS brány](log-analytics-oms-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="48be8-110">For computers that do not have Internet connectivity, you can use a proxy or the [OMS Gateway](log-analytics-oms-gateway.md).</span></span>

<span data-ttu-id="48be8-111">Připojení počítače se systémem Windows k OMS je jednoduchý pomocí tří jednoduché kroky:</span><span class="sxs-lookup"><span data-stu-id="48be8-111">Connecting your Windows computers to OMS is straightforward using three simple steps:</span></span>

1. <span data-ttu-id="48be8-112">Stáhněte si instalační soubor agenta z portálu OMS</span><span class="sxs-lookup"><span data-stu-id="48be8-112">Download the agent setup file from the OMS portal</span></span>
2. <span data-ttu-id="48be8-113">Instalace agenta pomocí metodu, kterou si zvolíte</span><span class="sxs-lookup"><span data-stu-id="48be8-113">Install the agent using the method you choose</span></span>
3. <span data-ttu-id="48be8-114">Konfiguraci agenta nebo přidejte další pracovní prostory, v případě potřeby</span><span class="sxs-lookup"><span data-stu-id="48be8-114">Configure the agent or add additional workspaces, if necessary</span></span>

<span data-ttu-id="48be8-115">Následující diagram znázorňuje vztah mezi OMS a počítačů s Windows po instalaci a nakonfigurovali agenty.</span><span class="sxs-lookup"><span data-stu-id="48be8-115">The following diagram shows the relationship between your Windows computers and OMS after you’ve installed and configured agents.</span></span>

![OMS-direct agenta – diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)

<span data-ttu-id="48be8-117">Pokud vaše zásady zabezpečení IT neumožňují počítače v síti pro připojení k Internetu, můžete konfigurovat počítače pro připojení k bráně OMS.</span><span class="sxs-lookup"><span data-stu-id="48be8-117">If your IT security policies do not allow computers on your network to connect to the Internet, you can configure your computers to connect to the OMS Gateway.</span></span> <span data-ttu-id="48be8-118">Další informace a postup pro konfiguraci serverů pro komunikaci přes bránu OMS ke službě OMS najdete v tématu [počítače připojit k OMS pomocí brány OMS](log-analytics-oms-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="48be8-118">For more information and steps on how to configure your servers to communicate through an OMS Gateway to the OMS service, see [Connect computers to OMS using the OMS Gateway](log-analytics-oms-gateway.md).</span></span>

## <a name="system-requirements-and-required-configuration"></a><span data-ttu-id="48be8-119">Požadavky na systém a požadované konfiguraci</span><span class="sxs-lookup"><span data-stu-id="48be8-119">System requirements and required configuration</span></span>
<span data-ttu-id="48be8-120">Před instalací nebo nasadit agenty, ujistěte se, že splňujete požadavky následující podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="48be8-120">Before you install or deploy agents, review the following details to ensure you meet the requirements.</span></span>

- <span data-ttu-id="48be8-121">OMS MMA můžete nainstalovat pouze na počítačích se systémem Windows Server 2008 SP 1 nebo novější nebo Windows 7 SP1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="48be8-121">You can only install the OMS MMA on computers running Windows Server 2008 SP 1 or later or Windows 7 SP1 or later.</span></span>
- <span data-ttu-id="48be8-122">Budete potřebovat předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="48be8-122">You need an Azure subscription.</span></span>  <span data-ttu-id="48be8-123">Další informace najdete v tématu [začít pracovat s analýzy protokolů](log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="48be8-123">For more information, see [Get started with Log Analytics](log-analytics-get-started.md).</span></span>
- <span data-ttu-id="48be8-124">Každý počítač se systémem Windows musí být schopni připojit k Internetu pomocí protokolu HTTPS nebo k bráně OMS.</span><span class="sxs-lookup"><span data-stu-id="48be8-124">Each Windows computer must be able to connect to the Internet using HTTPS or to the OMS Gateway.</span></span> <span data-ttu-id="48be8-125">Toto připojení může být direct, prostřednictvím proxy serveru nebo přes bránu OMS.</span><span class="sxs-lookup"><span data-stu-id="48be8-125">This connection can be direct, via a proxy, or through the OMS Gateway.</span></span>
- <span data-ttu-id="48be8-126">OMS MMA můžete nainstalovat na samostatných počítačů, serverů a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="48be8-126">You can install the OMS MMA on stand-alone computers, servers, and virtual machines.</span></span> <span data-ttu-id="48be8-127">Pokud chcete se připojit k OMS virtuálních počítačů hostovaných v Azure, najdete v části [virtuálních počítačích Azure připojit k analýze protokolů](log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="48be8-127">If you want to connect Azure-hosted virtual machines to OMS, see [Connect Azure virtual machines to Log Analytics](log-analytics-azure-vm-extension.md).</span></span>
- <span data-ttu-id="48be8-128">Agenta je potřeba použít port 443 protokolu TCP pro různé prostředky.</span><span class="sxs-lookup"><span data-stu-id="48be8-128">The agent needs to use TCP port 443 for various resources.</span></span>

### <a name="network"></a><span data-ttu-id="48be8-129">Síť</span><span class="sxs-lookup"><span data-stu-id="48be8-129">Network</span></span>

<span data-ttu-id="48be8-130">Pro agenty se systémem Windows pro připojení k a zaregistrovat službu OMS musí mít přístup k síťovým prostředkům, včetně čísla portů a adres URL domény.</span><span class="sxs-lookup"><span data-stu-id="48be8-130">For Windows agents to connect to and register with the OMS service, they must have access to network resources, including the port numbers and domain URLs.</span></span>

- <span data-ttu-id="48be8-131">U proxy serverů musíte zajistit konfiguraci příslušných prostředků proxy serveru v nastavení agenta.</span><span class="sxs-lookup"><span data-stu-id="48be8-131">For proxy servers, you need to ensure that the appropriate proxy server resources are configured in agent settings.</span></span>
- <span data-ttu-id="48be8-132">Pro brány firewall, které omezují přístup k Internetu vám nebo vaší sítě technici nutné nakonfigurovat bránu firewall tak, aby povolovala přístup k OMS.</span><span class="sxs-lookup"><span data-stu-id="48be8-132">For firewalls that restrict access to the Internet, you or your networking engineers need to configure your firewall to permit access to OMS.</span></span> <span data-ttu-id="48be8-133">V nastavení agenta nemusíte nic konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="48be8-133">No action is needed in agent settings.</span></span>

<span data-ttu-id="48be8-134">V následující tabulce najdete přehled prostředků potřebných pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="48be8-134">The following table shows resources needed for communication.</span></span>

>[!NOTE]
><span data-ttu-id="48be8-135">Některé z těchto prostředků zmínili Operational Insights, která byla předchozí název pro analýzu protokolu.</span><span class="sxs-lookup"><span data-stu-id="48be8-135">Some of the following resources mention Operational Insights, which was a previous name for Log Analytics.</span></span>

| <span data-ttu-id="48be8-136">Prostředek agenta</span><span class="sxs-lookup"><span data-stu-id="48be8-136">Agent Resource</span></span> | <span data-ttu-id="48be8-137">Porty</span><span class="sxs-lookup"><span data-stu-id="48be8-137">Ports</span></span> | <span data-ttu-id="48be8-138">Obejít kontrolu protokolu HTTPS</span><span class="sxs-lookup"><span data-stu-id="48be8-138">Bypass HTTPS inspection</span></span> |
|---|---|---|
| <span data-ttu-id="48be8-139">*.ods.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="48be8-139">*.ods.opinsights.azure.com</span></span> | <span data-ttu-id="48be8-140">443</span><span class="sxs-lookup"><span data-stu-id="48be8-140">443</span></span> | <span data-ttu-id="48be8-141">Ano</span><span class="sxs-lookup"><span data-stu-id="48be8-141">Yes</span></span> |
| <span data-ttu-id="48be8-142">*.oms.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="48be8-142">*.oms.opinsights.azure.com</span></span> | <span data-ttu-id="48be8-143">443</span><span class="sxs-lookup"><span data-stu-id="48be8-143">443</span></span> | <span data-ttu-id="48be8-144">Ano</span><span class="sxs-lookup"><span data-stu-id="48be8-144">Yes</span></span> |
| <span data-ttu-id="48be8-145">*.blob.core.windows.net</span><span class="sxs-lookup"><span data-stu-id="48be8-145">*.blob.core.windows.net</span></span> | <span data-ttu-id="48be8-146">443</span><span class="sxs-lookup"><span data-stu-id="48be8-146">443</span></span> | <span data-ttu-id="48be8-147">Ano</span><span class="sxs-lookup"><span data-stu-id="48be8-147">Yes</span></span> |
| <span data-ttu-id="48be8-148">*.azure-automation.net</span><span class="sxs-lookup"><span data-stu-id="48be8-148">*.azure-automation.net</span></span> | <span data-ttu-id="48be8-149">443</span><span class="sxs-lookup"><span data-stu-id="48be8-149">443</span></span> | <span data-ttu-id="48be8-150">Ano</span><span class="sxs-lookup"><span data-stu-id="48be8-150">Yes</span></span> |



## <a name="download-the-agent-setup-file-from-oms"></a><span data-ttu-id="48be8-151">Stáhněte si instalační soubor agenta z OMS</span><span class="sxs-lookup"><span data-stu-id="48be8-151">Download the agent setup file from OMS</span></span>
1. <span data-ttu-id="48be8-152">Na portálu OMS na **přehled** klikněte na **nastavení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="48be8-152">In the OMS portal, on the **Overview** page, click the **Settings** tile.</span></span>  <span data-ttu-id="48be8-153">Klikněte **připojené zdroje** v horní části.</span><span class="sxs-lookup"><span data-stu-id="48be8-153">Click the **Connected Sources** tab at the top.</span></span>  
    <span data-ttu-id="48be8-154">![Karta připojené zdroje](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)</span><span class="sxs-lookup"><span data-stu-id="48be8-154">![Connected Sources tab](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)</span></span>
2. <span data-ttu-id="48be8-155">Klikněte na tlačítko **servery Windows** a pak klikněte na **stáhnout agenta Windows** vztahuje na typ procesoru počítače stáhnout instalační soubor.</span><span class="sxs-lookup"><span data-stu-id="48be8-155">Click **Windows Servers** and then click **Download Windows Agent** applicable to your computer processor type to download the setup file.</span></span>
3. <span data-ttu-id="48be8-156">Na pravé straně **ID pracovního prostoru**, klikněte na ikonu kopírování a vložení ID do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="48be8-156">On the right of **Workspace ID**, click the copy icon and paste the ID into Notepad.</span></span>
4. <span data-ttu-id="48be8-157">Na pravé straně **primární klíč**, klikněte na ikonu kopírování a vložení klíč do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="48be8-157">On the right of **Primary Key**, click the copy icon and paste the key into Notepad.</span></span>     

## <a name="install-the-agent-using-setup"></a><span data-ttu-id="48be8-158">Instalace agenta pomocí instalačního programu</span><span class="sxs-lookup"><span data-stu-id="48be8-158">Install the agent using setup</span></span>
1. <span data-ttu-id="48be8-159">Spusťte instalační program a nainstalujte agenta na počítači, který chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="48be8-159">Run Setup to install the agent on a computer that you want to manage.</span></span>
2. <span data-ttu-id="48be8-160">Na úvodní stránce klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="48be8-160">On the Welcome page, click **Next**.</span></span>
3. <span data-ttu-id="48be8-161">Na stránce Licenční podmínky, přečtěte si licenční a pak klikněte na tlačítko **souhlasím**.</span><span class="sxs-lookup"><span data-stu-id="48be8-161">On the License Terms page, read the license and then click **I Agree**.</span></span>
4. <span data-ttu-id="48be8-162">Na stránce cílovou složku změnit nebo ponechat výchozí instalační složku a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="48be8-162">On the Destination Folder page, change or keep the default installation folder and then click **Next**.</span></span>
5. <span data-ttu-id="48be8-163">Na stránce Možnosti instalace agenta můžete připojit agenta k Azure Log Analytics (OMS), Operations Manager, nebo můžete nechat volby prázdné Pokud chcete provést konfiguraci agenta později.</span><span class="sxs-lookup"><span data-stu-id="48be8-163">On the Agent Setup Options page, you can choose to connect the agent to Azure Log Analytics (OMS), Operations Manager, or you can leave the choices blank if you want to configure the agent later.</span></span> <span data-ttu-id="48be8-164">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="48be8-164">Click **Next**.</span></span>   
    - <span data-ttu-id="48be8-165">Pokud jste zvolili pro připojení k Azure Log Analytics (OMS), vložte **ID pracovního prostoru** a **klíč pracovního prostoru (primární klíč)** který jste zkopírovali do poznámkového bloku v předchozím postupu a pak klikněte na tlačítko **další** .</span><span class="sxs-lookup"><span data-stu-id="48be8-165">If you chose to connect to Azure Log Analytics (OMS), paste the **Workspace ID** and **Workspace Key (Primary Key)** that you copied into Notepad in the previous procedure and then click **Next**.</span></span>  
        <span data-ttu-id="48be8-166">![Vložte ID pracovního prostoru a primární klíč](./media/log-analytics-windows-agents/connect-workspace.png)</span><span class="sxs-lookup"><span data-stu-id="48be8-166">![paste Workspace ID and Primary Key](./media/log-analytics-windows-agents/connect-workspace.png)</span></span>
    - <span data-ttu-id="48be8-167">Pokud jste vybrali možnost připojení k nástroji Operations Manager, zadejte **název skupiny pro správu**, **serveru pro správu** název, a **Port serveru pro správu**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="48be8-167">If you chose to connect to Operations Manager, type the **Management Group Name**, **Management Server** name, and **Management Server Port**, and then click **Next**.</span></span> <span data-ttu-id="48be8-168">Na stránce účet akce agenta zvolit účet místní systém nebo účet místní domény a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="48be8-168">On the Agent Action Account page, choose either the Local System account or a local domain account and then click **Next**.</span></span>  
        <span data-ttu-id="48be8-169">![Konfigurace skupiny pro správu](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![účet akce agenta](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)</span><span class="sxs-lookup"><span data-stu-id="48be8-169">![management group configuration](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![agent action account](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)</span></span>

6. <span data-ttu-id="48be8-170">Na stránce Připraveno k instalaci, zkontrolujte vybrané možnosti a pak klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="48be8-170">On the Ready to Install page, review your choices and then click **Install**.</span></span>
7. <span data-ttu-id="48be8-171">Konfigurace byla úspěšně dokončena stránky, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="48be8-171">On the Configuration completed successfully page, click **Finish**.</span></span>
8. <span data-ttu-id="48be8-172">Po dokončení **agenta Microsoft Monitoring Agent** se zobrazí v **ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="48be8-172">When complete, the **Microsoft Monitoring Agent** appears in **Control Panel**.</span></span> <span data-ttu-id="48be8-173">Můžete zkontrolovat konfiguraci existuje a ověřte, zda agent je připojena k provozní přehledy (OMS).</span><span class="sxs-lookup"><span data-stu-id="48be8-173">You can review your configuration there and verify that the agent is connected to Operational Insights (OMS).</span></span> <span data-ttu-id="48be8-174">Při připojení k OMS, agent zobrazí zpráva s oznámením: **Microsoft Monitoring Agent úspěšně připojil ke službě Microsoft Operations Management Suite.**</span><span class="sxs-lookup"><span data-stu-id="48be8-174">When connected to OMS, the agent displays a message stating: **The Microsoft Monitoring Agent has successfully connected to the Microsoft Operations Management Suite service.**</span></span>

## <a name="configure-proxy-settings"></a><span data-ttu-id="48be8-175">Konfigurace nastavení proxy serveru</span><span class="sxs-lookup"><span data-stu-id="48be8-175">Configure proxy settings</span></span>

<span data-ttu-id="48be8-176">Následující postup slouží ke konfiguraci nastavení proxy serveru pro Microsoft Monitoring Agent pomocí Ovládacích panelů.</span><span class="sxs-lookup"><span data-stu-id="48be8-176">You can use the following procedure to configure proxy settings for the Microsoft Monitoring Agent using Control Panel.</span></span> <span data-ttu-id="48be8-177">Budete muset použít tento postup pro každý server.</span><span class="sxs-lookup"><span data-stu-id="48be8-177">You need to use this procedure for each server.</span></span> <span data-ttu-id="48be8-178">pokud máte mnoho serverů, které je nutné nakonfigurovat, může být jednodušší použít skript, který tento proces zautomatizuje.</span><span class="sxs-lookup"><span data-stu-id="48be8-178">If you have many servers that you need to configure, you might find it easier to use a script to automate this process.</span></span> <span data-ttu-id="48be8-179">Je-li to váš případ, podívejte se na další postup [Konfigurace nastavení proxy serveru pro Microsoft Monitoring Agent pomocí skriptu](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).</span><span class="sxs-lookup"><span data-stu-id="48be8-179">If so, see the next procedure [To configure proxy settings for the Microsoft Monitoring Agent using a script](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).</span></span>

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a><span data-ttu-id="48be8-180">Konfigurace nastavení proxy serveru pro Microsoft Monitoring Agent pomocí Ovládacích panelů</span><span class="sxs-lookup"><span data-stu-id="48be8-180">To configure proxy settings for the Microsoft Monitoring Agent using Control Panel</span></span>
1. <span data-ttu-id="48be8-181">Otevřete **Ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="48be8-181">Open **Control Panel**.</span></span>
2. <span data-ttu-id="48be8-182">Otevřete **Microsoft Monitoring Agent**.</span><span class="sxs-lookup"><span data-stu-id="48be8-182">Open **Microsoft Monitoring Agent**.</span></span>
3. <span data-ttu-id="48be8-183">Klikněte na kartu **Nastavení proxy serveru**.</span><span class="sxs-lookup"><span data-stu-id="48be8-183">Click the **Proxy Settings** tab.</span></span>  
    <span data-ttu-id="48be8-184">![karta nastavení proxy serveru](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)</span><span class="sxs-lookup"><span data-stu-id="48be8-184">![proxy settings tab](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)</span></span>
4. <span data-ttu-id="48be8-185">Zaškrtněte políčko **Použít proxy server** a zadejte adresu URL a v případě potřeby i číslo portu, podobně jako v příkladu výše.</span><span class="sxs-lookup"><span data-stu-id="48be8-185">Select **Use a proxy server** and type the URL and port number, if one is needed, similar to the example shown.</span></span> <span data-ttu-id="48be8-186">Pokud váš proxy server vyžaduje ověření, zadejte uživatelské jméno a heslo pro přístup k proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="48be8-186">If your proxy server requires authentication, type the username and password to access the proxy server.</span></span>


### <a name="verify-agent-connectivity-to-oms"></a><span data-ttu-id="48be8-187">Ověření připojení agenta k OMS</span><span class="sxs-lookup"><span data-stu-id="48be8-187">Verify agent connectivity to OMS</span></span>

<span data-ttu-id="48be8-188">Snadno můžete ověřit, zda jsou agenty komunikaci s OMS následujícím postupem:</span><span class="sxs-lookup"><span data-stu-id="48be8-188">You can easily verify whether your agents are communicating with OMS using the following procedure:</span></span>

1.  <span data-ttu-id="48be8-189">Na počítači s agentem Windows otevřete ovládací panely.</span><span class="sxs-lookup"><span data-stu-id="48be8-189">On the computer with the Windows agent, open Control Panel.</span></span>
2.  <span data-ttu-id="48be8-190">Otevřete agenta Microsoft Monitoring Agent.</span><span class="sxs-lookup"><span data-stu-id="48be8-190">Open Microsoft Monitoring Agent.</span></span>
3.  <span data-ttu-id="48be8-191">Klikněte na kartu Azure Log Analytics (OMS).</span><span class="sxs-lookup"><span data-stu-id="48be8-191">Click the Azure Log Analytics (OMS) tab.</span></span>
4.  <span data-ttu-id="48be8-192">Ve sloupci Stav byste měli vidět, že agent úspěšně připojené ke službě Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="48be8-192">In the Status column, you should see that the agent connected successfully to the Operations Management Suite service.</span></span>

![připojení agenta](./media/log-analytics-windows-agents/mma-connected.png)


### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a><span data-ttu-id="48be8-194">Konfigurace nastavení proxy serveru pro Microsoft Monitoring Agent pomocí skriptu</span><span class="sxs-lookup"><span data-stu-id="48be8-194">To configure proxy settings for the Microsoft Monitoring Agent using a script</span></span>
<span data-ttu-id="48be8-195">Zkopírujte následující ukázku kódu, aktualizujte ji pomocí informací specifických pro vaše prostředí, uložte ji s příponou názvu souboru PS1 a následně tento skript spusťte na každém počítači, který se připojuje přímo ke službě OMS.</span><span class="sxs-lookup"><span data-stu-id="48be8-195">Copy the following sample, update it with information specific to your environment, save it with a PS1 file name extension, and then run the script on each computer that connects directly to the OMS service.</span></span>

    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)



## <a name="install-the-agent-using-the-command-line"></a><span data-ttu-id="48be8-196">Instalace agenta pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="48be8-196">Install the agent using the command line</span></span>
- <span data-ttu-id="48be8-197">Upravit a potom použijte následující příklad pro instalaci agenta pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="48be8-197">Modify and then use the following example to install the agent using the command line.</span></span> <span data-ttu-id="48be8-198">V příkladu provede plně bezobslužnou instalaci.</span><span class="sxs-lookup"><span data-stu-id="48be8-198">The example performs a fully silent installation.</span></span>

    >[!NOTE]
    <span data-ttu-id="48be8-199">Pokud chcete provést upgrade agenta, budete muset použít analýzy protokolů skriptování rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="48be8-199">If you want to upgrade an agent, you need to use the Log Analytics scripting API.</span></span> <span data-ttu-id="48be8-200">Najdete v další části Postup při upgradu agenta.</span><span class="sxs-lookup"><span data-stu-id="48be8-200">See the next section to upgrade an agent.</span></span>

    ```dos
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

<span data-ttu-id="48be8-201">Agent používá IExpress jako jeho Self-Extractor pomocí `/c` příkaz.</span><span class="sxs-lookup"><span data-stu-id="48be8-201">The agent uses IExpress as its self-extractor using the `/c` command.</span></span> <span data-ttu-id="48be8-202">Zobrazí se přepínače příkazového řádku v [přepínače příkazového řádku pro IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) a aktualizujte příklad podle svých potřeb.</span><span class="sxs-lookup"><span data-stu-id="48be8-202">You can see the command-line switches at [Command-line switches for IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) and then update the example to suit your needs.</span></span>

|<span data-ttu-id="48be8-203">MMA specifické možnosti</span><span class="sxs-lookup"><span data-stu-id="48be8-203">MMA-specific options</span></span>                   |<span data-ttu-id="48be8-204">Poznámky</span><span class="sxs-lookup"><span data-stu-id="48be8-204">Notes</span></span>         |
|---------------------------------------|--------------|
|<span data-ttu-id="48be8-205">ADD_OPINSIGHTS_WORKSPACE</span><span class="sxs-lookup"><span data-stu-id="48be8-205">ADD_OPINSIGHTS_WORKSPACE</span></span>               | <span data-ttu-id="48be8-206">1 = konfigurace agenta tak, aby odesílaly pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="48be8-206">1 = Configure the agent to report to a workspace</span></span>                |
|<span data-ttu-id="48be8-207">OPINSIGHTS_WORKSPACE_ID</span><span class="sxs-lookup"><span data-stu-id="48be8-207">OPINSIGHTS_WORKSPACE_ID</span></span>                | <span data-ttu-id="48be8-208">Id pracovního prostoru (guid) pro pracovní prostor pro přidání</span><span class="sxs-lookup"><span data-stu-id="48be8-208">Workspace Id (guid) for the workspace to add</span></span>                    |
|<span data-ttu-id="48be8-209">OPINSIGHTS_WORKSPACE_KEY</span><span class="sxs-lookup"><span data-stu-id="48be8-209">OPINSIGHTS_WORKSPACE_KEY</span></span>               | <span data-ttu-id="48be8-210">Klíč pracovního prostoru se používá k ověření původně s pracovním prostoru</span><span class="sxs-lookup"><span data-stu-id="48be8-210">Workspace key used to initially authenticate with the workspace</span></span> |
|<span data-ttu-id="48be8-211">OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE</span><span class="sxs-lookup"><span data-stu-id="48be8-211">OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE</span></span>  | <span data-ttu-id="48be8-212">Zadejte cloudovém prostředí, kde se nachází v pracovním prostoru</span><span class="sxs-lookup"><span data-stu-id="48be8-212">Specify the cloud environment where the workspace is located</span></span> <br> <span data-ttu-id="48be8-213">0 = komerční cloudu azure (výchozí)</span><span class="sxs-lookup"><span data-stu-id="48be8-213">0 = Azure commercial cloud (default)</span></span> <br> <span data-ttu-id="48be8-214">1 = azure Government</span><span class="sxs-lookup"><span data-stu-id="48be8-214">1 = Azure Government</span></span> |
|<span data-ttu-id="48be8-215">OPINSIGHTS_PROXY_URL</span><span class="sxs-lookup"><span data-stu-id="48be8-215">OPINSIGHTS_PROXY_URL</span></span>               | <span data-ttu-id="48be8-216">Identifikátor URI pro proxy server používat</span><span class="sxs-lookup"><span data-stu-id="48be8-216">URI for the proxy to use</span></span> |
|<span data-ttu-id="48be8-217">OPINSIGHTS_PROXY_USERNAME</span><span class="sxs-lookup"><span data-stu-id="48be8-217">OPINSIGHTS_PROXY_USERNAME</span></span>               | <span data-ttu-id="48be8-218">Uživatelské jméno pro přístup k ověřený server proxy</span><span class="sxs-lookup"><span data-stu-id="48be8-218">Username to access an authenticated proxy</span></span> |
|<span data-ttu-id="48be8-219">OPINSIGHTS_PROXY_PASSWORD</span><span class="sxs-lookup"><span data-stu-id="48be8-219">OPINSIGHTS_PROXY_PASSWORD</span></span>               | <span data-ttu-id="48be8-220">Heslo pro přístup ověřený server proxy</span><span class="sxs-lookup"><span data-stu-id="48be8-220">Password to access an authenticated proxy</span></span> |

>[!NOTE]
<span data-ttu-id="48be8-221">Abyste se vyhnuli stiskne maximální délku příkazového řádku IExpress, nainstalujte agenta se žádný pracovní prostor nakonfigurované a potom pomocí skriptu pro nastavení konfigurace pro pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="48be8-221">To avoid hitting the command-line length limit of IExpress, install the agent with no workspace configured and then use a script to set configuration for the workspace.</span></span>

>[!NOTE]
<span data-ttu-id="48be8-222">Pokud dojde `Command line option syntax error.` při použití `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` parametr, můžete použít následující alternativní řešení:</span><span class="sxs-lookup"><span data-stu-id="48be8-222">If you get a `Command line option syntax error.` when using the `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` parameter, you can use the following workaround:</span></span>
```dos
MMASetup-AMD64.exe /C /T:.\MMAExtract
cd .\MMAExtract
setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
```

## <a name="add-a-workspace-using-a-script"></a><span data-ttu-id="48be8-223">Přidat pracovní prostor pomocí skriptu</span><span class="sxs-lookup"><span data-stu-id="48be8-223">Add a workspace using a script</span></span>
<span data-ttu-id="48be8-224">Přidáte pracovní prostor pomocí rozhraní API skriptování agenta analýzy protokolů v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="48be8-224">Add a workspace using the Log Analytics agent scripting API with the following example:</span></span>

```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

<span data-ttu-id="48be8-225">Pokud chcete přidat do pracovního prostoru v Azure pro US Government, použijte následující ukázka skriptu:</span><span class="sxs-lookup"><span data-stu-id="48be8-225">To add a workspace in Azure for US Government, use the following script sample:</span></span>
```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
<span data-ttu-id="48be8-226">Pokud jste použili příkazového řádku nebo skriptu dříve pro instalaci nebo konfiguraci agenta, `EnableAzureOperationalInsights` byl nahrazen `AddCloudWorkspace`.</span><span class="sxs-lookup"><span data-stu-id="48be8-226">If you've used the command line or script previously to install or configure the agent, `EnableAzureOperationalInsights` was replaced by `AddCloudWorkspace`.</span></span>

## <a name="install-the-agent-using-dsc-in-azure-automation"></a><span data-ttu-id="48be8-227">Instalace agenta pomocí DSC v Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="48be8-227">Install the agent using DSC in Azure Automation</span></span>

<span data-ttu-id="48be8-228">Následující ukázkový skript můžete použít k instalaci agenta pomocí ve službě Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="48be8-228">You can use the following script example to install the agent using DSC in Azure Automation.</span></span> <span data-ttu-id="48be8-229">Tento příklad nainstaluje agenta nástroje 64-bit, identifikovaný `URI` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="48be8-229">The example installs the 64-bit agent, identified by the `URI` value.</span></span> <span data-ttu-id="48be8-230">Můžete taky 32bitová verze nahrazením hodnota identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="48be8-230">You can also use the 32-bit version by replacing the URI value.</span></span> <span data-ttu-id="48be8-231">Identifikátory URI pro obě verze jsou:</span><span class="sxs-lookup"><span data-stu-id="48be8-231">The URIs for both versions are:</span></span>

- <span data-ttu-id="48be8-232">64bitový agent Windows - https://go.microsoft.com/fwlink/?LinkId=828603</span><span class="sxs-lookup"><span data-stu-id="48be8-232">Windows 64-bit agent - https://go.microsoft.com/fwlink/?LinkId=828603</span></span>
- <span data-ttu-id="48be8-233">32bitový agent Windows - https://go.microsoft.com/fwlink/?LinkId=828604</span><span class="sxs-lookup"><span data-stu-id="48be8-233">Windows 32-bit agent - https://go.microsoft.com/fwlink/?LinkId=828604</span></span>


>[!NOTE]
<span data-ttu-id="48be8-234">Tento postup a skriptu příklad neprovede upgrade existujícího agenta.</span><span class="sxs-lookup"><span data-stu-id="48be8-234">This procedure and script example will not upgrade an existing agent.</span></span>

1. <span data-ttu-id="48be8-235">Import xPSDesiredStateConfiguration DSC modulu z [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) do Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="48be8-235">Import the xPSDesiredStateConfiguration DSC Module from [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) into Azure Automation.</span></span>  
2.  <span data-ttu-id="48be8-236">Vytvoření proměnných assetů Azure Automation pro *OPSINSIGHTS_WS_ID* a *OPSINSIGHTS_WS_KEY*.</span><span class="sxs-lookup"><span data-stu-id="48be8-236">Create Azure Automation variable assets for *OPSINSIGHTS_WS_ID* and *OPSINSIGHTS_WS_KEY*.</span></span> <span data-ttu-id="48be8-237">Nastavit *OPSINSIGHTS_WS_ID* ID pracovního prostoru analýzy protokolů OMS a sadu *OPSINSIGHTS_WS_KEY* na primární klíč pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="48be8-237">Set *OPSINSIGHTS_WS_ID* to your OMS Log Analytics workspace ID and set *OPSINSIGHTS_WS_KEY* to the primary key of your workspace.</span></span>
3.  <span data-ttu-id="48be8-238">Pomocí následujícího skriptu a uložte ho jako MMAgent.ps1</span><span class="sxs-lookup"><span data-stu-id="48be8-238">Use the following script and save it as MMAgent.ps1</span></span>
4.  <span data-ttu-id="48be8-239">Upravit a potom použijte následující příklad pro instalaci agenta pomocí ve službě Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="48be8-239">Modify and then use the following example to install the agent using DSC in Azure Automation.</span></span> <span data-ttu-id="48be8-240">Importujte MMAgent.ps1 do Azure Automation pomocí rozhraní Azure Automation nebo rutinu.</span><span class="sxs-lookup"><span data-stu-id="48be8-240">Import MMAgent.ps1 into Azure Automation by using the Azure Automation interface or cmdlet.</span></span>
5.  <span data-ttu-id="48be8-241">Přiřadíte konfigurace uzlu.</span><span class="sxs-lookup"><span data-stu-id="48be8-241">Assign a node to the configuration.</span></span> <span data-ttu-id="48be8-242">Během 15 minut uzlu zkontroluje konfiguraci a MMA vložena do uzlu.</span><span class="sxs-lookup"><span data-stu-id="48be8-242">Within 15 minutes, the node checks its configuration and the MMA is pushed to the node.</span></span>

```PowerShell
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}


```

### <a name="get-the-latest-productid-value"></a><span data-ttu-id="48be8-243">Získat nejnovější hodnotu ProductId</span><span class="sxs-lookup"><span data-stu-id="48be8-243">Get the latest ProductId value</span></span>

<span data-ttu-id="48be8-244">`ProductId value` MMAgent.ps1 skript je jedinečné pro jednotlivé verze agenta.</span><span class="sxs-lookup"><span data-stu-id="48be8-244">The `ProductId value` in the MMAgent.ps1 script is unique to each agent version.</span></span> <span data-ttu-id="48be8-245">Při publikování aktualizovaná verze všech agentů, změní se hodnota ProductId.</span><span class="sxs-lookup"><span data-stu-id="48be8-245">When an updated version of each agent is published, the ProductId value changes.</span></span> <span data-ttu-id="48be8-246">Takže když ProductId v budoucnu změní, můžete najít pomocí jednoduchého skriptu verze agenta.</span><span class="sxs-lookup"><span data-stu-id="48be8-246">So, when the ProductId changes in the future, you can find the agent version using a simple script.</span></span> <span data-ttu-id="48be8-247">Až budete mít nejnovější verze agenta nainstalovat na testovací server, můžete použít následující skript k získání nainstalované ProductId hodnoty.</span><span class="sxs-lookup"><span data-stu-id="48be8-247">After you have the latest agent version installed on a test server, you can use the following script to get the installed ProductId value.</span></span> <span data-ttu-id="48be8-248">Pomocí nejnovější ProductId hodnoty, můžete aktualizovat hodnotu ve skriptu MMAgent.ps1.</span><span class="sxs-lookup"><span data-stu-id="48be8-248">Using the latest ProductId value, you can update the value in the MMAgent.ps1 script.</span></span>

```PowerShell
$InstalledApplications  = Get-ChildItem hklm:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall


foreach ($Application in $InstalledApplications)

{

     $Key = Get-ItemProperty $Application.PSPath

     if ($Key.DisplayName -eq "Microsoft Monitoring Agent")

     {

        $Key.DisplayName

        Write-Output ("Product ID is: " + $Key.PSChildName.Substring(1,$Key.PSChildName.Length -2))

     }

}  
```

## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a><span data-ttu-id="48be8-249">Nakonfigurujte agenta ručně, nebo přidejte další pracovní prostory</span><span class="sxs-lookup"><span data-stu-id="48be8-249">Configure an agent manually or add additional workspaces</span></span>
<span data-ttu-id="48be8-250">Pokud jste nainstalovali agenty, ale nenakonfigurovali nebo pokud chcete agenta tak, aby odesílaly několik pracovních prostorů, můžete použít následující informace pro povolení agenta nebo překonfigurujte.</span><span class="sxs-lookup"><span data-stu-id="48be8-250">If you've installed agents but did not configure them or if you want the agent to report to multiple workspaces, you can use the following information to enable an agent or reconfigure it.</span></span> <span data-ttu-id="48be8-251">Po dokončení konfigurace agenta, bude zaregistrovat službu agenta a získají potřebné informace o konfiguraci a sady management Pack, které obsahují informace o řešení.</span><span class="sxs-lookup"><span data-stu-id="48be8-251">After you've configured the agent, it will register with the agent service and will get necessary configuration information and management packs that contain solution information.</span></span>

1. <span data-ttu-id="48be8-252">Po instalaci agenta Microsoft Monitoring Agent, otevřete **ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="48be8-252">After you've installed the Microsoft Monitoring Agent, open **Control Panel**.</span></span>
2. <span data-ttu-id="48be8-253">Otevřete **agenta Microsoft Monitoring Agent** a klikněte **Azure Log Analytics (OMS)** kartě.</span><span class="sxs-lookup"><span data-stu-id="48be8-253">Open **Microsoft Monitoring Agent** and then click the **Azure Log Analytics (OMS)** tab.</span></span>   
3. <span data-ttu-id="48be8-254">Klikněte na tlačítko **přidat** otevřete **přidat pracovní prostor Log Analytics** pole.</span><span class="sxs-lookup"><span data-stu-id="48be8-254">Click **Add** to open the **Add a Log Analytics Workspace** box.</span></span>
4. <span data-ttu-id="48be8-255">Vložení **ID pracovního prostoru** a **klíč pracovního prostoru (primární klíč)** kterou jste zkopírovali do poznámkového bloku v předchozím postupu pro pracovní prostor, který chcete přidat a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="48be8-255">Paste the **Workspace ID** and **Workspace Key (Primary Key)** that you copied into Notepad in a previous procedure for the workspace that you want to add and then click **OK**.</span></span>  
    <span data-ttu-id="48be8-256">![Konfigurace služby Operational Insights](./media/log-analytics-windows-agents/add-workspace.png)</span><span class="sxs-lookup"><span data-stu-id="48be8-256">![configure Operational Insights](./media/log-analytics-windows-agents/add-workspace.png)</span></span>

<span data-ttu-id="48be8-257">Po shromáždění dat z počítačů monitorovaných agentem, počet počítačů monitorovaných pomocí OMS se zobrazí na portálu OMS na **připojené zdroje** kartě v **nastavení** jako **servery Připojení**.</span><span class="sxs-lookup"><span data-stu-id="48be8-257">After data is collected from computers monitored by the agent, the number of computers monitored by OMS will appear in the OMS portal on the **Connected Sources** tab in **Settings** as **Servers Connected**.</span></span>


## <a name="to-disable-an-agent"></a><span data-ttu-id="48be8-258">Chcete-li zakázat agenta</span><span class="sxs-lookup"><span data-stu-id="48be8-258">To disable an agent</span></span>
1. <span data-ttu-id="48be8-259">Po instalaci agenta, otevřete **ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="48be8-259">After installing the agent, open **Control Panel**.</span></span>
2. <span data-ttu-id="48be8-260">Otevřete Microsoft Monitoring Agent a klikněte **Azure Log Analytics (OMS)** kartě.</span><span class="sxs-lookup"><span data-stu-id="48be8-260">Open Microsoft Monitoring Agent and then click the **Azure Log Analytics (OMS)** tab.</span></span>
3. <span data-ttu-id="48be8-261">Vyberte pracovní prostor a pak klikněte na tlačítko **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="48be8-261">Select a workspace and then click **Remove**.</span></span> <span data-ttu-id="48be8-262">Tento krok opakujte pro všechny jiné pracovní prostory.</span><span class="sxs-lookup"><span data-stu-id="48be8-262">Repeat this step for all other workspaces.</span></span>


## <a name="optionally-configure-agents-to-report-to-an-operations-manager-management-group"></a><span data-ttu-id="48be8-263">Volitelně nakonfigurujte agenty informuje o skupině pro správu nástroje Operations Manager</span><span class="sxs-lookup"><span data-stu-id="48be8-263">Optionally, configure agents to report to an Operations Manager management group</span></span>

<span data-ttu-id="48be8-264">Pokud používáte nástroje Operations Manager v infrastruktuře IT, můžete také použít agenta MMA jako agenta nástroje Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="48be8-264">If you use Operations Manager in your IT infrastructure, you can also use the MMA agent as an Operations Manager agent.</span></span>

### <a name="to-configure-mma-agents-to-report-to-an-operations-manager-management-group"></a><span data-ttu-id="48be8-265">Konfigurace agentů MMA informuje o skupině pro správu nástroje Operations Manager</span><span class="sxs-lookup"><span data-stu-id="48be8-265">To configure MMA agents to report to an Operations Manager management group</span></span>
1.  <span data-ttu-id="48be8-266">Na počítači, kde je nainstalován agent, otevřete **ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="48be8-266">On the computer where the agent is installed, open **Control Panel**.</span></span>  
2.  <span data-ttu-id="48be8-267">Otevřete **agenta Microsoft Monitoring Agent** a klikněte **nástroje Operations Manager** karta.</span><span class="sxs-lookup"><span data-stu-id="48be8-267">Open **Microsoft Monitoring Agent** and then click the **Operations Manager** tab.</span></span>  
    <span data-ttu-id="48be8-268">![Karta Microsoft Monitoring Agent Operations Manager](./media/log-analytics-windows-agents/om-mg01.png)</span><span class="sxs-lookup"><span data-stu-id="48be8-268">![Microsoft Monitoring Agent Operations Manager tab](./media/log-analytics-windows-agents/om-mg01.png)</span></span>
3.  <span data-ttu-id="48be8-269">Pokud vaše servery nástroje Operations Manager integrace se službou Active Directory, klikněte na tlačítko **automaticky aktualizovat přiřazení skupin pro správu ze služby AD DS**.</span><span class="sxs-lookup"><span data-stu-id="48be8-269">If your Operations Manager servers have integration with Active Directory, click **Automatically update management group assignments from AD DS**.</span></span>
4.  <span data-ttu-id="48be8-270">Klikněte na tlačítko **přidat** otevřete **přidat skupinu pro správu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="48be8-270">Click **Add** to open the **Add a Management Group** dialog box.</span></span>  
    <span data-ttu-id="48be8-271">![Přidat skupinu pro správu agenta Microsoft Monitoring Agent](./media/log-analytics-windows-agents/oms-mma-om02.png)</span><span class="sxs-lookup"><span data-stu-id="48be8-271">![Microsoft Monitoring Agent Add a Management Group](./media/log-analytics-windows-agents/oms-mma-om02.png)</span></span>
5.  <span data-ttu-id="48be8-272">V **název skupiny pro správu** zadejte název skupiny pro správu.</span><span class="sxs-lookup"><span data-stu-id="48be8-272">In **Management group name** box, type the name of your management group.</span></span>
6.  <span data-ttu-id="48be8-273">V **primární server pro správu** zadejte název počítače primární server pro správu.</span><span class="sxs-lookup"><span data-stu-id="48be8-273">In the **Primary management server** box, type the computer name of the primary management server.</span></span>
7.  <span data-ttu-id="48be8-274">V **port serveru pro správu** zadejte číslo portu TCP.</span><span class="sxs-lookup"><span data-stu-id="48be8-274">In the **Management server port** box, type the TCP port number.</span></span>
8.  <span data-ttu-id="48be8-275">V části **účet akce agenta**, zvolte účet místní systém nebo účet místní domény.</span><span class="sxs-lookup"><span data-stu-id="48be8-275">Under **Agent Action Account**, choose either the Local System account or a local domain account.</span></span>
9.  <span data-ttu-id="48be8-276">Klikněte na tlačítko **OK** zavřete **přidat skupinu pro správu** dialogové okno a pak klikněte na tlačítko **OK** zavřete **vlastnosti agenta monitorování Microsoft** Dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="48be8-276">Click **OK** to close the **Add a Management Group** dialog box and then click **OK** to close the **Microsoft Monitoring Agent Properties** dialog box.</span></span>


## <a name="next-steps"></a><span data-ttu-id="48be8-277">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48be8-277">Next steps</span></span>

- <span data-ttu-id="48be8-278">Článek [Přidání řešení Log Analytics z galerie řešení](log-analytics-add-solutions.md) popisuje přidání funkcí a shromažďování dat.</span><span class="sxs-lookup"><span data-stu-id="48be8-278">[Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md) to add functionality and gather data.</span></span>
