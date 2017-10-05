---
title: "Pouze ve virtuálním počítači čas přístup v Azure Security Center | Microsoft Docs"
description: "Tento dokument nevystavíte slabé stránky zabezpečení prostřednictvím jak jenom na dobu virtuálních počítačů přístup v Azure Security Center pomáhá můžete řídit přístup k virtuální počítače Azure."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: terrylan
ms.openlocfilehash: 5bb87488dcfc79ed4baa1dbd81dc4e1174f84e4b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a><span data-ttu-id="cf851-103">Spravovat přístup k virtuálním počítačům pomocí právě v čase</span><span class="sxs-lookup"><span data-stu-id="cf851-103">Manage virtual machine access using just in time</span></span>

<span data-ttu-id="cf851-104">Právě v čas virtuální počítač (VM) přístupu slouží zamknout příchozí přenosy na virtuální počítače Azure, snižuje riziko napadení a snadného přístupu pro připojení k virtuálním počítačům v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="cf851-104">Just in time virtual machine (VM) access can be used to lock down inbound traffic to your Azure VMs, reducing exposure to attacks while providing easy access to connect to VMs when needed.</span></span>

> [!NOTE]
> <span data-ttu-id="cf851-105">Právě v čase je funkce ve verzi preview a je k dispozici ve standardní vrstvě služby Security Center.</span><span class="sxs-lookup"><span data-stu-id="cf851-105">The just in time feature is in preview and available on the Standard tier of Security Center.</span></span>  <span data-ttu-id="cf851-106">V tématu [cenová](security-center-pricing.md) Další informace o službě Security Center je cenové úrovně.</span><span class="sxs-lookup"><span data-stu-id="cf851-106">See [Pricing](security-center-pricing.md) to learn more about Security Center's pricing tiers.</span></span>
>
>

## <a name="attack-scenario"></a><span data-ttu-id="cf851-107">Scénář útoku</span><span class="sxs-lookup"><span data-stu-id="cf851-107">Attack scenario</span></span>

<span data-ttu-id="cf851-108">Útok hrubou silou útokům běžně cílové porty správy jako prostředek k získání přístupu k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="cf851-108">Brute force attacks commonly target management ports as a means to gain access to a VM.</span></span> <span data-ttu-id="cf851-109">V případě úspěšného útočník může převzít kontrolu nad virtuálního počítače a vytvořit dostane do vašeho prostředí.</span><span class="sxs-lookup"><span data-stu-id="cf851-109">If successful, an attacker can take control over the VM and establish a foothold into your environment.</span></span>

<span data-ttu-id="cf851-110">Chcete-li omezit množství času, který je otevřený port je jedním ze způsobů, aby se snížila zranitelnost vůči útoku hrubou silou.</span><span class="sxs-lookup"><span data-stu-id="cf851-110">One way to reduce exposure to a brute force attack is to limit the amount of time that a port is open.</span></span> <span data-ttu-id="cf851-111">Porty pro správu se nemusíte být otevřený za všech okolností.</span><span class="sxs-lookup"><span data-stu-id="cf851-111">Management ports do not need to be open at all times.</span></span> <span data-ttu-id="cf851-112">Pouze musí být otevřený, pokud jsou připojeny k virtuálnímu počítači, například k provádění úkolů údržby nebo správy.</span><span class="sxs-lookup"><span data-stu-id="cf851-112">They only need to be open while you are connected to the VM, for example to perform management or maintenance tasks.</span></span> <span data-ttu-id="cf851-113">Pokud právě v čase je povoleno, Security Center používá [skupinu zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) (NSG) pravidla, která omezit přístup k portům správy, takže nemůžou být cílem útočníků.</span><span class="sxs-lookup"><span data-stu-id="cf851-113">When just in time is enabled, Security Center uses [Network Security Group](../virtual-network/virtual-networks-nsg.md) (NSG) rules, which restrict access to management ports so they cannot be targeted by attackers.</span></span>

![Jenom v případě čas][1]

## <a name="how-does-just-in-time-access-work"></a><span data-ttu-id="cf851-115">Jak jenom při přístup k časovému funguje?</span><span class="sxs-lookup"><span data-stu-id="cf851-115">How does just in time access work?</span></span>

<span data-ttu-id="cf851-116">Pokud je povolený přístup JIT (právě včas), Security Center uzamkne příchozí provoz do vašich virtuálních počítačů Azure vytvořením pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="cf851-116">When just in time is enabled, Security Center locks down inbound traffic to your Azure VMs by creating an NSG rule.</span></span> <span data-ttu-id="cf851-117">Můžete vybrat porty na virtuálním počítači, do které se uzamkne příchozí provoz směrem dolů.</span><span class="sxs-lookup"><span data-stu-id="cf851-117">You select the ports on the VM to which inbound traffic will be locked down.</span></span> <span data-ttu-id="cf851-118">Tyto porty jsou řízeny jenom v době řešení.</span><span class="sxs-lookup"><span data-stu-id="cf851-118">These ports are controlled by the just in time solution.</span></span>

<span data-ttu-id="cf851-119">Když uživatel požaduje přístup k virtuálnímu počítači, Security Center zkontroluje, zda má uživatel [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md) oprávnění, které poskytují přístup pro zápis pro prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="cf851-119">When a user requests access to a VM, Security Center checks that the user has [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) permissions that provide write access for the Azure resource.</span></span> <span data-ttu-id="cf851-120">Pokud mají oprávnění k zápisu, jeho žádost se schválí a Security Center automaticky nakonfiguruje skupin zabezpečení sítě (Nsg) chcete povolit příchozí přenosy na správu porty pro množství času, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="cf851-120">If they have write permissions, the request is approved and Security Center automatically configures the Network Security Groups (NSGs) to allow inbound traffic to the management ports for the amount of time you specified.</span></span> <span data-ttu-id="cf851-121">Po dobu vypršení platnosti, Security Center obnoví skupin Nsg do předchozího stavu.</span><span class="sxs-lookup"><span data-stu-id="cf851-121">After the time has expired, Security Center restores the NSGs to their previous states.</span></span>

> [!NOTE]
> <span data-ttu-id="cf851-122">Security Center, které jsou právě čas virtuálních počítačů přístup aktuálně podporuje pouze virtuální počítače nasazené prostřednictvím Správce Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="cf851-122">Security Center just in time VM access currently supports only VMs deployed through Azure Resource Manager.</span></span> <span data-ttu-id="cf851-123">Další informace o modelu nasazení Resource Manager i classic najdete [Azure Resource Manager oproti nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="cf851-123">To learn more about the classic and Resource Manager deployment models see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>
>
>

## <a name="using-just-in-time-access"></a><span data-ttu-id="cf851-124">Použití pouze v přístup k časovému</span><span class="sxs-lookup"><span data-stu-id="cf851-124">Using just in time access</span></span>

<span data-ttu-id="cf851-125">**Těsně v čas virtuálních počítačů přístup** na dlaždici **Security Center** okno zobrazuje počet virtuálních počítačů, které jsou nakonfigurované pro jenom v čas přístupu a počet požadavků schválené přístup provedených během posledního týdne.</span><span class="sxs-lookup"><span data-stu-id="cf851-125">The **Just in time VM access** tile on the **Security Center** blade shows the number of VMs configured for just in time access and the number of approved access requests made in the last week.</span></span>

<span data-ttu-id="cf851-126">Vyberte **těsně v čas virtuálních počítačů přístup** dlaždici a **těsně v čas virtuálních počítačů přístup** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="cf851-126">Select the **Just in time VM access** tile and the **Just in time VM access** blade opens.</span></span>

![Právě v čase virtuální počítač přístup k dlaždici][2]

<span data-ttu-id="cf851-128">**Těsně v čas virtuálních počítačů přístup** okno poskytuje informace o stavu virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="cf851-128">The **Just in time VM access** blade provides information on the state of your VMs:</span></span>

- <span data-ttu-id="cf851-129">**Nakonfigurované** -virtuálních počítačů, které jsou nakonfigurované pro podporu právě v přístup k časovému virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cf851-129">**Configured** - VMs that have been configured to support just in time VM access.</span></span> <span data-ttu-id="cf851-130">Data uvedená za poslední týden a zahrnuje počet schválené žádosti, datum posledního přístupu a čas a naposledy uživatele pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="cf851-130">The data presented is for the last week and includes for each VM the number of approved requests, last access date and time, and last user.</span></span>
- <span data-ttu-id="cf851-131">**Doporučená** -virtuálních počítačů, které může podporovat jenom v přístup k časovému virtuálních počítačů, ale nebyly nakonfigurovány na.</span><span class="sxs-lookup"><span data-stu-id="cf851-131">**Recommended** - VMs that can support just in time VM access but have not been configured to.</span></span> <span data-ttu-id="cf851-132">Doporučujeme povolit jenom při řízení přístupu čas virtuálních počítačů pro tyto virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="cf851-132">We recommend that you enable just in time VM access control for these VMs.</span></span> <span data-ttu-id="cf851-133">V tématu [Povolit jenom v čas virtuálních počítačů přístup](#enable-just-in-time-vm-access).</span><span class="sxs-lookup"><span data-stu-id="cf851-133">See [Enable just in time VM access](#enable-just-in-time-vm-access).</span></span>
- <span data-ttu-id="cf851-134">**Žádná doporučení** -důvody, které můžou způsobit, že virtuální počítač tak, aby se doporučuje jsou:</span><span class="sxs-lookup"><span data-stu-id="cf851-134">**No recommendation** - Reasons that can cause a VM not to be recommended are:</span></span>
  - <span data-ttu-id="cf851-135">Chybí NSG - právě v čase řešení vyžaduje skupinu NSG se.</span><span class="sxs-lookup"><span data-stu-id="cf851-135">Missing NSG - The just in time solution requires an NSG to be in place.</span></span>
  - <span data-ttu-id="cf851-136">Classic virtuálního počítače – Security Center, které jsou právě čas virtuálních počítačů přístup aktuálně podporuje pouze virtuální počítače nasazené prostřednictvím Správce Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="cf851-136">Classic VM - Security Center just in time VM access currently supports only VMs deployed through Azure Resource Manager.</span></span> <span data-ttu-id="cf851-137">Nasazení classic není podporována pouze v době řešení.</span><span class="sxs-lookup"><span data-stu-id="cf851-137">A classic deployment is not supported by the just in time solution.</span></span>
  - <span data-ttu-id="cf851-138">Druhá - virtuální počítač je v této kategorii Pokud právě v čase řešení vypnutý v zásadách zabezpečení předplatné nebo skupinu prostředků nebo že virtuálního počítače chybí veřejnou IP adresu a nemá skupinu NSG na místě.</span><span class="sxs-lookup"><span data-stu-id="cf851-138">Other - A VM is in this category if the just in time solution is turned off in the security policy of the subscription or the resource group, or that the VM is missing a public IP and doesn't have an NSG in place.</span></span>

## <a name="configuring-a-just-in-time-access-policy"></a><span data-ttu-id="cf851-139">Konfigurace jenom v zásadách přístupu čas</span><span class="sxs-lookup"><span data-stu-id="cf851-139">Configuring a just in time access policy</span></span>

<span data-ttu-id="cf851-140">Vyberte virtuální počítače, které chcete povolit:</span><span class="sxs-lookup"><span data-stu-id="cf851-140">To select the VMs that you want to enable:</span></span>

1. <span data-ttu-id="cf851-141">Na **těsně v čas virtuálních počítačů přístup** okně, vyberte **doporučeno** kartě.</span><span class="sxs-lookup"><span data-stu-id="cf851-141">On the **Just in time VM access** blade, select the **Recommended** tab.</span></span>

  ![Povolit přístup k časovému][3]

2. <span data-ttu-id="cf851-143">V části **virtuální počítače**, vyberte virtuální počítače, které chcete povolit.</span><span class="sxs-lookup"><span data-stu-id="cf851-143">Under **VMs**, select the VMs that you want to enable.</span></span> <span data-ttu-id="cf851-144">To převádí zatržení vedle virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cf851-144">This puts a checkmark next to a VM.</span></span>
3. <span data-ttu-id="cf851-145">Vyberte **povolení JIT na virtuálních počítačích**.</span><span class="sxs-lookup"><span data-stu-id="cf851-145">Select **Enable JIT on VMs**.</span></span>
4. <span data-ttu-id="cf851-146">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cf851-146">Select **Save**.</span></span>

### <a name="default-ports"></a><span data-ttu-id="cf851-147">Výchozí porty</span><span class="sxs-lookup"><span data-stu-id="cf851-147">Default ports</span></span>

<span data-ttu-id="cf851-148">Můžete zobrazit výchozí porty, které Security Center doporučuje povolení jenom v čase.</span><span class="sxs-lookup"><span data-stu-id="cf851-148">You can see the default ports that Security Center recommends enabling just in time.</span></span>

1. <span data-ttu-id="cf851-149">Na **těsně v čas virtuálních počítačů přístup** okně, vyberte **doporučeno** kartě.</span><span class="sxs-lookup"><span data-stu-id="cf851-149">On the **Just in time VM access** blade, select the **Recommended** tab.</span></span>

  ![Zobrazit výchozí porty][6]

2. <span data-ttu-id="cf851-151">V části **virtuální počítače**, vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="cf851-151">Under **VMs**, select a VM.</span></span> <span data-ttu-id="cf851-152">To vloží zatržení vedle virtuálního počítače a otevře **JIT konfiguraci přístupu** okno.</span><span class="sxs-lookup"><span data-stu-id="cf851-152">This puts a checkmark next to the VM and opens the **JIT VM access configuration** blade.</span></span> <span data-ttu-id="cf851-153">Toto okno se zobrazí výchozí porty.</span><span class="sxs-lookup"><span data-stu-id="cf851-153">This blade displays the default ports.</span></span>

### <a name="add-ports"></a><span data-ttu-id="cf851-154">Přidat porty</span><span class="sxs-lookup"><span data-stu-id="cf851-154">Add ports</span></span>

<span data-ttu-id="cf851-155">Z **JIT konfiguraci přístupu** okno, můžete také přidávat a konfigurovat nový port, na kterém chcete povolit právě v době řešení.</span><span class="sxs-lookup"><span data-stu-id="cf851-155">From the **JIT VM access configuration** blade, you can also add and configure a new port on which you want to enable the just in time solution.</span></span>

1. <span data-ttu-id="cf851-156">Na **JIT konfiguraci přístupu** vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="cf851-156">On the **JIT VM access configuration** blade, select **Add**.</span></span> <span data-ttu-id="cf851-157">Tím se otevře **konfiguraci portů přidat** okno.</span><span class="sxs-lookup"><span data-stu-id="cf851-157">This opens the **Add port configuration** blade.</span></span>

  ![Konfigurace portu][7]

2. <span data-ttu-id="cf851-159">Na **konfiguraci portů přidat** okně identifikaci portů, typ protokolu zdrojové adresy IP a maximální doba požadavku povolena.</span><span class="sxs-lookup"><span data-stu-id="cf851-159">On **Add port configuration** blade, you identify the port, protocol type, allowed source IPs, and maximum request time.</span></span>

  <span data-ttu-id="cf851-160">Povolené zdrojové adresy IP jsou rozsahy IP moct získat přístup na schválené žádost.</span><span class="sxs-lookup"><span data-stu-id="cf851-160">Allowed source IPs are the IP ranges allowed to get access upon an approved request.</span></span>

  <span data-ttu-id="cf851-161">Doba požadavku maximální je maximální časový interval, může být otevřena specifického portu.</span><span class="sxs-lookup"><span data-stu-id="cf851-161">Maximum request time is the maximum time window that a specific port can be opened.</span></span>

3. <span data-ttu-id="cf851-162">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="cf851-162">Select **OK**.</span></span>

## <a name="requesting-access-to-a-vm"></a><span data-ttu-id="cf851-163">Žádají o přístup do virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cf851-163">Requesting access to a VM</span></span>

<span data-ttu-id="cf851-164">Chcete-li požádat o přístup k virtuálnímu počítači:</span><span class="sxs-lookup"><span data-stu-id="cf851-164">To request access to a VM:</span></span>

1. <span data-ttu-id="cf851-165">Na **těsně v čas virtuálních počítačů přístup** okně, vyberte **konfigurovaná** kartě.</span><span class="sxs-lookup"><span data-stu-id="cf851-165">On the **Just in time VM access** blade, select the **Configured** tab.</span></span>
2. <span data-ttu-id="cf851-166">V části **virtuální počítače**, vyberte virtuální počítače, které chcete povolit přístup.</span><span class="sxs-lookup"><span data-stu-id="cf851-166">Under **VMs**, select the VMs that you want to enable access.</span></span> <span data-ttu-id="cf851-167">To převádí zatržení vedle virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cf851-167">This puts a checkmark next to a VM.</span></span>
3. <span data-ttu-id="cf851-168">Vyberte **požádat o přístup**.</span><span class="sxs-lookup"><span data-stu-id="cf851-168">Select **Request access**.</span></span> <span data-ttu-id="cf851-169">Tím se otevře **požádat o přístup** okno.</span><span class="sxs-lookup"><span data-stu-id="cf851-169">This opens the **Request access** blade.</span></span>

  ![Požádat o přístup k virtuálnímu počítači][4]

4. <span data-ttu-id="cf851-171">Na **požádat o přístup** okno, můžete nakonfigurovat pro každý virtuální počítač portech, které spolu s zdrojové IP adresy, který se otevře port pro a časový interval, pro kterou je otevřen port.</span><span class="sxs-lookup"><span data-stu-id="cf851-171">On the **Request access** blade, you configure for each VM the ports to open along with the source IP that the port is opened to and the time window for which the port is opened.</span></span> <span data-ttu-id="cf851-172">Může vyžádat přístup pouze k porty, které jsou nakonfigurované v jenom v zásadách čas.</span><span class="sxs-lookup"><span data-stu-id="cf851-172">You can request access only to the ports that are configured in the just in time policy.</span></span> <span data-ttu-id="cf851-173">Každý z portů je maximální povolená doba odvozené od jenom v zásadách čas.</span><span class="sxs-lookup"><span data-stu-id="cf851-173">Each port has a maximum allowed time derived from the just in time policy.</span></span>
5. <span data-ttu-id="cf851-174">Vyberte **otevřít porty**.</span><span class="sxs-lookup"><span data-stu-id="cf851-174">Select **Open ports**.</span></span>

## <a name="editing-a-just-in-time-access-policy"></a><span data-ttu-id="cf851-175">Úpravy jenom v zásadách přístupu čas</span><span class="sxs-lookup"><span data-stu-id="cf851-175">Editing a just in time access policy</span></span>

<span data-ttu-id="cf851-176">Můžete změnit Virtuálního počítače existující jenom v zásadách čas přidáním a konfigurace nový port otevřete tohoto virtuálního počítače nebo změnou druhý parametr, související s již chráněné portu.</span><span class="sxs-lookup"><span data-stu-id="cf851-176">You can change a VM's existing just in time policy by adding and configuring a new port to open for that VM, or by changing any other parameter related to an already protected port.</span></span>

<span data-ttu-id="cf851-177">Chcete-li upravit stávající jenom v zásadách čas virtuálního počítače, **konfigurovaná** karta se používá:</span><span class="sxs-lookup"><span data-stu-id="cf851-177">In order to edit an existing just in time policy of a VM, the **Configured** tab is used:</span></span>

1. <span data-ttu-id="cf851-178">V části **virtuální počítače**, vyberte virtuální počítač port na přidáte kliknutím na tři tečky v řádku tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cf851-178">Under **VMs**, select a VM to add a port to by clicking on the three dots within the row for that VM.</span></span> <span data-ttu-id="cf851-179">Tím otevřete nabídku.</span><span class="sxs-lookup"><span data-stu-id="cf851-179">This opens a menu.</span></span>
2. <span data-ttu-id="cf851-180">Vyberte **upravit** v nabídce.</span><span class="sxs-lookup"><span data-stu-id="cf851-180">Select **Edit** in the menu.</span></span> <span data-ttu-id="cf851-181">Tím se otevře **JIT konfiguraci přístupu** okno.</span><span class="sxs-lookup"><span data-stu-id="cf851-181">This opens the **JIT VM access configuration** blade.</span></span>

  ![Upravit zásady][8]

3. <span data-ttu-id="cf851-183">Na **JIT konfiguraci přístupu** okno, můžete buď upravit existující nastavení, již chráněné portu kliknutím na její port, nebo můžete vybrat **přidat**.</span><span class="sxs-lookup"><span data-stu-id="cf851-183">On the **JIT VM access configuration** blade, you can either edit the existing settings of an already protected port by clicking on its port, or you can select **Add**.</span></span> <span data-ttu-id="cf851-184">Tím se otevře **konfiguraci portů přidat** okno.</span><span class="sxs-lookup"><span data-stu-id="cf851-184">This opens the **Add port configuration** blade.</span></span>

  ![Přidat na port][7]

4. <span data-ttu-id="cf851-186">Na **konfiguraci portů přidat** okno identifikovat port, typ protokolu, povolené zdrojové adresy IP a doba maximální požadavku.</span><span class="sxs-lookup"><span data-stu-id="cf851-186">On **Add port configuration** blade identify the port, protocol type, allowed source IPs, and maximum request time.</span></span>
5. <span data-ttu-id="cf851-187">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="cf851-187">Select **OK**.</span></span>
6. <span data-ttu-id="cf851-188">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cf851-188">Select **Save**.</span></span>

## <a name="auditing-just-in-time-access-activity"></a><span data-ttu-id="cf851-189">Auditování, právě aktivity přístup času.</span><span class="sxs-lookup"><span data-stu-id="cf851-189">Auditing just in time access activity</span></span>

<span data-ttu-id="cf851-190">Můžete získat přehledy aktivity virtuálního počítače pomocí protokolů hledání.</span><span class="sxs-lookup"><span data-stu-id="cf851-190">You can gain insights into VM activities using log search.</span></span> <span data-ttu-id="cf851-191">Pokud chcete zobrazit protokoly:</span><span class="sxs-lookup"><span data-stu-id="cf851-191">To view logs:</span></span>

1. <span data-ttu-id="cf851-192">Na **těsně v čas virtuálních počítačů přístup** okně, vyberte **konfigurovaná** kartě.</span><span class="sxs-lookup"><span data-stu-id="cf851-192">On the **Just in time VM access** blade, select the **Configured** tab.</span></span>
2. <span data-ttu-id="cf851-193">V části **virtuální počítače**, vyberte virtuální počítač k zobrazení informací o kliknutím na tři tečky v řádku tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cf851-193">Under **VMs**, select a VM to view information about by clicking on the three dots within the row for that VM.</span></span> <span data-ttu-id="cf851-194">Tím otevřete nabídku.</span><span class="sxs-lookup"><span data-stu-id="cf851-194">This opens a menu.</span></span>
3. <span data-ttu-id="cf851-195">Vyberte **protokol aktivit** v nabídce.</span><span class="sxs-lookup"><span data-stu-id="cf851-195">Select **Activity Log** in the menu.</span></span> <span data-ttu-id="cf851-196">Tím se otevře **protokol aktivit** okno.</span><span class="sxs-lookup"><span data-stu-id="cf851-196">This opens the **Activity log** blade.</span></span>

![Vyberte protokol aktivit][9]

<span data-ttu-id="cf851-198">**Protokol aktivit** okno poskytuje filtrované zobrazení předchozí operace pro tento virtuální počítač společně s čas, datum a předplatného.</span><span class="sxs-lookup"><span data-stu-id="cf851-198">The **Activity log** blade provides a filtered view of previous operations for that VM along with time, date, and subscription.</span></span>

![Protokol aktivit zobrazení][5]

<span data-ttu-id="cf851-200">Informace protokolu si můžete stáhnout tak, že vyberete **kliknutím sem stáhnete všechny položky jako sdílený svazek clusteru**.</span><span class="sxs-lookup"><span data-stu-id="cf851-200">You can download the log information by selecting **Click here to download all the items as CSV**.</span></span>

<span data-ttu-id="cf851-201">Upravit filtry a vyberte **použít** vytvořit vyhledávání a protokolu.</span><span class="sxs-lookup"><span data-stu-id="cf851-201">Modify the filters and select **Apply** to create a search and log.</span></span>

## <a name="using-just-in-time-vm-access-via-powershell"></a><span data-ttu-id="cf851-202">Pomocí jenom na dobu přístup virtuálních počítačů pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="cf851-202">Using just in time VM access via PowerShell</span></span>

<span data-ttu-id="cf851-203">Chcete-li použít jenom v době řešení pomocí prostředí PowerShell, zajistěte, aby byla [nejnovější](/powershell/azure/install-azurerm-ps) prostředí Azure PowerShell verze.</span><span class="sxs-lookup"><span data-stu-id="cf851-203">In order to use the just in time solution via PowerShell, make sure you have the [latest](/powershell/azure/install-azurerm-ps) Azure PowerShell version.</span></span>
<span data-ttu-id="cf851-204">Až to uděláte, musíte nainstalovat [nejnovější](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.12) Azure Security Center z Galerie prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cf851-204">Once you do, you need to install the [latest](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.12) Azure Security Center from the PowerShell gallery.</span></span>

### <a name="configuring-a-just-in-time-policy-for-a-vm"></a><span data-ttu-id="cf851-205">Konfigurace jenom v zásadách čas pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="cf851-205">Configuring a just in time policy for a VM</span></span>

<span data-ttu-id="cf851-206">Ke konfiguraci jenom v zásadách čas na konkrétní virtuální počítač, budete muset spustit tento příkaz v relaci prostředí PowerShell: Set-ASCJITAccessPolicy.</span><span class="sxs-lookup"><span data-stu-id="cf851-206">To configure a just in time policy on a specific VM, you need to run this command in your PowerShell session: Set-ASCJITAccessPolicy.</span></span>
<span data-ttu-id="cf851-207">Postupujte podle dokumentace rutiny Další informace.</span><span class="sxs-lookup"><span data-stu-id="cf851-207">Follow the cmdlet documentation to learn more.</span></span>

### <a name="requesting-access-to-a-vm"></a><span data-ttu-id="cf851-208">Žádají o přístup do virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cf851-208">Requesting access to a VM</span></span>

<span data-ttu-id="cf851-209">Pro přístup k konkrétní virtuální počítač, který je chráněný právě v doba řešení, budete muset spustit tento příkaz v relaci prostředí PowerShell: vyvolání ASCJITAccess.</span><span class="sxs-lookup"><span data-stu-id="cf851-209">To access a specific VM that is protected with the just in time solution, you need to run this command in your PowerShell session: Invoke-ASCJITAccess.</span></span>
<span data-ttu-id="cf851-210">Postupujte podle dokumentace rutiny Další informace.</span><span class="sxs-lookup"><span data-stu-id="cf851-210">Follow the cmdlet documentation to learn more.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cf851-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cf851-211">Next steps</span></span>
<span data-ttu-id="cf851-212">V tomto článku jste se dozvěděli, jak jenom na dobu přístup virtuálních počítačů v Security Center pomáhá, že můžete řídit přístup k virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="cf851-212">In this article, you learned how just in time VM access in Security Center helps you control access to your Azure virtual machines.</span></span>

<span data-ttu-id="cf851-213">Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:</span><span class="sxs-lookup"><span data-stu-id="cf851-213">To learn more about Security Center, see the following:</span></span>

- <span data-ttu-id="cf851-214">[Nastavení zásad zabezpečení](security-center-policies.md) – zjistěte, jak nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="cf851-214">[Setting security policies](security-center-policies.md) — Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
- <span data-ttu-id="cf851-215">[Správa doporučení zabezpečení](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="cf851-215">[Managing security recommendations](security-center-recommendations.md) — Learn how recommendations help you protect your Azure resources.</span></span>
- <span data-ttu-id="cf851-216">[Sledování stavu zabezpečení](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="cf851-216">[Security health monitoring](security-center-monitoring.md) — Learn how to monitor the health of your Azure resources.</span></span>
- <span data-ttu-id="cf851-217">[Správa a zpracování výstrah zabezpečení](security-center-managing-and-responding-alerts.md) – zjistěte, jak spravovat a reakce na výstrahy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="cf851-217">[Managing and responding to security alerts](security-center-managing-and-responding-alerts.md) — Learn how to manage and respond to security alerts.</span></span>
- <span data-ttu-id="cf851-218">[Sledování partnerských řešení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="cf851-218">[Monitoring partner solutions](security-center-partner-solutions.md) — Learn how to monitor the health status of your partner solutions.</span></span>
- <span data-ttu-id="cf851-219">[Security Center – nejčastější dotazy](security-center-faq.md) – přečtěte si nejčastější dotazy o použití této služby.</span><span class="sxs-lookup"><span data-stu-id="cf851-219">[Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using the service.</span></span>
- <span data-ttu-id="cf851-220">[Blog o zabezpečení Azure](https://blogs.msdn.microsoft.com/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="cf851-220">[Azure Security blog](https://blogs.msdn.microsoft.com/azuresecurity/) — Find blog posts about Azure security and compliance.</span></span>


<!--Image references-->
[1]: ./media/security-center-just-in-time/just-in-time-scenario.png
[2]: ./media/security-center-just-in-time/just-in-time.png
[3]: ./media/security-center-just-in-time/enable-just-in-time-access.png
[4]: ./media/security-center-just-in-time/request-access-to-a-vm.png
[5]: ./media/security-center-just-in-time/activity-log.png
[6]: ./media/security-center-just-in-time/default-ports.png
[7]: ./media/security-center-just-in-time/add-a-port.png
[8]: ./media/security-center-just-in-time/edit-policy.png
[9]: ./media/security-center-just-in-time/select-activity-log.png
