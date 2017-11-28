---
title: "výstrahy v reálném čase CDN aaaAzure | Microsoft Docs"
description: "V reálném čase výstrah v Microsoft Azure CDN. Výstrahy v reálném čase poskytují oznámení o výkonu hello hello koncových bodů ve vašem profilu CDN."
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
ms.openlocfilehash: 269c90437088bbc41bf899a8c02749e8e6f3006c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-alerts-in-microsoft-azure-cdn"></a><span data-ttu-id="7e48e-104">V reálném čase výstrah v Microsoft Azure CDN</span><span class="sxs-lookup"><span data-stu-id="7e48e-104">Real-time alerts in Microsoft Azure CDN</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="7e48e-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="7e48e-105">Overview</span></span>
<span data-ttu-id="7e48e-106">Tento dokument popisuje, v reálném čase výstrah v Microsoft Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="7e48e-106">This document explains real-time alerts in Microsoft Azure CDN.</span></span> <span data-ttu-id="7e48e-107">Tato funkce poskytuje v reálném čase oznámení o výkonu hello hello koncových bodů ve vašem profilu CDN.</span><span class="sxs-lookup"><span data-stu-id="7e48e-107">This functionality provides real-time notifications about hello performance of hello endpoints in your CDN profile.</span></span>  <span data-ttu-id="7e48e-108">Můžete nastavit e-mailu nebo HTTP upozornění na základě:</span><span class="sxs-lookup"><span data-stu-id="7e48e-108">You can set up email or HTTP alerts based on:</span></span>

* <span data-ttu-id="7e48e-109">Šířka pásma</span><span class="sxs-lookup"><span data-stu-id="7e48e-109">Bandwidth</span></span>
* <span data-ttu-id="7e48e-110">Stavové kódy</span><span class="sxs-lookup"><span data-stu-id="7e48e-110">Status Codes</span></span>
* <span data-ttu-id="7e48e-111">Stavy mezipaměti</span><span class="sxs-lookup"><span data-stu-id="7e48e-111">Cache Statuses</span></span>
* <span data-ttu-id="7e48e-112">Připojení</span><span class="sxs-lookup"><span data-stu-id="7e48e-112">Connections</span></span>

## <a name="creating-a-real-time-alert"></a><span data-ttu-id="7e48e-113">Vytváření v reálném čase výstrahy</span><span class="sxs-lookup"><span data-stu-id="7e48e-113">Creating a real-time alert</span></span>
1. <span data-ttu-id="7e48e-114">V hello [portálu Azure](https://portal.azure.com), procházet tooyour profil CDN.</span><span class="sxs-lookup"><span data-stu-id="7e48e-114">In hello [Azure Portal](https://portal.azure.com), browse tooyour CDN profile.</span></span>
   
    ![Okno profil CDN](./media/cdn-real-time-alerts/cdn-profile-blade.png)
2. <span data-ttu-id="7e48e-116">Z hello okno profil CDN, klikněte na tlačítko hello **spravovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7e48e-116">From hello CDN profile blade, click hello **Manage** button.</span></span>
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-real-time-alerts/cdn-manage-btn.png)
   
    <span data-ttu-id="7e48e-118">Otevře portál pro správu Hello CDN.</span><span class="sxs-lookup"><span data-stu-id="7e48e-118">hello CDN management portal opens.</span></span>
3. <span data-ttu-id="7e48e-119">Hover přes hello **Analytics** kartu a potom hover přes hello **statistiky v reálném čase** plovoucím panelem.</span><span class="sxs-lookup"><span data-stu-id="7e48e-119">Hover over hello **Analytics** tab, then hover over hello **Real-Time Stats** flyout.</span></span>  <span data-ttu-id="7e48e-120">Klikněte na **v reálném čase výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="7e48e-120">Click on **Real-Time Alerts**.</span></span>
   
    ![Portál pro správu CDN](./media/cdn-real-time-alerts/cdn-premium-portal.png)
   
    <span data-ttu-id="7e48e-122">Zobrazí se Hello seznam stávajících výstrahy konfigurací (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="7e48e-122">hello list of existing alert configurations (if any) is displayed.</span></span>
4. <span data-ttu-id="7e48e-123">Klikněte na tlačítko hello **přidat výstrahy** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7e48e-123">Click hello **Add Alert** button.</span></span>
   
    ![Výstrahy tlačítko Přidat](./media/cdn-real-time-alerts/cdn-add-alert.png)
   
    <span data-ttu-id="7e48e-125">Formulář pro vytvoření nového upozornění se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="7e48e-125">A form for creating a new alert is displayed.</span></span>
   
    ![Formulář nové výstrahy](./media/cdn-real-time-alerts/cdn-new-alert.png)
5. <span data-ttu-id="7e48e-127">Pokud chcete tuto výstrahu toobe active když kliknete na tlačítko **Uložit**, zkontrolujte hello **upozornění povolené** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="7e48e-127">If you want this alert toobe active when you click **Save**, check hello **Alert Enabled** checkbox.</span></span>
6. <span data-ttu-id="7e48e-128">Zadejte popisný název pro upozornění v hello **název** pole.</span><span class="sxs-lookup"><span data-stu-id="7e48e-128">Enter a descriptive name for your alert in hello **Name** field.</span></span>
7. <span data-ttu-id="7e48e-129">V hello **typ média** rozevíracího seznamu vyberte **velkého objektu HTTP**.</span><span class="sxs-lookup"><span data-stu-id="7e48e-129">In hello **Media Type** dropdown, select **HTTP Large Object**.</span></span>
   
    ![Typ média HTTP velký objekt vybrán](./media/cdn-real-time-alerts/cdn-http-large.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="7e48e-131">Je nutné vybrat **velkého objektu HTTP** jako hello **typ média**.</span><span class="sxs-lookup"><span data-stu-id="7e48e-131">You must select **HTTP Large Object** as hello **Media Type**.</span></span>  <span data-ttu-id="7e48e-132">Hello jiné možnosti nejsou používány nástrojem **Azure CDN společnosti Verizon**.</span><span class="sxs-lookup"><span data-stu-id="7e48e-132">hello other choices are not used by **Azure CDN from Verizon**.</span></span>  <span data-ttu-id="7e48e-133">Selhání tooselect **velkého objektu HTTP** způsobí, že upozornění aktivována toonever.</span><span class="sxs-lookup"><span data-stu-id="7e48e-133">Failure tooselect **HTTP Large Object** will cause your alert toonever be triggered.</span></span>
   > 
   > 
8. <span data-ttu-id="7e48e-134">Vytvoření **výraz** toomonitor výběrem **metrika**, **operátor**, a **aktivovat hodnotu**.</span><span class="sxs-lookup"><span data-stu-id="7e48e-134">Create an **Expression** toomonitor by selecting a **Metric**, **Operator**, and **Trigger value**.</span></span>
   
   * <span data-ttu-id="7e48e-135">Pro **metrika**, vyberte typ podmínky, které chcete monitorovaných hello.</span><span class="sxs-lookup"><span data-stu-id="7e48e-135">For **Metric**, select hello type of condition you want monitored.</span></span>  <span data-ttu-id="7e48e-136">**MB/s šířky pásma** je hello množství využití šířky pásma v MB za sekundu.</span><span class="sxs-lookup"><span data-stu-id="7e48e-136">**Bandwidth Mbps** is hello amount of bandwidth usage in megabits per second.</span></span>  <span data-ttu-id="7e48e-137">**Celkový počet připojení** je hello počet souběžných připojení tooour HTTP servery edge.</span><span class="sxs-lookup"><span data-stu-id="7e48e-137">**Total Connections** is hello number of concurrent HTTP connections tooour edge servers.</span></span>  <span data-ttu-id="7e48e-138">Definice hello různé stavy, které jsou mezipaměti a stavových kódů, najdete v části [Azure CDN mezipaměti stavové kódy](https://msdn.microsoft.com/library/mt759237.aspx) a [stavové kódy HTTP CDN Azure](https://msdn.microsoft.com/library/mt759238.aspx)</span><span class="sxs-lookup"><span data-stu-id="7e48e-138">For definitions of hello various cache statuses and status codes, see [Azure CDN Cache Status Codes](https://msdn.microsoft.com/library/mt759237.aspx) and [Azure CDN HTTP Status Codes](https://msdn.microsoft.com/library/mt759238.aspx)</span></span>
   * <span data-ttu-id="7e48e-139">**Operátor** je hello matematický operátor, který stanoví hello vztah mezi metrika hello a hodnota hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="7e48e-139">**Operator** is hello mathematical operator that establishes hello relationship between hello metric and hello trigger value.</span></span>
   * <span data-ttu-id="7e48e-140">**Aktivovat hodnota** je hello prahovou hodnotu, která musí být splněny, než bude odesláno oznámení.</span><span class="sxs-lookup"><span data-stu-id="7e48e-140">**Trigger Value** is hello threshold value that must be met before a notification will be sent out.</span></span>
     
     <span data-ttu-id="7e48e-141">V následujícím příkladu se text hello hello výraz, který vytvořil jsem označuje, že nastavit jako toobe upozorněni, když je větší než 25 hello počet stavový kód 404.</span><span class="sxs-lookup"><span data-stu-id="7e48e-141">In hello below example, hello expression I have created indicates that I would like toobe notified when hello number of 404 status codes is greater than 25.</span></span>
     
     ![V reálném čase výstrahy ukázka výrazu](./media/cdn-real-time-alerts/cdn-expression.png)
9. <span data-ttu-id="7e48e-143">Pro **Interval**, zadejte, jak často chcete hello výrazu vyhodnoceného.</span><span class="sxs-lookup"><span data-stu-id="7e48e-143">For **Interval**, enter how frequently you would like hello expression evaluated.</span></span>
10. <span data-ttu-id="7e48e-144">V hello **upozornit na** rozevíracího seznamu vyberte, pokud chcete toobe upozorněni, když hello je výraz pravdivý.</span><span class="sxs-lookup"><span data-stu-id="7e48e-144">In hello **Notify on** dropdown, select when you would like toobe notified when hello expression is true.</span></span>
    
    * <span data-ttu-id="7e48e-145">**Podmínka spuštění** označuje, že bude být oznámení odesláno, pokud hello zadána, je nejprve zjistil podmínku.</span><span class="sxs-lookup"><span data-stu-id="7e48e-145">**Condition Start** indicates that a notification will be sent when hello specified condition is first detected.</span></span>
    * <span data-ttu-id="7e48e-146">**Podmínka End** označuje, že oznámení odešle při hello zadaná podmínka je již nebudou zjištěna.</span><span class="sxs-lookup"><span data-stu-id="7e48e-146">**Condition End** indicates that a notification will be sent when hello specified condition is no longer detected.</span></span> <span data-ttu-id="7e48e-147">Toto oznámení se spustí pouze po zjištěna naše síť systému pro monitorování této hello zadaná podmínka došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="7e48e-147">This notification can only be triggered after our network monitoring system detected that hello specified condition occurred.</span></span>
    * <span data-ttu-id="7e48e-148">**Průběžné** označuje bude odesláno upozornění pokaždé, když hello systém monitorování sítě zjistí hello zadaná podmínka.</span><span class="sxs-lookup"><span data-stu-id="7e48e-148">**Continuous** indicates that a notification will be sent each time that hello network monitoring system detects hello specified condition.</span></span> <span data-ttu-id="7e48e-149">Mějte na paměti tuto síť hello monitorování systému bude pouze kontrola jednou intervalu pro hello zadaná podmínka.</span><span class="sxs-lookup"><span data-stu-id="7e48e-149">Keep in mind that hello network monitoring system will only check once per interval for hello specified condition.</span></span>
    * <span data-ttu-id="7e48e-150">**Podmínka počáteční a koncové** označuje, že hello prvním hello zadanou podmínku je a znovu při hello podmínka je už bude odesláno upozornění.</span><span class="sxs-lookup"><span data-stu-id="7e48e-150">**Condition Start and End** indicates that a notification will be sent hello first time that hello specified condition is detected and once again when hello condition is no longer detected.</span></span>
11. <span data-ttu-id="7e48e-151">Pokud chcete tooreceive oznámení e-mailem, zkontrolujte hello **oznámení e-mailem** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="7e48e-151">If you want tooreceive notifications by email, check hello **Notify by Email** checkbox.</span></span>  
    
    ![Upozornit e-mailu formuláři](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    <span data-ttu-id="7e48e-153">V hello **k** pole, zadejte je místo, kam chcete oznámení odeslaných hello e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="7e48e-153">In hello **To** field, enter hello email address you where you want notifications sent.</span></span> <span data-ttu-id="7e48e-154">Pro **subjektu** a **textu**, můžete ponechat výchozí hello nebo může přizpůsobit uvítací zprávu pomocí hello **k dispozici klíčová slova** seznamu toodynamically vložení dat výstrah při je odeslána zpráva Hello.</span><span class="sxs-lookup"><span data-stu-id="7e48e-154">For **Subject** and **Body**, you may leave hello default, or you may customize hello message using hello **Available keywords** list toodynamically insert alert data when hello message is sent.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="7e48e-155">Hello e-mailové oznámení můžete otestovat klepnutím hello **testovací oznámení** tlačítko, ale pouze po uložení konfigurace výstrah hello.</span><span class="sxs-lookup"><span data-stu-id="7e48e-155">You can test hello email notification by clicking hello **Test Notification** button, but only after hello alert configuration has been saved.</span></span>
    > 
    > 
12. <span data-ttu-id="7e48e-156">Pokud chcete oznámení toobe odeslány tooa webového serveru, zkontrolujte hello **oznámení pomocí HTTP Post** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="7e48e-156">If you want notifications toobe posted tooa web server, check hello **Notify by HTTP Post** checkbox.</span></span>
    
    ![Upozornit pomocí HTTP Post formuláře](./media/cdn-real-time-alerts/cdn-notify-http.png)
    
    <span data-ttu-id="7e48e-158">V hello **Url** pole, zadejte adresu URL hello je místo, kam chcete zprávu hello HTTP odeslány.</span><span class="sxs-lookup"><span data-stu-id="7e48e-158">In hello **Url** field, enter hello URL you where you want hello HTTP message posted.</span></span> <span data-ttu-id="7e48e-159">V hello **hlavičky** textovému poli, zadejte hello HTTP hlavičky toobe odeslaných v požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="7e48e-159">In hello **Headers** textbox, enter hello HTTP headers toobe sent in hello request.</span></span>  <span data-ttu-id="7e48e-160">Pro **textu** může přizpůsobit uvítací zprávu pomocí hello **k dispozici klíčová slova** seznamu toodynamically vložení dat výstrah při odeslání zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="7e48e-160">For **Body** you may customize hello message using hello **Available keywords** list toodynamically insert alert data when hello message is sent.</span></span>  <span data-ttu-id="7e48e-161">**Hlavičky** a **textu** výchozí tooan XML datové části podobné toohello následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="7e48e-161">**Headers** and **Body** default tooan XML payload similar toohello below example.</span></span>
    
    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```
    
    > [!NOTE]
    > <span data-ttu-id="7e48e-162">Hello HTTP Post oznámení můžete otestovat klepnutím hello **testovací oznámení** tlačítko, ale pouze po uložení konfigurace výstrah hello.</span><span class="sxs-lookup"><span data-stu-id="7e48e-162">You can test hello HTTP Post notification by clicking hello **Test Notification** button, but only after hello alert configuration has been saved.</span></span>
    > 
    > 
13. <span data-ttu-id="7e48e-163">Klikněte na tlačítko hello **Uložit** tlačítko toosave konfiguraci výstrah.</span><span class="sxs-lookup"><span data-stu-id="7e48e-163">Click hello **Save** button toosave your alert configuration.</span></span>  <span data-ttu-id="7e48e-164">Pokud je zaškrtnuto **upozornění povolené** v kroku 5, upozornění je teď aktivní.</span><span class="sxs-lookup"><span data-stu-id="7e48e-164">If you checked **Alert Enabled** in step 5, your alert is now active.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e48e-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7e48e-165">Next Steps</span></span>
* <span data-ttu-id="7e48e-166">Analýza [statistiky v reálném čase v Azure CDN](cdn-real-time-stats.md)</span><span class="sxs-lookup"><span data-stu-id="7e48e-166">Analyze [Real-time stats in Azure CDN](cdn-real-time-stats.md)</span></span>
* <span data-ttu-id="7e48e-167">Dig hlubší s [Rozšířené sestavy HTTP](cdn-advanced-http-reports.md)</span><span class="sxs-lookup"><span data-stu-id="7e48e-167">Dig deeper with [advanced HTTP reports](cdn-advanced-http-reports.md)</span></span>
* <span data-ttu-id="7e48e-168">Analýza [vzorce používání](cdn-analyze-usage-patterns.md)</span><span class="sxs-lookup"><span data-stu-id="7e48e-168">Analyze [usage patterns](cdn-analyze-usage-patterns.md)</span></span>

