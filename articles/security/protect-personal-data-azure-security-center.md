---
title: "aaaProtect osobních dat pomocí Azure Security Center | Microsoft Docs"
description: "Ochrana osobních dat pomocí Azure security center"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 8e2c893d13318392f47fa912089d52618f9e7b45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protect-personal-data-from-breaches-and-attacks-azure-security-center"></a><span data-ttu-id="81c17-103">Ochrana osobních údajů z narušení a útoky: Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="81c17-103">Protect personal data from breaches and attacks: Azure Security Center</span></span>

<span data-ttu-id="81c17-104">Tento článek vám pomůže pochopit, jak toouse Azure Security Center tooprotect osobních dat od poruší a útoky.</span><span class="sxs-lookup"><span data-stu-id="81c17-104">This article will help you understand how toouse Azure Security Center tooprotect personal data from breaches and attacks.</span></span>

## <a name="scenario"></a><span data-ttu-id="81c17-105">Scénář</span><span class="sxs-lookup"><span data-stu-id="81c17-105">Scenario</span></span> 

<span data-ttu-id="81c17-106">Velké výletních společnosti, centrálou v USA, hello je rozšířit jeho itineráře toooffer operace v hello Středozemního a baltský moři, jakož i hello Britské ostrovy.</span><span class="sxs-lookup"><span data-stu-id="81c17-106">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="81c17-107">toohelp přitom získala menší výletních Víceřádkový na základě v Itálii, Německo, Dánsko a hello Spojené království</span><span class="sxs-lookup"><span data-stu-id="81c17-107">toohelp in those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark, and hello U.K.</span></span>

<span data-ttu-id="81c17-108">Hello společnost používá Microsoft Azure toostore firemní data v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="81c17-108">hello company uses Microsoft Azure toostore corporate data in hello cloud.</span></span> <span data-ttu-id="81c17-109">To zahrnuje osobní identifikovatelné údaje, jako je například jména, adresy, telefonních čísel a informace o kreditní kartě.</span><span class="sxs-lookup"><span data-stu-id="81c17-109">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information.</span></span> <span data-ttu-id="81c17-110">Také obsahuje informace o lidských zdrojů, jako:</span><span class="sxs-lookup"><span data-stu-id="81c17-110">It also includes Human Resources information such as:</span></span>

- <span data-ttu-id="81c17-111">Adresy</span><span class="sxs-lookup"><span data-stu-id="81c17-111">Addresses</span></span>
- <span data-ttu-id="81c17-112">telefonní čísla</span><span class="sxs-lookup"><span data-stu-id="81c17-112">Phone numbers</span></span>
- <span data-ttu-id="81c17-113">daň identifikační čísla</span><span class="sxs-lookup"><span data-stu-id="81c17-113">Tax identification numbers</span></span>
- <span data-ttu-id="81c17-114">lékařské informace</span><span class="sxs-lookup"><span data-stu-id="81c17-114">Medical information</span></span>

<span data-ttu-id="81c17-115">Hello výletních řádku také udržuje velké databáze potřebu a věrnost program členů.</span><span class="sxs-lookup"><span data-stu-id="81c17-115">hello cruise line also maintains a large database of reward and loyalty program members.</span></span> <span data-ttu-id="81c17-116">Zaměstnanci společnosti přístupu hello síti ze vzdálených pobočkách a agenty cesta hello společnosti nachází kolem hello, world mít přístup k prostředkům společnosti toosome.</span><span class="sxs-lookup"><span data-stu-id="81c17-116">Corporate employees access hello network from hello company’s remote offices and travel agents located around hello world have access toosome company resources.</span></span>
<span data-ttu-id="81c17-117">Osobní data se přenáší po síti hello mezi těmito umístění a hello Microsoft datového centra.</span><span class="sxs-lookup"><span data-stu-id="81c17-117">Personal data travels across hello network between these locations and hello Microsoft data center.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="81c17-118">Popis problému</span><span class="sxs-lookup"><span data-stu-id="81c17-118">Problem statement</span></span>

<span data-ttu-id="81c17-119">Společnost Hello je zajímá hello riziko útoků na svých prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="81c17-119">hello company is concerned about hello threat of attacks on their Azure resources.</span></span> <span data-ttu-id="81c17-120">Chtějí tooprevent ohrožení zaměstnanců a zákazníků osob toounauthorized osobní data.</span><span class="sxs-lookup"><span data-stu-id="81c17-120">They want tooprevent exposure of customers’ and employees’ personal data toounauthorized persons.</span></span> <span data-ttu-id="81c17-121">Chtějí pokyny k zabránění a odpovědi nebo nápravná, jak efektivní způsob toomonitor hello probíhající zabezpečení cloudových prostředků.</span><span class="sxs-lookup"><span data-stu-id="81c17-121">They want guidance on both prevention and response/remediation, as well as an effective way toomonitor hello ongoing security of their cloud resources.</span></span>
<span data-ttu-id="81c17-122">Potřebují silné linií obrany před dnešní sofistikovaná a uspořádány útočníkům.</span><span class="sxs-lookup"><span data-stu-id="81c17-122">They need a strong line of defense against today’s sophisticated and organized attackers.</span></span>

## <a name="company-goal"></a><span data-ttu-id="81c17-123">Cílem společnosti</span><span class="sxs-lookup"><span data-stu-id="81c17-123">Company goal</span></span>

<span data-ttu-id="81c17-124">Jedním z cílů společnosti hello tooensure hello ochrany osobních údajů zaměstnanců a zákazníků osobních údajů je chrání před hrozbami.</span><span class="sxs-lookup"><span data-stu-id="81c17-124">One of hello company’s goals is tooensure hello privacy of customers’ and employees’ personal data by protecting it from threats.</span></span> <span data-ttu-id="81c17-125">Jedním ze svých cílů je toorespond okamžitě toosigns porušení toomitigate hello dopad.</span><span class="sxs-lookup"><span data-stu-id="81c17-125">One of their goals is toorespond immediately toosigns of breach toomitigate hello impact.</span></span> <span data-ttu-id="81c17-126">Vyžaduje způsob tooassess hello aktuální stav zabezpečení, identifikovat citlivé konfigurace a opravit.</span><span class="sxs-lookup"><span data-stu-id="81c17-126">It requires a way tooassess hello current state of security, identify vulnerable configurations, and remediate them.</span></span>

## <a name="solutions"></a><span data-ttu-id="81c17-127">Řešení</span><span class="sxs-lookup"><span data-stu-id="81c17-127">Solutions</span></span>

<span data-ttu-id="81c17-128">Microsoft Azure Security Center (ASC) poskytuje do řešení pro správu zásad a monitorování integrované zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="81c17-128">Microsoft Azure Security Center (ASC) provides an integrated security monitoring and policy management solution.</span></span> <span data-ttu-id="81c17-129">Zajišťuje threat snadno použitelné a efektivní funkce prevence, zjišťování a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="81c17-129">It delivers easy-to-use and effective threat prevention, detection, and response capabilities.</span></span>

### <a name="prevention"></a><span data-ttu-id="81c17-130">Prevention (Prevence)</span><span class="sxs-lookup"><span data-stu-id="81c17-130">Prevention</span></span>

<span data-ttu-id="81c17-131">ASC pomáhá zabránit narušení tím, že vám zásady zabezpečení tooset, poskytovat přístup za běhu a implementace doporučení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="81c17-131">ASC helps you prevent breaches by enabling you tooset security policies, provide just-in-time access, and implement security recommendations.</span></span>

<span data-ttu-id="81c17-132">Zásady zabezpečení definuje hello sadu ovládacích prvků, které jsou doporučené pro prostředky v rámci hello zadaný odběr.</span><span class="sxs-lookup"><span data-stu-id="81c17-132">A security policy defines hello set of controls recommended for resources within hello specified subscription.</span></span> <span data-ttu-id="81c17-133">Přístup jenom na dobu lze použít toolock dolů tooyour příchozí přenosy virtuálních počítačích Azure, snižuje tooattacks ohrožení.</span><span class="sxs-lookup"><span data-stu-id="81c17-133">Just in time access can be used toolock down inbound traffic tooyour Azure VMs, reducing exposure tooattacks.</span></span> <span data-ttu-id="81c17-134">Doporučení zabezpečení jsou vytvořené pomocí ASC po analýze hello stav zabezpečení vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="81c17-134">Security recommendations are created by ASC after analyzing hello security state of your Azure resources.</span></span>

#### <a name="how-do-i-set-security-policies-in-asc"></a><span data-ttu-id="81c17-135">Jak nastavit zásady zabezpečení v ASC?</span><span class="sxs-lookup"><span data-stu-id="81c17-135">How do I set security policies in ASC?</span></span>

<span data-ttu-id="81c17-136">Zásady zabezpečení můžete nakonfigurovat pro každé předplatné.</span><span class="sxs-lookup"><span data-stu-id="81c17-136">You can configure security policies for each subscription.</span></span> <span data-ttu-id="81c17-137">toomodify zásady zabezpečení, musí být roli vlastníka nebo přispěvatele daného předplatného.</span><span class="sxs-lookup"><span data-stu-id="81c17-137">toomodify a security policy, you must be an owner or contributor of that subscription.</span></span> <span data-ttu-id="81c17-138">V hello portálu Azure hello následující:</span><span class="sxs-lookup"><span data-stu-id="81c17-138">In hello Azure portal, do hello following:</span></span>

1. <span data-ttu-id="81c17-139">Vyberte **zásad** hello ASC řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="81c17-139">Select **Policy** in hello ASC dashboard.</span></span>

2. <span data-ttu-id="81c17-140">Vyberte předplatné hello, na kterém chcete tooenable hello zásady.</span><span class="sxs-lookup"><span data-stu-id="81c17-140">Select hello subscription on which you want tooenable hello policy.</span></span>

3. <span data-ttu-id="81c17-141">Zvolte **zásada Zabránění** tooconfigure zásady podle předplatného.</span><span class="sxs-lookup"><span data-stu-id="81c17-141">Choose **Prevention policy** tooconfigure policies per subscription.</span></span> <span data-ttu-id="81c17-142">**Shromažďování dat z virtuálních počítačů** by mělo být nastavené příliš**na.**</span><span class="sxs-lookup"><span data-stu-id="81c17-142">**Collect data from virtual machines** should be set too**On.**</span></span>

4. <span data-ttu-id="81c17-143">V hello **zásada Zabránění** možnosti, vyberte **na** tooenable hello zabezpečení doporučení, které jsou relevantní pro předplatné hello.</span><span class="sxs-lookup"><span data-stu-id="81c17-143">In hello **Prevention policy** options, select **On** tooenable hello security recommendations that are relevant for hello subscription.</span></span>

![](media/protect-personal-data-azure-security-center/prevention-policy.png)

<span data-ttu-id="81c17-144">Podrobné pokyny a vysvětlení jednotlivých doporučení hello zásady, které je možné povolit, najdete v části [nastavení zásad zabezpečení v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).</span><span class="sxs-lookup"><span data-stu-id="81c17-144">For more detailed instructions and an explanation of each of hello policy recommendations that can be enabled, see [Set security policies in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).</span></span>

#### <a name="how-do-i-configure-just-in-time-access-jit"></a><span data-ttu-id="81c17-145">Jak lze nakonfigurovat pouze v přístup čas (JIT)?</span><span class="sxs-lookup"><span data-stu-id="81c17-145">How do I configure Just in Time Access (JIT)?</span></span>

<span data-ttu-id="81c17-146">Pokud je povoleno JIT, Security Center zamyká tooyour příchozí přenosy virtuálních počítačů Azure ve vytvoření pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="81c17-146">When JIT is enabled, Security Center locks down inbound traffic tooyour Azure VMs by creating an NSG rule.</span></span> <span data-ttu-id="81c17-147">Vybrat hello porty na toowhich hello virtuálních počítačů bude uzamčené příchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="81c17-147">You select hello ports on hello VM toowhich inbound traffic will be locked down.</span></span> <span data-ttu-id="81c17-148">toouse JIT přístup, hello následující:</span><span class="sxs-lookup"><span data-stu-id="81c17-148">toouse JIT access, do hello following:</span></span>

1. <span data-ttu-id="81c17-149">Vyberte hello **těsně čas virtuálních počítačů přístup k dlaždici** v okně ASC hello.</span><span class="sxs-lookup"><span data-stu-id="81c17-149">Select hello **Just in time VM access tile** on hello ASC blade.</span></span>

2. <span data-ttu-id="81c17-150">Vyberte hello **doporučeno** kartě.</span><span class="sxs-lookup"><span data-stu-id="81c17-150">Select hello **Recommended** tab.</span></span>

3. <span data-ttu-id="81c17-151">V části **virtuální počítače**, vyberte hello virtuálních počítačů, které chcete tooenable.</span><span class="sxs-lookup"><span data-stu-id="81c17-151">Under **VMs**, select hello VMs that you want tooenable.</span></span> <span data-ttu-id="81c17-152">To umístí další tooa zaškrtnutí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="81c17-152">This puts a checkmark next tooa VM.</span></span> 
4. <span data-ttu-id="81c17-153">Vyberte **povolení JIT** na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="81c17-153">Select **Enable JIT** on VMs.</span></span>
5. <span data-ttu-id="81c17-154">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="81c17-154">Select **Save**.</span></span>

<span data-ttu-id="81c17-155">Potom můžete zobrazit hello výchozí porty, které doporučuje ASC povolený pro JIT.</span><span class="sxs-lookup"><span data-stu-id="81c17-155">Then you can see hello default ports that ASC recommends being enabled for JIT.</span></span> <span data-ttu-id="81c17-156">Můžete také přidávat a konfigurovat nový port, na kterém chcete tooenable hello pouze v době řešení.</span><span class="sxs-lookup"><span data-stu-id="81c17-156">You can also add and configure a new port on which you want tooenable hello just in time solution.</span></span> <span data-ttu-id="81c17-157">Hello **těsně v čas virtuálních počítačů přístup** dlaždice v hello Security Center zobrazuje hello počet virtuálních počítačů nakonfigurovaná pro přístup za běhu.</span><span class="sxs-lookup"><span data-stu-id="81c17-157">hello **Just in time VM access** tile in hello Security Center shows hello number of VMs configured for JIT access.</span></span> <span data-ttu-id="81c17-158">Také ukazuje hello počet požadavků schválené přístup provedené v hello minulého týdne.</span><span class="sxs-lookup"><span data-stu-id="81c17-158">It also shows hello number of approved access requests made in hello last week.</span></span>

![](media/protect-personal-data-azure-security-center/jit-vm.png)

<span data-ttu-id="81c17-159">Návod, jak toodo, a další informace o těsně v čas přístup, zobrazit [spravovat přístup k virtuálním počítačům pomocí právě v čase.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)</span><span class="sxs-lookup"><span data-stu-id="81c17-159">For instructions on how toodo this, and additional information about Just in Time access, see [Manage virtual machine access using just in time.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)</span></span>

#### <a name="how-do-i-implement-asc-security-recommendations"></a><span data-ttu-id="81c17-160">Jak implementace doporučení zabezpečení ASC?</span><span class="sxs-lookup"><span data-stu-id="81c17-160">How do I implement ASC security recommendations?</span></span>

<span data-ttu-id="81c17-161">Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří doporučení.</span><span class="sxs-lookup"><span data-stu-id="81c17-161">When Security Center identifies potential security vulnerabilities, it creates recommendations.</span></span> <span data-ttu-id="81c17-162">Hello doporučení vás provede procesem hello konfigurace hello potřebné ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="81c17-162">hello recommendations guide you through hello process of configuring hello needed controls.</span></span> 
1. <span data-ttu-id="81c17-163">Vyberte hello **doporučení** dlaždici na řídicím panelu ASC hello.</span><span class="sxs-lookup"><span data-stu-id="81c17-163">Select hello **Recommendations** tile on hello ASC dashboard.</span></span>

2. <span data-ttu-id="81c17-164">Zobrazit hello doporučení, které jsou uvedeny ve formátu tabulky, kde každý řádek představuje jeden doporučení.</span><span class="sxs-lookup"><span data-stu-id="81c17-164">View hello recommendations, which are shown in a table format where each line represents one recommendation.</span></span>

3. <span data-ttu-id="81c17-165">Vyberte toofilter doporučení **filtru** a vyberte hodnoty závažnosti a stavu hello chcete toosee.</span><span class="sxs-lookup"><span data-stu-id="81c17-165">toofilter recommendations, select **Filter** and select hello severity and state values you wish toosee.</span></span>

4. <span data-ttu-id="81c17-166">toodismiss doporučení, která se nedá použít, můžete klikněte pravým tlačítkem a vyberte **přeskočení.**</span><span class="sxs-lookup"><span data-stu-id="81c17-166">toodismiss a recommendation that is not applicable, you can right click and select **Dismiss.**</span></span>

5. <span data-ttu-id="81c17-167">Vyhodnoťte, které doporučení by měla být použije první.</span><span class="sxs-lookup"><span data-stu-id="81c17-167">Evaluate which recommendation should be applied first.</span></span>

6. <span data-ttu-id="81c17-168">Použijte hello doporučení v pořadí podle priority.</span><span class="sxs-lookup"><span data-stu-id="81c17-168">Apply hello recommendations in order of priority.</span></span>

<span data-ttu-id="81c17-169">Seznam možných doporučení a návodů na postupy najdete v části tooapply, [Správa doporučení zabezpečení v Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)</span><span class="sxs-lookup"><span data-stu-id="81c17-169">For a list of possible recommendations and walk-throughs on how tooapply each, see [Managing security recommendations in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)</span></span>

### <a name="detection-and-response"></a><span data-ttu-id="81c17-170">Detekce a reakce</span><span class="sxs-lookup"><span data-stu-id="81c17-170">Detection and Response</span></span>

<span data-ttu-id="81c17-171">Detekce a reakce přejděte společně, jak chcete toorespond co nejdříve po zjištěno ohrožení.</span><span class="sxs-lookup"><span data-stu-id="81c17-171">Detection and response go together, as you want toorespond as quickly as possible after a threat is detected.</span></span>
<span data-ttu-id="81c17-172">Detekce hrozeb ASC funguje tak, že automaticky shromažďování informací o zabezpečení z vaše prostředky Azure, sítě hello a připojených partnerských řešení.</span><span class="sxs-lookup"><span data-stu-id="81c17-172">ASC threat detection works by automatically collecting security information from your Azure resources, hello network, and connected partner solutions.</span></span> <span data-ttu-id="81c17-173">ASC můžete rychle aktualizovat své detekční algoritmy, protože útočníci vydání nové a stále sofistikované zneužití.</span><span class="sxs-lookup"><span data-stu-id="81c17-173">ASC can rapidly update its detection algorithms as attackers release new and increasingly sophisticated exploits.</span></span> <span data-ttu-id="81c17-174">Podrobnější informace o tom, jak funguje služba detekce hrozeb je ASC, najdete v části [funkce zjišťování služby Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).</span><span class="sxs-lookup"><span data-stu-id="81c17-174">For more detailed information on how ASC’s threat detection works, see [Azure Security Center detection capabilities](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).</span></span>

#### <a name="how-do-i-manage-and-respond-toosecurity-alerts"></a><span data-ttu-id="81c17-175">Jak spravovat a reagovat výstrahy toosecurity?</span><span class="sxs-lookup"><span data-stu-id="81c17-175">How do I manage and respond toosecurity alerts?</span></span>

<span data-ttu-id="81c17-176">Seznam upřednostňovaných výstrah zabezpečení se zobrazí v Centru zabezpečení společně s hello informace, které potřebujete tooquickly prozkoumat hello problém.</span><span class="sxs-lookup"><span data-stu-id="81c17-176">A list of prioritized security alerts is shown in Security Center along with hello information you need tooquickly investigate hello problem.</span></span> <span data-ttu-id="81c17-177">Security Center také obsahuje doporučení, jak tooremediate útoku.</span><span class="sxs-lookup"><span data-stu-id="81c17-177">Security Center also includes recommendations for how tooremediate an attack.</span></span> <span data-ttu-id="81c17-178">toomanage výstrahy zabezpečení, proveďte následující hello:</span><span class="sxs-lookup"><span data-stu-id="81c17-178">toomanage your security alerts, do hello following:</span></span>

1. <span data-ttu-id="81c17-179">Vyberte hello **výstrahy zabezpečení** dlaždici na řídicím panelu ASC hello.</span><span class="sxs-lookup"><span data-stu-id="81c17-179">Select hello **Security alerts** tile in hello ASC dashboard.</span></span> <span data-ttu-id="81c17-180">To obsahuje podrobnosti pro každou výstrahu.</span><span class="sxs-lookup"><span data-stu-id="81c17-180">This shows details for each alert.</span></span>

2. <span data-ttu-id="81c17-181">Vyberte podle data, stav a závažnost výstrahy toofilter **filtru** a potom vyberte hodnoty hello chcete toosee.</span><span class="sxs-lookup"><span data-stu-id="81c17-181">toofilter alerts based on date, state, and severity, select **Filter** and then select hello values you want toosee.</span></span>

3. <span data-ttu-id="81c17-182">Výstraha tooan toorespond, vyberte ho a zkontrolujte hello informace a potom vyberte hello prostředek, který byl napaden.</span><span class="sxs-lookup"><span data-stu-id="81c17-182">toorespond tooan alert, select it and review hello information, then select hello resource that was attacked.</span></span>

4. <span data-ttu-id="81c17-183">V hello **popis** poli se zobrazí podrobnosti, včetně doporučené nápravy.</span><span class="sxs-lookup"><span data-stu-id="81c17-183">In hello **Description** field, you’ll see details, including recommended remediation.</span></span>

![](media/protect-personal-data-azure-security-center/security-alerts.png)

<span data-ttu-id="81c17-184">Podrobné pokyny, odpovídá toosecurity výstrahy, najdete v části [správy a zda odpovídá toosecurity výstrahy v Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)</span><span class="sxs-lookup"><span data-stu-id="81c17-184">For more detailed instructions on responding toosecurity alerts, see [Managing and responding toosecurity alerts in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)</span></span>

<span data-ttu-id="81c17-185">Další informace v prošetřování výstrah zabezpečení hello společnosti může integrovat ASC výstrahy vlastní řešení SIEM pomocí [integrace se službou Azure protokolu](https://aka.ms/AzLog).</span><span class="sxs-lookup"><span data-stu-id="81c17-185">For further help in investigating security alerts, hello company can integrate ASC alerts with its own SIEM solution, using [Azure Log Integration](https://aka.ms/AzLog).</span></span>

#### <a name="how-do-i-manage-security-incidents"></a><span data-ttu-id="81c17-186">Jak lze spravovat incidenty zabezpečení?</span><span class="sxs-lookup"><span data-stu-id="81c17-186">How do I manage security incidents?</span></span>

<span data-ttu-id="81c17-187">V ASC je incidentu zabezpečení agregaci všechny výstrahy pro prostředek, které zarovnat kill řetězu vzory.</span><span class="sxs-lookup"><span data-stu-id="81c17-187">In ASC, a security incident is an aggregation of all alerts for a resource that align with kill chain patterns.</span></span> <span data-ttu-id="81c17-188">Další informace o každém výskytu bude hello seznamu související výstrahy, což umožňuje vám tooobtain odhalit incidentu.</span><span class="sxs-lookup"><span data-stu-id="81c17-188">An Incident will reveal hello list of related alerts, which enables you tooobtain more information about each occurrence.</span></span> <span data-ttu-id="81c17-189">Hello dlaždice výstrahy zabezpečení a okno zobrazí incidenty.</span><span class="sxs-lookup"><span data-stu-id="81c17-189">Incidents appear in hello Security Alerts tile and blade.</span></span>

<span data-ttu-id="81c17-190">tooreview a spravovat incidenty zabezpečení, hello následující:</span><span class="sxs-lookup"><span data-stu-id="81c17-190">tooreview and manage security incidents, do hello following:</span></span>

1. <span data-ttu-id="81c17-191">Vyberte hello **výstrahy zabezpečení** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="81c17-191">Select hello **Security alerts** tile.</span></span> <span data-ttu-id="81c17-192">Pokud je zjištěn bezpečnostního incidentu, zobrazí se v grafu výstrahy zabezpečení hello.</span><span class="sxs-lookup"><span data-stu-id="81c17-192">if a security incident is detected, it will appear under hello security alerts graph.</span></span> <span data-ttu-id="81c17-193">Bude mít ikonu, která se liší od dalších výstrah.</span><span class="sxs-lookup"><span data-stu-id="81c17-193">It will have an icon that’s different from other alerts.</span></span>

2. <span data-ttu-id="81c17-194">Vyberte hello incident toosee další podrobnosti o této incidentu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="81c17-194">Select hello incident toosee more details about this security incident.</span></span> <span data-ttu-id="81c17-195">Další podrobnosti jsou jeho úplný popis jeho závažnost, jeho aktuální stav, hello napadení prostředků, postup nápravy hello hello incident a hello výstrahy, které byly součástí tohoto incidentu.</span><span class="sxs-lookup"><span data-stu-id="81c17-195">Additional details include its full description, its severity, its current state, hello attacked resource, hello remediation steps for hello incident, and hello alerts that were included in this incident.</span></span>

<span data-ttu-id="81c17-196">Můžete filtrovat toosee **pouze incidenty**, **výstrahy pouze**, nebo **i**.</span><span class="sxs-lookup"><span data-stu-id="81c17-196">You can filter toosee **incidents only**, **alerts only**, or **both**.</span></span>

#### <a name="how-do-i-access-hello-threat-intelligence-report"></a><span data-ttu-id="81c17-197">Přístupu hello Threat Intelligence sestavy</span><span class="sxs-lookup"><span data-stu-id="81c17-197">How do I access hello Threat Intelligence Report?</span></span>

<span data-ttu-id="81c17-198">ASC analyzují informace z více zdrojů tooidentify hrozeb.</span><span class="sxs-lookup"><span data-stu-id="81c17-198">ASC analyzes information from multiple sources tooidentify threats.</span></span> <span data-ttu-id="81c17-199">reakce na incidenty týmy tooassist prozkoumat ohrožení a oprava, Security Center obsahuje sestavu intelligence hrozba, která obsahuje informace o hello hrozba, která byla zjištěna.</span><span class="sxs-lookup"><span data-stu-id="81c17-199">tooassist incident response teams investigate and remediate threats, Security Center includes a threat intelligence report that contains information about hello threat that was detected.</span></span>

<span data-ttu-id="81c17-200">Security Center má tři typy sestav hrozeb, které se může lišit na útok.</span><span class="sxs-lookup"><span data-stu-id="81c17-200">Security Center has three types of threat reports, which can vary per attack.</span></span>
<span data-ttu-id="81c17-201">jsou k dispozici Hello sestavy:</span><span class="sxs-lookup"><span data-stu-id="81c17-201">hello reports available are:</span></span>

- <span data-ttu-id="81c17-202">Sestava skupiny aktivit: obsahuje přímý dives do útočníci, jejich cíle a svoji.</span><span class="sxs-lookup"><span data-stu-id="81c17-202">Activity Group Report: provides deep dives into attackers, their objectives and tactics.</span></span>

- <span data-ttu-id="81c17-203">Sestava kampaně: zaměřuje se na podrobnosti o konkrétní útoku kampaně.</span><span class="sxs-lookup"><span data-stu-id="81c17-203">Campaign Report: focuses on details of specific attack campaigns.</span></span>

- <span data-ttu-id="81c17-204">Souhrnná sestava Threat: obsahuje všechny položky v předchozí dva sestavy hello.</span><span class="sxs-lookup"><span data-stu-id="81c17-204">Threat Summary Report: covers all items in hello previous two reports.</span></span>

<span data-ttu-id="81c17-205">Tento typ informací je velmi užitečná během procesu hello reakcí na incidenty, kde dojde k probíhající šetření toounderstand hello zdroji hello útoku, hello útočníka motivace a jaké toodo toomitigate-li tento problém přesunutí předat dál.</span><span class="sxs-lookup"><span data-stu-id="81c17-205">This type of information is very useful during hello incident response process, where there is an ongoing investigation toounderstand hello source of hello attack, hello attacker’s motivations, and what toodo toomitigate this issue moving forward.</span></span>

1. <span data-ttu-id="81c17-206">tooaccess hello analýzou hrozeb sestavy, hello následující:</span><span class="sxs-lookup"><span data-stu-id="81c17-206">tooaccess hello threat intelligence report, do hello following:</span></span>

2. <span data-ttu-id="81c17-207">Vyberte hello **výstrahy zabezpečení** dlaždici na řídicím panelu ASC hello.</span><span class="sxs-lookup"><span data-stu-id="81c17-207">Select hello **Security alerts** tile on hello ASC dashboard.</span></span>

3. <span data-ttu-id="81c17-208">Vyberte výstrahu zabezpečení hello, pro které chcete tooobtain Další informace.</span><span class="sxs-lookup"><span data-stu-id="81c17-208">Select hello security alert for which you want tooobtain more information.</span></span>

4. <span data-ttu-id="81c17-209">V hello **sestavy** pole, klikněte na tlačítko hello odkaz toohello threat intelligence sestavy.</span><span class="sxs-lookup"><span data-stu-id="81c17-209">In hello **Reports** field, click hello link toohello threat intelligence report.</span></span>

5. <span data-ttu-id="81c17-210">Otevře se soubor PDF hello, kterou si můžete stáhnout.</span><span class="sxs-lookup"><span data-stu-id="81c17-210">This will open hello PDF file, which you can download.</span></span>

![](media/protect-personal-data-azure-security-center/security-alerts-suspicious-process.png)

<span data-ttu-id="81c17-211">Další informace o hello ASC threat intelligence sestav najdete v tématu [Azure Security Center Threat Intelligence sestavy.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)</span><span class="sxs-lookup"><span data-stu-id="81c17-211">For additional information about hello ASC threat intelligence report, see [Azure Security Center Threat Intelligence Report.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)</span></span>

### <a name="assessment"></a><span data-ttu-id="81c17-212">Hodnocení</span><span class="sxs-lookup"><span data-stu-id="81c17-212">Assessment</span></span>

<span data-ttu-id="81c17-213">toohelp s testování, hodnocení a vyhodnocení lepšímu zabezpečení ASC poskytuje pro vyhodnocení integrované ohrožení zabezpečení s Qualys cloudu agentů, jako součást své doporučení součásti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="81c17-213">toohelp with testing, assessment and evaluation of your security posture, ASC provides for integrated vulnerability assessment with Qualys cloud agents, as a part of its virtual machine recommendations component.</span></span>

<span data-ttu-id="81c17-214">Hello Qualys agent hlásí ohrožení zabezpečení dat toohello Qualys platformy správy, které pak odešle ohrožení zabezpečení a sledování dat stavu zpět tooASC.</span><span class="sxs-lookup"><span data-stu-id="81c17-214">hello Qualys agent reports vulnerability data toohello Qualys management platform, which then sends vulnerability and health monitoring data back tooASC.</span></span> <span data-ttu-id="81c17-215">Hello doporučení tooadd řešení pro vyhodnocení ohrožení zabezpečení se zobrazí v hello **doporučení** okno na řídicí panel ASC hello.</span><span class="sxs-lookup"><span data-stu-id="81c17-215">hello recommendation tooadd a vulnerability assessment solution is displayed in hello **Recommendations** blade on hello ASC dashboard.</span></span>

<span data-ttu-id="81c17-216">Po instalaci řešení pro vyhodnocení hello ohrožení zabezpečení na hello cílovém virtuálním počítači Security Center kontroly hello toodetect virtuálních počítačů a zjištění chyb zabezpečení, systém a aplikace.</span><span class="sxs-lookup"><span data-stu-id="81c17-216">After hello vulnerability assessment solution is installed on hello target VM, Security Center scans hello VM toodetect and identify system and application vulnerabilities.</span></span> <span data-ttu-id="81c17-217">Zjistil problémy jsou uvedeny v části hello **virtuální počítače doporučení** možnost.</span><span class="sxs-lookup"><span data-stu-id="81c17-217">Detected issues are shown under hello **Virtual Machines Recommendations** option.</span></span>

#### <a name="how-do-i-implement-a-vulnerability-assessment-solution"></a><span data-ttu-id="81c17-218">Jak implementovat řešení pro vyhodnocení ohrožení zabezpečení?</span><span class="sxs-lookup"><span data-stu-id="81c17-218">How do I implement a vulnerability assessment solution?</span></span> 

<span data-ttu-id="81c17-219">Pokud virtuální počítač nemá řešení pro vyhodnocení integrované ohrožení zabezpečení už nasazená, Security Center doporučuje ji nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="81c17-219">If a Virtual Machine does not have an integrated vulnerability assessment solution already deployed, Security Center recommends that it be installed.</span></span>

1. <span data-ttu-id="81c17-220">Hello ASC řídicím panelu na hello **doporučení** vyberte **přidat řešení pro vyhodnocení ohrožení zabezpečení.**</span><span class="sxs-lookup"><span data-stu-id="81c17-220">In hello ASC dashboard, on hello **Recommendations** blade, select **Add a vulnerability assessment solution.**</span></span>

2. <span data-ttu-id="81c17-221">Vyberte virtuální počítače hello místo, kam chcete řešení pro vyhodnocení tooinstall hello ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="81c17-221">Select hello VMs where you want tooinstall hello vulnerability assessment solution.</span></span>

3. <span data-ttu-id="81c17-222">Klikněte na **nainstalovat na virtuální počítače [počet].**</span><span class="sxs-lookup"><span data-stu-id="81c17-222">Click on **Install on [number of] VMs.**</span></span>

4. <span data-ttu-id="81c17-223">Vyberte jedno partnerské řešení v Azure Marketplace hello nebo pod **použít existující řešení** vyberte **Qualys.**</span><span class="sxs-lookup"><span data-stu-id="81c17-223">Select a partner solution in hello Azure Marketplace, or under **Use existing solution,** select **Qualys.**</span></span>

5. <span data-ttu-id="81c17-224">Můžete zapnout hello nastavení automatické aktualizace zapnout nebo vypnout v hello **partnerských řešení** okno.</span><span class="sxs-lookup"><span data-stu-id="81c17-224">You can turn hello auto update settings on or off in hello **Partner Solutions** blade.</span></span>

<span data-ttu-id="81c17-225">Další pokyny najdete v části tooimplement řešení pro vyhodnocení ohrožení zabezpečení [vyhodnocení ohrožení zabezpečení v Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)</span><span class="sxs-lookup"><span data-stu-id="81c17-225">For further instructions on how tooimplement a vulnerability assessment solution, see [Vulnerability Assessment in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)</span></span>

## <a name="next-steps"></a><span data-ttu-id="81c17-226">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81c17-226">Next steps</span></span>

- [<span data-ttu-id="81c17-227">Úvodní příručka Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="81c17-227">Azure Security Center quick start guide</span></span>](https://docs.microsoft.com/azure/security-center/security-center-get-started)

- [<span data-ttu-id="81c17-228">Úvod tooAzure Security Center</span><span class="sxs-lookup"><span data-stu-id="81c17-228">Introduction tooAzure Security Center</span></span>](https://docs.microsoft.com/azure/security-center/security-center-intro)

- [<span data-ttu-id="81c17-229">Integrace Azure Security Center výstrahy s integrací protokolů Azure</span><span class="sxs-lookup"><span data-stu-id="81c17-229">Integrating Azure Security Center alerts with Azure log integration</span></span>](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration)

- [<span data-ttu-id="81c17-230">Nárůst Azure Security Center s vyhodnocení integrované ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="81c17-230">Boost Azure Security Center with Integrated Vulnerability Assessment</span></span>](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/16/boost-azure-security-center-with-integrated-vulnerability-assessment/)
