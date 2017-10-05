---
title: "Azure CDN v reálném čase výstrahy | Microsoft Docs"
description: "V reálném čase výstrah v Microsoft Azure CDN. Výstrahy v reálném čase poskytují oznámení o výkonu z koncových bodů ve vašem profilu CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 1e85b809-e1a9-4473-b835-69d1b4ed3393
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 6e66eb076ac7220823a848b5047f147d4101cd55
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="real-time-alerts-in-microsoft-azure-cdn"></a><span data-ttu-id="7b0e9-104">V reálném čase výstrah v Microsoft Azure CDN</span><span class="sxs-lookup"><span data-stu-id="7b0e9-104">Real-time alerts in Microsoft Azure CDN</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="7b0e9-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="7b0e9-105">Overview</span></span>
<span data-ttu-id="7b0e9-106">Tento dokument popisuje, v reálném čase výstrah v Microsoft Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-106">This document explains real-time alerts in Microsoft Azure CDN.</span></span> <span data-ttu-id="7b0e9-107">Tato funkce poskytuje v reálném čase oznámení o výkonu z koncových bodů ve vašem profilu CDN.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-107">This functionality provides real-time notifications about the performance of the endpoints in your CDN profile.</span></span>  <span data-ttu-id="7b0e9-108">Můžete nastavit e-mailu nebo HTTP upozornění na základě:</span><span class="sxs-lookup"><span data-stu-id="7b0e9-108">You can set up email or HTTP alerts based on:</span></span>

* <span data-ttu-id="7b0e9-109">Šířka pásma</span><span class="sxs-lookup"><span data-stu-id="7b0e9-109">Bandwidth</span></span>
* <span data-ttu-id="7b0e9-110">Stavové kódy</span><span class="sxs-lookup"><span data-stu-id="7b0e9-110">Status Codes</span></span>
* <span data-ttu-id="7b0e9-111">Stavy mezipaměti</span><span class="sxs-lookup"><span data-stu-id="7b0e9-111">Cache Statuses</span></span>
* <span data-ttu-id="7b0e9-112">Připojení</span><span class="sxs-lookup"><span data-stu-id="7b0e9-112">Connections</span></span>

## <a name="creating-a-real-time-alert"></a><span data-ttu-id="7b0e9-113">Vytváření v reálném čase výstrahy</span><span class="sxs-lookup"><span data-stu-id="7b0e9-113">Creating a real-time alert</span></span>
1. <span data-ttu-id="7b0e9-114">V [portálu Azure](https://portal.azure.com), přejděte na svůj profil CDN.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-114">In the [Azure Portal](https://portal.azure.com), browse to your CDN profile.</span></span>
   
    ![Okno profil CDN](./media/cdn-real-time-alerts/cdn-profile-blade.png)
2. <span data-ttu-id="7b0e9-116">Okno profil CDN, klikněte **spravovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-116">From the CDN profile blade, click the **Manage** button.</span></span>
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-real-time-alerts/cdn-manage-btn.png)
   
    <span data-ttu-id="7b0e9-118">Otevře se na portálu pro správu CDN.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-118">The CDN management portal opens.</span></span>
3. <span data-ttu-id="7b0e9-119">Najeďte myší **Analytics** a potom přejděte myší **statistiky v reálném čase** plovoucím panelem.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-119">Hover over the **Analytics** tab, then hover over the **Real-Time Stats** flyout.</span></span>  <span data-ttu-id="7b0e9-120">Klikněte na **v reálném čase výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-120">Click on **Real-Time Alerts**.</span></span>
   
    ![Portál pro správu CDN](./media/cdn-real-time-alerts/cdn-premium-portal.png)
   
    <span data-ttu-id="7b0e9-122">Zobrazí se seznam existující výstrahy konfigurací (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="7b0e9-122">The list of existing alert configurations (if any) is displayed.</span></span>
4. <span data-ttu-id="7b0e9-123">Klikněte **přidat výstrahy** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-123">Click the **Add Alert** button.</span></span>
   
    ![Výstrahy tlačítko Přidat](./media/cdn-real-time-alerts/cdn-add-alert.png)
   
    <span data-ttu-id="7b0e9-125">Formulář pro vytvoření nového upozornění se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-125">A form for creating a new alert is displayed.</span></span>
   
    ![Formulář nové výstrahy](./media/cdn-real-time-alerts/cdn-new-alert.png)
5. <span data-ttu-id="7b0e9-127">Pokud chcete tuto výstrahu být aktivní, když kliknete na tlačítko **Uložit**, zkontrolujte **upozornění povolené** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-127">If you want this alert to be active when you click **Save**, check the **Alert Enabled** checkbox.</span></span>
6. <span data-ttu-id="7b0e9-128">Zadejte popisný název pro vaše upozornění **název** pole.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-128">Enter a descriptive name for your alert in the **Name** field.</span></span>
7. <span data-ttu-id="7b0e9-129">V **typ média** rozevíracího seznamu vyberte **velkého objektu HTTP**.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-129">In the **Media Type** dropdown, select **HTTP Large Object**.</span></span>
   
    ![Typ média HTTP velký objekt vybrán](./media/cdn-real-time-alerts/cdn-http-large.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="7b0e9-131">Je nutné vybrat **velkého objektu HTTP** jako **typ média**.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-131">You must select **HTTP Large Object** as the **Media Type**.</span></span>  <span data-ttu-id="7b0e9-132">Jiné možnosti, které nejsou používány nástrojem **Azure CDN společnosti Verizon**.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-132">The other choices are not used by **Azure CDN from Verizon**.</span></span>  <span data-ttu-id="7b0e9-133">Selhání a vyberte **velkého objektu HTTP** způsobí výstrahu až nikdy se spustí.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-133">Failure to select **HTTP Large Object** will cause your alert to never be triggered.</span></span>
   > 
   > 
8. <span data-ttu-id="7b0e9-134">Vytvoření **výraz** monitorování tak, že vyberete **metrika**, **operátor**, a **aktivovat hodnotu**.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-134">Create an **Expression** to monitor by selecting a **Metric**, **Operator**, and **Trigger value**.</span></span>
   
   * <span data-ttu-id="7b0e9-135">Pro **metrika**, vyberte typ podmínky, které chcete monitorovaných.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-135">For **Metric**, select the type of condition you want monitored.</span></span>  <span data-ttu-id="7b0e9-136">**MB/s šířky pásma** je množství využití šířky pásma v MB za sekundu.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-136">**Bandwidth Mbps** is the amount of bandwidth usage in megabits per second.</span></span>  <span data-ttu-id="7b0e9-137">**Celkový počet připojení** je počet souběžných připojení protokolu HTTP k naše servery edge.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-137">**Total Connections** is the number of concurrent HTTP connections to our edge servers.</span></span>  <span data-ttu-id="7b0e9-138">Definice různé stavy, které jsou mezipaměti a stavové kódy, najdete v části [Azure CDN mezipaměti stavové kódy](https://msdn.microsoft.com/library/mt759237.aspx) a [stavové kódy HTTP CDN Azure](https://msdn.microsoft.com/library/mt759238.aspx)</span><span class="sxs-lookup"><span data-stu-id="7b0e9-138">For definitions of the various cache statuses and status codes, see [Azure CDN Cache Status Codes](https://msdn.microsoft.com/library/mt759237.aspx) and [Azure CDN HTTP Status Codes](https://msdn.microsoft.com/library/mt759238.aspx)</span></span>
   * <span data-ttu-id="7b0e9-139">**Operátor** je matematický operátor, který vytváří vztah mezi metriky a hodnotu aktivační události.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-139">**Operator** is the mathematical operator that establishes the relationship between the metric and the trigger value.</span></span>
   * <span data-ttu-id="7b0e9-140">**Aktivovat hodnota** je prahovou hodnotu, která musí být splněny, než bude odesláno oznámení.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-140">**Trigger Value** is the threshold value that must be met before a notification will be sent out.</span></span>
     
     <span data-ttu-id="7b0e9-141">V následujícím příkladu výraz vytvořil jsem označuje, že I chcete být upozorněni, když počet 404 stavové kódy je větší než 25.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-141">In the below example, the expression I have created indicates that I would like to be notified when the number of 404 status codes is greater than 25.</span></span>
     
     ![V reálném čase výstrahy ukázka výrazu](./media/cdn-real-time-alerts/cdn-expression.png)
9. <span data-ttu-id="7b0e9-143">Pro **Interval**, zadejte, jak často chcete výrazu vyhodnoceného.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-143">For **Interval**, enter how frequently you would like the expression evaluated.</span></span>
10. <span data-ttu-id="7b0e9-144">V **upozornit na** rozevíracího seznamu vyberte, pokud chcete být upozorněni, když výraz hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-144">In the **Notify on** dropdown, select when you would like to be notified when the expression is true.</span></span>
    
    * <span data-ttu-id="7b0e9-145">**Podmínka spuštění** označuje, že bude odesláno upozornění, při prvním zjištění zadanou podmínku.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-145">**Condition Start** indicates that a notification will be sent when the specified condition is first detected.</span></span>
    * <span data-ttu-id="7b0e9-146">**Podmínka End** označuje, že bude být oznámení odesláno, když už je zjištěna zadanou podmínku.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-146">**Condition End** indicates that a notification will be sent when the specified condition is no longer detected.</span></span> <span data-ttu-id="7b0e9-147">Toto oznámení můžete spustit pouze po naše síť systému pro monitorování zjistil, že došlo k chybě zadanou podmínku.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-147">This notification can only be triggered after our network monitoring system detected that the specified condition occurred.</span></span>
    * <span data-ttu-id="7b0e9-148">**Průběžné** označuje, že bude odesláno upozornění pokaždé, když zjistí, že monitorování systému sítě zadanou podmínku.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-148">**Continuous** indicates that a notification will be sent each time that the network monitoring system detects the specified condition.</span></span> <span data-ttu-id="7b0e9-149">Mějte na paměti, který monitorování systému sítě pouze jednou zkontrolujte intervalu pro zadanou podmínku.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-149">Keep in mind that the network monitoring system will only check once per interval for the specified condition.</span></span>
    * <span data-ttu-id="7b0e9-150">**Podmínka počáteční a koncové** označuje, že bude odesláno upozornění poprvé že zadanou podmínku je a znovu při podmínka je už.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-150">**Condition Start and End** indicates that a notification will be sent the first time that the specified condition is detected and once again when the condition is no longer detected.</span></span>
11. <span data-ttu-id="7b0e9-151">Pokud chcete dostávat oznámení e-mailem, podívejte se **oznámení e-mailem** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-151">If you want to receive notifications by email, check the **Notify by Email** checkbox.</span></span>  
    
    ![Upozornit e-mailu formuláři](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    <span data-ttu-id="7b0e9-153">V **k** pole, zadejte e-mailovou adresu, můžete místo, kam chcete oznámení odesílat.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-153">In the **To** field, enter the email address you where you want notifications sent.</span></span> <span data-ttu-id="7b0e9-154">Pro **subjektu** a **textu**, můžete ponechat výchozí nastavení, nebo může přizpůsobit zprávu pomocí **k dispozici klíčová slova** seznamu dynamicky vložení dat výstrah při odeslání zprávy.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-154">For **Subject** and **Body**, you may leave the default, or you may customize the message using the **Available keywords** list to dynamically insert alert data when the message is sent.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="7b0e9-155">E-mailové oznámení můžete otestovat klepnutím **testovací oznámení** tlačítko, ale pouze po uložení konfigurace výstrahy.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-155">You can test the email notification by clicking the **Test Notification** button, but only after the alert configuration has been saved.</span></span>
    > 
    > 
12. <span data-ttu-id="7b0e9-156">Pokud chcete oznámení odeslat do webového serveru, podívejte se **oznámení pomocí HTTP Post** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-156">If you want notifications to be posted to a web server, check the **Notify by HTTP Post** checkbox.</span></span>
    
    ![Upozornit pomocí HTTP Post formuláře](./media/cdn-real-time-alerts/cdn-notify-http.png)
    
    <span data-ttu-id="7b0e9-158">V **Url** pole, zadejte adresu URL pro odeslání je místo, kam chcete zprávy HTTP.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-158">In the **Url** field, enter the URL you where you want the HTTP message posted.</span></span> <span data-ttu-id="7b0e9-159">V **hlavičky** textovému poli, zadejte hlavičky protokolu HTTP k odeslání v požadavku.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-159">In the **Headers** textbox, enter the HTTP headers to be sent in the request.</span></span>  <span data-ttu-id="7b0e9-160">Pro **textu** může přizpůsobit zprávu pomocí **k dispozici klíčová slova** seznamu dynamicky vložení dat výstrah při odeslání zprávy.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-160">For **Body** you may customize the message using the **Available keywords** list to dynamically insert alert data when the message is sent.</span></span>  <span data-ttu-id="7b0e9-161">**Hlavičky** a **textu** výchozí nastavení datovou část XML podobně jako následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-161">**Headers** and **Body** default to an XML payload similar to the below example.</span></span>
    
    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```
    
    > [!NOTE]
    > <span data-ttu-id="7b0e9-162">HTTP Post oznámení můžete otestovat klepnutím **testovací oznámení** tlačítko, ale pouze po uložení konfigurace výstrahy.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-162">You can test the HTTP Post notification by clicking the **Test Notification** button, but only after the alert configuration has been saved.</span></span>
    > 
    > 
13. <span data-ttu-id="7b0e9-163">Klikněte **Uložit** tlačítko Uložit konfiguraci výstrah.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-163">Click the **Save** button to save your alert configuration.</span></span>  <span data-ttu-id="7b0e9-164">Pokud je zaškrtnuto **upozornění povolené** v kroku 5, upozornění je teď aktivní.</span><span class="sxs-lookup"><span data-stu-id="7b0e9-164">If you checked **Alert Enabled** in step 5, your alert is now active.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7b0e9-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7b0e9-165">Next Steps</span></span>
* <span data-ttu-id="7b0e9-166">Analýza [statistiky v reálném čase v Azure CDN](cdn-real-time-stats.md)</span><span class="sxs-lookup"><span data-stu-id="7b0e9-166">Analyze [Real-time stats in Azure CDN](cdn-real-time-stats.md)</span></span>
* <span data-ttu-id="7b0e9-167">Dig hlubší s [Rozšířené sestavy HTTP](cdn-advanced-http-reports.md)</span><span class="sxs-lookup"><span data-stu-id="7b0e9-167">Dig deeper with [advanced HTTP reports](cdn-advanced-http-reports.md)</span></span>
* <span data-ttu-id="7b0e9-168">Analýza [vzorce používání](cdn-analyze-usage-patterns.md)</span><span class="sxs-lookup"><span data-stu-id="7b0e9-168">Analyze [usage patterns](cdn-analyze-usage-patterns.md)</span></span>

