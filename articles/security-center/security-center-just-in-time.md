---
title: "přístup k aaaJust čas virtuální počítač v Azure Security Center | Microsoft Docs"
description: "Tento dokument vás provede procesem jak jenom na dobu přístup virtuálních počítačů v Azure Security Center pomáhá řídit přístup k tooyour Azure virtuální počítače."
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
ms.openlocfilehash: e6b58dd2c686cb227392b294e85914df5a546016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a><span data-ttu-id="bd95b-103">Spravovat přístup k virtuálním počítačům pomocí právě v čase</span><span class="sxs-lookup"><span data-stu-id="bd95b-103">Manage virtual machine access using just in time</span></span>

<span data-ttu-id="bd95b-104">Právě v čas virtuální počítač (VM) může být přístup používané toolock dolů tooyour příchozí přenosy virtuálních počítačích Azure, snižuje tooattacks ohrožení a poskytuje snadný přístup tooconnect tooVMs v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="bd95b-104">Just in time virtual machine (VM) access can be used toolock down inbound traffic tooyour Azure VMs, reducing exposure tooattacks while providing easy access tooconnect tooVMs when needed.</span></span>

> [!NOTE]
> <span data-ttu-id="bd95b-105">Hello pouze ve funkci čas je ve verzi preview a je k dispozici na hello úrovně Standard služby Security Center.</span><span class="sxs-lookup"><span data-stu-id="bd95b-105">hello just in time feature is in preview and available on hello Standard tier of Security Center.</span></span>  <span data-ttu-id="bd95b-106">V tématu [cenová](security-center-pricing.md) toolearn Další informace o Security Center je cenové úrovně.</span><span class="sxs-lookup"><span data-stu-id="bd95b-106">See [Pricing](security-center-pricing.md) toolearn more about Security Center's pricing tiers.</span></span>
>
>

## <a name="attack-scenario"></a><span data-ttu-id="bd95b-107">Scénář útoku</span><span class="sxs-lookup"><span data-stu-id="bd95b-107">Attack scenario</span></span>

<span data-ttu-id="bd95b-108">Útok hrubou silou útokům běžně cílové porty správy jako tooa přístup toogain prostředky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bd95b-108">Brute force attacks commonly target management ports as a means toogain access tooa VM.</span></span> <span data-ttu-id="bd95b-109">V případě úspěšného útočník může převzít kontrolu nad hello virtuálních počítačů a vytvořit dostane do vašeho prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd95b-109">If successful, an attacker can take control over hello VM and establish a foothold into your environment.</span></span>

<span data-ttu-id="bd95b-110">Útoku hrubou silou tooa ohrožení jedním ze způsobů tooreduce je toolimit hello množství času, který je otevřený port.</span><span class="sxs-lookup"><span data-stu-id="bd95b-110">One way tooreduce exposure tooa brute force attack is toolimit hello amount of time that a port is open.</span></span> <span data-ttu-id="bd95b-111">Porty pro správu to není nutné toobe otevřete za všech okolností.</span><span class="sxs-lookup"><span data-stu-id="bd95b-111">Management ports do not need toobe open at all times.</span></span> <span data-ttu-id="bd95b-112">Potřebují toobe otevřete při jsou připojené toohello virtuálních počítačů, například tooperform úkolů údržby nebo správy.</span><span class="sxs-lookup"><span data-stu-id="bd95b-112">They only need toobe open while you are connected toohello VM, for example tooperform management or maintenance tasks.</span></span> <span data-ttu-id="bd95b-113">Pokud právě v čase je povoleno, Security Center používá [skupinu zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) pravidla (NSG), které omezují přístup toomanagement porty, takže nemůžou být cílem útočníků.</span><span class="sxs-lookup"><span data-stu-id="bd95b-113">When just in time is enabled, Security Center uses [Network Security Group](../virtual-network/virtual-networks-nsg.md) (NSG) rules, which restrict access toomanagement ports so they cannot be targeted by attackers.</span></span>

![Jenom v případě čas][1]

## <a name="how-does-just-in-time-access-work"></a><span data-ttu-id="bd95b-115">Jak jenom při přístup k časovému funguje?</span><span class="sxs-lookup"><span data-stu-id="bd95b-115">How does just in time access work?</span></span>

<span data-ttu-id="bd95b-116">Pokud právě v čase je povoleno, Security Center zamyká tooyour příchozí přenosy virtuálních počítačů Azure ve vytvoření pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="bd95b-116">When just in time is enabled, Security Center locks down inbound traffic tooyour Azure VMs by creating an NSG rule.</span></span> <span data-ttu-id="bd95b-117">Vybrat hello porty na toowhich hello virtuálních počítačů bude uzamčené příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="bd95b-117">You select hello ports on hello VM toowhich inbound traffic will be locked down.</span></span> <span data-ttu-id="bd95b-118">Tyto porty jsou řízeny hello pouze v době řešení.</span><span class="sxs-lookup"><span data-stu-id="bd95b-118">These ports are controlled by hello just in time solution.</span></span>

<span data-ttu-id="bd95b-119">Když si uživatel vyžádá tooa přístup virtuálních počítačů, Security Center kontroluje má tento uživatel hello [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md) oprávnění, které poskytují přístup pro zápis pro hello prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="bd95b-119">When a user requests access tooa VM, Security Center checks that hello user has [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) permissions that provide write access for hello Azure resource.</span></span> <span data-ttu-id="bd95b-120">Pokud mají oprávnění k zápisu, hello žádost se schválí a Security Center automaticky nakonfiguruje hello skupiny zabezpečení sítě (Nsg) tooallow příchozí provoz toohello správu porty pro hello množství času, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="bd95b-120">If they have write permissions, hello request is approved and Security Center automatically configures hello Network Security Groups (NSGs) tooallow inbound traffic toohello management ports for hello amount of time you specified.</span></span> <span data-ttu-id="bd95b-121">Po vypršení doby hello Security Center obnoví předchozí stavy tootheir hello skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="bd95b-121">After hello time has expired, Security Center restores hello NSGs tootheir previous states.</span></span>

> [!NOTE]
> <span data-ttu-id="bd95b-122">Security Center, které jsou právě čas virtuálních počítačů přístup aktuálně podporuje pouze virtuální počítače nasazené prostřednictvím Správce Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="bd95b-122">Security Center just in time VM access currently supports only VMs deployed through Azure Resource Manager.</span></span> <span data-ttu-id="bd95b-123">toolearn informace o modelu nasazení Resource Manager i hello classic najdete [Azure Resource Manager oproti nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="bd95b-123">toolearn more about hello classic and Resource Manager deployment models see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>
>
>

## <a name="using-just-in-time-access"></a><span data-ttu-id="bd95b-124">Použití pouze v přístup k časovému</span><span class="sxs-lookup"><span data-stu-id="bd95b-124">Using just in time access</span></span>

<span data-ttu-id="bd95b-125">Hello **těsně v čas virtuálních počítačů přístup** na hello dlaždici **Security Center** se zobrazí okno hello počet virtuálních počítačů, které jsou nakonfigurované pro právě v čas přístupu a hello číslo požadavků na přístup schválené provedené v hello minulého týdne.</span><span class="sxs-lookup"><span data-stu-id="bd95b-125">hello **Just in time VM access** tile on hello **Security Center** blade shows hello number of VMs configured for just in time access and hello number of approved access requests made in hello last week.</span></span>

<span data-ttu-id="bd95b-126">Vyberte hello **těsně v čas virtuálních počítačů přístup** dlaždice a hello **těsně v čas virtuálních počítačů přístup** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="bd95b-126">Select hello **Just in time VM access** tile and hello **Just in time VM access** blade opens.</span></span>

![Právě v čase virtuální počítač přístup k dlaždici][2]

<span data-ttu-id="bd95b-128">Hello **těsně v čas virtuálních počítačů přístup** okno poskytuje informace o stavu hello virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="bd95b-128">hello **Just in time VM access** blade provides information on hello state of your VMs:</span></span>

- <span data-ttu-id="bd95b-129">**Nakonfigurované** – virtuální počítače, které byly nakonfigurované toosupport jenom při přístup k časovému virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bd95b-129">**Configured** - VMs that have been configured toosupport just in time VM access.</span></span> <span data-ttu-id="bd95b-130">Hello data uvedená pro hello minulého týdne a zahrnuje pro každý virtuální počítač hello číslo schválené žádosti, datum posledního přístupu a čas posledního uživatele.</span><span class="sxs-lookup"><span data-stu-id="bd95b-130">hello data presented is for hello last week and includes for each VM hello number of approved requests, last access date and time, and last user.</span></span>
- <span data-ttu-id="bd95b-131">**Doporučená** -virtuálních počítačů, které může podporovat jenom v přístup k časovému virtuálních počítačů, ale nebyly nakonfigurovány na.</span><span class="sxs-lookup"><span data-stu-id="bd95b-131">**Recommended** - VMs that can support just in time VM access but have not been configured to.</span></span> <span data-ttu-id="bd95b-132">Doporučujeme povolit jenom při řízení přístupu čas virtuálních počítačů pro tyto virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="bd95b-132">We recommend that you enable just in time VM access control for these VMs.</span></span> <span data-ttu-id="bd95b-133">V tématu [Povolit jenom v čas virtuálních počítačů přístup](#enable-just-in-time-vm-access).</span><span class="sxs-lookup"><span data-stu-id="bd95b-133">See [Enable just in time VM access](#enable-just-in-time-vm-access).</span></span>
- <span data-ttu-id="bd95b-134">**Žádná doporučení** -důvody, které můžou způsobit, že virtuální počítač není toobe doporučená jsou:</span><span class="sxs-lookup"><span data-stu-id="bd95b-134">**No recommendation** - Reasons that can cause a VM not toobe recommended are:</span></span>
  - <span data-ttu-id="bd95b-135">Chybí NSG - hello pouze v době řešení vyžaduje toobe NSG na místě.</span><span class="sxs-lookup"><span data-stu-id="bd95b-135">Missing NSG - hello just in time solution requires an NSG toobe in place.</span></span>
  - <span data-ttu-id="bd95b-136">Classic virtuálního počítače – Security Center, které jsou právě čas virtuálních počítačů přístup aktuálně podporuje pouze virtuální počítače nasazené prostřednictvím Správce Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="bd95b-136">Classic VM - Security Center just in time VM access currently supports only VMs deployed through Azure Resource Manager.</span></span> <span data-ttu-id="bd95b-137">Nasazení classic nepodporuje hello pouze v době řešení.</span><span class="sxs-lookup"><span data-stu-id="bd95b-137">A classic deployment is not supported by hello just in time solution.</span></span>
  - <span data-ttu-id="bd95b-138">Druhá - virtuální počítač je v této kategorii Pokud hello jenom na dobu, kterou řešení vypnutý v zásadách zabezpečení hello hello předplatné nebo skupinu prostředků hello nebo že hello virtuálních počítačů chybí veřejnou IP adresu a nemá skupinu NSG na místě.</span><span class="sxs-lookup"><span data-stu-id="bd95b-138">Other - A VM is in this category if hello just in time solution is turned off in hello security policy of hello subscription or hello resource group, or that hello VM is missing a public IP and doesn't have an NSG in place.</span></span>

## <a name="configuring-a-just-in-time-access-policy"></a><span data-ttu-id="bd95b-139">Konfigurace jenom v zásadách přístupu čas</span><span class="sxs-lookup"><span data-stu-id="bd95b-139">Configuring a just in time access policy</span></span>

<span data-ttu-id="bd95b-140">tooselect hello virtuálních počítačů, které chcete tooenable:</span><span class="sxs-lookup"><span data-stu-id="bd95b-140">tooselect hello VMs that you want tooenable:</span></span>

1. <span data-ttu-id="bd95b-141">Na hello **těsně v čas virtuálních počítačů přístup** okně, vyberte hello **doporučeno** kartě.</span><span class="sxs-lookup"><span data-stu-id="bd95b-141">On hello **Just in time VM access** blade, select hello **Recommended** tab.</span></span>

  ![Povolit přístup k časovému][3]

2. <span data-ttu-id="bd95b-143">V části **virtuální počítače**, vyberte hello virtuálních počítačů, které chcete tooenable.</span><span class="sxs-lookup"><span data-stu-id="bd95b-143">Under **VMs**, select hello VMs that you want tooenable.</span></span> <span data-ttu-id="bd95b-144">To umístí další tooa zaškrtnutí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bd95b-144">This puts a checkmark next tooa VM.</span></span>
3. <span data-ttu-id="bd95b-145">Vyberte **povolení JIT na virtuálních počítačích**.</span><span class="sxs-lookup"><span data-stu-id="bd95b-145">Select **Enable JIT on VMs**.</span></span>
4. <span data-ttu-id="bd95b-146">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bd95b-146">Select **Save**.</span></span>

### <a name="default-ports"></a><span data-ttu-id="bd95b-147">Výchozí porty</span><span class="sxs-lookup"><span data-stu-id="bd95b-147">Default ports</span></span>

<span data-ttu-id="bd95b-148">Můžete zobrazit hello výchozí porty, které Security Center doporučuje povolení jenom v čase.</span><span class="sxs-lookup"><span data-stu-id="bd95b-148">You can see hello default ports that Security Center recommends enabling just in time.</span></span>

1. <span data-ttu-id="bd95b-149">Na hello **těsně v čas virtuálních počítačů přístup** okně, vyberte hello **doporučeno** kartě.</span><span class="sxs-lookup"><span data-stu-id="bd95b-149">On hello **Just in time VM access** blade, select hello **Recommended** tab.</span></span>

  ![Zobrazit výchozí porty][6]

2. <span data-ttu-id="bd95b-151">V části **virtuální počítače**, vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="bd95b-151">Under **VMs**, select a VM.</span></span> <span data-ttu-id="bd95b-152">To umístí značku zaškrtnutí další toohello virtuálních počítačů a otevře se okno hello **JIT konfiguraci přístupu** okno.</span><span class="sxs-lookup"><span data-stu-id="bd95b-152">This puts a checkmark next toohello VM and opens hello **JIT VM access configuration** blade.</span></span> <span data-ttu-id="bd95b-153">Toto okno zobrazuje hello výchozí porty.</span><span class="sxs-lookup"><span data-stu-id="bd95b-153">This blade displays hello default ports.</span></span>

### <a name="add-ports"></a><span data-ttu-id="bd95b-154">Přidat porty</span><span class="sxs-lookup"><span data-stu-id="bd95b-154">Add ports</span></span>

<span data-ttu-id="bd95b-155">Z hello **JIT konfiguraci přístupu** okno, můžete také přidávat a konfigurovat nový port, na kterém chcete tooenable hello pouze v době řešení.</span><span class="sxs-lookup"><span data-stu-id="bd95b-155">From hello **JIT VM access configuration** blade, you can also add and configure a new port on which you want tooenable hello just in time solution.</span></span>

1. <span data-ttu-id="bd95b-156">Na hello **JIT konfiguraci přístupu** vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="bd95b-156">On hello **JIT VM access configuration** blade, select **Add**.</span></span> <span data-ttu-id="bd95b-157">Tím se otevře hello **konfiguraci portů přidat** okno.</span><span class="sxs-lookup"><span data-stu-id="bd95b-157">This opens hello **Add port configuration** blade.</span></span>

  ![Konfigurace portu][7]

2. <span data-ttu-id="bd95b-159">Na **konfiguraci portů přidat** okně identifikovat hello port, protokol typ zdrojové adresy IP a maximální doba požadavku povolená.</span><span class="sxs-lookup"><span data-stu-id="bd95b-159">On **Add port configuration** blade, you identify hello port, protocol type, allowed source IPs, and maximum request time.</span></span>

  <span data-ttu-id="bd95b-160">Povolené zdrojové adresy IP jsou hello IP rozsahů povolených tooget přístupu při schválené žádosti.</span><span class="sxs-lookup"><span data-stu-id="bd95b-160">Allowed source IPs are hello IP ranges allowed tooget access upon an approved request.</span></span>

  <span data-ttu-id="bd95b-161">Doba požadavku maximální je hello maximální časový interval, může být otevřena specifického portu.</span><span class="sxs-lookup"><span data-stu-id="bd95b-161">Maximum request time is hello maximum time window that a specific port can be opened.</span></span>

3. <span data-ttu-id="bd95b-162">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="bd95b-162">Select **OK**.</span></span>

## <a name="requesting-access-tooa-vm"></a><span data-ttu-id="bd95b-163">Požaduje přístup tooa virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="bd95b-163">Requesting access tooa VM</span></span>

<span data-ttu-id="bd95b-164">toorequest přístup tooa virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="bd95b-164">toorequest access tooa VM:</span></span>

1. <span data-ttu-id="bd95b-165">Na hello **těsně v čas virtuálních počítačů přístup** okně, vyberte hello **konfigurovaná** kartě.</span><span class="sxs-lookup"><span data-stu-id="bd95b-165">On hello **Just in time VM access** blade, select hello **Configured** tab.</span></span>
2. <span data-ttu-id="bd95b-166">V části **virtuální počítače**, vyberte hello virtuální počítače, který má přístup tooenable.</span><span class="sxs-lookup"><span data-stu-id="bd95b-166">Under **VMs**, select hello VMs that you want tooenable access.</span></span> <span data-ttu-id="bd95b-167">To umístí další tooa zaškrtnutí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bd95b-167">This puts a checkmark next tooa VM.</span></span>
3. <span data-ttu-id="bd95b-168">Vyberte **požádat o přístup**.</span><span class="sxs-lookup"><span data-stu-id="bd95b-168">Select **Request access**.</span></span> <span data-ttu-id="bd95b-169">Tím se otevře hello **požádat o přístup** okno.</span><span class="sxs-lookup"><span data-stu-id="bd95b-169">This opens hello **Request access** blade.</span></span>

  ![Žádost o přístup tooa virtuálních počítačů][4]

4. <span data-ttu-id="bd95b-171">Na hello **požádat o přístup** okně nakonfigurovat pro každý virtuální počítač hello porty tooopen společně s hello zdrojovou IP, který hello port je otevřít tooand hello časový interval, pro které hello je otevřen port.</span><span class="sxs-lookup"><span data-stu-id="bd95b-171">On hello **Request access** blade, you configure for each VM hello ports tooopen along with hello source IP that hello port is opened tooand hello time window for which hello port is opened.</span></span> <span data-ttu-id="bd95b-172">Může požádat o přístup pouze toohello porty, které jsou nakonfigurované v hello jenom v zásadách čas.</span><span class="sxs-lookup"><span data-stu-id="bd95b-172">You can request access only toohello ports that are configured in hello just in time policy.</span></span> <span data-ttu-id="bd95b-173">Každý z portů je maximální povolená doba odvozené od hello jenom v zásadách čas.</span><span class="sxs-lookup"><span data-stu-id="bd95b-173">Each port has a maximum allowed time derived from hello just in time policy.</span></span>
5. <span data-ttu-id="bd95b-174">Vyberte **otevřít porty**.</span><span class="sxs-lookup"><span data-stu-id="bd95b-174">Select **Open ports**.</span></span>

## <a name="editing-a-just-in-time-access-policy"></a><span data-ttu-id="bd95b-175">Úpravy jenom v zásadách přístupu čas</span><span class="sxs-lookup"><span data-stu-id="bd95b-175">Editing a just in time access policy</span></span>

<span data-ttu-id="bd95b-176">Virtuálního počítače existující jenom v zásadách čas přidání a konfigurace nové tooopen portu pro tento virtuální počítač, můžete změnit nebo změnou druhý parametr související tooan už chráněný portu.</span><span class="sxs-lookup"><span data-stu-id="bd95b-176">You can change a VM's existing just in time policy by adding and configuring a new port tooopen for that VM, or by changing any other parameter related tooan already protected port.</span></span>

<span data-ttu-id="bd95b-177">V pořadí tooedit existující jenom v zásadách čas virtuálního počítače, hello **konfigurovaná** karta se používá:</span><span class="sxs-lookup"><span data-stu-id="bd95b-177">In order tooedit an existing just in time policy of a VM, hello **Configured** tab is used:</span></span>

1. <span data-ttu-id="bd95b-178">V části **virtuální počítače**, vyberte virtuální počítač tooadd port tooby, kliknutím na hello tři tečky v rámci hello řádek tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bd95b-178">Under **VMs**, select a VM tooadd a port tooby clicking on hello three dots within hello row for that VM.</span></span> <span data-ttu-id="bd95b-179">Tím otevřete nabídku.</span><span class="sxs-lookup"><span data-stu-id="bd95b-179">This opens a menu.</span></span>
2. <span data-ttu-id="bd95b-180">Vyberte **upravit** v nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="bd95b-180">Select **Edit** in hello menu.</span></span> <span data-ttu-id="bd95b-181">Tím se otevře hello **JIT konfiguraci přístupu** okno.</span><span class="sxs-lookup"><span data-stu-id="bd95b-181">This opens hello **JIT VM access configuration** blade.</span></span>

  ![Upravit zásady][8]

3. <span data-ttu-id="bd95b-183">Na hello **JIT konfiguraci přístupu** okně hello existující nastavení už chráněný portu můžete buď upravit kliknutím na její port, nebo můžete vybrat **přidat**.</span><span class="sxs-lookup"><span data-stu-id="bd95b-183">On hello **JIT VM access configuration** blade, you can either edit hello existing settings of an already protected port by clicking on its port, or you can select **Add**.</span></span> <span data-ttu-id="bd95b-184">Tím se otevře hello **konfiguraci portů přidat** okno.</span><span class="sxs-lookup"><span data-stu-id="bd95b-184">This opens hello **Add port configuration** blade.</span></span>

  ![Přidat na port][7]

4. <span data-ttu-id="bd95b-186">Na **konfiguraci portů přidat** okno identifikovat hello port, typ protokolu, povolené zdrojové adresy IP a doba maximální požadavku.</span><span class="sxs-lookup"><span data-stu-id="bd95b-186">On **Add port configuration** blade identify hello port, protocol type, allowed source IPs, and maximum request time.</span></span>
5. <span data-ttu-id="bd95b-187">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="bd95b-187">Select **OK**.</span></span>
6. <span data-ttu-id="bd95b-188">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bd95b-188">Select **Save**.</span></span>

## <a name="auditing-just-in-time-access-activity"></a><span data-ttu-id="bd95b-189">Auditování, právě aktivity přístup času.</span><span class="sxs-lookup"><span data-stu-id="bd95b-189">Auditing just in time access activity</span></span>

<span data-ttu-id="bd95b-190">Můžete získat přehledy aktivity virtuálního počítače pomocí protokolů hledání.</span><span class="sxs-lookup"><span data-stu-id="bd95b-190">You can gain insights into VM activities using log search.</span></span> <span data-ttu-id="bd95b-191">tooview protokoly:</span><span class="sxs-lookup"><span data-stu-id="bd95b-191">tooview logs:</span></span>

1. <span data-ttu-id="bd95b-192">Na hello **těsně v čas virtuálních počítačů přístup** okně, vyberte hello **konfigurovaná** kartě.</span><span class="sxs-lookup"><span data-stu-id="bd95b-192">On hello **Just in time VM access** blade, select hello **Configured** tab.</span></span>
2. <span data-ttu-id="bd95b-193">V části **virtuální počítače**, vyberte virtuální počítač tooview informací o kliknutím na hello tři tečky v rámci hello řádek tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bd95b-193">Under **VMs**, select a VM tooview information about by clicking on hello three dots within hello row for that VM.</span></span> <span data-ttu-id="bd95b-194">Tím otevřete nabídku.</span><span class="sxs-lookup"><span data-stu-id="bd95b-194">This opens a menu.</span></span>
3. <span data-ttu-id="bd95b-195">Vyberte **protokol aktivit** v nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="bd95b-195">Select **Activity Log** in hello menu.</span></span> <span data-ttu-id="bd95b-196">Tím se otevře hello **protokol aktivit** okno.</span><span class="sxs-lookup"><span data-stu-id="bd95b-196">This opens hello **Activity log** blade.</span></span>

![Vyberte protokol aktivit][9]

<span data-ttu-id="bd95b-198">Hello **protokol aktivit** okno poskytuje filtrované zobrazení předchozí operace pro tento virtuální počítač společně s čas, datum a předplatného.</span><span class="sxs-lookup"><span data-stu-id="bd95b-198">hello **Activity log** blade provides a filtered view of previous operations for that VM along with time, date, and subscription.</span></span>

![Protokol aktivit zobrazení][5]

<span data-ttu-id="bd95b-200">Informace protokolu hello si můžete stáhnout tak, že vyberete **kliknutím sem toodownload všechny položky hello jako sdílený svazek clusteru**.</span><span class="sxs-lookup"><span data-stu-id="bd95b-200">You can download hello log information by selecting **Click here toodownload all hello items as CSV**.</span></span>

<span data-ttu-id="bd95b-201">Upravit hello filtry a vyberte **použít** toocreate vyhledávání a protokolu.</span><span class="sxs-lookup"><span data-stu-id="bd95b-201">Modify hello filters and select **Apply** toocreate a search and log.</span></span>

## <a name="using-just-in-time-vm-access-via-powershell"></a><span data-ttu-id="bd95b-202">Pomocí jenom na dobu přístup virtuálních počítačů pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="bd95b-202">Using just in time VM access via PowerShell</span></span>

<span data-ttu-id="bd95b-203">V pořadí toouse hello právě v doba řešení pomocí prostředí PowerShell, zajistěte, aby byla hello [nejnovější](/powershell/azure/install-azurerm-ps) prostředí Azure PowerShell verze.</span><span class="sxs-lookup"><span data-stu-id="bd95b-203">In order toouse hello just in time solution via PowerShell, make sure you have hello [latest](/powershell/azure/install-azurerm-ps) Azure PowerShell version.</span></span>
<span data-ttu-id="bd95b-204">Až to uděláte, musíte tooinstall hello [nejnovější](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.12) Azure Security Center z Galerie prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="bd95b-204">Once you do, you need tooinstall hello [latest](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.12) Azure Security Center from hello PowerShell gallery.</span></span>

### <a name="configuring-a-just-in-time-policy-for-a-vm"></a><span data-ttu-id="bd95b-205">Konfigurace jenom v zásadách čas pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="bd95b-205">Configuring a just in time policy for a VM</span></span>

<span data-ttu-id="bd95b-206">tooconfigure jenom v zásadách čas na konkrétním virtuálním počítači, je nutné toorun tento příkaz v relaci prostředí PowerShell: Set-ASCJITAccessPolicy.</span><span class="sxs-lookup"><span data-stu-id="bd95b-206">tooconfigure a just in time policy on a specific VM, you need toorun this command in your PowerShell session: Set-ASCJITAccessPolicy.</span></span>
<span data-ttu-id="bd95b-207">Postupujte podle hello rutiny dokumentaci toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="bd95b-207">Follow hello cmdlet documentation toolearn more.</span></span>

### <a name="requesting-access-tooa-vm"></a><span data-ttu-id="bd95b-208">Požaduje přístup tooa virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="bd95b-208">Requesting access tooa VM</span></span>

<span data-ttu-id="bd95b-209">tooaccess konkrétní virtuální počítač, který je chráněný pomocí hello pouze v době řešení, musíte tento příkaz pro toorun v relaci prostředí PowerShell: vyvolání ASCJITAccess.</span><span class="sxs-lookup"><span data-stu-id="bd95b-209">tooaccess a specific VM that is protected with hello just in time solution, you need toorun this command in your PowerShell session: Invoke-ASCJITAccess.</span></span>
<span data-ttu-id="bd95b-210">Postupujte podle hello rutiny dokumentaci toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="bd95b-210">Follow hello cmdlet documentation toolearn more.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd95b-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bd95b-211">Next steps</span></span>
<span data-ttu-id="bd95b-212">V tomto článku jste se dozvěděli, jak jenom na dobu přístup virtuálních počítačů v Security Center pomáhá řídit přístup tooyour Azure virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="bd95b-212">In this article, you learned how just in time VM access in Security Center helps you control access tooyour Azure virtual machines.</span></span>

<span data-ttu-id="bd95b-213">toolearn Další informace o Security Center, najdete v části hello následující:</span><span class="sxs-lookup"><span data-stu-id="bd95b-213">toolearn more about Security Center, see hello following:</span></span>

- <span data-ttu-id="bd95b-214">[Nastavení zásad zabezpečení](security-center-policies.md) – Další informace jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="bd95b-214">[Setting security policies](security-center-policies.md) — Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
- <span data-ttu-id="bd95b-215">[Správa doporučení zabezpečení](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="bd95b-215">[Managing security recommendations](security-center-recommendations.md) — Learn how recommendations help you protect your Azure resources.</span></span>
- <span data-ttu-id="bd95b-216">[Sledování stavu zabezpečení](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="bd95b-216">[Security health monitoring](security-center-monitoring.md) — Learn how toomonitor hello health of your Azure resources.</span></span>
- <span data-ttu-id="bd95b-217">[Správa a zpracování výstrah toosecurity](security-center-managing-and-responding-alerts.md) – Další informace jak toomanage a reakce toosecurity výstrahy.</span><span class="sxs-lookup"><span data-stu-id="bd95b-217">[Managing and responding toosecurity alerts](security-center-managing-and-responding-alerts.md) — Learn how toomanage and respond toosecurity alerts.</span></span>
- <span data-ttu-id="bd95b-218">[Sledování partnerských řešení](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="bd95b-218">[Monitoring partner solutions](security-center-partner-solutions.md) — Learn how toomonitor hello health status of your partner solutions.</span></span>
- <span data-ttu-id="bd95b-219">[Security Center – nejčastější dotazy](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="bd95b-219">[Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using hello service.</span></span>
- <span data-ttu-id="bd95b-220">[Blog o zabezpečení Azure](https://blogs.msdn.microsoft.com/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="bd95b-220">[Azure Security blog](https://blogs.msdn.microsoft.com/azuresecurity/) — Find blog posts about Azure security and compliance.</span></span>


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
