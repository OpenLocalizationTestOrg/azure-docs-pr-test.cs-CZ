---
title: aaaMonitor Surface Huby se s Azure Log Analytics | Microsoft Docs
description: "Použijte hello Surface Hub řešení tootrack hello stav Surface Huby a pochopit, jak se právě používají."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 8b4e56bc-2d4f-4648-a236-16e9e732ebef
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 623d30e749cafdd4a34ba0c5b3408164f1b4a95b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-surface-hubs-with-log-analytics-tootrack-their-health"></a><span data-ttu-id="80584-103">Monitorování Surface Huby s tootrack analýzy protokolů jejich stav</span><span class="sxs-lookup"><span data-stu-id="80584-103">Monitor Surface Hubs with Log Analytics tootrack their health</span></span>

![Surface Hub – symbol](./media/log-analytics-surface-hubs/surface-hub-symbol.png)

<span data-ttu-id="80584-105">Tento článek popisuje, jak můžete použít hello řešení Surface Hub v zařízení Microsoft Surface Hub toomonitor analýzy protokolů s hello Microsoft Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="80584-105">This article describes how you can use hello Surface Hub solution in Log Analytics toomonitor Microsoft Surface Hub devices with hello Microsoft Operations Management Suite (OMS).</span></span> <span data-ttu-id="80584-106">Přihlaste se analýza pomáhá sledovat stav hello Surface Huby stejně jako pochopit, jak se právě používají.</span><span class="sxs-lookup"><span data-stu-id="80584-106">Log Analytics helps you track hello health of your Surface Hubs as well as understand how they are being used.</span></span>

<span data-ttu-id="80584-107">Každý Surface Hub má hello instalace agenta Microsoft Monitoring Agent.</span><span class="sxs-lookup"><span data-stu-id="80584-107">Each Surface Hub has hello Microsoft Monitoring Agent installed.</span></span> <span data-ttu-id="80584-108">Jeho prostřednictvím, může odesílat data z vaší tooOMS Surface Hub agenta hello.</span><span class="sxs-lookup"><span data-stu-id="80584-108">Its through hello agent that you can send data from your Surface Hub tooOMS.</span></span> <span data-ttu-id="80584-109">Soubory protokolu se načítají z Surface Huby a jsou pak odešlou toohello OMS služby.</span><span class="sxs-lookup"><span data-stu-id="80584-109">Log files are read from your Surface Hubs and are then are sent toohello OMS service.</span></span> <span data-ttu-id="80584-110">Problémy jako servery je offline, hello kalendáře nesynchronizuje, nebo pokud je účet zařízení hello nelze toolog do Skype se zobrazí v OMS na řídicím panelu hello Surface Hub.</span><span class="sxs-lookup"><span data-stu-id="80584-110">Issues like servers being offline, hello calendar not syncing, or if hello device account is unable toolog into Skype are shown in OMS in hello Surface Hub dashboard.</span></span> <span data-ttu-id="80584-111">Pomocí dat hello hello řídicím panelu můžete identifikovat zařízení, nejsou spuštěné nebo které jsou jiné problémy s a potenciálně použít opravy problémů hello zjištěna.</span><span class="sxs-lookup"><span data-stu-id="80584-111">By using hello data in hello dashboard, you can identify devices that are not running, or that are having other problems, and potentially apply fixes for hello detected issues.</span></span>

## <a name="installing-and-configuring-hello-solution"></a><span data-ttu-id="80584-112">Instalace a konfigurace řešení hello</span><span class="sxs-lookup"><span data-stu-id="80584-112">Installing and configuring hello solution</span></span>
<span data-ttu-id="80584-113">Použijte následující informace tooinstall hello a nakonfigurujte hello řešení.</span><span class="sxs-lookup"><span data-stu-id="80584-113">Use hello following information tooinstall and configure hello solution.</span></span> <span data-ttu-id="80584-114">V pořadí toomanage Surface Huby se z hello Microsoft Operations Management Suite (OMS), budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="80584-114">In order toomanage your Surface Hubs from hello Microsoft Operations Management Suite (OMS), you'll need hello following:</span></span>

* <span data-ttu-id="80584-115">Platné předplatné příliš[OMS](http://www.microsoft.com/oms).</span><span class="sxs-lookup"><span data-stu-id="80584-115">A valid subscription too[OMS](http://www.microsoft.com/oms).</span></span>
* <span data-ttu-id="80584-116">[OMS předplatné](https://azure.microsoft.com/pricing/details/log-analytics/) úroveň, která bude podporovat hello číslo zařízení chcete toomonitor.</span><span class="sxs-lookup"><span data-stu-id="80584-116">An [OMS subscription](https://azure.microsoft.com/pricing/details/log-analytics/) level that will support hello number of devices you want toomonitor.</span></span> <span data-ttu-id="80584-117">OMS ceny se liší v závislosti na tom, kolik zařízení jsou zaregistrovaná a kolik dat se procesy.</span><span class="sxs-lookup"><span data-stu-id="80584-117">OMS pricing varies depending on how many devices are enrolled, and how much data it processes.</span></span> <span data-ttu-id="80584-118">Budete muset tootake to v úvahu při plánování zavádění řešení Surface Hub.</span><span class="sxs-lookup"><span data-stu-id="80584-118">You'll want tootake this into consideration when planning your Surface Hub rollout.</span></span>

<span data-ttu-id="80584-119">Dále budete buď přidejte OMS předplatné tooyour stávající Microsoft Azure předplatné nebo vytvořit nový pracovní prostor přímo přes portál OMS hello.</span><span class="sxs-lookup"><span data-stu-id="80584-119">Next, you will either add an OMS subscription tooyour existing Microsoft Azure subscription or create a new workspace directly through hello OMS portal.</span></span> <span data-ttu-id="80584-120">Podrobné pokyny pro použití metody je v [začít pracovat s analýzy protokolů](log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="80584-120">Detailed instructions for using either method is at [Get started with Log Analytics](log-analytics-get-started.md).</span></span> <span data-ttu-id="80584-121">Až nastavíte hello OMS odběr existují dva způsoby tooenroll zařízení Surface Hub:</span><span class="sxs-lookup"><span data-stu-id="80584-121">Once hello OMS subscription is set up, there are two ways tooenroll your Surface Hub devices:</span></span>

* <span data-ttu-id="80584-122">Automaticky prostřednictvím služby Intune</span><span class="sxs-lookup"><span data-stu-id="80584-122">Automatically through Intune</span></span>
* <span data-ttu-id="80584-123">Ručně pomocí **nastavení** na vašem zařízení Surface Hub.</span><span class="sxs-lookup"><span data-stu-id="80584-123">Manually through **Settings** on your Surface Hub device.</span></span>

## <a name="set-up-monitoring"></a><span data-ttu-id="80584-124">Nastavte monitorování</span><span class="sxs-lookup"><span data-stu-id="80584-124">Set up monitoring</span></span>
<span data-ttu-id="80584-125">Můžete monitorovat hello stavu a činností vaší Surface Hub pomocí analýzy protokolů v OMS.</span><span class="sxs-lookup"><span data-stu-id="80584-125">You can monitor hello health and activity of your Surface Hub using Log Analytics in OMS.</span></span> <span data-ttu-id="80584-126">Hello Surface Hub v OMS můžete zaregistrovat pomocí Intune nebo místně pomocí **nastavení** na hello Surface Hub.</span><span class="sxs-lookup"><span data-stu-id="80584-126">You can enroll hello Surface Hub in OMS by using Intune, or locally by using **Settings** on hello Surface Hub.</span></span>

## <a name="connect-surface-hubs-toooms-through-intune"></a><span data-ttu-id="80584-127">Připojení Surface Huby tooOMS přes Intune</span><span class="sxs-lookup"><span data-stu-id="80584-127">Connect Surface Hubs tooOMS through Intune</span></span>
<span data-ttu-id="80584-128">ID pracovního prostoru a klíč pracovního prostoru pro pracovní prostor OMS hello, která bude spravovat Surface Huby, budete potřebovat hello.</span><span class="sxs-lookup"><span data-stu-id="80584-128">You'll need hello workspace ID and workspace key for hello OMS workspace that will manage your Surface Hubs.</span></span> <span data-ttu-id="80584-129">Můžete získat z portálu OMS hello.</span><span class="sxs-lookup"><span data-stu-id="80584-129">You can get those from hello OMS portal.</span></span>

<span data-ttu-id="80584-130">Intune je produkt společnosti Microsoft, který vám umožní toocentrally spravovat hello OMS nastavení, které jsou použité tooone nebo více zařízení.</span><span class="sxs-lookup"><span data-stu-id="80584-130">Intune is a Microsoft product that allows you toocentrally manage hello OMS configuration settings that are applied tooone or more of your devices.</span></span> <span data-ttu-id="80584-131">Postupujte podle těchto kroků tooconfigure zařízení prostřednictvím služby Intune:</span><span class="sxs-lookup"><span data-stu-id="80584-131">Follow these steps tooconfigure your devices through Intune:</span></span>

1. <span data-ttu-id="80584-132">Přihlaste se tooIntune.</span><span class="sxs-lookup"><span data-stu-id="80584-132">Sign in tooIntune.</span></span>
2. <span data-ttu-id="80584-133">Přejděte příliš**nastavení** > **připojené zdroje**.</span><span class="sxs-lookup"><span data-stu-id="80584-133">Navigate too**Settings** > **Connected Sources**.</span></span>
3. <span data-ttu-id="80584-134">Vytvořte nebo upravte zásadu na základě šablony hello Surface Hub.</span><span class="sxs-lookup"><span data-stu-id="80584-134">Create or edit a policy based on hello Surface Hub template.</span></span>
4. <span data-ttu-id="80584-135">Přejděte toohello OMS (Statistika provozu Azure) části hello zásad a přidejte hello *ID pracovního prostoru* a *klíč pracovního prostoru* toohello zásad.</span><span class="sxs-lookup"><span data-stu-id="80584-135">Navigate toohello OMS (Azure Operational Insights) section of hello policy, and add hello *Workspace ID* and *Workspace Key* toohello policy.</span></span>
5. <span data-ttu-id="80584-136">Uložte zásadu hello.</span><span class="sxs-lookup"><span data-stu-id="80584-136">Save hello policy.</span></span>
6. <span data-ttu-id="80584-137">Přidružení zásad hello hello příslušné skupiny zařízení.</span><span class="sxs-lookup"><span data-stu-id="80584-137">Associate hello policy with hello appropriate group of devices.</span></span>

   ![Zásady Intune](./media/log-analytics-surface-hubs/intune.png)

<span data-ttu-id="80584-139">Intune pak synchronizuje hello OMS nastavení s hello zařízení v hello cílové skupiny je zaregistrujete v pracovním prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="80584-139">Intune then syncs hello OMS settings with hello devices in hello target group, enrolling them in your OMS workspace.</span></span>

## <a name="connect-surface-hubs-toooms-using-hello-settings-app"></a><span data-ttu-id="80584-140">Připojit tooOMS Surface Huby pomocí hello nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="80584-140">Connect Surface Hubs tooOMS using hello Settings app</span></span>
<span data-ttu-id="80584-141">ID pracovního prostoru a klíč pracovního prostoru pro pracovní prostor OMS hello, která bude spravovat Surface Huby, budete potřebovat hello.</span><span class="sxs-lookup"><span data-stu-id="80584-141">You'll need hello workspace ID and workspace key for hello OMS workspace that will manage your Surface Hubs.</span></span> <span data-ttu-id="80584-142">Můžete získat z portálu OMS hello.</span><span class="sxs-lookup"><span data-stu-id="80584-142">You can get those from hello OMS portal.</span></span>

<span data-ttu-id="80584-143">Pokud nepoužíváte Intune toomanage prostředí, můžete zaregistrovat zařízení ručně pomocí **nastavení** na každý Surface Hub:</span><span class="sxs-lookup"><span data-stu-id="80584-143">If you don't use Intune toomanage your environment, you can enroll devices manually through **Settings** on each Surface Hub:</span></span>

1. <span data-ttu-id="80584-144">Surface Hub, otevřete **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="80584-144">From your Surface Hub, open **Settings**.</span></span>
2. <span data-ttu-id="80584-145">Zadejte pověření správce zařízení hello po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="80584-145">Enter hello device admin credentials when prompted.</span></span>
3. <span data-ttu-id="80584-146">Klikněte na tlačítko **toto zařízení**a v části hello **monitorování**, klikněte na tlačítko **konfigurovat nastavení OMS**.</span><span class="sxs-lookup"><span data-stu-id="80584-146">Click **This device**, and hello under **Monitoring**, click **Configure OMS Settings**.</span></span>
4. <span data-ttu-id="80584-147">Vyberte **povolit sledování**.</span><span class="sxs-lookup"><span data-stu-id="80584-147">Select **Enable monitoring**.</span></span>
5. <span data-ttu-id="80584-148">V dialogovém okně Nastavení hello OMS, zadejte hello **ID pracovního prostoru** a typ hello **klíč pracovního prostoru**.</span><span class="sxs-lookup"><span data-stu-id="80584-148">In hello OMS settings dialog, type hello **Workspace ID** and type hello **Workspace Key**.</span></span>  
   <span data-ttu-id="80584-149">![nastavení](./media/log-analytics-surface-hubs/settings.png)</span><span class="sxs-lookup"><span data-stu-id="80584-149">![settings](./media/log-analytics-surface-hubs/settings.png)</span></span>
6. <span data-ttu-id="80584-150">Klikněte na tlačítko **OK** toocomplete hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="80584-150">Click **OK** toocomplete hello configuration.</span></span>

<span data-ttu-id="80584-151">Zobrazení potvrzení o tom, zda text hello, které OMS konfigurace byla úspěšně použít toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="80584-151">A confirmation appears telling you whether or not hello OMS configuration was successfully applied toohello device.</span></span> <span data-ttu-id="80584-152">Pokud byl, zobrazí se zpráva s informacemi o tom, že tento agent hello úspěšně připojeno toohello OMS služby.</span><span class="sxs-lookup"><span data-stu-id="80584-152">If it was, a message appears stating that hello agent successfully connected toohello OMS service.</span></span> <span data-ttu-id="80584-153">Hello zařízení pak spustí odesílání dat tooOMS, kde můžete zobrazit a pracovat.</span><span class="sxs-lookup"><span data-stu-id="80584-153">hello device then starts sending data tooOMS where you can view and act on it.</span></span>

## <a name="monitor-surface-hubs"></a><span data-ttu-id="80584-154">Monitorování Surface Hubů</span><span class="sxs-lookup"><span data-stu-id="80584-154">Monitor Surface Hubs</span></span>
<span data-ttu-id="80584-155">Monitorování Surface Huby pomocí OMS je mnohem jako monitorování jiných zaregistrovaných zařízení.</span><span class="sxs-lookup"><span data-stu-id="80584-155">Monitoring your Surface Hubs using OMS is much like monitoring any other enrolled devices.</span></span>

1. <span data-ttu-id="80584-156">Přihlaste se toohello portálu OMS.</span><span class="sxs-lookup"><span data-stu-id="80584-156">Sign in toohello OMS portal.</span></span>
2. <span data-ttu-id="80584-157">Přejděte toohello Surface Hub řešení pack řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="80584-157">Navigate toohello Surface Hub solution pack dashboard.</span></span>
3. <span data-ttu-id="80584-158">Zobrazí se stav vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="80584-158">Your device's health is displayed.</span></span>

   ![Surface Hub řídicí panel](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

<span data-ttu-id="80584-160">Můžete vytvořit [výstrahy](log-analytics-alerts.md) založené na protokolu hledání existujících nebo vlastních.</span><span class="sxs-lookup"><span data-stu-id="80584-160">You can create [alerts](log-analytics-alerts.md) based on existing or custom log searches.</span></span> <span data-ttu-id="80584-161">Pomocí hello hello dat, které OMS shromažďuje z Surface Huby, můžete vyhledat problémy a výstrahy na hello podmínky, které definujete pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="80584-161">Using hello data hello OMS collects from your Surface Hubs, you can search for issues and alert on hello conditions that you define for your devices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="80584-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="80584-162">Next steps</span></span>
* <span data-ttu-id="80584-163">Použití [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) tooview podrobná data Surface Hub.</span><span class="sxs-lookup"><span data-stu-id="80584-163">Use [Log searches in Log Analytics](log-analytics-log-searches.md) tooview detailed Surface Hub data.</span></span>
* <span data-ttu-id="80584-164">Vytvoření [výstrahy](log-analytics-alerts.md) toonotify případě problémy, ke kterým dochází u Surface Huby.</span><span class="sxs-lookup"><span data-stu-id="80584-164">Create [alerts](log-analytics-alerts.md) toonotify you when issues occur with your Surface Hubs.</span></span>
