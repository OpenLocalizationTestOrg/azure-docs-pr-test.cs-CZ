---
title: "aaaConnect Windows počítače tooAzure Log Analytics | Microsoft Docs"
description: "Tento článek ukazuje počítače se systémem Windows hello hello kroky tooconnect ve vaší místní infrastruktury toohello analýzy protokolů služby pomocí přizpůsobená verze hello Microsoft Monitoring Agent (MMA)."
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
ms.openlocfilehash: 7e15f9eeb0440bd2f6557d7215df701526e4f9aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-windows-computers-toohello-log-analytics-service-in-azure"></a><span data-ttu-id="44cd8-103">Připojení Windows počítače toohello analýzy protokolů služby v Azure</span><span class="sxs-lookup"><span data-stu-id="44cd8-103">Connect Windows computers toohello Log Analytics service in Azure</span></span>

<span data-ttu-id="44cd8-104">Tento článek popisuje kroky hello počítače se systémem Windows tooconnect pracovních prostorů tooOMS místní infrastruktury, pomocí přizpůsobená verze hello Microsoft Monitoring Agent (MMA).</span><span class="sxs-lookup"><span data-stu-id="44cd8-104">This article shows hello steps tooconnect Windows computers in your on-premises infrastructure tooOMS workspaces by using a customized version of hello Microsoft Monitoring Agent (MMA).</span></span> <span data-ttu-id="44cd8-105">Potřebujete tooinstall a agentů pro všechny počítače hello má tooonboard, aby služba toosend data toohello analýzy protokolů a tooview a postupovat podle dat připojit.</span><span class="sxs-lookup"><span data-stu-id="44cd8-105">You need tooinstall and connect agents for all of hello computers that you want tooonboard in order for them toosend data toohello Log Analytics service and tooview and act on that data.</span></span> <span data-ttu-id="44cd8-106">Každý agent může hlásit toomultiple pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="44cd8-106">Each agent can report toomultiple workspaces.</span></span>

<span data-ttu-id="44cd8-107">Můžete nainstalovat agenty pomocí instalačního programu, příkazového řádku nebo pomocí požadovaného stavu konfigurace (DSC) ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="44cd8-107">You can install agents using Setup, command line, or with Desired State Configuration (DSC) in Azure Automation.</span></span>  

>[!NOTE]
<span data-ttu-id="44cd8-108">Pro virtuální počítače běžící v Azure, můžete zjednodušit instalace pomocí hello [rozšíření virtuálního počítače](log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="44cd8-108">For virtual machines running in Azure, you can simplify installation by using hello [virtual machine extension](log-analytics-azure-vm-extension.md).</span></span>

<span data-ttu-id="44cd8-109">Na počítačích s připojením k Internetu hello agent používá hello připojení toohello Internet toosend data tooOMS.</span><span class="sxs-lookup"><span data-stu-id="44cd8-109">On computers with Internet connectivity, hello agent uses hello connection toohello Internet toosend data tooOMS.</span></span> <span data-ttu-id="44cd8-110">Pro počítače, které nemají připojení k Internetu, můžete použít server proxy nebo hello [OMS brány](log-analytics-oms-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="44cd8-110">For computers that do not have Internet connectivity, you can use a proxy or hello [OMS Gateway](log-analytics-oms-gateway.md).</span></span>

<span data-ttu-id="44cd8-111">Připojení počítače tooOMS vašeho systému Windows je jednoduchý pomocí tří jednoduché kroky:</span><span class="sxs-lookup"><span data-stu-id="44cd8-111">Connecting your Windows computers tooOMS is straightforward using three simple steps:</span></span>

1. <span data-ttu-id="44cd8-112">Stáhněte instalační soubor agenta hello z portálu OMS hello</span><span class="sxs-lookup"><span data-stu-id="44cd8-112">Download hello agent setup file from hello OMS portal</span></span>
2. <span data-ttu-id="44cd8-113">Instalace agenta hello pomocí zvolené metodě hello</span><span class="sxs-lookup"><span data-stu-id="44cd8-113">Install hello agent using hello method you choose</span></span>
3. <span data-ttu-id="44cd8-114">Konfigurace agenta hello nebo přidejte další pracovní prostory, v případě potřeby</span><span class="sxs-lookup"><span data-stu-id="44cd8-114">Configure hello agent or add additional workspaces, if necessary</span></span>

<span data-ttu-id="44cd8-115">Hello následující diagram znázorňuje hello vztah mezi počítače se systémem Windows a OMS po nainstalovat a nakonfigurovat agenty.</span><span class="sxs-lookup"><span data-stu-id="44cd8-115">hello following diagram shows hello relationship between your Windows computers and OMS after you’ve installed and configured agents.</span></span>

![OMS-direct agenta – diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)

<span data-ttu-id="44cd8-117">Pokud vaše zásady zabezpečení IT neumožňují počítače ve vaší síti tooconnect toohello Internetu, můžete nakonfigurovat vaše počítače tooconnect toohello OMS brány.</span><span class="sxs-lookup"><span data-stu-id="44cd8-117">If your IT security policies do not allow computers on your network tooconnect toohello Internet, you can configure your computers tooconnect toohello OMS Gateway.</span></span> <span data-ttu-id="44cd8-118">Další informace a na tom, jak tooconfigure toocommunicate vaše servery prostřednictvím brány OMS toohello OMS služby, najdete v části kroky [připojit počítače tooOMS pomocí hello OMS brány](log-analytics-oms-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="44cd8-118">For more information and steps on how tooconfigure your servers toocommunicate through an OMS Gateway toohello OMS service, see [Connect computers tooOMS using hello OMS Gateway](log-analytics-oms-gateway.md).</span></span>

## <a name="system-requirements-and-required-configuration"></a><span data-ttu-id="44cd8-119">Požadavky na systém a požadované konfiguraci</span><span class="sxs-lookup"><span data-stu-id="44cd8-119">System requirements and required configuration</span></span>
<span data-ttu-id="44cd8-120">Před instalací nebo nasadit agenty, zkontrolujte následující podrobnosti tooensure hello požadavky splňujete hello.</span><span class="sxs-lookup"><span data-stu-id="44cd8-120">Before you install or deploy agents, review hello following details tooensure you meet hello requirements.</span></span>

- <span data-ttu-id="44cd8-121">Hello OMS MMA můžete nainstalovat pouze na počítačích se systémem Windows Server 2008 s aktualizací SP 1 nebo novější nebo Windows 7 SP1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="44cd8-121">You can only install hello OMS MMA on computers running Windows Server 2008 SP 1 or later or Windows 7 SP1 or later.</span></span>
- <span data-ttu-id="44cd8-122">Budete potřebovat předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="44cd8-122">You need an Azure subscription.</span></span>  <span data-ttu-id="44cd8-123">Další informace najdete v tématu [začít pracovat s analýzy protokolů](log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="44cd8-123">For more information, see [Get started with Log Analytics](log-analytics-get-started.md).</span></span>
- <span data-ttu-id="44cd8-124">Každý počítač se systémem Windows musí být schopný tooconnect toohello Internetu pomocí protokolu HTTPS nebo toohello OMS brány.</span><span class="sxs-lookup"><span data-stu-id="44cd8-124">Each Windows computer must be able tooconnect toohello Internet using HTTPS or toohello OMS Gateway.</span></span> <span data-ttu-id="44cd8-125">Toto připojení může být direct, prostřednictvím proxy serveru, nebo prostřednictvím hello OMS brány.</span><span class="sxs-lookup"><span data-stu-id="44cd8-125">This connection can be direct, via a proxy, or through hello OMS Gateway.</span></span>
- <span data-ttu-id="44cd8-126">Hello OMS MMA můžete nainstalovat na samostatných počítačů, serverů a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="44cd8-126">You can install hello OMS MMA on stand-alone computers, servers, and virtual machines.</span></span> <span data-ttu-id="44cd8-127">Pokud chcete tooOMS tooconnect virtuálních počítačů hostovaných v Azure, najdete v části [tooLog virtuální počítače Azure připojit Analytics](log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="44cd8-127">If you want tooconnect Azure-hosted virtual machines tooOMS, see [Connect Azure virtual machines tooLog Analytics](log-analytics-azure-vm-extension.md).</span></span>
- <span data-ttu-id="44cd8-128">Hello agent potřebuje toouse port 443 protokolu TCP pro různé prostředky.</span><span class="sxs-lookup"><span data-stu-id="44cd8-128">hello agent needs toouse TCP port 443 for various resources.</span></span>

### <a name="network"></a><span data-ttu-id="44cd8-129">Síť</span><span class="sxs-lookup"><span data-stu-id="44cd8-129">Network</span></span>

<span data-ttu-id="44cd8-130">Pro Windows agenty tooconnect tooand registrace službou hello OMS musí mít přístup k prostředkům toonetwork, včetně hello čísla portů a adres URL domény.</span><span class="sxs-lookup"><span data-stu-id="44cd8-130">For Windows agents tooconnect tooand register with hello OMS service, they must have access toonetwork resources, including hello port numbers and domain URLs.</span></span>

- <span data-ttu-id="44cd8-131">Pro proxy servery budete potřebovat tooensure, který hello odpovídající proxy serveru, které prostředky jsou nakonfigurované v nastavení agenta.</span><span class="sxs-lookup"><span data-stu-id="44cd8-131">For proxy servers, you need tooensure that hello appropriate proxy server resources are configured in agent settings.</span></span>
- <span data-ttu-id="44cd8-132">Pro brány firewall, které omezují přístup toohello Internet, vám nebo vaší sítě technici potřebovat tooconfigure tooOMS přístup toopermit vaší brány firewall.</span><span class="sxs-lookup"><span data-stu-id="44cd8-132">For firewalls that restrict access toohello Internet, you or your networking engineers need tooconfigure your firewall toopermit access tooOMS.</span></span> <span data-ttu-id="44cd8-133">V nastavení agenta nemusíte nic konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="44cd8-133">No action is needed in agent settings.</span></span>

<span data-ttu-id="44cd8-134">Hello následující tabulka ukazuje prostředky potřebné pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="44cd8-134">hello following table shows resources needed for communication.</span></span>

>[!NOTE]
><span data-ttu-id="44cd8-135">Některé z následujících prostředků hello zmínili Operational Insights, která byla předchozí název pro analýzu protokolu.</span><span class="sxs-lookup"><span data-stu-id="44cd8-135">Some of hello following resources mention Operational Insights, which was a previous name for Log Analytics.</span></span>

| <span data-ttu-id="44cd8-136">Prostředek agenta</span><span class="sxs-lookup"><span data-stu-id="44cd8-136">Agent Resource</span></span> | <span data-ttu-id="44cd8-137">Porty</span><span class="sxs-lookup"><span data-stu-id="44cd8-137">Ports</span></span> | <span data-ttu-id="44cd8-138">Obejít kontrolu protokolu HTTPS</span><span class="sxs-lookup"><span data-stu-id="44cd8-138">Bypass HTTPS inspection</span></span> |
|---|---|---|
| <span data-ttu-id="44cd8-139">*.ods.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="44cd8-139">*.ods.opinsights.azure.com</span></span> | <span data-ttu-id="44cd8-140">443</span><span class="sxs-lookup"><span data-stu-id="44cd8-140">443</span></span> | <span data-ttu-id="44cd8-141">Ano</span><span class="sxs-lookup"><span data-stu-id="44cd8-141">Yes</span></span> |
| <span data-ttu-id="44cd8-142">*.oms.opinsights.azure.com</span><span class="sxs-lookup"><span data-stu-id="44cd8-142">*.oms.opinsights.azure.com</span></span> | <span data-ttu-id="44cd8-143">443</span><span class="sxs-lookup"><span data-stu-id="44cd8-143">443</span></span> | <span data-ttu-id="44cd8-144">Ano</span><span class="sxs-lookup"><span data-stu-id="44cd8-144">Yes</span></span> |
| <span data-ttu-id="44cd8-145">*.blob.core.windows.net</span><span class="sxs-lookup"><span data-stu-id="44cd8-145">*.blob.core.windows.net</span></span> | <span data-ttu-id="44cd8-146">443</span><span class="sxs-lookup"><span data-stu-id="44cd8-146">443</span></span> | <span data-ttu-id="44cd8-147">Ano</span><span class="sxs-lookup"><span data-stu-id="44cd8-147">Yes</span></span> |
| <span data-ttu-id="44cd8-148">*.azure-automation.net</span><span class="sxs-lookup"><span data-stu-id="44cd8-148">*.azure-automation.net</span></span> | <span data-ttu-id="44cd8-149">443</span><span class="sxs-lookup"><span data-stu-id="44cd8-149">443</span></span> | <span data-ttu-id="44cd8-150">Ano</span><span class="sxs-lookup"><span data-stu-id="44cd8-150">Yes</span></span> |



## <a name="download-hello-agent-setup-file-from-oms"></a><span data-ttu-id="44cd8-151">Stáhněte instalační soubor agenta hello od OMS</span><span class="sxs-lookup"><span data-stu-id="44cd8-151">Download hello agent setup file from OMS</span></span>
1. <span data-ttu-id="44cd8-152">Na portálu OMS hello, na hello **přehled** klikněte na tlačítko hello **nastavení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="44cd8-152">In hello OMS portal, on hello **Overview** page, click hello **Settings** tile.</span></span>  <span data-ttu-id="44cd8-153">Klikněte na tlačítko hello **připojené zdroje** karty v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="44cd8-153">Click hello **Connected Sources** tab at hello top.</span></span>  
    <span data-ttu-id="44cd8-154">![Karta připojené zdroje](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)</span><span class="sxs-lookup"><span data-stu-id="44cd8-154">![Connected Sources tab](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)</span></span>
2. <span data-ttu-id="44cd8-155">Klikněte na tlačítko **servery Windows** a pak klikněte na **stáhnout agenta Windows** použít tooyour počítač procesor typ toodownload hello instalační soubor.</span><span class="sxs-lookup"><span data-stu-id="44cd8-155">Click **Windows Servers** and then click **Download Windows Agent** applicable tooyour computer processor type toodownload hello setup file.</span></span>
3. <span data-ttu-id="44cd8-156">Na hello napravo od **ID pracovního prostoru**, klikněte na ikonu hello kopírování a vložení hello ID do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="44cd8-156">On hello right of **Workspace ID**, click hello copy icon and paste hello ID into Notepad.</span></span>
4. <span data-ttu-id="44cd8-157">Na hello napravo od **primární klíč**, klikněte na ikonu hello kopírování a vložení hello klíč do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="44cd8-157">On hello right of **Primary Key**, click hello copy icon and paste hello key into Notepad.</span></span>     

## <a name="install-hello-agent-using-setup"></a><span data-ttu-id="44cd8-158">Nainstalujte agenta hello pomocí instalačního programu</span><span class="sxs-lookup"><span data-stu-id="44cd8-158">Install hello agent using setup</span></span>
1. <span data-ttu-id="44cd8-159">Spusťte instalační program tooinstall hello agenta na počítači, které chcete toomanage.</span><span class="sxs-lookup"><span data-stu-id="44cd8-159">Run Setup tooinstall hello agent on a computer that you want toomanage.</span></span>
2. <span data-ttu-id="44cd8-160">Na úvodní stránku hello, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-160">On hello Welcome page, click **Next**.</span></span>
3. <span data-ttu-id="44cd8-161">Na stránce hello licenční podmínky přečíst hello licencí a pak klikněte na **souhlasím**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-161">On hello License Terms page, read hello license and then click **I Agree**.</span></span>
4. <span data-ttu-id="44cd8-162">Na stránce hello cílovou složku, změnit nebo zachovat hello výchozí instalační složku a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-162">On hello Destination Folder page, change or keep hello default installation folder and then click **Next**.</span></span>
5. <span data-ttu-id="44cd8-163">Na stránce Možnosti instalace agenta hello můžete zvolit tooconnect hello agenta tooAzure Log Analytics (OMS), Operations Manager, nebo můžete nechat hello volby prázdné budete chtít tooconfigure hello později.</span><span class="sxs-lookup"><span data-stu-id="44cd8-163">On hello Agent Setup Options page, you can choose tooconnect hello agent tooAzure Log Analytics (OMS), Operations Manager, or you can leave hello choices blank if you want tooconfigure hello agent later.</span></span> <span data-ttu-id="44cd8-164">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-164">Click **Next**.</span></span>   
    - <span data-ttu-id="44cd8-165">Pokud jste zvolili tooconnect tooAzure analýzy protokolů (OMS), vložte hello **ID pracovního prostoru** a **klíč pracovního prostoru (primární klíč)** zkopírovali v předchozím postupu hello do poznámkového bloku a pak klikněte na  **Další**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-165">If you chose tooconnect tooAzure Log Analytics (OMS), paste hello **Workspace ID** and **Workspace Key (Primary Key)** that you copied into Notepad in hello previous procedure and then click **Next**.</span></span>  
        <span data-ttu-id="44cd8-166">![Vložte ID pracovního prostoru a primární klíč](./media/log-analytics-windows-agents/connect-workspace.png)</span><span class="sxs-lookup"><span data-stu-id="44cd8-166">![paste Workspace ID and Primary Key](./media/log-analytics-windows-agents/connect-workspace.png)</span></span>
    - <span data-ttu-id="44cd8-167">Pokud jste zvolili tooconnect tooOperations správce, zadejte hello **název skupiny pro správu**, **serveru pro správu** název, a **Port serveru pro správu**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-167">If you chose tooconnect tooOperations Manager, type hello **Management Group Name**, **Management Server** name, and **Management Server Port**, and then click **Next**.</span></span> <span data-ttu-id="44cd8-168">Na stránce hello účet akce agenta, zvolte hello místní systémový účet nebo účet místní domény a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-168">On hello Agent Action Account page, choose either hello Local System account or a local domain account and then click **Next**.</span></span>  
        <span data-ttu-id="44cd8-169">![Konfigurace skupiny pro správu](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![účet akce agenta](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)</span><span class="sxs-lookup"><span data-stu-id="44cd8-169">![management group configuration](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![agent action account](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)</span></span>

6. <span data-ttu-id="44cd8-170">Na stránce Připraveno tooInstall hello zkontrolujte vybrané možnosti a pak klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-170">On hello Ready tooInstall page, review your choices and then click **Install**.</span></span>
7. <span data-ttu-id="44cd8-171">Na hello konfigurace byla úspěšně dokončena stránky, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-171">On hello Configuration completed successfully page, click **Finish**.</span></span>
8. <span data-ttu-id="44cd8-172">Po dokončení hello **agenta Microsoft Monitoring Agent** se zobrazí v **ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-172">When complete, hello **Microsoft Monitoring Agent** appears in **Control Panel**.</span></span> <span data-ttu-id="44cd8-173">Můžete zkontrolovat konfiguraci existuje a ujistěte se, že tento agent hello připojené tooOperational přehledy (OMS).</span><span class="sxs-lookup"><span data-stu-id="44cd8-173">You can review your configuration there and verify that hello agent is connected tooOperational Insights (OMS).</span></span> <span data-ttu-id="44cd8-174">Když připojené tooOMS hello agenta zobrazí zpráva s oznámením: **hello agenta Microsoft Monitoring Agent byl úspěšně připojen toohello služby Microsoft Operations Management Suite.**</span><span class="sxs-lookup"><span data-stu-id="44cd8-174">When connected tooOMS, hello agent displays a message stating: **hello Microsoft Monitoring Agent has successfully connected toohello Microsoft Operations Management Suite service.**</span></span>

## <a name="configure-proxy-settings"></a><span data-ttu-id="44cd8-175">Konfigurace nastavení proxy serveru</span><span class="sxs-lookup"><span data-stu-id="44cd8-175">Configure proxy settings</span></span>

<span data-ttu-id="44cd8-176">Můžete použít následující postup tooconfigure nastavení proxy serveru pro hello Microsoft Monitoring Agent pomocí ovládacích panelů hello.</span><span class="sxs-lookup"><span data-stu-id="44cd8-176">You can use hello following procedure tooconfigure proxy settings for hello Microsoft Monitoring Agent using Control Panel.</span></span> <span data-ttu-id="44cd8-177">Toouse musíte pro každý server, tento postup.</span><span class="sxs-lookup"><span data-stu-id="44cd8-177">You need toouse this procedure for each server.</span></span> <span data-ttu-id="44cd8-178">Pokud máte mnoho serverů, je nutné, aby tooconfigure, může pro vás snadnější toouse skriptu tooautomate tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="44cd8-178">If you have many servers that you need tooconfigure, you might find it easier toouse a script tooautomate this process.</span></span> <span data-ttu-id="44cd8-179">Pokud ano, najdete v dalším postupu hello [tooconfigure nastavení proxy serveru pro hello Microsoft Monitoring Agent pomocí skriptu](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).</span><span class="sxs-lookup"><span data-stu-id="44cd8-179">If so, see hello next procedure [tooconfigure proxy settings for hello Microsoft Monitoring Agent using a script](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).</span></span>

### <a name="tooconfigure-proxy-settings-for-hello-microsoft-monitoring-agent-using-control-panel"></a><span data-ttu-id="44cd8-180">nastavení proxy serveru tooconfigure hello Microsoft Monitoring Agent pomocí ovládacích panelů</span><span class="sxs-lookup"><span data-stu-id="44cd8-180">tooconfigure proxy settings for hello Microsoft Monitoring Agent using Control Panel</span></span>
1. <span data-ttu-id="44cd8-181">Otevřete **Ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-181">Open **Control Panel**.</span></span>
2. <span data-ttu-id="44cd8-182">Otevřete **Microsoft Monitoring Agent**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-182">Open **Microsoft Monitoring Agent**.</span></span>
3. <span data-ttu-id="44cd8-183">Klikněte na tlačítko hello **nastavení proxy serveru** kartě.</span><span class="sxs-lookup"><span data-stu-id="44cd8-183">Click hello **Proxy Settings** tab.</span></span>  
    <span data-ttu-id="44cd8-184">![karta nastavení proxy serveru](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)</span><span class="sxs-lookup"><span data-stu-id="44cd8-184">![proxy settings tab](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)</span></span>
4. <span data-ttu-id="44cd8-185">Vyberte **použít proxy server** a zadejte adresu URL hello a čísla portu, pokud je potřebné, podobně jako toohello příkladu.</span><span class="sxs-lookup"><span data-stu-id="44cd8-185">Select **Use a proxy server** and type hello URL and port number, if one is needed, similar toohello example shown.</span></span> <span data-ttu-id="44cd8-186">Pokud proxy server vyžaduje ověřování, zadejte hello uživatelské jméno a heslo tooaccess hello proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="44cd8-186">If your proxy server requires authentication, type hello username and password tooaccess hello proxy server.</span></span>


### <a name="verify-agent-connectivity-toooms"></a><span data-ttu-id="44cd8-187">Ověřte připojení tooOMS agenta</span><span class="sxs-lookup"><span data-stu-id="44cd8-187">Verify agent connectivity tooOMS</span></span>

<span data-ttu-id="44cd8-188">Snadno můžete ověřit, zda jsou agenty komunikaci s OMS pomocí hello následující postup:</span><span class="sxs-lookup"><span data-stu-id="44cd8-188">You can easily verify whether your agents are communicating with OMS using hello following procedure:</span></span>

1.  <span data-ttu-id="44cd8-189">Otevřete ovládací panely v počítači hello s agentem Windows hello.</span><span class="sxs-lookup"><span data-stu-id="44cd8-189">On hello computer with hello Windows agent, open Control Panel.</span></span>
2.  <span data-ttu-id="44cd8-190">Otevřete agenta Microsoft Monitoring Agent.</span><span class="sxs-lookup"><span data-stu-id="44cd8-190">Open Microsoft Monitoring Agent.</span></span>
3.  <span data-ttu-id="44cd8-191">Klikněte na kartu hello Azure Log Analytics (OMS).</span><span class="sxs-lookup"><span data-stu-id="44cd8-191">Click hello Azure Log Analytics (OMS) tab.</span></span>
4.  <span data-ttu-id="44cd8-192">Ve sloupci Stav hello měli byste vidět, že tento agent hello připojení úspěšně toohello služby Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="44cd8-192">In hello Status column, you should see that hello agent connected successfully toohello Operations Management Suite service.</span></span>

![připojení agenta](./media/log-analytics-windows-agents/mma-connected.png)


### <a name="tooconfigure-proxy-settings-for-hello-microsoft-monitoring-agent-using-a-script"></a><span data-ttu-id="44cd8-194">nastavení proxy serveru tooconfigure hello Microsoft Monitoring Agent pomocí skriptu</span><span class="sxs-lookup"><span data-stu-id="44cd8-194">tooconfigure proxy settings for hello Microsoft Monitoring Agent using a script</span></span>
<span data-ttu-id="44cd8-195">Zkopírujte hello následující ukázkové, jej aktualizovat s informace o konkrétní tooyour prostředí, uložit s příponou PS1 a spusťte skript hello na každém počítači, který se připojuje přímo toohello OMS služby.</span><span class="sxs-lookup"><span data-stu-id="44cd8-195">Copy hello following sample, update it with information specific tooyour environment, save it with a PS1 file name extension, and then run hello script on each computer that connects directly toohello OMS service.</span></span>

    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get hello Health Service configuration object.  We need toodetermine if we
    #have hello right update rollup with hello API we need.  If not, no need toorun hello rest of hello script.
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

    Write-Output "Setting proxy too$ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)



## <a name="install-hello-agent-using-hello-command-line"></a><span data-ttu-id="44cd8-196">Nainstalujte agenta hello hello příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="44cd8-196">Install hello agent using hello command line</span></span>
- <span data-ttu-id="44cd8-197">Upravit a pak použijte následující příklad tooinstall hello agenta pomocí příkazového řádku hello hello.</span><span class="sxs-lookup"><span data-stu-id="44cd8-197">Modify and then use hello following example tooinstall hello agent using hello command line.</span></span> <span data-ttu-id="44cd8-198">Příklad Hello provede plně bezobslužnou instalaci.</span><span class="sxs-lookup"><span data-stu-id="44cd8-198">hello example performs a fully silent installation.</span></span>

    >[!NOTE]
    <span data-ttu-id="44cd8-199">Pokud chcete, aby tooupgrade agenta, je třeba toouse hello analýzy protokolů skriptování rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="44cd8-199">If you want tooupgrade an agent, you need toouse hello Log Analytics scripting API.</span></span> <span data-ttu-id="44cd8-200">Najdete v další části tooupgrade hello agenta.</span><span class="sxs-lookup"><span data-stu-id="44cd8-200">See hello next section tooupgrade an agent.</span></span>

    ```dos
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

<span data-ttu-id="44cd8-201">Hello agent používá IExpress jako jeho Self-Extractor pomocí hello `/c` příkaz.</span><span class="sxs-lookup"><span data-stu-id="44cd8-201">hello agent uses IExpress as its self-extractor using hello `/c` command.</span></span> <span data-ttu-id="44cd8-202">Můžete zobrazit hello přepínače příkazového řádku na [přepínače příkazového řádku pro IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) a pak aktualizaci hello příklad toosuit vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="44cd8-202">You can see hello command-line switches at [Command-line switches for IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) and then update hello example toosuit your needs.</span></span>

|<span data-ttu-id="44cd8-203">MMA specifické možnosti</span><span class="sxs-lookup"><span data-stu-id="44cd8-203">MMA-specific options</span></span>                   |<span data-ttu-id="44cd8-204">Poznámky</span><span class="sxs-lookup"><span data-stu-id="44cd8-204">Notes</span></span>         |
|---------------------------------------|--------------|
|<span data-ttu-id="44cd8-205">ADD_OPINSIGHTS_WORKSPACE</span><span class="sxs-lookup"><span data-stu-id="44cd8-205">ADD_OPINSIGHTS_WORKSPACE</span></span>               | <span data-ttu-id="44cd8-206">1 = konfigurovat hello agenta tooreport tooa prostoru</span><span class="sxs-lookup"><span data-stu-id="44cd8-206">1 = Configure hello agent tooreport tooa workspace</span></span>                |
|<span data-ttu-id="44cd8-207">OPINSIGHTS_WORKSPACE_ID</span><span class="sxs-lookup"><span data-stu-id="44cd8-207">OPINSIGHTS_WORKSPACE_ID</span></span>                | <span data-ttu-id="44cd8-208">Id pracovního prostoru (guid) pro tooadd prostoru hello</span><span class="sxs-lookup"><span data-stu-id="44cd8-208">Workspace Id (guid) for hello workspace tooadd</span></span>                    |
|<span data-ttu-id="44cd8-209">OPINSIGHTS_WORKSPACE_KEY</span><span class="sxs-lookup"><span data-stu-id="44cd8-209">OPINSIGHTS_WORKSPACE_KEY</span></span>               | <span data-ttu-id="44cd8-210">Klíče používané tooinitially prostoru ověřování pomocí pracovního prostoru hello</span><span class="sxs-lookup"><span data-stu-id="44cd8-210">Workspace key used tooinitially authenticate with hello workspace</span></span> |
|<span data-ttu-id="44cd8-211">OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE</span><span class="sxs-lookup"><span data-stu-id="44cd8-211">OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE</span></span>  | <span data-ttu-id="44cd8-212">Zadejte hello cloudového prostředí, kde se nachází hello prostoru</span><span class="sxs-lookup"><span data-stu-id="44cd8-212">Specify hello cloud environment where hello workspace is located</span></span> <br> <span data-ttu-id="44cd8-213">0 = komerční cloudu azure (výchozí)</span><span class="sxs-lookup"><span data-stu-id="44cd8-213">0 = Azure commercial cloud (default)</span></span> <br> <span data-ttu-id="44cd8-214">1 = azure Government</span><span class="sxs-lookup"><span data-stu-id="44cd8-214">1 = Azure Government</span></span> |
|<span data-ttu-id="44cd8-215">OPINSIGHTS_PROXY_URL</span><span class="sxs-lookup"><span data-stu-id="44cd8-215">OPINSIGHTS_PROXY_URL</span></span>               | <span data-ttu-id="44cd8-216">Identifikátor URI pro hello proxy toouse</span><span class="sxs-lookup"><span data-stu-id="44cd8-216">URI for hello proxy toouse</span></span> |
|<span data-ttu-id="44cd8-217">OPINSIGHTS_PROXY_USERNAME</span><span class="sxs-lookup"><span data-stu-id="44cd8-217">OPINSIGHTS_PROXY_USERNAME</span></span>               | <span data-ttu-id="44cd8-218">Uživatelské jméno tooaccess ověřený server proxy</span><span class="sxs-lookup"><span data-stu-id="44cd8-218">Username tooaccess an authenticated proxy</span></span> |
|<span data-ttu-id="44cd8-219">OPINSIGHTS_PROXY_PASSWORD</span><span class="sxs-lookup"><span data-stu-id="44cd8-219">OPINSIGHTS_PROXY_PASSWORD</span></span>               | <span data-ttu-id="44cd8-220">Heslo tooaccess ověřený server proxy</span><span class="sxs-lookup"><span data-stu-id="44cd8-220">Password tooaccess an authenticated proxy</span></span> |

>[!NOTE]
<span data-ttu-id="44cd8-221">tooavoid stiskne hello příkazového řádku maximální délku IExpress, nainstalujte agenta hello se žádný pracovní prostor nakonfigurované a pak používat konfigurace tooset skript pro hello prostoru.</span><span class="sxs-lookup"><span data-stu-id="44cd8-221">tooavoid hitting hello command-line length limit of IExpress, install hello agent with no workspace configured and then use a script tooset configuration for hello workspace.</span></span>

>[!NOTE]
<span data-ttu-id="44cd8-222">Pokud dojde `Command line option syntax error.` při použití hello `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` parametr, můžete použít hello následující alternativní řešení:</span><span class="sxs-lookup"><span data-stu-id="44cd8-222">If you get a `Command line option syntax error.` when using hello `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` parameter, you can use hello following workaround:</span></span>
```dos
MMASetup-AMD64.exe /C /T:.\MMAExtract
cd .\MMAExtract
setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
```

## <a name="add-a-workspace-using-a-script"></a><span data-ttu-id="44cd8-223">Přidat pracovní prostor pomocí skriptu</span><span class="sxs-lookup"><span data-stu-id="44cd8-223">Add a workspace using a script</span></span>
<span data-ttu-id="44cd8-224">Přidáte pracovní prostor pomocí skriptování API agenta hello analýzy protokolů hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="44cd8-224">Add a workspace using hello Log Analytics agent scripting API with hello following example:</span></span>

```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

<span data-ttu-id="44cd8-225">tooadd pracovního prostoru v Azure US Government, použijte hello následující ukázka skriptu:</span><span class="sxs-lookup"><span data-stu-id="44cd8-225">tooadd a workspace in Azure for US Government, use hello following script sample:</span></span>
```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
<span data-ttu-id="44cd8-226">Pokud jste použili hello příkazového řádku nebo skriptu dříve tooinstall nebo konfiguraci agenta hello `EnableAzureOperationalInsights` byl nahrazen `AddCloudWorkspace`.</span><span class="sxs-lookup"><span data-stu-id="44cd8-226">If you've used hello command line or script previously tooinstall or configure hello agent, `EnableAzureOperationalInsights` was replaced by `AddCloudWorkspace`.</span></span>

## <a name="install-hello-agent-using-dsc-in-azure-automation"></a><span data-ttu-id="44cd8-227">Instalace agenta hello pomocí DSC v Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="44cd8-227">Install hello agent using DSC in Azure Automation</span></span>

<span data-ttu-id="44cd8-228">Můžete použít následující skript příklad tooinstall hello agenta pomocí ve službě Azure Automation DSC hello.</span><span class="sxs-lookup"><span data-stu-id="44cd8-228">You can use hello following script example tooinstall hello agent using DSC in Azure Automation.</span></span> <span data-ttu-id="44cd8-229">Příklad Hello nainstaluje hello 64bitový agent, identifikovaný hello `URI` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="44cd8-229">hello example installs hello 64-bit agent, identified by hello `URI` value.</span></span> <span data-ttu-id="44cd8-230">Můžete taky hello 32bitová verze nahrazením hello hodnota identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="44cd8-230">You can also use hello 32-bit version by replacing hello URI value.</span></span> <span data-ttu-id="44cd8-231">Hello identifikátory URI pro obě verze jsou:</span><span class="sxs-lookup"><span data-stu-id="44cd8-231">hello URIs for both versions are:</span></span>

- <span data-ttu-id="44cd8-232">64bitový agent Windows - https://go.microsoft.com/fwlink/?LinkId=828603</span><span class="sxs-lookup"><span data-stu-id="44cd8-232">Windows 64-bit agent - https://go.microsoft.com/fwlink/?LinkId=828603</span></span>
- <span data-ttu-id="44cd8-233">32bitový agent Windows - https://go.microsoft.com/fwlink/?LinkId=828604</span><span class="sxs-lookup"><span data-stu-id="44cd8-233">Windows 32-bit agent - https://go.microsoft.com/fwlink/?LinkId=828604</span></span>


>[!NOTE]
<span data-ttu-id="44cd8-234">Tento postup a skriptu příklad neprovede upgrade existujícího agenta.</span><span class="sxs-lookup"><span data-stu-id="44cd8-234">This procedure and script example will not upgrade an existing agent.</span></span>

1. <span data-ttu-id="44cd8-235">Import hello xPSDesiredStateConfiguration DSC modul z [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) do Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="44cd8-235">Import hello xPSDesiredStateConfiguration DSC Module from [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) into Azure Automation.</span></span>  
2.  <span data-ttu-id="44cd8-236">Vytvoření proměnných assetů Azure Automation pro *OPSINSIGHTS_WS_ID* a *OPSINSIGHTS_WS_KEY*.</span><span class="sxs-lookup"><span data-stu-id="44cd8-236">Create Azure Automation variable assets for *OPSINSIGHTS_WS_ID* and *OPSINSIGHTS_WS_KEY*.</span></span> <span data-ttu-id="44cd8-237">Nastavit *OPSINSIGHTS_WS_ID* ID pracovního prostoru analýzy protokolů OMS tooyour a nastavte *OPSINSIGHTS_WS_KEY* toohello primární klíč pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="44cd8-237">Set *OPSINSIGHTS_WS_ID* tooyour OMS Log Analytics workspace ID and set *OPSINSIGHTS_WS_KEY* toohello primary key of your workspace.</span></span>
3.  <span data-ttu-id="44cd8-238">Použít následující skript a uložte ho jako MMAgent.ps1 hello</span><span class="sxs-lookup"><span data-stu-id="44cd8-238">Use hello following script and save it as MMAgent.ps1</span></span>
4.  <span data-ttu-id="44cd8-239">Upravit a pak pomocí hello následující příklad tooinstall hello agenta pomocí DSC ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="44cd8-239">Modify and then use hello following example tooinstall hello agent using DSC in Azure Automation.</span></span> <span data-ttu-id="44cd8-240">Importujte MMAgent.ps1 do Azure Automation pomocí rozhraní Azure Automation hello nebo rutinu.</span><span class="sxs-lookup"><span data-stu-id="44cd8-240">Import MMAgent.ps1 into Azure Automation by using hello Azure Automation interface or cmdlet.</span></span>
5.  <span data-ttu-id="44cd8-241">Přiřaďte toohello konfigurace uzlu.</span><span class="sxs-lookup"><span data-stu-id="44cd8-241">Assign a node toohello configuration.</span></span> <span data-ttu-id="44cd8-242">Během 15 minut uzel hello zkontroluje konfiguraci a hello MMA se posune toohello uzlu.</span><span class="sxs-lookup"><span data-stu-id="44cd8-242">Within 15 minutes, hello node checks its configuration and hello MMA is pushed toohello node.</span></span>

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

### <a name="get-hello-latest-productid-value"></a><span data-ttu-id="44cd8-243">Získat nejnovější hodnotu ProductId hello</span><span class="sxs-lookup"><span data-stu-id="44cd8-243">Get hello latest ProductId value</span></span>

<span data-ttu-id="44cd8-244">Hello `ProductId value` v hello MMAgent.ps1 skript je jedinečný tooeach verze agenta.</span><span class="sxs-lookup"><span data-stu-id="44cd8-244">hello `ProductId value` in hello MMAgent.ps1 script is unique tooeach agent version.</span></span> <span data-ttu-id="44cd8-245">Při publikování aktualizovaná verze všech agentů, změní se hello ProductId hodnotu.</span><span class="sxs-lookup"><span data-stu-id="44cd8-245">When an updated version of each agent is published, hello ProductId value changes.</span></span> <span data-ttu-id="44cd8-246">Ano při hello ProductId změn v hello budoucí, můžete nalézt hello verze agenta pomocí jednoduchého skriptu.</span><span class="sxs-lookup"><span data-stu-id="44cd8-246">So, when hello ProductId changes in hello future, you can find hello agent version using a simple script.</span></span> <span data-ttu-id="44cd8-247">Až budete mít nejnovější verze agenta hello nainstalovaná na testovací server, můžete použít následující skript tooget hello nainstalován ProductId hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="44cd8-247">After you have hello latest agent version installed on a test server, you can use hello following script tooget hello installed ProductId value.</span></span> <span data-ttu-id="44cd8-248">Pomocí hodnoty nejnovější ProductId hello, můžete aktualizovat hodnotu hello v hello MMAgent.ps1 skriptu.</span><span class="sxs-lookup"><span data-stu-id="44cd8-248">Using hello latest ProductId value, you can update hello value in hello MMAgent.ps1 script.</span></span>

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

## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a><span data-ttu-id="44cd8-249">Nakonfigurujte agenta ručně, nebo přidejte další pracovní prostory</span><span class="sxs-lookup"><span data-stu-id="44cd8-249">Configure an agent manually or add additional workspaces</span></span>
<span data-ttu-id="44cd8-250">Pokud jste nainstalovali agenty, ale nenakonfigurovali nebo pokud chcete hello agenta tooreport toomultiple pracovní prostory, můžete použít následující informace tooenable agenta hello nebo překonfigurování.</span><span class="sxs-lookup"><span data-stu-id="44cd8-250">If you've installed agents but did not configure them or if you want hello agent tooreport toomultiple workspaces, you can use hello following information tooenable an agent or reconfigure it.</span></span> <span data-ttu-id="44cd8-251">Po dokončení konfigurace agenta hello, bude zaregistrujte službou hello agenta a získají potřebné informace o konfiguraci a sady management Pack, které obsahují informace o řešení.</span><span class="sxs-lookup"><span data-stu-id="44cd8-251">After you've configured hello agent, it will register with hello agent service and will get necessary configuration information and management packs that contain solution information.</span></span>

1. <span data-ttu-id="44cd8-252">Po instalaci agenta Microsoft Monitoring Agent hello, otevřete **ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-252">After you've installed hello Microsoft Monitoring Agent, open **Control Panel**.</span></span>
2. <span data-ttu-id="44cd8-253">Otevřete **agenta Microsoft Monitoring Agent** a pak klikněte na tlačítko hello **Azure Log Analytics (OMS)** kartě.</span><span class="sxs-lookup"><span data-stu-id="44cd8-253">Open **Microsoft Monitoring Agent** and then click hello **Azure Log Analytics (OMS)** tab.</span></span>   
3. <span data-ttu-id="44cd8-254">Klikněte na tlačítko **přidat** tooopen hello **přidat pracovní prostor Log Analytics** pole.</span><span class="sxs-lookup"><span data-stu-id="44cd8-254">Click **Add** tooopen hello **Add a Log Analytics Workspace** box.</span></span>
4. <span data-ttu-id="44cd8-255">Vložení hello **ID pracovního prostoru** a **klíč pracovního prostoru (primární klíč)** zkopírovali do poznámkového bloku v předchozím postupu pro hello prostoru má tooadd a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-255">Paste hello **Workspace ID** and **Workspace Key (Primary Key)** that you copied into Notepad in a previous procedure for hello workspace that you want tooadd and then click **OK**.</span></span>  
    <span data-ttu-id="44cd8-256">![Konfigurace služby Operational Insights](./media/log-analytics-windows-agents/add-workspace.png)</span><span class="sxs-lookup"><span data-stu-id="44cd8-256">![configure Operational Insights](./media/log-analytics-windows-agents/add-workspace.png)</span></span>

<span data-ttu-id="44cd8-257">Po shromáždění dat z počítačů monitorovaných agentem hello hello počet počítačů monitorovaných pomocí OMS objeví na portálu OMS hello na hello **připojené zdroje** kartě v **nastavení** jako  **Připojené servery**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-257">After data is collected from computers monitored by hello agent, hello number of computers monitored by OMS will appear in hello OMS portal on hello **Connected Sources** tab in **Settings** as **Servers Connected**.</span></span>


## <a name="toodisable-an-agent"></a><span data-ttu-id="44cd8-258">toodisable agenta</span><span class="sxs-lookup"><span data-stu-id="44cd8-258">toodisable an agent</span></span>
1. <span data-ttu-id="44cd8-259">Po instalaci agenta hello, otevřete **ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-259">After installing hello agent, open **Control Panel**.</span></span>
2. <span data-ttu-id="44cd8-260">Otevřete Microsoft Monitoring Agent a pak klikněte na tlačítko hello **Azure Log Analytics (OMS)** kartě.</span><span class="sxs-lookup"><span data-stu-id="44cd8-260">Open Microsoft Monitoring Agent and then click hello **Azure Log Analytics (OMS)** tab.</span></span>
3. <span data-ttu-id="44cd8-261">Vyberte pracovní prostor a pak klikněte na tlačítko **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-261">Select a workspace and then click **Remove**.</span></span> <span data-ttu-id="44cd8-262">Tento krok opakujte pro všechny jiné pracovní prostory.</span><span class="sxs-lookup"><span data-stu-id="44cd8-262">Repeat this step for all other workspaces.</span></span>


## <a name="optionally-configure-agents-tooreport-tooan-operations-manager-management-group"></a><span data-ttu-id="44cd8-263">Volitelně můžete nakonfigurujte skupinu správy agentů tooreport tooan nástroje Operations Manager</span><span class="sxs-lookup"><span data-stu-id="44cd8-263">Optionally, configure agents tooreport tooan Operations Manager management group</span></span>

<span data-ttu-id="44cd8-264">Pokud používáte nástroje Operations Manager v infrastruktuře IT, můžete také použít agenta MMA hello jako agenta nástroje Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="44cd8-264">If you use Operations Manager in your IT infrastructure, you can also use hello MMA agent as an Operations Manager agent.</span></span>

### <a name="tooconfigure-mma-agents-tooreport-tooan-operations-manager-management-group"></a><span data-ttu-id="44cd8-265">skupinu správy nástroje Operations Manager tooreport tooconfigure MMA agenty tooan</span><span class="sxs-lookup"><span data-stu-id="44cd8-265">tooconfigure MMA agents tooreport tooan Operations Manager management group</span></span>
1.  <span data-ttu-id="44cd8-266">Otevřete v počítači hello kde je nainstalován hello agent **ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-266">On hello computer where hello agent is installed, open **Control Panel**.</span></span>  
2.  <span data-ttu-id="44cd8-267">Otevřete **agenta Microsoft Monitoring Agent** a pak klikněte na tlačítko hello **nástroje Operations Manager** kartě.</span><span class="sxs-lookup"><span data-stu-id="44cd8-267">Open **Microsoft Monitoring Agent** and then click hello **Operations Manager** tab.</span></span>  
    <span data-ttu-id="44cd8-268">![Karta Microsoft Monitoring Agent Operations Manager](./media/log-analytics-windows-agents/om-mg01.png)</span><span class="sxs-lookup"><span data-stu-id="44cd8-268">![Microsoft Monitoring Agent Operations Manager tab](./media/log-analytics-windows-agents/om-mg01.png)</span></span>
3.  <span data-ttu-id="44cd8-269">Pokud vaše servery nástroje Operations Manager integrace se službou Active Directory, klikněte na tlačítko **automaticky aktualizovat přiřazení skupin pro správu ze služby AD DS**.</span><span class="sxs-lookup"><span data-stu-id="44cd8-269">If your Operations Manager servers have integration with Active Directory, click **Automatically update management group assignments from AD DS**.</span></span>
4.  <span data-ttu-id="44cd8-270">Klikněte na tlačítko **přidat** tooopen hello **přidat skupinu pro správu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="44cd8-270">Click **Add** tooopen hello **Add a Management Group** dialog box.</span></span>  
    <span data-ttu-id="44cd8-271">![Přidat skupinu pro správu agenta Microsoft Monitoring Agent](./media/log-analytics-windows-agents/oms-mma-om02.png)</span><span class="sxs-lookup"><span data-stu-id="44cd8-271">![Microsoft Monitoring Agent Add a Management Group](./media/log-analytics-windows-agents/oms-mma-om02.png)</span></span>
5.  <span data-ttu-id="44cd8-272">V **název skupiny pro správu** pole, hello název typu skupiny pro správu.</span><span class="sxs-lookup"><span data-stu-id="44cd8-272">In **Management group name** box, type hello name of your management group.</span></span>
6.  <span data-ttu-id="44cd8-273">V hello **primární server pro správu** pole, zadejte název počítače hello hello primární server pro správu.</span><span class="sxs-lookup"><span data-stu-id="44cd8-273">In hello **Primary management server** box, type hello computer name of hello primary management server.</span></span>
7.  <span data-ttu-id="44cd8-274">V hello **port serveru pro správu** pole, číslo portu TCP hello typu.</span><span class="sxs-lookup"><span data-stu-id="44cd8-274">In hello **Management server port** box, type hello TCP port number.</span></span>
8.  <span data-ttu-id="44cd8-275">V části **účet akce agenta**, zvolte hello místní systémový účet nebo účet místní domény.</span><span class="sxs-lookup"><span data-stu-id="44cd8-275">Under **Agent Action Account**, choose either hello Local System account or a local domain account.</span></span>
9.  <span data-ttu-id="44cd8-276">Klikněte na tlačítko **OK** tooclose hello **přidat skupinu pro správu** dialogové okno a pak klikněte na tlačítko **OK** tooclose hello **Microsoft Monitoring Agent vlastnosti**dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="44cd8-276">Click **OK** tooclose hello **Add a Management Group** dialog box and then click **OK** tooclose hello **Microsoft Monitoring Agent Properties** dialog box.</span></span>


## <a name="next-steps"></a><span data-ttu-id="44cd8-277">Další kroky</span><span class="sxs-lookup"><span data-stu-id="44cd8-277">Next steps</span></span>

- <span data-ttu-id="44cd8-278">[Přidat řešení pro analýzu protokolu z hello řešení Galerie](log-analytics-add-solutions.md) tooadd funkce a shromáždění dat.</span><span class="sxs-lookup"><span data-stu-id="44cd8-278">[Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md) tooadd functionality and gather data.</span></span>
