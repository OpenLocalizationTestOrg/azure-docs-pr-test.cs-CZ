---
title: "Správa doporučení zabezpečení v Azure Security Center | Microsoft Docs"
description: "Tento dokument vás provede jak doporučení v Azure Security Center pomáhá zůstat souladu se zásadami zabezpečení a ochraně vašich prostředků Azure."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: terrylan
ms.openlocfilehash: 37419e40808fc8104cb89f6a742874ad6f8c838f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="managing-security-recommendations-in-azure-security-center"></a><span data-ttu-id="71e77-103">Správa doporučení zabezpečení v Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="71e77-103">Managing security recommendations in Azure Security Center</span></span>
<span data-ttu-id="71e77-104">Tento dokument vás provede procesem jak používat doporučení v Azure Security Center k ochraně vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="71e77-104">This document walks you through how to use recommendations in Azure Security Center to help you protect your Azure resources.</span></span>

> [!NOTE]
> <span data-ttu-id="71e77-105">Tento dokument vám tuto službu představí formou ukázkového nasazení.</span><span class="sxs-lookup"><span data-stu-id="71e77-105">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="71e77-106">Tento dokument není to podrobný průvodce.</span><span class="sxs-lookup"><span data-stu-id="71e77-106">This document is not a step-by-step guide.</span></span>
>
>

## <a name="what-are-security-recommendations"></a><span data-ttu-id="71e77-107">Jaké jsou doporučení zabezpečení?</span><span class="sxs-lookup"><span data-stu-id="71e77-107">What are security recommendations?</span></span>
<span data-ttu-id="71e77-108">Security Center pravidelně analyzuje stav zabezpečení vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="71e77-108">Security Center periodically analyzes the security state of your Azure resources.</span></span> <span data-ttu-id="71e77-109">Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří doporučení.</span><span class="sxs-lookup"><span data-stu-id="71e77-109">When Security Center identifies potential security vulnerabilities, it creates recommendations.</span></span> <span data-ttu-id="71e77-110">Doporučení vás provedou procesem konfigurace potřebných kontrol.</span><span class="sxs-lookup"><span data-stu-id="71e77-110">The recommendations guide you through the process of configuring the needed controls.</span></span>

## <a name="implementing-security-recommendations"></a><span data-ttu-id="71e77-111">Implementace doporučení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="71e77-111">Implementing security recommendations</span></span>
### <a name="set-recommendations"></a><span data-ttu-id="71e77-112">Sadu doporučení</span><span class="sxs-lookup"><span data-stu-id="71e77-112">Set recommendations</span></span>
<span data-ttu-id="71e77-113">V [nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md), Naučte se:</span><span class="sxs-lookup"><span data-stu-id="71e77-113">In [Setting security policies in Azure Security Center](security-center-policies.md), you learn to:</span></span>

* <span data-ttu-id="71e77-114">Konfigurovat zásady zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="71e77-114">Configure security policies.</span></span>
* <span data-ttu-id="71e77-115">Shromažďování dat zapněte.</span><span class="sxs-lookup"><span data-stu-id="71e77-115">Turn on data collection.</span></span>
* <span data-ttu-id="71e77-116">Vyberte, které doporučení zobrazíte v rámci svých zásad zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="71e77-116">Choose which recommendations to see as part of your security policy.</span></span>

<span data-ttu-id="71e77-117">Aktuální zásady doporučení center kolem aktualizací systému, standardních pravidel, antimalwarových programů [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) na podsítí a síťových rozhraní, auditování databáze SQL, SQL database transparentní šifrování dat, a webové aplikace brány firewall.</span><span class="sxs-lookup"><span data-stu-id="71e77-117">Current policy recommendations center around system updates, baseline rules, antimalware programs, [network security groups](../virtual-network/virtual-networks-nsg.md) on subnets and network interfaces, SQL database auditing, SQL database transparent data encryption, and web application firewalls.</span></span>  <span data-ttu-id="71e77-118">[Nastavení zásad zabezpečení](security-center-policies.md) obsahuje popis jednotlivých možností doporučení.</span><span class="sxs-lookup"><span data-stu-id="71e77-118">[Setting security policies](security-center-policies.md) provides a description of each recommendation option.</span></span>

### <a name="monitor-recommendations"></a><span data-ttu-id="71e77-119">Monitorování doporučení</span><span class="sxs-lookup"><span data-stu-id="71e77-119">Monitor recommendations</span></span>
<span data-ttu-id="71e77-120">Po nastavení zásad zabezpečení bude Security Center analyzovat stav zabezpečení vašich prostředků Azure za účelem identifikace potenciálních ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="71e77-120">After setting a security policy, Security Center analyzes the security state of your resources to identify potential vulnerabilities.</span></span> <span data-ttu-id="71e77-121">**Doporučení** na dlaždici **Security Center** okno vám oznamuje, celkový počet doporučení identifikovaný Security Center.</span><span class="sxs-lookup"><span data-stu-id="71e77-121">The **Recommendations** tile on the **Security Center** blade lets you know the total number of recommendations identified by Security Center.</span></span>

![Dlaždice doporučení][1]

<span data-ttu-id="71e77-123">Chcete-li zobrazit podrobnosti o každé doporučení:</span><span class="sxs-lookup"><span data-stu-id="71e77-123">To see the details of each recommendation:</span></span>

<span data-ttu-id="71e77-124">Vyberte **doporučení dlaždici** na **Security Center** okno.</span><span class="sxs-lookup"><span data-stu-id="71e77-124">Select the **Recommendations tile** on the **Security Center** blade.</span></span> <span data-ttu-id="71e77-125">**Doporučení** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="71e77-125">The **Recommendations** blade opens.</span></span>

<span data-ttu-id="71e77-126">Doporučení jsou zobrazena ve formátu tabulky, kde každý řádek představuje jedno konkrétní doporučení.</span><span class="sxs-lookup"><span data-stu-id="71e77-126">The recommendations are shown in a table format where each line represents one particular recommendation.</span></span> <span data-ttu-id="71e77-127">Sloupce v této tabulce jsou:</span><span class="sxs-lookup"><span data-stu-id="71e77-127">The columns of this table are:</span></span>

* <span data-ttu-id="71e77-128">**Popis**: vysvětluje doporučení a co je třeba provést k vyřešení ho.</span><span class="sxs-lookup"><span data-stu-id="71e77-128">**DESCRIPTION**: Explains the recommendation and what needs to be done to address it.</span></span>
* <span data-ttu-id="71e77-129">**PROSTŘEDEK**: jsou uvedeny prostředky, u kterých bude použito toto doporučení.</span><span class="sxs-lookup"><span data-stu-id="71e77-129">**RESOURCE**: Lists the resources to which this recommendation applies.</span></span>
* <span data-ttu-id="71e77-130">**Stav**: popisuje aktuální stav doporučení:</span><span class="sxs-lookup"><span data-stu-id="71e77-130">**STATE**: Describes the current state of the recommendation:</span></span>
  * <span data-ttu-id="71e77-131">**Otevřete**: doporučení dosud nebylo řešeno..</span><span class="sxs-lookup"><span data-stu-id="71e77-131">**Open**: The recommendation hasn't been addressed yet.</span></span>
  * <span data-ttu-id="71e77-132">**V průběhu**: doporučení se aktuálně na prostředky a není třeba žádné akce.</span><span class="sxs-lookup"><span data-stu-id="71e77-132">**In Progress**: The recommendation is currently being applied to the resources, and no action is required by you.</span></span>
  * <span data-ttu-id="71e77-133">**Vyřešit**: doporučení již byla dokončena (v tomto případě řádku je zobrazena šedě).</span><span class="sxs-lookup"><span data-stu-id="71e77-133">**Resolved**: The recommendation has already been completed (in this case, the line is grayed out).</span></span>
* <span data-ttu-id="71e77-134">**ZÁVAŽNOST**: Popisuje závažnost tohoto konkrétního doporučení:</span><span class="sxs-lookup"><span data-stu-id="71e77-134">**SEVERITY**: Describes the severity of that particular recommendation:</span></span>
  * <span data-ttu-id="71e77-135">**Vysoká**: ohrožení zabezpečení existuje u významného prostředku (například aplikace, virtuální počítač nebo skupinu zabezpečení sítě) a vyžaduje pozornost.</span><span class="sxs-lookup"><span data-stu-id="71e77-135">**High**: A vulnerability exists with a meaningful resource (such as an application, a VM, or a network security group) and requires attention.</span></span>
  * <span data-ttu-id="71e77-136">**Střední**: byla zjištěna chyba zabezpečení a nekritické nebo další kroky jsou požadovány k jeho odstranění nebo k dokončení procesu.</span><span class="sxs-lookup"><span data-stu-id="71e77-136">**Medium**: A vulnerability exists and non-critical or additional steps are required to eliminate it or to complete a process.</span></span>
  * <span data-ttu-id="71e77-137">**Nízká**: obsahuje chybu, mělo by se řešit, ale nevyžaduje okamžitou pozornost.</span><span class="sxs-lookup"><span data-stu-id="71e77-137">**Low**: A vulnerability exists that should be addressed but does not require immediate attention.</span></span> <span data-ttu-id="71e77-138">(Ve výchozím nastavení, nejsou přítomny nízkou doporučení, ale můžete filtrovat podle nízkou doporučení, pokud chcete, aby byla zobrazena.)</span><span class="sxs-lookup"><span data-stu-id="71e77-138">(By default, low recommendations aren't presented, but you can filter on low recommendations if you want to see them.)</span></span>

<span data-ttu-id="71e77-139">Použijte v následující tabulce vám pomohou pochopit dostupné doporučení a co každé z nich dělá Pokud použijete ho jako odkaz.</span><span class="sxs-lookup"><span data-stu-id="71e77-139">Use the table below as a reference to help you understand the available recommendations and what each one does if you apply it.</span></span>

> [!NOTE]
> <span data-ttu-id="71e77-140">Můžete zjistit [classic a modelech nasazení Resource Manager](../azure-classic-rm.md) pro prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="71e77-140">You will want to understand the [classic and Resource Manager deployment models](../azure-classic-rm.md) for Azure resources.</span></span>
>
>

| <span data-ttu-id="71e77-141">Doporučení</span><span class="sxs-lookup"><span data-stu-id="71e77-141">Recommendation</span></span> | <span data-ttu-id="71e77-142">Popis</span><span class="sxs-lookup"><span data-stu-id="71e77-142">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="71e77-143">Povolení shromažďování dat pro předplatná</span><span class="sxs-lookup"><span data-stu-id="71e77-143">Enable data collection for subscriptions</span></span>](security-center-enable-data-collection.md) |<span data-ttu-id="71e77-144">Doporučuje, abyste zapnuli shromažďování dat v zásadách zabezpečení pro každé ze svých předplatných a všechny virtuální počítače ve svých předplatných.</span><span class="sxs-lookup"><span data-stu-id="71e77-144">Recommends that you turn on data collection in the security policy for each of your subscriptions and all virtual machines (VMs) in your subscriptions.</span></span> |
| [<span data-ttu-id="71e77-145">Náprava ohrožení zabezpečení operačního systému</span><span class="sxs-lookup"><span data-stu-id="71e77-145">Remediate OS vulnerabilities</span></span>](security-center-remediate-os-vulnerabilities.md) |<span data-ttu-id="71e77-146">Doporučuje zarovnat vaše konfigurace operačního systému pomocí pravidel doporučená konfigurace, například, zakázat ukládání hesel.</span><span class="sxs-lookup"><span data-stu-id="71e77-146">Recommends that you align your OS configurations with the recommended configuration rules, for example, do not allow passwords to be saved.</span></span> |
| [<span data-ttu-id="71e77-147">Instalace aktualizací systému</span><span class="sxs-lookup"><span data-stu-id="71e77-147">Apply system updates</span></span>](security-center-apply-system-updates.md) |<span data-ttu-id="71e77-148">Doporučuje nasazení chybějících aktualizací zabezpečení systému a kritických aktualizací do virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="71e77-148">Recommends that you deploy missing system security and critical updates to VMs.</span></span> |
| [<span data-ttu-id="71e77-149">Použít pouze v době provedená sítě řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="71e77-149">Apply a Just-In-Time network access control</span></span>](security-center-just-in-time.md) | <span data-ttu-id="71e77-150">Doporučuje se použít jenom v přístup k časovému virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="71e77-150">Recommends that you apply just in time VM access.</span></span> <span data-ttu-id="71e77-151">Právě v čase je funkce ve verzi preview a je k dispozici ve standardní vrstvě služby Security Center.</span><span class="sxs-lookup"><span data-stu-id="71e77-151">The just in time feature is in preview and available on the Standard tier of Security Center.</span></span> <span data-ttu-id="71e77-152">V tématu [cenová](security-center-pricing.md) Další informace o službě Security Center je cenové úrovně.</span><span class="sxs-lookup"><span data-stu-id="71e77-152">See [Pricing](security-center-pricing.md) to learn more about Security Center's pricing tiers.</span></span> |
| [<span data-ttu-id="71e77-153">Restartování po aktualizacích systému</span><span class="sxs-lookup"><span data-stu-id="71e77-153">Reboot after system updates</span></span>](security-center-apply-system-updates.md#reboot-after-system-updates) |<span data-ttu-id="71e77-154">Doporučuje, abyste restartovali virtuální počítač k dokončení procesu instalace aktualizací systému.</span><span class="sxs-lookup"><span data-stu-id="71e77-154">Recommends that you reboot a VM to complete the process of applying system updates.</span></span> |
| [<span data-ttu-id="71e77-155">Přidání brány firewall webových aplikací</span><span class="sxs-lookup"><span data-stu-id="71e77-155">Add a web application firewall</span></span>](security-center-add-web-application-firewall.md) |<span data-ttu-id="71e77-156">Doporučuje, která můžete nasadit brány firewall webových aplikací (firewall webových aplikací) pro koncových bodů webové.</span><span class="sxs-lookup"><span data-stu-id="71e77-156">Recommends that you deploy a web application firewall (WAF) for web endpoints.</span></span> <span data-ttu-id="71e77-157">Doporučení firewall webových aplikací je zobrazený pro všechny veřejné přístupných IP adresy (IP úrovni Instance nebo IP skupinu s vyrovnáváním zatížení), skupinu zabezpečení sítě spojenou s otevřete příchozí webovými porty (80,443).</span><span class="sxs-lookup"><span data-stu-id="71e77-157">A WAF recommendation is shown for any public facing IP (either Instance Level IP or Load Balanced IP) that has an associated network security group with open inbound web ports (80,443).</span></span> </br><span data-ttu-id="71e77-158">Security Center doporučuje zřízení firewall webových aplikací, které pomáhají bránit proti útokům na cílení na vaše webové aplikace na virtuální počítače a služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="71e77-158">Security Center recommends that you provision a WAF to help defend against attacks targeting your web applications on virtual machines and on App Service Environment.</span></span> <span data-ttu-id="71e77-159">Je aplikaci služby prostředí (App Service Environment) [Premium](https://azure.microsoft.com/pricing/details/app-service/) služby možnost plánu služby Azure App Service, která poskytuje plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="71e77-159">An App Service Environment (ASE) is a [Premium](https://azure.microsoft.com/pricing/details/app-service/) service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps.</span></span> <span data-ttu-id="71e77-160">Další informace o App Service Environment, najdete v článku [dokumentace k aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="71e77-160">To learn more about ASE, see the [App Service Environment Documentation](../app-service/app-service-app-service-environments-readme.md).</span></span></br><span data-ttu-id="71e77-161">Přidáním těchto aplikací na vaše stávající nasazení firewall webových aplikací můžete chránit několika webových aplikací ve službě Security Center.</span><span class="sxs-lookup"><span data-stu-id="71e77-161">You can protect multiple web applications in Security Center by adding these applications to your existing WAF deployments.</span></span> |
| [<span data-ttu-id="71e77-162">Finalizace ochrany aplikací</span><span class="sxs-lookup"><span data-stu-id="71e77-162">Finalize application protection</span></span>](security-center-add-web-application-firewall.md#finalize-application-protection) |<span data-ttu-id="71e77-163">K dokončení konfigurace firewall webových aplikací, musí být přenos přesměruje do zařízení firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="71e77-163">To complete the configuration of a WAF, traffic must be rerouted to the WAF appliance.</span></span> <span data-ttu-id="71e77-164">Následující toto doporučení dokončení změny potřebné instalační.</span><span class="sxs-lookup"><span data-stu-id="71e77-164">Following this recommendation completes the necessary setup changes.</span></span> |
| [<span data-ttu-id="71e77-165">Přidání brány firewall příští generace</span><span class="sxs-lookup"><span data-stu-id="71e77-165">Add a Next Generation Firewall</span></span>](security-center-add-next-generation-firewall.md) |<span data-ttu-id="71e77-166">Doporučuje přidat brány Firewall pro další generace (NGFW) z partnera společnosti Microsoft pro zvýšení ochrany vaší zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="71e77-166">Recommends that you add a Next Generation Firewall (NGFW) from a Microsoft partner to increase your security protections.</span></span> |
| [<span data-ttu-id="71e77-167">Směrování provozu jenom přes NGFW</span><span class="sxs-lookup"><span data-stu-id="71e77-167">Route traffic through NGFW only</span></span>](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |<span data-ttu-id="71e77-168">Doporučuje konfiguraci pravidla zabezpečení skupiny (NSG) sítě, které vynutit příchozí provoz do virtuálního počítače prostřednictvím vaší NGFW.</span><span class="sxs-lookup"><span data-stu-id="71e77-168">Recommends that you configure network security group (NSG) rules that force inbound traffic to your VM through your NGFW.</span></span> |
| [<span data-ttu-id="71e77-169">Instalace Endpoint Protection</span><span class="sxs-lookup"><span data-stu-id="71e77-169">Install Endpoint Protection</span></span>](security-center-install-endpoint-protection.md) |<span data-ttu-id="71e77-170">Doporučuje, abyste do virtuálních počítačů nainstalovali antimalwarové programy (platí pouze pro virtuální počítače s Windows).</span><span class="sxs-lookup"><span data-stu-id="71e77-170">Recommends that you provision antimalware programs to VMs (Windows VMs only).</span></span> |
| [<span data-ttu-id="71e77-171">Vyřešení výstrah stavu služby Endpoint Protection</span><span class="sxs-lookup"><span data-stu-id="71e77-171">Resolve Endpoint Protection health alerts</span></span>](security-center-resolve-endpoint-protection-health-alerts.md) |<span data-ttu-id="71e77-172">Doporučuje, abyste vyřešili selhání ochrany koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="71e77-172">Recommends that you resolve endpoint protection failures.</span></span> |
| [<span data-ttu-id="71e77-173">Povolení skupin zabezpečení sítě pro podsítě nebo virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="71e77-173">Enable Network Security Groups on subnets or virtual machines</span></span>](security-center-enable-network-security-groups.md) |<span data-ttu-id="71e77-174">Doporučuje, abyste povolili skupiny Nsg na podsítě nebo virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="71e77-174">Recommends that you enable NSGs on subnets or VMs.</span></span> |
| [<span data-ttu-id="71e77-175">Omezení přístupu prostřednictvím internetové koncový bod</span><span class="sxs-lookup"><span data-stu-id="71e77-175">Restrict access through Internet facing endpoint</span></span>](security-center-restrict-access-through-internet-facing-endpoints.md) |<span data-ttu-id="71e77-176">Doporučuje konfigurace pravidla pro příchozí provoz pro skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="71e77-176">Recommends that you configure inbound traffic rules for NSGs.</span></span> |
| [<span data-ttu-id="71e77-177">Povolení auditování a detekce hrozeb na SQL serverech</span><span class="sxs-lookup"><span data-stu-id="71e77-177">Enable auditing and threat detection on SQL servers</span></span>](security-center-enable-auditing-on-sql-servers.md) |<span data-ttu-id="71e77-178">Doporučuje zapnout auditování a zjišťování hrozeb pro servery Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="71e77-178">Recommends that you turn on auditing and threat detection for Azure SQL servers.</span></span> <span data-ttu-id="71e77-179">(Jenom služba azure SQL.</span><span class="sxs-lookup"><span data-stu-id="71e77-179">(Azure SQL service only.</span></span> <span data-ttu-id="71e77-180">Neobsahuje SQL běžících na virtuálních počítačích.)</span><span class="sxs-lookup"><span data-stu-id="71e77-180">Doesn't include SQL running on your virtual machines.)</span></span> |
| [<span data-ttu-id="71e77-181">Povolení auditování a detekce hrozeb v databázích SQL</span><span class="sxs-lookup"><span data-stu-id="71e77-181">Enable auditing and threat detection on SQL databases</span></span>](security-center-enable-auditing-on-sql-databases.md) |<span data-ttu-id="71e77-182">Doporučuje zapnout auditování a zjišťování hrozeb pro databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="71e77-182">Recommends that you turn on auditing and threat detection for Azure SQL databases.</span></span> <span data-ttu-id="71e77-183">(Jenom služba azure SQL.</span><span class="sxs-lookup"><span data-stu-id="71e77-183">(Azure SQL service only.</span></span> <span data-ttu-id="71e77-184">Neobsahuje SQL běžících na virtuálních počítačích.)</span><span class="sxs-lookup"><span data-stu-id="71e77-184">Doesn't include SQL running on your virtual machines.)</span></span> |
| [<span data-ttu-id="71e77-185">Povolit transparentní šifrování dat v databázích SQL</span><span class="sxs-lookup"><span data-stu-id="71e77-185">Enable Transparent Data Encryption on SQL databases</span></span>](security-center-enable-transparent-data-encryption.md) |<span data-ttu-id="71e77-186">Doporučuje se, že povolíte šifrování pro databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="71e77-186">Recommends that you enable encryption for SQL databases.</span></span> <span data-ttu-id="71e77-187">(Služba azure SQL pouze.)</span><span class="sxs-lookup"><span data-stu-id="71e77-187">(Azure SQL service only.)</span></span> |
| [<span data-ttu-id="71e77-188">Povolení agenta virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="71e77-188">Enable VM Agent</span></span>](security-center-enable-vm-agent.md) |<span data-ttu-id="71e77-189">Umožňuje vám zobrazit, které virtuální počítače vyžadují agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="71e77-189">Enables you to see which VMs require the VM Agent.</span></span> <span data-ttu-id="71e77-190">Agent virtuálního počítače musí být nainstalován na virtuálních počítačích na opravu zřizování, kontrolu, kontrolu směrného plánu a antimalwarových programů.</span><span class="sxs-lookup"><span data-stu-id="71e77-190">The VM Agent must be installed on VMs to provision patch scanning, baseline scanning, and antimalware programs.</span></span> <span data-ttu-id="71e77-191">Agent virtuálního počítače je ve výchozím nastavení nainstalován na virtuálních počítačích nasazených z Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="71e77-191">The VM Agent is installed by default for VMs that are deployed from the Azure Marketplace.</span></span> <span data-ttu-id="71e77-192">V článku [Agenti a rozšíření virtuálních počítačů – Část 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) najdete informace o tom, jak agenta virtuálního počítače nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="71e77-192">The article [VM Agent and Extensions – Part 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) provides information on how to install the VM Agent.</span></span> |
| [<span data-ttu-id="71e77-193">Použití šifrování disku</span><span class="sxs-lookup"><span data-stu-id="71e77-193">Apply disk encryption</span></span>](security-center-apply-disk-encryption.md) |<span data-ttu-id="71e77-194">Doporučuje, abyste disky svých virtuálních počítačů zašifrovali pomocí služby Azure Disk Encryption (platí pro virtuální počítače s Windows a Linuxem).</span><span class="sxs-lookup"><span data-stu-id="71e77-194">Recommends that you encrypt your VM disks using Azure Disk Encryption (Windows and Linux VMs).</span></span> <span data-ttu-id="71e77-195">Na virtuálním počítači se doporučuje šifrování svazku operačního systému i svazku s daty.</span><span class="sxs-lookup"><span data-stu-id="71e77-195">Encryption is recommended for both the OS and data volumes on your VM.</span></span> |
| [<span data-ttu-id="71e77-196">Poskytnutí podrobností kontaktů zabezpečení</span><span class="sxs-lookup"><span data-stu-id="71e77-196">Provide security contact details</span></span>](security-center-provide-security-contact-details.md) |<span data-ttu-id="71e77-197">Doporučuje se, že zadáte zabezpečení kontaktní informace pro každé z vašich předplatných.</span><span class="sxs-lookup"><span data-stu-id="71e77-197">Recommends that you provide security contact information for each of your subscriptions.</span></span> <span data-ttu-id="71e77-198">Kontaktní informace je e-mailovou adresu a telefonní číslo.</span><span class="sxs-lookup"><span data-stu-id="71e77-198">Contact information is an email address and phone number.</span></span> <span data-ttu-id="71e77-199">Tyto informace slouží k vás kontaktovat, pokud náš tým zabezpečení zjistí, že jsou ohrožení zabezpečení vašich prostředků.</span><span class="sxs-lookup"><span data-stu-id="71e77-199">The information is used to contact you if our security team finds that your resources are compromised.</span></span> |
| [<span data-ttu-id="71e77-200">Aktualizace verze operačního systému</span><span class="sxs-lookup"><span data-stu-id="71e77-200">Update OS version</span></span>](security-center-update-os-version.md) |<span data-ttu-id="71e77-201">Doporučuje aktualizovat na verzi operačního systému (OS) pro cloudové služby na nejnovější dostupnou verzi pro operačních systémů.</span><span class="sxs-lookup"><span data-stu-id="71e77-201">Recommends that you update the operating system (OS) version for your Cloud Service to the most recent version available for your OS family.</span></span>  <span data-ttu-id="71e77-202">Další informace o službách Cloud Services najdete v tématu [cloudové služby přehled](../cloud-services/cloud-services-choose-me.md).</span><span class="sxs-lookup"><span data-stu-id="71e77-202">To learn more about Cloud Services, see the [Cloud Services overview](../cloud-services/cloud-services-choose-me.md).</span></span> |
| [<span data-ttu-id="71e77-203">Není nainstalováno posouzení ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="71e77-203">Vulnerability assessment not installed</span></span>](security-center-vulnerability-assessment-recommendations.md) |<span data-ttu-id="71e77-204">Doporučuje, abyste na vašem virtuálním počítači nainstalovali řešení posouzení ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="71e77-204">Recommends that you install a vulnerability assessment solution on your VM.</span></span> |
| [<span data-ttu-id="71e77-205">Náprava ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="71e77-205">Remediate vulnerabilities</span></span>](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |<span data-ttu-id="71e77-206">Umožňuje vám zobrazit ohrožení zabezpečení systému a aplikací zjištěná řešením posouzení ohrožení zabezpečení nainstalovaným na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="71e77-206">Enables you to see system and application vulnerabilities detected by the vulnerability assessment solution installed on your VM.</span></span> |
| [<span data-ttu-id="71e77-207">Povolit šifrování pro účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="71e77-207">Enable encryption for Azure Storage Account</span></span>](security-center-enable-encryption-for-storage-account.md) | <span data-ttu-id="71e77-208">Doporučuje povolit šifrování služby úložiště Azure pro data v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="71e77-208">Recommends that you enable Azure Storage Service Encryption for data at rest.</span></span> <span data-ttu-id="71e77-209">Šifrování služby úložiště (SSE) funguje tak, že šifrování dat při je zapsán do úložiště Azure a dešifruje před načtení.</span><span class="sxs-lookup"><span data-stu-id="71e77-209">Storage Service Encryption (SSE) works by encrypting the data when it is written to Azure storage and decrypts before retrieval.</span></span> <span data-ttu-id="71e77-210">SSE aktuálně nejsou k dispozici pouze pro službu Azure Blob lze použít pro objekty BLOB bloku, objektů BLOB stránky a doplňovacích objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="71e77-210">SSE is currently available only for the Azure Blob service and can be used for block blobs, page blobs, and append blobs.</span></span> <span data-ttu-id="71e77-211">Další informace najdete v tématu [šifrování služby úložiště pro data v klidovém stavu](../storage/common/storage-service-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="71e77-211">To learn more, see [Storage Service Encryption for data at rest](../storage/common/storage-service-encryption.md).</span></span></br><span data-ttu-id="71e77-212">SSE je podporována pouze na účty úložiště Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="71e77-212">SSE is only supported on Resource Manager storage accounts.</span></span> |

<span data-ttu-id="71e77-213">Můžete filtrovat a zavřít doporučení.</span><span class="sxs-lookup"><span data-stu-id="71e77-213">You can filter and dismiss recommendations.</span></span>

1. <span data-ttu-id="71e77-214">Vyberte **filtru** na **doporučení** okno.</span><span class="sxs-lookup"><span data-stu-id="71e77-214">Select **Filter** on the **Recommendations** blade.</span></span> <span data-ttu-id="71e77-215">**Filtru** otevře se okno a vybrat stav a závažnost hodnoty, které chcete vidět.</span><span class="sxs-lookup"><span data-stu-id="71e77-215">The **Filter** blade opens and you select the severity and state values you wish to see.</span></span>

    ![Filtrovat doporučení][2]
2. <span data-ttu-id="71e77-217">Pokud zjistíte, že doporučení se nedá použít, můžete zrušit doporučení a pak ji odfiltrovat ze zobrazení.</span><span class="sxs-lookup"><span data-stu-id="71e77-217">If you determine that a recommendation is not applicable, you can dismiss the recommendation and then filter it out of your view.</span></span> <span data-ttu-id="71e77-218">Existují dva způsoby zamítnutí doporučení.</span><span class="sxs-lookup"><span data-stu-id="71e77-218">There are two ways to dismiss a recommendation.</span></span> <span data-ttu-id="71e77-219">Jedním ze způsobů je klikněte pravým tlačítkem na položku a pak vyberte **přeskočení**.</span><span class="sxs-lookup"><span data-stu-id="71e77-219">One way is to right click an item, and then select **Dismiss**.</span></span> <span data-ttu-id="71e77-220">Druhá je pozastavte ukazatel myši nad položku, klikněte na tlačítko se třemi tečkami, které se zobrazí napravo a potom vyberte **přeskočení**.</span><span class="sxs-lookup"><span data-stu-id="71e77-220">The other is to hover over an item, click the three dots that appear to the right, and then select **Dismiss**.</span></span> <span data-ttu-id="71e77-221">Kliknutím můžete zobrazit dismissed doporučení **filtru**a potom vyberete **zamítnuto**.</span><span class="sxs-lookup"><span data-stu-id="71e77-221">You can view dismissed recommendations by clicking **Filter**, and then selecting **Dismissed**.</span></span>

    ![Zavření doporučení][3]

### <a name="apply-recommendations"></a><span data-ttu-id="71e77-223">Použít doporučení</span><span class="sxs-lookup"><span data-stu-id="71e77-223">Apply recommendations</span></span>
<span data-ttu-id="71e77-224">Po kontrole všech doporučení, rozhodněte, který jeden byste měli použít první.</span><span class="sxs-lookup"><span data-stu-id="71e77-224">After reviewing all recommendations, decide which one you should apply first.</span></span> <span data-ttu-id="71e77-225">Doporučujeme používat hodnocení závažnosti, jako parametr hlavní k vyhodnocení, která doporučení by měl použít první.</span><span class="sxs-lookup"><span data-stu-id="71e77-225">We recommend that you use the severity rating as the main parameter to evaluate which recommendations should be applied first.</span></span>

<span data-ttu-id="71e77-226">V tabulce předchozí doporučení vyberte doporučení a provede ji jako příklad tom, jak používat doporučení.</span><span class="sxs-lookup"><span data-stu-id="71e77-226">In the table of recommendations above, select a recommendation and walk through it as an example of how to apply a recommendation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="71e77-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="71e77-227">Next steps</span></span>
<span data-ttu-id="71e77-228">V tomto dokumentu jste se seznámili s doporučení zabezpečení v Centru zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="71e77-228">In this document, you were introduced to security recommendations in Security Center.</span></span> <span data-ttu-id="71e77-229">Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:</span><span class="sxs-lookup"><span data-stu-id="71e77-229">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="71e77-230">[Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="71e77-230">[Setting security policies in Azure Security Center](security-center-policies.md) — Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="71e77-231">[Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se sledovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="71e77-231">[Security health monitoring in Azure Security Center](security-center-monitoring.md) — Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="71e77-232">[Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak spravovat a reakce na výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="71e77-232">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) — Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="71e77-233">[Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="71e77-233">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) — Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="71e77-234">[Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.</span><span class="sxs-lookup"><span data-stu-id="71e77-234">[Azure Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="71e77-235">[Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="71e77-235">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png