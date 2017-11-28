---
title: "aaaOperations Management Suite zabezpečení a Audit řešení webové základní | Microsoft Docs"
description: "Tento dokument popisuje, jak toouse OMS zabezpečení a Audit tooperform řešení webové směrného plánu hodnocení všech monitorovaných webových serverů za účelem dodržování předpisů a zabezpečení."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ROBOTS: NOINDEX
redirect_url: https://www.microsoft.com/cloud-platform/security-and-compliance
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.openlocfilehash: 8aa87fa404ff97ab549dda3f9bebb75766055963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a><span data-ttu-id="da954-103">Vyhodnocování standardních hodnot webu v řešení Zabezpečení a audit pro Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="da954-103">Web Baseline Assessment in Operations Management Suite Security and Audit Solution</span></span>
<span data-ttu-id="da954-104">Tento dokument vám pomůže toouse [Operations Management Suite (OMS) zabezpečení a Audit řešení](operations-management-suite-overview.md) webové funkce tooaccess hello zabezpečené stavu monitorovaných prostředků vyhodnocení směrného plánu.</span><span class="sxs-lookup"><span data-stu-id="da954-104">This document helps you toouse [Operations Management Suite (OMS) Security and Audit Solution](operations-management-suite-overview.md) web baseline assessment capabilities tooaccess hello secure state of your monitored resources.</span></span>

## <a name="what-is-web-baseline-assessment"></a><span data-ttu-id="da954-105">Co je vyhodnocování standardních hodnot webu?</span><span class="sxs-lookup"><span data-stu-id="da954-105">What is Web Baseline Assessment?</span></span>
<span data-ttu-id="da954-106">Zabezpečení v OMS v současné době umožňuje vyhodnocování standardních hodnot zabezpečení pro operační systémy.</span><span class="sxs-lookup"><span data-stu-id="da954-106">Currently OMS Security provides security baseline assessment for operating systems.</span></span> <span data-ttu-id="da954-107">Prohledá hello nastavení operačního systému serverů každých 24 hodin a poskytuje pohled na potenciálně citlivé nastavení.</span><span class="sxs-lookup"><span data-stu-id="da954-107">It scans hello operating system settings of your servers every 24 hours and provides a view into potentially vulnerable settings.</span></span> <span data-ttu-id="da954-108">Další informace najdete v tématu [Vyhodnocování standardních hodnot v řešení Zabezpečení a audit pro Operations Management Suite](oms-security-baseline.md).</span><span class="sxs-lookup"><span data-stu-id="da954-108">Read [Baseline Assessment in Operations Management Suite Security and Audit Solution](oms-security-baseline.md) for more information on this.</span></span>

<span data-ttu-id="da954-109">cílem Hello hello webové směrného plánu hodnocení je nastavení toofind potenciálně citlivé webového serveru.</span><span class="sxs-lookup"><span data-stu-id="da954-109">hello goal of hello web baseline assessment is toofind potentially vulnerable web server settings.</span></span> <span data-ttu-id="da954-110">Hello tři primární zdroje pro hello webové standardních hodnot konfigurace jsou: Konfigurace rozhraní .NET, ASP.NET a služby IIS.</span><span class="sxs-lookup"><span data-stu-id="da954-110">hello three primary sources for hello web baseline configurations are: .NET, ASP.NET, and IIS configuration.</span></span>  <span data-ttu-id="da954-111">Stejně jako hello vyhodnocení směrného plánu operační systém, zabezpečení OMS přechází tooscan vaše webových serverů, každý 24hrs a zadejte pohled na stav zabezpečení z nich.</span><span class="sxs-lookup"><span data-stu-id="da954-111">Just like hello operating system baseline assessment, OMS Security is going tooscan your web servers every 24hrs and provide a view into security state of them.</span></span>  <span data-ttu-id="da954-112">V Internetové informační služby (IIS), konfigurace jsou vysoce přizpůsobitelné, která umožňuje různé webu nebo aplikace toobe úrovně přepsat.</span><span class="sxs-lookup"><span data-stu-id="da954-112">In Internet Information Service (IIS), configurations are highly customizable, which allows various site and application levels toobe overridden.</span></span> <span data-ttu-id="da954-113">skener Hello kontroluje hello nastavení na úrovni jednotlivých aplikací nebo na webu v přidání toohello výchozí kořenové úrovni.</span><span class="sxs-lookup"><span data-stu-id="da954-113">hello scanner checks hello settings at each application/site level in addition toohello default root level.</span></span> <span data-ttu-id="da954-114">To vám pomůže tooidentify potenciální ohrožení zabezpečení nastavení umístění a rychle napravit.</span><span class="sxs-lookup"><span data-stu-id="da954-114">This helps you tooidentify potential vulnerability settings locations and quickly remediate.</span></span>


## <a name="web-security-baseline-assessment"></a><span data-ttu-id="da954-115">Vyhodnocování standardních hodnot zabezpečení webu</span><span class="sxs-lookup"><span data-stu-id="da954-115">Web Security Baseline Assessment</span></span>
<span data-ttu-id="da954-116">Pro tuto verzi preview bude tato funkce toobe získat přístup pomocí možnost hledání OMS hello.</span><span class="sxs-lookup"><span data-stu-id="da954-116">For this preview this feature is going toobe accessed using hello OMS Search option.</span></span> <span data-ttu-id="da954-117">Postupujte podle kroků hello níže tooperform hello přidělena dotazu:</span><span class="sxs-lookup"><span data-stu-id="da954-117">Follow hello steps below tooperform hello appropriated query:</span></span>

1. <span data-ttu-id="da954-118">V hello **Microsoft Operations Management Suite** klikněte na hlavním řídicím **zabezpečení a Audit** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="da954-118">In hello **Microsoft Operations Management Suite** main dashboard click **Security and Audit** tile.</span></span>
2. <span data-ttu-id="da954-119">V hello **zabezpečení a Audit** řídicí panel, klikněte na tlačítko **hledání protokolů** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="da954-119">In hello **Security and Audit** dashboard, click **Log Search** button.</span></span>
3. <span data-ttu-id="da954-120">Hello první dotaz, který můžete použít se hello **souhrn hodnocení základní webové**.</span><span class="sxs-lookup"><span data-stu-id="da954-120">hello first query that you can use is hello **Web Baseline Assessment Summary**.</span></span> <span data-ttu-id="da954-121">V hello **Begin vyhledávání zde** pole, zadejte tento dotaz: typ*= SecurityBaselineSummary BaselineType = webové*.</span><span class="sxs-lookup"><span data-stu-id="da954-121">In hello **Begin search here** field, type this query: Type*=SecurityBaselineSummary BaselineType=web*.</span></span> <span data-ttu-id="da954-122">Hello následující obrazovka má ukázkový výstup:</span><span class="sxs-lookup"><span data-stu-id="da954-122">hello following screen has an output sample:</span></span>

![Souhrn vyhodnocení standardních hodnot webu](./media/oms-security-web-baseline/oms-security-web-baseline-fig1-new.png)

> [!NOTE]
> <span data-ttu-id="da954-124">V tomto dotazu každý záznam označuje souhrn vyhodnocení na jednom serveru.</span><span class="sxs-lookup"><span data-stu-id="da954-124">In this query, each record indicates assessment summary on a single server.</span></span>

<span data-ttu-id="da954-125">Jakmile jste na hello **hledání protokolů**, tooobtain různé dotazy můžete zadat další informace o vyhodnocení směrného plánu webové hello.</span><span class="sxs-lookup"><span data-stu-id="da954-125">Once you are in hello **Log Search**, you can type different queries tooobtain more information about hello web baseline assessment.</span></span> <span data-ttu-id="da954-126">Kromě toho toohello předchozího dotazu, můžete také použít hello následující těch, které jsou v této verzi preview.</span><span class="sxs-lookup"><span data-stu-id="da954-126">In addition toohello previous query, you can also use hello following ones in this preview.</span></span>

<span data-ttu-id="da954-127">**Vyhodnocení pravidel standardních hodnot webu:** Každý záznam představuje jedno vyhodnocení pravidla standardních hodnot webu na jednom serveru.</span><span class="sxs-lookup"><span data-stu-id="da954-127">**Web Baseline Rule Assessment**: Each record represents a single web baseline rule evaluation on a single server.</span></span> <span data-ttu-id="da954-128">Obsahuje všechna data pro pravidlo hello, umístění, hello očekávaný výsledek a skutečný výsledek hello.</span><span class="sxs-lookup"><span data-stu-id="da954-128">It includes all data for hello rule, location, hello expected result, and hello actual result.</span></span>

<span data-ttu-id="da954-129">**Dotaz:** Type*=SecurityBaseline BaselineType=web*</span><span class="sxs-lookup"><span data-stu-id="da954-129">**Query**: Type*=SecurityBaseline BaselineType=web*</span></span>

![Vyhodnocení pravidel standardních hodnot webu](./media/oms-security-web-baseline/oms-security-web-baseline-fig2.png)

<span data-ttu-id="da954-131">**Zobrazit všechny výsledky pro určitý server**: Tento dotaz zobrazí jak toosee výsledků konkrétní serveru.</span><span class="sxs-lookup"><span data-stu-id="da954-131">**Show all results for a specific server**: This query shows how toosee results of a specific server.</span></span>

<span data-ttu-id="da954-132">**Dotaz:***Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*</span><span class="sxs-lookup"><span data-stu-id="da954-132">**Query**: *Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*</span></span>

![Všechny výsledky](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

<span data-ttu-id="da954-134">Také můžete tyto záznamy nebo dotazy toocreate vlastní řídicí panely, sestavy nebo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="da954-134">You can also use these records/queries toocreate your own dashboards, reports, or alerts.</span></span> <span data-ttu-id="da954-135">úvodní obrazovka níže má kontrolu uživatelského rozhraní, můžete přidat tooyour řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="da954-135">hello screen below has a sample UI control that you can add tooyour dashboard.</span></span> <span data-ttu-id="da954-136">Dozvíte, jak toovisualize svá data pomocí návrháře zobrazení OMS [zde](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/).</span><span class="sxs-lookup"><span data-stu-id="da954-136">You can learn how toovisualize your data using OMS View Designer [here](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/).</span></span> <span data-ttu-id="da954-137">úvodní obrazovka dole je příklad jak hello dlaždici bude vypadat podobně jako Jakmile provedete toto vlastní nastavení.</span><span class="sxs-lookup"><span data-stu-id="da954-137">hello screen below is an example of how hello tile will look like once you make this customization.</span></span>

![Ukázka uživatelského rozhraní](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

> [!NOTE]
> <span data-ttu-id="da954-139">Pokud chcete tooknow hello nastavení, které jsou zaškrtnutá políčka pro vyhodnocení směrného plánu hello, si můžete stáhnout [této tabulky aplikace Excel](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690) obsahující tato nastavení.</span><span class="sxs-lookup"><span data-stu-id="da954-139">If you would like tooknow hello settings that are checked for hello baseline assessment, you can download [this Excel spreadsheet](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690) that contains these settings.</span></span>

## <a name="see-also"></a><span data-ttu-id="da954-140">Viz také</span><span class="sxs-lookup"><span data-stu-id="da954-140">See also</span></span>
<span data-ttu-id="da954-141">V tomto dokumentu jste se dozvěděli o vyhodnocování standardních hodnot webu v řešení Zabezpečení a audit pro OMS.</span><span class="sxs-lookup"><span data-stu-id="da954-141">In this document, you learned about OMS Security and Audit web baseline assessment.</span></span> <span data-ttu-id="da954-142">Další informace o zabezpečení OMS toolearn najdete hello následující články:</span><span class="sxs-lookup"><span data-stu-id="da954-142">toolearn more about OMS Security, see hello following articles:</span></span>

* [<span data-ttu-id="da954-143">Přehled Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="da954-143">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="da954-144">Monitorování a výstrahy tooSecurity odpovídá v Operations Management Suite zabezpečení a Audit řešení</span><span class="sxs-lookup"><span data-stu-id="da954-144">Monitoring and Responding tooSecurity Alerts in Operations Management Suite Security and Audit Solution</span></span>](oms-security-responding-alerts.md)
* [<span data-ttu-id="da954-145">Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="da954-145">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

